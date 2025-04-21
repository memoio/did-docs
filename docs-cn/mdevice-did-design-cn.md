# 1. MDevice DID设计

MDevice DID指的是Memolabs为物联网设备（比如智能穿戴、摄像头等）设计的一套去中心化身份（DID，Decentralized Identifier），目的是赋予设备在去中心化网络中的唯一身份，并支持设备进行安全的通信和交易。主要需要考虑到DID的唯一性、可验证性、隐私保护和互操作性。

## 1.1 DID格式

MDevice DID组成如下：

>**MDevice DID组成**
>
>```http
>did:mdevice:<device-specific-id>
>```

其中<font color="red">`<device-specific-id>`</font>=<font color="red">`<address>`</font>

其中<font color="red">`<address>`</font>表示设备生成公私钥对，经过对公钥进行keccak256哈希生成的以太坊格式的十六进制地址（省略‘0x’前缀）。

>**MDevice DID例子**
>
>```http
>did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5
>```
>

## 1.2 DID URL格式

MDevice DID URL组成如下：

>**DID URL组成**
>
>```http
>did:mdevice:<device-specific-id> path [ ? <query> ] [ # <fragment> ]
>```

一些DIDURL例子如下：

> **DID URL例子1**
>
> ```http
> did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey
> ```

> **DID URL例子2**
>
> ```http
> did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1
> ```

支持以下path:

- 暂无

支持以下query：

- 暂无

支持以下fragment：

- <font color="red">`#masterKey`</font>：指定主公钥；
- <font color="red">`#key-<n>`</font>：指定其他的公钥；

## 1.3 设备DID核心属性

**DID文档**

