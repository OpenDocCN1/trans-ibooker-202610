
### 5.1.1 bikes (黑盒)

意图/职责：`bikes` 提供用于读取、创建和操作自行车及其里程的外部 API，以及计算统计数据和生成图表。

下表列出了不同的接口：

![图 4.11：REST 接口](image/B15546_04_11.jpg)

###### 图 4.11：REST 接口

#### 文件

`bikes` 模块及其所有依赖项都包含在名为 `ac.simons.biking2.bikes` 的 Java 包中。

### 5.1.2 tracks (黑盒)

意图/职责：`tracks` 管理文件上传（TCX 文件），将其转换为 GPX 文件，并使用 GPSBabel 计算其周围矩形（包络）。它还提供 oEmbed 接口，可将 URL 解析为可嵌入的轨迹。

#### 文件

![图 4.12：文件接口](image/B15546_04_12.jpg)

###### 图 4.12：文件接口

`tracks` 模块及其所有依赖项都包含在名为 `ac.simons.biking2.tracks` 的 Java 包中。

### 5.1.3 trips (黑盒)

意图/职责：`trips` 管理单日行驶的距离，且不与自行车关联：

![图 4.13：旅行](image/B15546_04_13.jpg)

###### 图 4.13：旅行

#### 文件

`trips` 模块及其所有依赖项都包含在名为 `ac.simons.biking2.trips` 的 Java 包中。

### 5.1.4 locations (黑盒)

`locations` 以接近实时的方式存储带时间戳的位置，并提供最近 30 分钟位置的访问。

#### 接口

下表描述了所使用的各种接口：

![图 4.14：接口](image/B15546_04_14.jpg)

###### 图 4.14：接口

#### 注释

