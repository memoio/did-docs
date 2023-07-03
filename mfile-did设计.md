# 1. MFILE DID设计

## 1.1 DID格式

MFILE DID组成如下：

>**MFILE DID组成**
>
>```http
>did:mfile:<mfile-spific-id>
>```

一个MFILE DID例子如下：

>**MFILE DID例子**
>
>```http
>did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4

其中其中<font color="red">`<mfile-specific-id>`</font>=<font color="red">`<file-hash>`</font>

mfile-specific-id包含多种编码方式，包括mid，cid以及md5等编码方式，编码方式在DID文档中encode属性中表明。



## 1.2 文件DID核心属性

**DID文档**

| 属性名     | 是否必要 | 表示方法                                                     | 用处                                 |
| ---------- | -------- | ------------------------------------------------------------ | ------------------------------------ |
| id         | 是       | 一个符合[MFILE DID](#1.1 DID格式)构成规则的字符串            | 文件ID                               |
| encode     | 是       | 一个字符串                                                   | 表示文件ID的编码方式(mid/cid/md5...) |
| type       | 是       | 一个字符串（private/public）                                 | 表明文件公开还是私有                 |
| controller | 是       | 一个符合[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)构成规则的字符串 | 文件的所有者，指向用户DID            |
| keywords   | 是       | 一组字符串                                                   | 描述文件的大概信息                   |
| price      | 否       | 一个数字                                                     | 表明文件读权限价格                   |
| read       | 否       | 一组符合[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)构成规则的字符串 | 读取文件的权限列表                   |



### 1.2.1 id属性

在MFILE DID文档中，id表示与DID文档对应的DID标识符。

**id**：在DID文档中，id属性必须给出且必须在DID文档的最顶层。id属性必须是一个字符串并且满足[MFILE DID](#1.1 DID格式)所描述的构成规则。

> **MFILE DID文档例子1**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
> }
> ```



### 1.2.2 encode属性

在MFILE DID文档中，encode表示id中`<mfile-spific-id>`的编码方式.

**encode**：在DID文档中，encode属性必须给出且必须在DID文档的最顶层。encode属性必须是一个字符串，包括mid，cid以及md5等编码方式。

> **MFILE DID文档例子2**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
>    	"encode": "mid",
> }
> ```



### 1.2.3 type属性

在MFILE DID文档中，type表示文件是可公开读取还是私有的。

**type**：在DID文档中，type属性是必须的。type属性必须是一个字符串，且必须是private或者public。

> **文件DID文档例子3**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
>     	"encode": "mid",
>     	"type": "private",
> }
> ```



### 1.2.4 controller属性

在MFILE DID文档中，controller表示MFILE DID的实际所有者以及对应文件的所有者。controller除了能够更新DID文档（添加或者删除某一属性）外，还能够线下读取或者删除对应的文件。controller表示一个MEMO DID，且只有`{controller}#masterKey`的证明被认为是有效的。

**controller**：在MFILE DID文档中，controller属性是必须的。controller属性必须是一个字符串并且满足[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)所描述的构成规则。

> **文件DID文档例子4**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
>     	"encode": "mid",
>     	"type": "private",
>     	"controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
> }
> ```



### 1.2.5 keywords属性

在MFILE DID文档中，keywords属性表示对应文件的关键词，用于描述文件的基本信息，便于其他人检索后购买读取权限。

**keywords**：在DID文档中，keywords属性是必须的。keywords属性必须是一组字符串。

> **文件DID文档例子5**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
>    	"encode": "mid",
>     	"type": "private",
>     	"controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
>    	"keywords": [
>             "movie",
>             "chinese"
>    	 ],
> }
> ```



### 1.2.6 price属性

在MFILE DID文档中，price只有在type等于private是有效，表示获取读权限需要的价格。当MFILE DID的所有者设置文件为私有文件时，其他用户可以通过付费的方式，获取读取私有文件的权限。

**price**：在MFILE DID文档中，price属性是可选的。若price属性存在，则price必须是一个数字。

> **文件DID文档例子6**
>
> ```json
> {
> 	"@context": "https://www.w3.org/ns/did/v1",
> 	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
>     	"encode": "mid",
>     	"type": "private",
>     	"controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
>     	"keywords": [
>             "movie",
>             "chinese"
>     	],
>     	"price": 50,
> }
> ```



### 1.2.7 read属性

在MFILE DID文档中，read属性只有在type等于private是有效，表示拥有读取私有文件权限的用户列表。当MFILE DID中type为private时，即对应的文件为私有文件时，read属性中所有的MEMO DID均有权读取该私有文件。

