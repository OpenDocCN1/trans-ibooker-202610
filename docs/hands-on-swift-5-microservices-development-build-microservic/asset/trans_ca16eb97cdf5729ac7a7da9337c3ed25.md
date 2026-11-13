

# 5. [前言](#ce03845f-67ac-492c-bd5f-c2641b361cb2.xhtml)

# 6. [贡献者](#b4736321-59e6-4b08-a65a-ab0cb79685c2.xhtml)

## 6.1 [关于作者](#af720da7-98bc-4d66-a2fb-63ab4b3fdd35.xhtml)

## 6.2 [关于审阅者](#2e5383e5-2948-409c-82ed-69b1591b695e.xhtml)

## 6.3 [Packt 正在寻找像您这样的作者](#25d9fdc4-a2e1-4727-8426-ad38a83b9802.xhtml)

# 7. [序言](#03663bda-6fa9-48e8-85ff-679622e4aa4d.xhtml)

## 7.1 [适读人群](#c6b71e12-c874-4126-8bdc-d98e3250191b.xhtml)

## 7.2 [本书内容](#18402aca-eda7-4a95-90e3-4e3f6c10fac8.xhtml)

## 7.3 [如何充分利用本书](#31363a23-7d8a-4461-8175-0194780c35ab.xhtml)

### 7.3.1 [下载示例代码文件](#117e517e-4c07-424a-866c-91111230a5fa.xhtml)

### 7.3.2 [约定俗成的用法](#875738ea-d3d7-4578-b1f4-2edfef41a5b0.xhtml)

## 7.4 [联系我们](#3460f269-0b27-40bf-ac82-b96b8dc1cc29.xhtml)

### 7.4.1 [评论](#cfad2cb3-523f-4234-bc82-d8fed62bd927.xhtml)

# 8. [微服务简介](#992b21d0-d31a-4c8c-9f75-40c93fdd4526.xhtml)

## 8.1 [什么是微服务？](#bc161592-a347-4d9a-abe0-61e5750bb04d.xhtml)

### 8.1.1 [使用微服务的规则](#342cce68-1336-45c4-80c5-92d6c59659cf.xhtml)

## 8.2 [理解为何选择微服务](#d65ccb3e-60a2-4402-b885-38e78b05c2d7.xhtml)

### 8.2.1 [高效的团队管理](#9ef88982-57fa-46da-8ebf-a0395d26c310.xhtml)

### 8.2.2 [可复用的代码库](#e8f601bc-6a16-4632-a626-ae0a57761295.xhtml)

### 8.2.3 [灵活的功能范围](#b935692e-fab2-46c7-8ce5-55cc14932eb0.xhtml)

### 8.2.4 [可维护性](#1b838fab-0082-4c3e-b1ba-d863ada3fa14.xhtml)

### 8.2.5 [可扩展性](#27758221-912e-4abd-ba4d-ed948a7519d4.xhtml)

#### 8.2.5.1 [自动扩缩容](#fff996fa-f7ba-42c5-a3c8-af3319940b21.xhtml)

#### 8.2.5.2 [灵活的成本](#e37e6492-46e9-496d-b7cb-c645a48d901a.xhtml)

#### 8.2.5.3 [面向服务的可用性](#19e4d7a3-4772-4246-9cd8-6c1b38a89db7.xhtml)

### 8.2.6 [混合技术栈](#3b29659a-8d8a-4152-9fd6-c2fb1d29d6c3.xhtml)

## 8.3 [理解为何不选择微服务](#f6e0eaf2-4e42-4464-a6af-69e605717308.xhtml)

### 8.3.1 [初期工作量更大](#89c519c3-bff6-422c-bbc0-6fdfa26e825b.xhtml)

### 8.3.2 [复杂性增加](#7f124d68-3b48-466a-9c86-3cb536767a88.xhtml)

### 8.3.3 [调试与故障排查](#9a979a8b-4f30-4bfd-93f2-babc1344d234.xhtml)

## 8.4 [谁在使用微服务？](#ee8e963f-d97a-4737-8d86-8cf6739e3a45.xhtml)

### 8.4.1 [Amazon](#aaffcf72-5fa7-474c-b198-3dc13950967f.xhtml)

### 8.4.2 [Netflix](#8e993317-4dc2-49ea-8b21-97b8ca0faba7.xhtml)