| 属性名     | 是否必要 | 表示方法                                                     | 用处                      |
| ---------- | -------- | ------------------------------------------------------------ | ------------------------- |
| id         | 是       | 一个符合[MDevice DID](#1.1 DID格式)构成规则的字符串            | 设备ID                    |
| type       | 是       | 一个字符串（设备类型，比如摄像头、音箱、智能手环、树莓派等）      | 表明设备的种类             |
| controller | 是       | 一个符合[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)构成规则的字符串或者一个符合[MDevice DID](#1.1 DID格式)构成规则的字符串 | 设备的所有者，指向用户DID或者设备DID |
| verificationMethods  | 是       | 一组符合[验证方法](#verificationMethod)构成规则的数组 | 一组用于验证的公钥集合      |
| authentication       | 否       | 一组符合[DID URL](#1.2 DID URL格式)构成规则的字符串       | 以该DID的名义认证登录      |


**<a name="verificationMethod">验证方法</a>**

| 属性名       | 是否必要 | 表示方法                                            | 用处                 |
| ------------ | -------- | --------------------------------------------------- | -------------------- |
| id           | 是       | 一组符合[DID URL](#1.2 DID URL格式)构成规则的字符串 | 验证方法标识符       |
| type         | 是       | 一个字符串                                          | 验证方法类型         |
| controller   | 是       | 一个符合[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)构成规则的字符串或者一个符合[MDevice DID](#1.1 DID格式)构成规则的字符串         | 验证方法的所有者     |
| publicKeyHex | 是       | 一个符合16进制编码的字符串                          | 16进制表示的公钥信息 |

### 1.3.1 id属性

在MDevice DID文档中，id表示与DID文档对应的DID标识符。

**id**：在MDevice DID文档中，id属性必须给出且必须在DID文档的最顶层。id属性必须是一个字符串并且满足[MDevice DID](#1.1 DID格式)所描述的构成规则。

> **MDevice DID文档例子**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
> }
> ```



### 1.3.2 type属性

在MDevice DID文档中，type表示设备的种类，比如摄像头、智能穿戴、智能音箱、个人计算机、智能汽车等。

**type**：在MDevice DID文档中，type属性是必须的。type属性必须是一个字符串。

> **MDevice DID文档例子**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>    "type": "nas",
> }
> ```

### 1.3.3 controller属性

在MDevice DID文档中，controller表示MDevice DID所代表的设备的所有者。controller能够更新DID文档（添加或者删除某一属性），拥有对设备的控制权。controller可以是一个MEMO DID或者MDevice DID，且只有`{controller}#masterKey`的证明被认为是有效的。

**controller**：在MDevice DID文档中，controller属性是必须的。controller属性必须是一个字符串，并且满足[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)或者[MDevice DID](#1.1 DID格式)所描述的构成规则。

> **MDevice DID文档例子1**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>   "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
> }
> ```

> **MDevice DID文档例子2**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>   "controller": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
> }
> ```

### 1.3.4 verificationMethods属性

在MDevice DID文档中，verificationMethods属性描述了一组验证身份的方式，通常使用公钥表示。通过公钥可以验证签名是否由对应的私钥签署，从而确定签名者的身份。verificationMethods属性通常用于线下两个MDevice DID主体之间的交互，以实现认证或授权功能。

**verificationMethods**：在MDevice DID文档中，verificationMethods是必须的且至少包含主验证方法masterkey，另外verificationMethods属性必须是一组验证方法且满足所描述的构成规则。且需要包含以下属性：

- **id**：在验证方法中，id属性必须给出，另外id属性必须是一个字符串且满足[1.2DID URL](#1.2 DID URL格式)描述的构成规则。
- **type**：在验证方法中，type属性必须给出，另外type属性必须是一个字符串。type属性对应唯一的hash函数以及签名函数，从而描述了如何使用verificationMehtods中的公钥验证签名。
- **controller**：在验证方法中，controller属性必须给出，另外controller属性必须是一个符合[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)构成规则的字符串或者一个符合[MDevice DID](#1.1 DID格式)构成规则的字符串，如果是全0，则表示其为DID文档中的controller。controller可以修改该verificationMethod中的type和publicKeyHex值。
- **publicKeyHex**：在验证方法中，publicKeyHex属性必须给出，另外publicKeyHex属性必须是一个符合16进制编码的字符串，表示验证方案的压缩公钥。

> **MDevice DID文档例子**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>     "verificationMethods": [
>        {
>            "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey",
>            "type": "EcdsaSecp256k1VerificationKey2019",
>            "controller": "did:mdevice:0000000000000000000000000000000000000000",
>            "publicKeyHex": "0x031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"
>        },
>        {
>            "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1",
>            "type": "EcdsaSecp256k1VerificationKey2019",
>            "controller": "did:memo:b69f63fe2f84d782b134624020802fa49c70f6fad8009c0be1da25229c2be99c",
>            "publicKeyHex": "0x02948b109c8298057c1d798d9a14ed30f33689d8397e2e60f89ef408b08d7f48a1"
>        }
>        ],
>    }
> ```

> **特殊验证方法masterKey**
>
> 在每个DID文档的verificationMethods属性中，必然存在一个特殊的验证方法，其id为`{did}#masterKey`。在上述例子中即为`did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey`。该验证方法除了能够在线下验证签名者的身份外，也是线上合约验证签名者是否具有修改DID文档权限的方法，其他验证方法不具备该功能。

>**验证方法的controller**
>
>由于密钥无法控制自身，并且无法从DID文档中推断出验证方法的控制器，因此需要在验证方法中明确表达密钥控制器的身份。验证方法的controller仅拥有对单个验证方法中type和publicKeyHex属性进行修改的权限。通常情况下，验证方法controller为设备本身（即设备的DID），或者是某个人（账户DID）。当controller为全0时，则表示其controller为MDevice DID文档中serialNumber属性下面的controller。

### 1.3.5 authentication属性

在DID文档中，authentication属性描述了网关或者服务商如何对设备DID进行线下身份认证，通常用于设备登录网关或者执行任何类型的询问-响应协议等。

**authentication**：在DID文档中，authentication是可选的。如果authentication存在，则必须是一组字符串且满足[1.2 DID URL](#1.2 DID URL格式)构成规则。

> **DID文档例子4**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
> 	...
>        "authentication": [
>            "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey",
>            "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-5"
>        ],
>    }
>    ```


## 1.4 MDevice DID相关功能设计

### 1.4.1 创建MDevice DID

创建MDevice DID的接口如下：

```go
Register(did, type, serialNumber, controller, publicKey)
```

在创建MDevice DID之前，需要生成did和publicKey，其流程如下：

- 生成一对公私钥，其中，公钥为压缩格式，并生成以太坊格式地址，作为mdevice-specific-id；
- 在mdevice-specific-id前添加"did:mdevice"生成最终的did；

调用Register接口，该接口会将did，type，serialNumber，controller以及publicKey添加到交易信息中，并使用私钥进行签名。随后合约会根据输入信息以及默认信息，生成初始MDevice DID文档。

初始MDevice DID文档例子如下：

>**MDevice DID例子**
>
>```json
>{
>	"@context": "https://www.w3.org/ns/did/v1",
>	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>	"type": "nas",
>   "serialNumber": "AB202410X1230001",
>   "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
>     "verificationMethods": [
>        {
>            "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey",
>            "type": "EcdsaSecp256k1VerificationKey2019",
>            "controller": "did:mdevice:0000000000000000000000000000000000000000",
>            "publicKeyHex": "0x031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"
>        },
>     ],
>}
>```



### 1.4.2 查询MDevice DID

**（1）解析MDevice DID**

解析MDevice DID，即通过MDevice DID获取DID文档，接口如下：

```go
Resolve(did)
```

在MDevice DID的合约中，通过did即可获取type、serialNumber、controller以及verificationMethods属性。

**（2）反引用解析DID URL**

反引用解析DID URL，即通过DID URL解析得到验证方法，其接口如下：

```go
Dereference(didUrl)
```

在MDevice DID中，DID URL即表示verificationMethods中的ID，可解析为DID以及fragment两部分。因此，可通过DID定位到对应的verificationMthods数组，同时可以通过fragment定位到最终verificationMthod所在的位置。



### 1.4.3 更新DID文档信息

对于MDevice DID文档的修改，存在如下几种情况：

- 修改设备种类**`changeType(mdeviceDid, type)`**，该接口仅DID所有者可以调用；
- 修改设备所有者**`changeController(mdeviceDid, controller)`**，该接口仅DID所有者可以调用；
- 添加验证方法**`addVerificationMethod(did, type, controller, publicKeyHex)`**，该接口仅masterKey可以调用；
- 修改验证方法**`updateVerificationMethod(methodID, type, publicKeyHex)`**，该接口仅<font color="red">`验证方法中controller`</font>可以调用；
- 撤销验证方法**`deactivateVerificationMethod(methodID)`**，该接口仅masterKey可以调用；
- 添加认证方式**`addAuthentication(did, methodID)`**，该接口仅masterKey可以调用；
- 撤销认证方式**`deactivateAuthentication(did, methodID)`**，该接口仅masterKey可以调用；

我们以一些典型的例子来说明：

**addVerificationMethod(did, type, controller, publicKeyHex)**

- 用户为设备选择did以及controller(默认和did相同，但是可能存在单个用户拥有多个did，设定主did控制其他did和其验证方法)；
- 用户生成一对公私钥，根据生成算法指定type;
- 使用masterKey的私钥签名，并访问合约接口`addVerificationMethod(did, type, controller, publicKeyHex)`；
  - 合约验证签名是否由did masterKey所签；
  - 合约根据递增的number和did生成methodID；
  - 合约根据methodID，type，controller，publicKeyHex生成验证方法；
  - 合约将验证方法添加到did文档的verificationMethods属性中；

在上述例子的基础上添加`did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1`后文档如下：

> **MDevice DID文档例子**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>	"type": "nas",
>   "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
> 	"verificationMethods": [
>     {
>         "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey",
>         "type": "EcdsaSecp256k1VerificationKey2019",
>         "controller": "did:mdevice:0000000000000000000000000000000000000000",
>         "publicKeyHex": "031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"
>     },
>     {
>         "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1",
>         "type": "EcdsaSecp256k1VerificationKey2019",
>         "controller": "did:mdevice:0000000000000000000000000000000000000000",
>         "publicKeyHex": "02948b109c8298057c1d798d9a14ed30f33689d8397e2e60f89ef408b08d7f48a1"
>     },
>     ],
> }
> ```

**modifyVerifycationMethod(did, methodIndex, type, publicKeyHex)**

- 用户选择methodID，解析出did以及methodIndex;
- 用户生成一对公私钥，并根据生成算法确定type；
- 用户利用验证方法 controller包含的masterKey对应的私钥签名，并访问合约接口`modifyVerifycationMethod(did, methodIndex, type, publicKeyHex)`；
  - 合约验证签名是否由methodID中controller所签；
  - 合约修改验证方法中type以及publicKeyHex属性；

在上述例子的基础上更新`did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1`后文档如下：

> **MDevice DID文档例子**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>	"type": "nas",
>   "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
> 	"verificationMethods": [
>     {
>         "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey",
>         "type": "EcdsaSecp256k1VerificationKey2019",
>         "controller": "did:mdevice:0000000000000000000000000000000000000000",
>         "publicKeyHex": "031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"
>     },
>     {
>         "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1",
>         "type": "EcdsaSecp256k1VerificationKey2019",
>         "controller": "did:mdevice:0000000000000000000000000000000000000000",
>         "publicKeyHex": "02fa46f35cecc57016e13c94ba3b0302c7d14daa482741ca18ede9dd2255aaeeeb"
>     },
>     ],
> }
> ```

**deactivateVerificationMethod(did, methodIndex)**

- 用户选择methodID，解析出did以及methodIndex;
- 用户使用did masterKey签名消息，访问合约接口`deactivateVerificationMethod(did, methodIndex)`；
  - 合约验证签名是否由did masterKey所签；
  - 合约删除对应的验证方法；

在上述例子的基础上删除`did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1`后文档如下：

> **MDevice DID文档例子**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
>	"type": "nas",
>   "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
> 	"verificationMethods": [
>     {
>         "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey",
>         "type": "EcdsaSecp256k1VerificationKey2019",
>         "controller": "did:mdevice:0000000000000000000000000000000000000000",
>         "publicKeyHex": "031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"
>     },
>     ],
> }
> ```

### 1.4.4 删除MDevice DID

删除MDevice DID，会将MDevice DID文档标志为已弃用状态，表示该DID代表的设备已经被弃用。
