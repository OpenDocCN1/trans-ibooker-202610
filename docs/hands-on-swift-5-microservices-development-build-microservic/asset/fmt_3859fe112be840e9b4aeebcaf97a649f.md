
# Chapter 10 (Partial Content)

## Comparing `Vapor` to `Kitura`, `Perfect`, and `Smoke` `` `#c6744c92-49a4-4625-ad28-ea732ad5701a.xhtml` ``

### `Views` `` `#ed5e8f67-9d63-446b-b9ab-9bbc76abb58e.xhtml` ``

#### Supported databases `` `#e733c019-d0ca-4ab5-9ca0-801870e51e44.xhtml` ``

##### Update `` `#fda7eb80-dffc-4b8d-8738-e700316c2dcb.xhtml` ``

##### Delete `` `#7523e069-851e-4e20-8434-ecdd8a178088.xhtml` ``

##### Querying `` `#133c7536-eaa6-4e80-a8af-e4bf3381dc1c.xhtml` ``

## Installing `Vapor` `` `#71e91b9b-6969-48f7-a0a0-cc8ee5fba9f7.xhtml` ``

### Prerequisites `` `#b219bd1c-b829-4da0-87f1-4537ea8bb0bf.xhtml` ``

## `Vapor` in Action `` `#6c2239c7-d15b-4145-a7d6-7cb9984efb56.xhtml` ``

## Summary `` `#db296d75-625e-4db1-8d70-5d8712aa861e.xhtml` ``

## Questions `` `#fcbb2823-9f92-4a43-951a-47dc29764c7d.xhtml` ``

# Chapter 11: Planning an Online Store Application `` `#05c39be0-aae8-4b46-899b-1458e1929ff6.xhtml` ``

## Technical requirements `` `#aca01036-b4f0-422a-a56b-4fba990a0b1c.xhtml` ``

## Application layout for an online store `` `#5d221fa7-340f-41dd-8b1b-63b4ad834ae3.xhtml` ``

### Frontend to `API` communication `` `#496f9f85-4793-436e-beb6-a1f4da3eb066.xhtml` ``

### Authentification `` `#79c80abe-5d3f-4666-b34f-bba13d87ec67.xhtml` ``

#### Generic authentication `` `#e3d8bbeb-579f-48cd-93af-e44d3c2223c4.xhtml` ``

#### Microservice authentication `` `#69f90e2e-50f0-46aa-b8ff-c845e3a222de.xhtml` ``

#### `JSON Web Tokens` `` `#449c0dcf-e7b7-4bdc-a397-f4637c0a8aa4.xhtml` ``

#### Renewing `JWTs` `` `#7a759bee-d498-4eb6-8600-f7a6b80883c1.xhtml` ``

##### Access token `` `#3579db72-2762-4291-8f5b-8d45352fe119.xhtml` ``

##### Refresh token `` `#d7800965-9807-4d24-a3d1-52aac4f67a6f.xhtml` ``

#### Block access `` `#bbd2294d-d965-43a7-a1c9-3ee30c2d36fb.xhtml` ``

### Database management `` `#b6e524eb-6c81-47ea-98d9-38cb1eaf258a.xhtml` ``

### Cloud support `` `#0f68b42d-b83c-4b44-9959-dbcd0143ac2e.xhtml` ``

## User Manager `` `#2049e5b7-f63f-4889-869c-fb8c7c522338.xhtml` ``

### Models `` `#f1b83739-d9c1-42fd-a964-640f1b9485d7.xhtml` ``

#### `User` `` `#7d520833-f238-4c13-acd3-4a37b7d38b77.xhtml` ``

#### `Address` `` `#727553e8-faad-4d65-9509-825b0e53d137.xhtml` ``

## Product Manager `` `#d688ee72-dc6b-4ad5-8b3b-557fcd54e95e.xhtml` ``

### Models `` `#3f1d98c3-157d-4ee0-bd7b-a4ea83723f10.xhtml` ``