有关 OwnTracks 的更多信息，请参阅：[https://owntracks.org/booklet/tech/json/](https://owntracks.org/booklet/tech/json/)

#### 文件

`locations` 模块及其所有依赖项都包含在名为 `ac.simons.biking2.tracker` 的 Java 包中。该模块通过 `ac.simons.biking2.config.TrackerConfig` 类进行配置。

### 5.1.5 bikingPictures (黑盒)

意图/职责：`bikingPictures` 用于定期检查 Daily Fratze 的 RSS 源，同时收集新图像并将其本地存储。它还提供获取所有收集图像的 API。

#### 接口

![图 4.15：接口](image/B15546_04_15.jpg)

###### 图 4.15：接口

#### 文件

`bikingPictures` 模块及其所有依赖项都包含在名为 `ac.simons.biking2.bikingpictures` 的 Java 包中。

### 5.1.6 galleryPictures (黑盒)

意图/职责：`galleryPictures` 管理文件上传（图像）。它将它们本地存储并提供 RSS 接口以获取元数据和图像数据。

#### 接口

![图 4.16：接口](image/B15546_04_16.jpg)

###### 图 4.16：接口

#### 文件

`galleryPictures` 模块及其所有依赖项都包含在名为 `ac.simons.biking2.gallerypictures` 的 Java 包中。

### 5.2 构建块 – 第 2 级

### 5.2.1 bikes (白盒)

![图 4.17：构建块 – bikes](image/B15546_04_17.jpg)

###### 图 4.17：构建块 – bikes

`BikeRepository` 是基于 Spring Data JPA 的 `BikeEntities` 仓库。`BikeController` 和 `ChartsController` 访问它以检索和存储 `BikeEntity` 实例并提供外部接口。

#### 包含的黑盒

![图 4.18：黑盒](image/B15546_04_18.jpg)

###### 图 4.18：黑盒

### 5.2.2 tracks (白盒)

![图 4.19：构建块 – tracks](image/B15546_04_19.jpg)

###### 图 4.19：构建块 – tracks

`TrackRepository` 是基于 Spring Data JPA 的 `TrackEntities` 仓库。`TracksController` 和 `OembedController` 访问它以检索和存储 `TrackEntity` 实例并提供外部接口。

#### 包含的黑盒

![图 4.20：黑盒](image/B15546_04_20.jpg)

###### 图 4.20：黑盒

### 5.2.3 trips (白盒)

![图 4.21：构建块 – trips](image/B15546_04_21.jpg)

###### 图 4.21：构建块 – trips

`AssortedTripRepository` 是基于 Spring Data JPA 的 `AssortedTripEntities` 仓库。`TripsController` 访问它以检索和存储 `TrackEntity` 实例并提供外部接口。

### 5.2.4 locations (白盒)

![图 4.22：构建块 – locations](image/B15546_04_22.jpg)

###### 图 4.22：构建块 – locations

位置通过名为 `LocationRepository` 的基于 Spring Data JPA 的仓库进行存储和读取。该仓库仅通过 `LocationService` 访问。这通过 `SimpMessagingTemplate` 为已连接客户端提供实时更新，而 `LocationController` 使用该服务提供对最近 30 分钟内创建的所有位置的访问。

新位置由服务通过 `LocationController` 形式的 REST 接口或通过 MQTT 通道上的 `MessageListener` 创建。

### 5.2.5 bikingPictures (白盒)

![图 4.23：构建块 – bikingPictures](image/B15546_04_23.jpg)

###### 图 4.23：构建块 – bikingPictures

使用名为 `BikingPicturesRepository` 的 Spring Data JPA 仓库来访问 `BikingPictureEntities`。用于读取图片的外部 REST API 由 `BikingPicturesController` 实现。RSS 源由 `FetchBikingPicturesJob` 使用名为 "`rss`" 的 `JAXBContext` 读取。受各种方式保护的图像文件 URL 通过 `DailyFratzeProvider` 提供给该作业。

#### 包含的黑盒

![图 4.24：黑盒](image/B15546_04_24.jpg)

###### 图 4.24：黑盒

### 5.2.6 galleryPictures (白盒)

![图 4.25：构建块 – galleryPictures](image/B15546_04_25.jpg)

###### 图 4.25：构建块 – galleryPictures

`GalleryPictureRepository` 是基于 Spring Data JPA 的 `GalleryPictureEntity` 仓库。

`GalleryController` 访问它以检索和存储 `GalleryPictureEntity` 实例并提供外部接口。

## IV.6 运行时视图

与 `biking2` 的用户交互和错误处理相当基础。我挑选了两个实际运行时视图很有趣用例。

### 创建新轨迹

下图描述了创建新轨迹的过程：

![图 4.26 – 创建新轨迹](image/B15546_04_26.jpg)

###### 图 4.26 – 创建新轨迹

### 从 Daily Fratze 获取骑行图片

下图显示了如何从 Daily Fratze 获取骑行图片：

![图 4.27：获取骑行图片](image/B15546_04_26.jpg)

###### 图 4.27：获取骑行图片

## IV.7 部署视图

下图显示了自行车应用的部署视图：

![图 4.28：部署视图](image/B15546_04_28.jpg)

###### 图 4.28：部署视图

下表描述了应用程序部署中使用的各种节点：

![图 4.29：节点](image/B15546_04_29.jpg)

###### 图 4.29：节点

## IV.8 技术和横切概念

本节重点介绍应用程序涉及的技术概念和其他重要概念。

### 领域模型

`biking2` 是一个以数据为中心的应用；因此，一切都基于以下实体关系图（ERD）：

![图 4.30：biking2 中的实体](image/B15546_04_30.jpg)

###### 图 4.30：biking2 中的实体

这些实体体现为表。

#### 表

![图 4.31：表](image/B15546_04_31.jpg)

###### 图 4.31：表

这些表映射到以下领域模型：

![图 4.32：领域模型（JPA 实体）](image/B15546_04_32.jpg)

###### 图 4.32：领域模型（JPA 实体）

#### 领域模型

![图 4.33：领域模型组件](image/B15546_04_33.jpg)

###### 图 4.33：领域模型组件

#### 关于 `BikeEntity` 的重要业务方法