**read**：在MFILE DID文档中，read属性是可选的。若read属性存在，则read属性必须是一组字符串且满足[MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md#11-did格式)所描述的构成规则。

>**MFILE DID文档例子7**
>
>```json
>{
>	"@context": "https://www.w3.org/ns/did/v1",
>	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
>    	"encode": "mid",
>    	"type": "private",
>        "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",
>    	"keywords": [
>             "movie",
>             "chinese"
>        ],
>    	"price": 50,
>	"read": [
>		"did:memo:0x031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"
>	]
>}
>```



**文件DID的实际控制者**

由于文件本身无法控制自己，因此需要指定文件所有者，文件所有者由文件DID的controller字段表示。同一时间，一个文件DID标识符，或者说一个文件，只能有一个用户DID所有。虽然用户可以在capabilityDelegation中添加验证方法，但是capabilityDelegation中添加验证方法只拥有读取文件的权限，无法完全控制文件。因此，一个文件DID标识符，只会由一对公私钥对控制。



## 1.3MFILE DID相关功能设计

### 1.3.1 创建MFILE DID

创建MFILE DID的接口如下：

```
Registe(did, type, controller, price)
```

在创建MFILE DID之前，需要首先为文件[生成cid](https://docs.ipfs.tech/concepts/content-addressing/#how-cids-are-created)。随后添加did:mfile前缀作为最终的did。

调用Registe接口，该接口会将did，type以及controller添加到交易信息中，并使用私钥进行签名。随后合约会根据did，type以及controller信息生成初始MFILE DID文档。

初始MFILE DID文档例子如下：

>**MFILE DID例子4-1**
>
>```json
>{
>	"@context": "https://www.w3.org/ns/did/v1",
>	"id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",
>	"type": "private",
>    	"price": 30,
>}
>```



### 1.3.2 解析MFILE DID

**（1）解析MFILE DID**

解析MFILE DID的接口如下：

```go
Resolve(did)
```

在MFILE DID的合约中，通过did即可获取controller以及type属性。

而read属性，则会在每次添加操作执行完成后，发出对应的事件，同时合约提供相应的查询接口，用以查询上述属性是否有效。因此，可以通过遍历相应的事件，同时调用查询接口，即可解析MEMO DID所有属性。

**（2）解析MFILE DID的读权限列表**

解析MFILE DID的读权限列表的接口如下：

```go
ResolveRead(did)
```

解析MFILE DID的读权限之前，需要首先获取MFILE DID的type属性，若type属性为public，则任何人均可以访问该文件。若type为private，则获取读权限列表可以分为3部分：

1）MFILE DID中read标识的MEMO DID；

2）MFILE DID中controller标识的MEMO DID；

3）MFILE DID中controller标识的MEMO DID中capabilityDelegation标识的所有MEMO DID；



### 1.3.3 修改DID文档信息

对于MFILE DID文档的修改，存在如下几种情况：

- 修改DID所有者**`changeController(mfileDid, controller)`**，该接口仅DID所有者可以调用；
- 修改文件类型**`changeType(mfileDid, type)`**，该接口仅DID所有者可以调用；
- 修改文件价格**`changePrice(mfileDid, price)`**，该接口仅DID所有者可以调用；
- 付费获取读权限**`AddRead(mfileDid, memoDid)`**，该接口任何人均可调用，但需要向DID所有者付费；
- 免费授予读权限**`GrantRead(mfileDid, memoDid)`**，该接口仅DID所有者可以调用，无需付费；
- 撤销读取权限**`DeactivateRead(mfileDid, memoDid)`**，该接口仅DID所有者可以调用，但是仅可删除免费授予的读权限，无法删除付费购买的部分。



我们以一些典型的例子来说明：

**转让DID所有权**

转让文件DID的所有权需要调用`changeController(mfileDid, controller)`接口，

首先会验证调用者的权限，通过mfileDid的controller找到所有者地址，详见[memo did](http://132.232.87.203:8088/did/docs/blob/master/memo-did设计.md)。从而在链上验证调用者是否是mfileDid的所有者。

随后会将controller替换为最新的MFILE DID所有者。



**授予读权限**

在MFILE DID中，授予他人读权限可以由其他任何人发起但需要支付一定的代币，也可以由所有人发起同时无需支付任何货币。

- 付费购买读权限

所有者仅仅会在创建私有文件（即type属性为private）时，添加文件读取权限的费用。其他人若想获取访问文件的权限，则需要支付给所有者相应的代币，则可将自己添加到read属性中。

- 免费添加读权限

所有者会在创建文件后，选择免费为其他人添加访问文件的权限，该过程完全由所有者发起并完成，他人无需付费。



### 1.3.4 删除MFILE DID

删除DID，会将MFILE DID文档标志为已弃用状态。



## 1.4 文件权限

| 字段                          | 读权限   | 完全控制 |
| ----------------------------- | -------- | -------- |
| read(文件did)                 | &#10004; | &#10006; |
| controller(文件did)           | &#10004; | &#10004; |
| capabilityDelegation(用户DID) | &#10004; | &#10006; |