#### `Products` `` `#1a7c5a6f-691f-4265-88e6-bfed35ff4b59.xhtml` ``

#### `Category` `` `#48dd1f38-a383-439f-8ca7-678f1d3e429a.xhtml` ``

#### `Product Categories` `` `#1b39c087-d319-4ed3-9074-b28321581faa.xhtml` ``

## Order Manager `` `#1cbf5108-02bd-4c45-b488-bb78fc8166d9.xhtml` ``

### Models `` `#118d5201-98ea-4fde-9b0c-7af85c9e52fa.xhtml` ``

#### `Order` `` `#73452bf0-fac9-4ceb-85e9-4894fb096c2d.xhtml` ``

#### `Order Product` `` `#d05b6d1f-1f1d-475f-8c27-52d10a1cdf8d.xhtml` ``

#### `Payment` `` `#a17a671b-45a3-455d-9253-07c68c35ae37.xhtml` ``

## `API` structure `` `#978fb9c7-90c7-41ed-b5fc-7511a69561a0.xhtml` ``

## Summary `` `#5d05ff01-77ee-4bc6-a03d-a85792fdf9f3.xhtml` ``

## Questions `` `#4160cf5b-2e78-4737-a160-06b2dd2bda22.xhtml` ``

# Chapter 12: Creating Your First Microservice `` `#045da3f4-89ae-4871-bde0-08285e687f02.xhtml` ``

## Technical requirements `` `#ec0d488d-54f1-452d-bac5-2d796d7724b6.xhtml` ``

## Starting a new service `` `#3b8b4895-c27f-4c92-b910-597b1f9d78dc.xhtml` ``

### Version control `` `#f8ed155f-12ab-4ae5-b46c-4f4f18fbfeea.xhtml` ``

### Using the `API` template `` `#fd75734b-f032-408a-83d7-6db3a54c8cbb.xhtml` ``

### General file folders `` `#c7b031d1-3231-4b54-a7d7-674763a2bba5.xhtml` ``

### Dependency management `` `#07e1a91e-ed2e-4139-96de-9a6298a2eddd.xhtml` ``

#### Adding and removing packages `` `#d75fc774-ad4e-4116-8c5b-a5c178d0a478.xhtml` ``

### Configuration `` `#a561b954-c17e-499d-bad6-8bc9ed11f55a.xhtml` ``

#### `Middleware` `` `#0888df5d-639b-4a33-ab9e-2a6904445d1d.xhtml` ``

##### `CORS` `` `#13fb851c-ca90-45a2-bede-dde14c88bf0f.xhtml` ``

##### `Error` `` `#ae402adb-3bf7-42b1-9b58-51a63f7a585f.xhtml` ``

#### Services `` `#6ec1016c-36b5-4c9e-a210-f4ad5ae05020.xhtml` ``

##### `Router` `` `#524406c1-0b5f-4157-8806-b810f70bdd6a.xhtml` ``

##### `DatabaseConfig` `` `#cc1981c0-c327-4175-954e-fc7e79db153e.xhtml` ``

##### `Fluent` `` `#fc5f064f-7fde-45e0-9e1c-d8f20387f581.xhtml` ``

### Routes `` `#23f438e2-46fd-4e25-b4f2-61a2855bbcb5.xhtml` ``

## Using models `` `#6c99fea2-51af-41b3-890a-99f7db68b5e4.xhtml` ``

### `I/O` models `` `#bbc4d8c2-d0fe-4c53-941a-2b3e9d299814.xhtml` ``

### `DB` models `` `#0fbb1f45-66bf-4d14-869e-6af60944a647.xhtml` ``

### Returning `DB` models `` `#1a0b4c8a-57fd-4a65-ab24-76a90ec24d47.xhtml` ``

### Predefined models `` `#21d69c70-93cb-4611-b660-2e7957a92cca.xhtml` ``

## Using controllers `` `#e411651e-c32f-41ca-962a-a7f6e1538b39.xhtml` ``

