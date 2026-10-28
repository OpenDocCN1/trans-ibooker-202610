

10. 根据区域设置选择数据库位置并单击 **Enable** 按钮：

   ![](img/efea53d01337e7b223389b38dcdb0160_192_0.png)

   *Figure 6.18 – Select the database location*

恭喜！你已创建了一个新的 Cloud Firestore 数据库。在下一节中，我们将学习如何将新数据库投入使用，通过我们的移动应用程序保存数据。

## Integrating the AngularFire library

AngularFire 库是一个 Angular 库，我们可以在 Angular 应用中使用它来与 Firebase 家族产品（如 Cloud Firestore 和 Storage 服务）交互。要在我们的应用中安装它，请运行以下 Angular CLI 命令：

```bash
ng add @angular/fire
```

前面的命令会要求你选择我们在上一节中创建的 Firebase 项目，并相应地修改应用结构以使用 AngularFire。现在让我们看看如何在我们的应用中使用 AngularFire 库：

1. 打开 `src\environments\environment.ts` 文件并将你的 Firebase 配置内容复制到 environment 对象中，如下所示：

   ```typescript
   export const environment = {
     production: false,
     firebaseConfig: {
       apiKey: '<Your API key>',
       authDomain: '<Your project auth domain>',
       projectId: '<Your project ID>',
       storageBucket: '<Your storage bucket>',
       messagingSenderId: '<Your messaging sender ID>',
       appId: '<Your application ID>'
     }
   };
   ```

   environment.ts 文件在我们以开发模式运行应用时使用。

2. 我们在生产模式下运行应用时也需要相同的 Firebase 配置。将 Firebase 配置内容复制到生产模式下使用的 `src\environments\environment.prod.ts` 文件中。

3. 打开 app.module.ts 文件并添加以下导入语句：

   ```typescript
   import { AngularFireModule } from '@angular/fire';
   import { AngularFirestoreModule } from '@angular/fire/firestore';
   import { AngularFireStorageModule } from '@angular/fire/storage';
   import { environment } from '../environments/environment';
   ```

   AngularFireModule 是 AngularFire 库的主模块。AngularFirestoreModule 是 AngularFire 的特定模块，我们在处理 Cloud Firestore 数据库时需要它。AngularFireStorageModule 是我们可以用来与 Storage 服务交互的模块。

   在前面的代码中，我们还导入了包含 Firebase 配置的 environment 对象。

   > **Important note**
   > 我们从开发环境文件导入 environment 对象，而不是生产环境文件。Angular 框架足够智能，可以理解我们当前以何种环境运行应用，并在运行时使用适当的文件。

4. 还将 AngularFire 模块添加到 @NgModule 装饰器的 imports 数组中：

   ```typescript
   imports: [BrowserModule, IonicModule.forRoot(),
     AppRoutingModule,
     AngularFireModule.initializeApp
       (environment.firebaseConfig),
     AngularFirestoreModule,
     AngularFireStorageModule
   ]
   ```

   在前面的代码中，我们使用 `AngularFireModule` 类的 `initializeApp` 方法将我们的应用注册到我们的 Firebase 项目。

5. 在继续之前，我们需要为将保存在 Cloud Firestore 数据库中的数据创建一个模型。执行以下 Ionic CLI 命令来创建它：

   ```bash
   ionic generate interface photo
   ```

6. 打开 `photo.ts` 文件并在 Photo 接口中添加以下属性：

   ```typescript
   export interface Photo {
     url: string;
     lat: number;
     lng: number;
   }
   ```

   url 属性将是实际照片的 URL，而 `lat`/`lng` 属性表示当前位置的纬度和经度。

7. 打开 `photo.service.ts` 文件并添加以下导入语句：

   ```typescript
   import { AngularFirestore } from '@angular/fire/firestore';
   import { AngularFireStorage } from '@angular/fire/storage';
   import { Photo } from './photo';
   ```

   AngularFirestore 服务包含我们与 Cloud Firestore 数据库交互所需的所有方法。AngularFireStorage 服务包含用于将文件上传到 Storage 服务的方法。

8. 将两个服务都注入到 PhotoService 类的构造函数中：

   ```typescript
   constructor(private firestore: AngularFirestore, private storage: AngularFireStorage) {}
   ```

