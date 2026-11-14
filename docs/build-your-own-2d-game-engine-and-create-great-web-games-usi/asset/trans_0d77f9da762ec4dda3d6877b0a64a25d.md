
# 实现音频组件

虽然音频和文本文件完全不同，但从游戏引擎实现的角度来看，有两个重要的相似之处。首先，两者都是外部资源，因此将在 `src/engine/resources` 文件夹中以引擎组件的形式类似地实现。其次，两者都涉及具有定义良好的 API 实用程序的标准化文件格式。将使用 Web Audio API 来实际检索和播放声音文件。尽管该 API 提供了广泛的功能，但为了专注于游戏引擎开发的其他部分，这里仅讨论对背景音频和效果提示的基本支持。

## 注意

感兴趣的读者可以从 [www.w3.org/TR/webaudio/](http://www.w3.org/TR/webaudio/) 了解更多关于 Web Audio API 的信息。

## 浏览器音频策略

包括 Chrome 在内的某些浏览器的最新策略是，在用户首次交互之前，不允许播放音频。这意味着上下文创建将导致 Chrome 发出初始警告，并输出到运行时浏览器控制台。音频仅在用户输入（例如鼠标点击或键盘事件）后才会播放。

## 实现步骤

### 步骤 1：创建 `audio.js`

在 `src/engine/resources` 文件夹中，创建一个新文件并将其命名为 `audio.js`。该文件将实现音频组件的模块。该组件必须支持两类功能：音频文件的加载和卸载，以及为游戏开发者提供音频文件内容的播放和控制。

### 步骤 2：导入核心资源管理

加载和卸载与文本和 xml 模块的实现类似，其中核心资源管理功能从 `resource_map` 导入：

```javascript
"use strict";
import * as map from "../core/resource_map.js";
// functions from resource_map
let unload = map.unload;
let has = map.has;
```

### 步骤 3：定义解码和解析函数

定义解码和解析函数，并调用 `resource_map.loadDecodeParse()` 函数来加载音频文件。注意，在 `resource_map` 和引擎其余基础设施的支持下，外部资源的加载和卸载变得简单直接。

```javascript
function decodeResource(data) {
    return data.arrayBuffer();
}
function parseResource(data) {
    return mAudioContext.decodeAudioData(data);
}
function load(path) {
    return map.loadDecodeParse(path, decodeResource, parseResource);
}
```

### 步骤 4：声明音频控制变量

随着加载功能的完成，现在可以定义音频控制和操作函数了。声明变量以维护对 Web Audio 上下文和背景音乐的引用，并控制音量。

```javascript
let mAudioContext = null;
let mBackgroundAudio = null;
// volume control support
let mBackgroundGain = null; // background volume
let mCueGain = null;        // cue/special effects volume
let mMasterGain = null;     // overall/master volume
let kDefaultInitGain = 0.1;
```

### 步骤 5：定义 `init()` 函数

定义 `init()` 函数以创建并存储对 Web Audio 上下文的引用在 `mAudioContext` 中，并初始化背景、提示以及影响两者的主音量增益控制。在所有情况下，音量增益 0 对应无音频，1 表示最大响度。

```javascript
function init() {
    try {
        let AudioContext = window.AudioContext ||
                           window.webkitAudioContext;
        mAudioContext = new AudioContext();
        // connect Master volume control
        mMasterGain = mAudioContext.createGain();
        mMasterGain.connect(mAudioContext.destination);
        // set default Master volume
        mMasterGain.gain.value = kDefaultInitGain;
        // connect Background volume control
        mBackgroundGain = mAudioContext.createGain();
        mBackgroundGain.connect(mMasterGain);
        // set default Background volume
        mBackgroundGain.gain.value = 1.0;
        // connect Cue volume control
        mCueGain = mAudioContext.createGain();
        mCueGain.connect(mMasterGain);
        // set default Cue volume
        mCueGain.gain.value = 1.0;
    } catch (e) {
        throw new Error("...");
    }
}
```

### 步骤 6：定义 `playCue()` 函数

定义 `playCue()` 函数以在适当的音量控制下播放音频剪辑的整个持续时间。此函数使用音频文件路径作为资源名称，从 `resource_map` 中查找已加载的资源，然后调用 Web Audio API 播放音频剪辑。注意，不保留对 source 变量的引用，因此一旦开始，就无法停止相应的音频剪辑。游戏应调用此函数播放短小的音频片段作为提示。

```javascript
function playCue(path, volume) {
    let source = mAudioContext.createBufferSource();
    source.buffer = map.get(path);
    source.start(0);
    // volume support for cue
    source.connect(mCueGain);
    mCueGain.gain.value = volume;
}
```

### 步骤 7：定义背景音乐控制

定义播放、停止、查询和控制背景音乐音量的功能。在这种情况下，`mBackgroundAudio` 变量保存对当前正在播放音频的引用，因此可以停止剪辑或更改其音量。

```javascript
function playBackground(path, volume) {
    if (has(path)) {
        stopBackground();
        mBackgroundAudio = mAudioContext.createBufferSource();
        mBackgroundAudio.buffer = map.get(path);
        mBackgroundAudio.loop = true;
        mBackgroundAudio.start(0);
        // connect volume accordingly
        mBackgroundAudio.connect(mBackgroundGain);
        setBackgroundVolume(volume);
    }
}
function stopBackground() {
    if (mBackgroundAudio !== null) {
        mBackgroundAudio.stop(0);
        mBackgroundAudio = null;
    }
}
function isBackgroundPlaying() {
    return (mBackgroundAudio !== null);
}
function setBackgroundVolume(volume) {
    if (mBackgroundGain !== null) {
        mBackgroundGain.gain.value = volume;
    }
}
function incBackgroundVolume(increment) {
    if (mBackgroundGain !== null) {
        mBackgroundGain.gain.value += increment;
        // need this since volume increases when negative
        if (mBackgroundGain.gain.value < 0) {
            setBackgroundVolume(0);
        }
    }
}
```

### 步骤 8：定义主音量控制

定义控制主音量的函数，它同时调整提示音和背景音乐的音量：

```javascript
function setMasterVolume(volume) {
    if (mMasterGain !== null) {
        mMasterGain.gain.value = volume;
    }
}
function incMasterVolume(increment) {
    if (mMasterGain !== null) {
        mMasterGain.gain.value += increment;
        // need this since volume increases when negative
        if (mMasterGain.gain.value < 0) {
            mMasterGain.gain.value = 0;
        }
    }
}
```

### 步骤 9：定义 `cleanUp()` 函数

定义 `cleanUp()` 函数以释放分配的 HTML5 资源：

```javascript
function cleanUp() {
    mAudioContext.close();
    mAudioContext = null;
}
```

### 步骤 10：导出模块函数

记得导出该模块的函数：

```javascript
export {init, cleanUp,
        has, load, unload,
        playCue,
        playBackground, stopBackground, isBackgroundPlaying,
        setBackgroundVolume, incBackgroundVolume,
        setMasterVolume, incMasterVolume }
```

## 将音频模块导出给客户端

编辑 `index.js` 文件以从 `audio.js` 导入，相应地初始化和清理模块，并导出给客户端：

```javascript
... identical to previous code ...
import * as audio from "./resources/audio.js";
... identical to previous code ...
function init(htmlCanvasID) {
    glSys.init(htmlCanvasID);
    vertexBuffer.init();
    shaderResources.init();
    input.init();
    audio.init();
}
function cleanUp() {
    loop.cleanUp();
    audio.cleanUp();
    input.cleanUp();
    shaderResources.cleanUp();
    vertexBuffer.cleanUp();
    glSys.cleanUp();
}
... identical to previous code ...
export default {
    // resource support
    audio, text, xml
    ... identical to previous code ...
}
```

## 测试音频组件