### 8.4.3 [Uber](#442bea8f-fe2e-4371-a412-94a5cae3f032.xhtml)

### 8.4.4 [许多其他公司](#2b3e9604-5111-45e1-9c54-6b4bf99ffeec.xhtml)

## 8.5 [对比单体架构与微服务](#aced40d2-7d33-4f35-ad85-9f9f3eb5eb26.xhtml)

### 8.5.1 [可复用的代码库](#7fdaf42d-6735-4b0e-82e1-86a809def97e.xhtml)

### 8.5.2 [多版本在线](#b088dcf0-03cd-4bfd-a8e1-26871b1db864.xhtml)

### 8.5.3 [零停机](#217a15b4-77e9-4ac4-8fa8-ad07bcb8542f.xhtml)

### 8.5.4 [更新速度可快可慢，按需而定](#e6b4ac0f-4a4a-4ccf-8a48-658f8b760a52.xhtml)

### 8.5.5 [常见微服务用例](#50bcddcf-a7e2-49aa-ac15-27cbfa572801.xhtml)

## 8.6 [从单体架构升级到微服务](#2db6c5bf-8155-459b-a121-d03e36d16af2.xhtml)

### 8.6.1 [解耦业务逻辑](#959ed527-38c7-4132-a5c5-721f80ac15f1.xhtml)

#### 8.6.1.1 [应用程序有何功能？](#4dc619c0-a40a-4763-8169-841481a3266a.xhtml)

#### 8.6.1.2 [有哪些通用组件？](#7c0b050e-c750-42b4-9fa4-99fed192e475.xhtml)

### 8.6.2 [重组您的单体应用](#26e9f47b-69e8-41a4-84cd-15fbb3726a79.xhtml)

#### 8.6.2.1 [使用认证](#f1d290fa-ff52-4826-86d5-ac01e3abbec3.xhtml)

#### 8.6.2.2 [调整业务逻辑](#ea1db505-4dd8-4c1e-a6c8-6a1ac86bf6d8.xhtml)

### 8.6.3 [集成微服务](#9a044cc1-24d3-4a5c-9c6c-0fb8335dd739.xhtml)

## 8.7 [总结](#240bf8f4-a3f8-4ee0-8898-884b551ea388.xhtml)

## 8.8 [问题](#e8f23fdb-9891-4493-b06d-e7390a0af905.xhtml)

# 9. [理解服务端 Swift](#0437cb27-3342-4d58-8253-7093af9626ac.xhtml)

## 9.1 [技术要求](#2796c012-00c4-4262-ad72-8439c9ab0867.xhtml)

## 9.2 [Swift 快速回顾](#58e16e2d-f3ad-461d-9f95-d3ba5e99d850.xhtml)

### 9.2.1 [从 Fortran 到 Swift](#ce70b511-a34b-496f-9f17-45882b885dde.xhtml)

## 9.3 [Swift 性能](#3ae6faa6-d2b7-4cfd-b9ca-a4160ade5cd9.xhtml)

### 9.3.1 [脚本语言](#713d43f1-52b9-4446-b6f8-cc6ee31e2858.xhtml)

#### 9.3.1.1 [Facebook](#368718bb-6e39-4be7-a839-b45b8f48b795.xhtml)

#### 9.3.1.2 [Twitter](#92d6a074-7240-49df-9646-a1df67c9a3ad.xhtml)

#### 9.3.1.3 [Uber](#f1073273-c6f4-4c47-8eba-a8b257d316a4.xhtml)

### 9.3.2 [虚拟机语言](#87a41c22-8cce-4ae8-a31b-2f53be20d404.xhtml)

### 9.3.3 [原生语言](#950ca400-4821-4263-bd2a-28a08e3fbdfc.xhtml)

### 9.3.4 [对比框架](#10a04cb4-f9e5-4c48-9d99-d95ac61516fa.xhtml)

### 9.3.5 [Swift 的特性](#1b342b68-2bb1-47c3-afcd-57cacfe2c6ad.xhtml)

## 9.4 [服务端上的 Swift](#e73d49e7-c23c-402c-8cd8-ccf057f5fdf1.xhtml)

### 9.4.1 [自包含服务器](#c35f7ed1-076e-43b5-b6cd-b63f9bf4b086.xhtml)

### 9.4.2 [Linux/Ubuntu](#0a8dc7e7-95b7-46ef-bb60-2a5b5c942953.xhtml)