9. 创建以下方法以将照片保存到 Firebase：

   ```typescript
   private async savePhoto(dataUrl: string, latitude: number, longitude: number) {
     const name = new Date().getUTCMilliseconds().toString();
     const upload = await this.storage.ref(name).putString(dataUrl, 'data_url');
     const photoUrl = await upload.ref.getDownloadURL();
     await this.firestore.collection<Photo>('photos').add({
       url: photoUrl,
       lat: latitude,
       lng: longitude
     });
   }
   ```

   首先，我们为照片创建一个随机名称，并使用 storage 变量的 putString 方法将其上传到 Firebase 存储。一旦上传完成，我们使用 getDownloadURL 方法获取可下载的 URL，可用于访问该照片。最后，我们使用 add 方法在 firestore 变量的 collection 属性中添加一个新的 Photo 对象。我们使用 collection 属性，因为我们希望在应用中处理照片列表。

   > **Tip**
   > firestore 变量还包含一个 doc 属性，可在我们想要处理单个对象时使用。collection 属性内部维护着一个 doc 对象的列表。

10. 修改 takePhoto 方法以调用我们在步骤 9 中创建的 savePhoto 方法：

    ```typescript
    async takePhoto() {
      const { latitude, longitude } = await this.getLocation();

      const cameraPhoto = await Camera.getPhoto({
        resultType: CameraResultType.DataUrl,
        source: CameraSource.Camera,
        quality: 100
      });

      await this.savePhoto(cameraPhoto.dataUrl, latitude, longitude);
    }
    ```

我们现在准备好检查拍照过程的完整功能：

1. 使用以下 Ionic CLI 命令构建应用：

   ```bash
   ionic build
   ```

2. 运行以下 Capacitor 命令将应用包复制到原生移动项目中：

   ```bash
   ionic cap copy
   ```

3. 使用 Capacitor 的 open 命令打开原生移动项目，并使用相应的 IDE 运行项目。

4. 打开你的应用的 Firebase 控制台并在 **Build** 部分选择 **Storage** 选项。选择 **Rules** 选项卡，删除第 5 行的身份验证检查并单击 **Publish** 按钮。 resulting rules 应如下所示：

   ```text
   rules_version = '2';
   service firebase.storage {
     match /b/{bucket}/o {
       match /{allPaths=**} {
         allow read, write;
       }
     }
   }
   ```

5. 使用应用拍摄一张漂亮的照片。为验证你的照片是否已成功上传到 Firebase，请选择 **Files** 选项卡。你应该会看到如下条目：

   ![](img/efea53d01337e7b223389b38dcdb0160_196_0.png)

   *Figure 6.19 – Firebase storage*

   在前面的屏幕截图中，名为 **669** 的文件是你拍摄的照片的物理文件。

6. 同样，选择 **Build** 部分中的 **Firestore Database** 选项，你应该会看到如下内容：

   ![](img/efea53d01337e7b223389b38dcdb0160_197_0.png)

   在前面的屏幕截图中，**1oFxxWgQseIwqWUrYBkN** 条目是照片的逻辑对象，其中包含实际文件的 URL 及其位置数据。

我们应用的第一页现在功能完整。我们已经完成了拍摄照片并将其连同位置数据一起上传到云端的全过程。我们从设置和配置 Firebase 项目开始，最后学习了如何使用 AngularFire 库与该项目交互。在下一节中，我们将通过实现应用的第二页到达最终目的地。

## Previewing photos with CesiumJS

我们应用的下一个功能将是在 3D 地图上显示我们用相机拍摄的所有照片。CesiumJS 库提供了一个带有 3D 地球仪的查看器，我们可以用它来可视化各种事物，例如特定位置的图像。我们应用的这个新功能将包括以下内容：

- Configuring CesiumJS
- Displaying photos on the viewer

我们将首先学习如何设置 CesiumJS 库。

### Configuring CesiumJS

CesiumJS 库是一个 npm 包，我们可以安装它来开始使用 3D 地图和可视化：

1. 运行以下 npm 命令安装 CesiumJS：

   ```bash
   npm install cesium
   ```