### Interacting with models `` `#0d1089b0-9b51-4529-9a72-3f5b8b1a4bcb.xhtml` ``

### File management `` `#81fb7b36-37f9-488b-a4a2-f38b487c99bd.xhtml` ``

### `SendGrid` `` `#e9924dd0-08e0-4249-89f4-c171dadc4829.xhtml` ``

## Using workers `` `#3e89bda4-e330-436f-82e5-4252363fce2f.xhtml` ``

## Summary `` `#09077b76-0768-4710-9cf5-528422c75678.xhtml` ``

## Questions `` `#f46c6d8e-864a-4b49-b1d6-f31695fede32.xhtml` ``

# Chapter 13: Application Structure and Database Design `` `#fb9141ef-8226-4f27-baaf-d85a84319f45.xhtml` ``

## Technical requirements `` `#7dafad55-3ce2-4bdb-917b-a4a8e13e23f9.xhtml` ``

## Understanding project setup and folder structure `` `#55a6cacd-075a-4345-b82a-4ee4ac91f089.xhtml` ``

## Installing `Git` and `Docker` `` `#01ef8f33-306e-4fcf-b222-f1ad4eff3cef.xhtml` ``

### Installing `Git` `` `#5b771102-63f6-489a-8dcb-e1ca07ff695a.xhtml` ``

### Installing `Docker` `` `#10a7cf0b-0cf2-4e88-b2d8-650b259174c6.xhtml` ``

### Using `Docker` with microservices `` `#5f2ece4d-293e-4791-824b-73ede4fc66ab.xhtml` ``

### Setting up `Docker` and `Git` with `AWS` and `GitHub` `` `#6fb8dc06-5072-4efb-be04-8e4e31bb5219.xhtml` ``

## Setting up `Vapor` and the database `` `#d989f1bc-f4d3-4d55-96c2-19784478346e.xhtml` ``

## Setting up our services `` `#4762235f-01a2-4d23-84aa-5c35214a175b.xhtml` ``

### Setting up `Dockerfiles` `` `#e172d8af-cd96-4357-9aef-063bdc17444f.xhtml` ``

### Setting environment variables `` `#967e0213-d5ae-4a20-9399-a0bc879166a3.xhtml` ``

### Running the entire application `` `#4d6f8fe5-5ba5-4c16-a729-c5df880d04dd.xhtml` ``

## Summary `` `#5fe30944-076a-46e9-9cf9-4413a3306245.xhtml` ``

## Questions `` `#89af3ba7-4709-4526-9365-05bcdb67db31.xhtml` ``

# Chapter 14: Writing the `User` Service `` `#de77c71a-d8d1-4d47-9dbe-b621d7690103.xhtml` ``

## Technical requirements `` `#35de0b50-7aa1-4e60-9ff0-42514c776d2a.xhtml` ``

## Setting up and taking the first step `` `#1774d856-aed2-4124-83cb-e747ecf19c7c.xhtml` ``

### Setting up the template `` `#12ba1d93-3b2a-4fed-b814-60fb45ea5168.xhtml` ``

### Installing `SendGrid` `` `#81a8b717-db0d-4009-8e0c-fb36191c7110.xhtml` ``

### Setting up a `JWT` and utility functions `` `#d745badb-9c15-4659-b9d1-235cc4141120.xhtml` ``

### Setting up the database `` `#d9208f6f-dd4a-49a2-9c57-f55f851f3e87.xhtml` ``

## Exploring routes `` `#c1dfa8c5-97dc-4ad1-9ff4-f5f4da27c892.xhtml` ``

## Exploring models `` `#bd4de911-c54f-4187-8d8e-514ac2aa887a.xhtml` ``

### Database models `` `#8bcd22a3-2703-435e-b0f3-23a1234a64bb.xhtml` ``

#### `User` `` `#2714580f-ac02-49bc-b787-a55a5fa64232.xhtml` ``

#### `Address` `` `#d8c8eff1-61fd-4d6d-80db-b66822d2652c.xhtml` ``