### 9.4.3 [Docker](#acfb82b5-ed07-47cb-8aab-caaac64e1e78.xhtml)

### 9.4.4 [SwiftNIO](#e2d47090-c151-4ee4-9cdf-dd1ad7561cae.xhtml)

#### 9.4.4.1 [异步事件](#914e129e-0ff6-4e93-b004-66a16d8a88d4.xhtml)

## 9.5 [Swift 5 的特性](#9b8c37bd-42e1-4a6e-9480-726cf32dfe24.xhtml)

### 9.5.1 [ABI 稳定性](#36d2e772-bae4-4fa4-af9c-209c1fac3e5f.xhtml)

### 9.5.2 [原始字符串和 UTF8 字符串](#b5212417-81ee-4699-a0eb-aed1ed920a0d.xhtml)

### 9.5.3 [Result 类型](#cfe0fd6a-516f-4fe7-8984-5d57e2979c6c.xhtml)

### 9.5.4 [未来的枚举成员](#fa1c1c8e-852f-4572-9ab9-800d693c3bb2.xhtml)

### 9.5.5 [扁平化嵌套可选类型](#2747082a-d500-4d57-900d-c8c7c14098d6.xhtml)

## 9.6 [总结](#636a7fd7-cff9-4795-89ff-e2156d1af8c7.xhtml)

## 9.7 [问题](#4d7ceaac-f7ec-4f8b-bf3f-59a6faff9cd0.xhtml)

# 10. [Vapor 框架入门](#9b085c0a-3b19-4c38-94b9-05739781ff96.xhtml)

## 10.1 [Vapor 框架](#ff8f1f43-a9bf-46a1-9256-e045b35960d2.xhtml)

### 10.1.1 [Vapor 的历史](#ef206e3e-0d87-409c-aeaa-7c9dc534cfcd.xhtml)

### 10.1.2 [Hello World](#7b0113bd-66e4-480c-8ef5-4472b841500c.xhtml)

### 10.1.3 [通用结构](#fcb19bc2-2ea7-444c-b3eb-36c6393836cc.xhtml)

#### 10.1.3.1 [配置](#504219f2-7b3a-4e0d-88a8-7f88e8a9602a.xhtml)

#### 10.1.3.2 [中间件](#949b8320-f2a1-4603-9ddb-7e400d9900f6.xhtml)

#### 10.1.3.3 [服务](#591ddd4f-0522-43d3-a4b1-ee1930475b6b.xhtml)

#### 10.1.3.4 [RoutesBuilder](#29063b63-c8f6-4273-9bf4-a31e94ad31db.xhtml)

### 10.1.4 [SwiftNIO – Future 与 Promise](#1f21bd7a-dcda-44e7-8ecd-1be15e8e6b9c.xhtml)

#### 10.1.4.1 [EventLoop](#a8291349-9bb6-43a4-9a58-236917019256.xhtml)

#### 10.1.4.2 [EventLoopFuture](#883ed53e-4bb6-416e-b2bb-be33bde58dd6.xhtml)

##### 10.1.4.2.1 [map](#0218b0d3-e1ef-4bd5-bd93-5b0aa5af8a60.xhtml)

##### 10.1.4.2.2 [flatMap](#2fa7376b-57cd-46bc-a99f-310ef891e0e7.xhtml)

#### 10.1.4.3 [EventLoopPromise](#fa83ec08-7ca8-4ca3-86e3-dc2eb984597b.xhtml)

### 10.1.5 [控制器](#07a29305-2842-49a4-afeb-f0a4ddc4d81b.xhtml)

#### 10.1.5.1 [Content](#96e72782-e007-44f4-beec-c2d369f27a8e.xhtml)

#### 10.1.5.2 [Request](#f5a584f0-92ac-4748-aa34-1163877be82d.xhtml)

### 10.1.6 [数据库与模型](#796b0f43-b365-4194-b101-6eeeaa455953.xhtml)

#### 10.1.6.1 [Fluent 与 FluentKit](#cc8be68a-d00e-45bf-9592-58351c372415.xhtml)

##### 10.1.6.1.1 [一个 Fluent 模型](#50f75971-efdd-4f32-8adf-498a5e23ad90.xhtml)

##### 10.1.6.1.2 [插入](#25f2dd1b-9878-42f4-9d24-a441817d060f.xhtml)

