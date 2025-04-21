# MFILE DID Design  

## DID Format  

The MFILE DID is composed as follows:  

> **MFILE DID Composition**  
>  
> ```http  
> did:mfile:<mfile-specific-id>  
> ```  

Where `<mfile-specific-id>` = `[<encoding-method> : ] <file-hash>`  

Here, `<encoding-method>` represents the file hashing method, including the **mid** hash method, **cid** hash method, and **md5** hash method. `<file-hash>` represents the hash value computed from the file content using the specified `<encoding-method>`.  

**NOTE**: `<encoding-method>` can be omitted. If unspecified, the default hashing method is **mid**.  

> **MFILE DID Example 1**  
>  
> ```http  
> did:mfile:bafkreih6n5g5w4y6u7uvc4mh7jhjm7gidmkrbbpi7phyiyg54gplvngcpm  
> ```  
>  
> **MFILE DID Example 2**  
>  
> ```http  
> did:mfile:mid:bafkreih6n5g5w4y6u7uvc4mh7jhjm7gidmkrbbpi7phyiyg54gplvngcpm  
> ```  
>  
> **MFILE DID Example 3**  
>  
> ```http  
> did:mfile:cid:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4  
> ```  
>  
> **MFILE DID Example 4**  
>  
> ```http  
> did:mfile:md5:c1ca03ad958a8e1753144263dc41893c  
> ```  

## Core Attributes of File DID  

**DID Document**  

| Attribute    | Required | Representation                                               | Purpose                     |  
| ----------- | -------- | ------------------------------------------------------------ | -------------------------- |  
| `id`        | Yes      | A string compliant with the [MFILE DID](#did-format) rules   | File ID                    |  
| `type`      | Yes      | A string (`private`/`public`)                                | Indicates file visibility  |  
| `controller`| Yes      | A string compliant with [MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did-design.md#did-format) rules | File owner (user DID)      |  
| `keywords`  | Yes      | An array of strings                                          | Describes file metadata    |  
| `price`     | No       | A number                                                     | Price for read permission  |  
| `read`      | No       | An array of strings compliant with [MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did-design.md#did-format) rules | List of users with read access |  

### `id` Attribute  

In the MFILE DID document, `id` represents the DID identifier corresponding to the document.  

- **`id`**: Must be provided at the top level of the DID document. It must be a string compliant with the [MFILE DID](#did-format) rules.  

> **MFILE DID Document Example 1**  
>  
> ```json  
> {  
>     "@context": "https://www.w3.org/ns/did/v1",  
>     "id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4"  
> }  
> ```  

### `type` Attribute  

In the MFILE DID document, `type` indicates whether the file is publicly readable or private.  

- **`type`**: Must be a string, either `private` or `public`.  

> **File DID Document Example 3**  
>  
> ```json  
> {  
>     "@context": "https://www.w3.org/ns/did/v1",  
>     "id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",  
>     "type": "private"  
> }  
> ```  

### `controller` Attribute  

In the MFILE DID document, `controller` represents the actual owner of the file. Only the `{controller}#masterKey` proof is considered valid for updating the DID document or managing file access.  

- **`controller`**: Must be a string compliant with [MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did-design.md#did-format) rules.  

> **File DID Document Example 4**  
>  
> ```json  
> {  
>     "@context": "https://www.w3.org/ns/did/v1",  
>     "id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",  
>     "type": "private",  
>     "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e"  
> }  
> ```  

### `keywords` Attribute  

In the MFILE DID document, `keywords` describe the file's metadata for searchability.  

- **`keywords`**: Must be an array of strings.  

> **File DID Document Example 5**  
>  
> ```json  
> {  
>     "@context": "https://www.w3.org/ns/did/v1",  
>     "id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",  
>     "type": "private",  
>     "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",  
>     "keywords": ["movie", "chinese"]  
> }  
> ```  

### `price` Attribute  

In the MFILE DID document, `price` is only valid when `type` is `private` and specifies the cost for read access.  

- **`price`**: Optional. If present, must be a number.  

> **File DID Document Example 6**  
>  
> ```json  
> {  
>     "@context": "https://www.w3.org/ns/did/v1",  
>     "id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",  
>     "type": "private",  
>     "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",  
>     "keywords": ["movie", "chinese"],  
>     "price": 50  
> }  
> ```  

### `read` Attribute  

In the MFILE DID document, `read` is only valid when `type` is `private` and lists users with read access.  

- **`read`**: Optional. If present, must be an array of strings compliant with [MEMO DID](http://132.232.87.203:8088/did/docs/blob/master/memo-did-design.md#did-format) rules.  

> **MFILE DID Document Example 7**  
>  
> ```json  
> {  
>     "@context": "https://www.w3.org/ns/did/v1",  
>     "id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",  
>     "type": "private",  
>     "controller": "did:memo:ce5ac89f84530a1cf2cdee5a0643045a8b0a4995b1c765ba289d7859cfb1193e",  
>     "keywords": ["movie", "chinese"],  
>     "price": 50,  
>     "read": ["did:memo:0x031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"]  
> }  
> ```  

**Actual Controller of File DID**  

Since a file cannot control itself, ownership is assigned via the `controller` field. A file DID can only have one owner at a time. While additional delegates can be granted read access via `capabilityDelegation`, only the `controller` has full control.  

## MFILE DID Functional Design  

### Creating an MFILE DID  

The registration interface is:  
```  
Registe(did, type, controller, price)  
```  

Before creation, a [CID](https://docs.ipfs.tech/concepts/content-addressing/#how-cids-are-created) must be generated for the file, prefixed with `did:mfile`.  

The `Registe` function adds `did`, `type`, and `controller` to the transaction, signed by the private key. The contract generates an initial DID document:  

> **MFILE DID Example 4-1**  
>  
> ```json  
> {  
>     "@context": "https://www.w3.org/ns/did/v1",  
>     "id": "did:mfile:bafybeicla35laadggrakpz37qlkrvfgobb7cxb74kyjn6556zxdu4gq3p4",  
>     "type": "private",  
>     "price": 30  
> }  
> ```  

### Resolving an MFILE DID  

**(1) Resolving the DID**  
```go  
Resolve(did)  
```  
The contract retrieves `controller` and `type` from the DID.  

**(2) Resolving Read Permissions**  
```go  
ResolveRead(did)  
```  
If `type` is `public`, anyone can access the file. If `private`, read access is granted to:  
1) MEMO DIDs listed in `read`;  
2) The `controller` MEMO DID;  
3) MEMO DIDs listed in the `controller`'s `capabilityDelegation`.  

### Modifying the DID Document  

Possible modifications:  
- **`changeController(mfileDid, controller)`** (owner-only);  
- **`changeType(mfileDid, type)`** (owner-only);  
- **`changePrice(mfileDid, price)`** (owner-only);  
- **`AddKeyWords(mfileDid, keywords)`** (owner-only);  
- **`DeactivateKeywords(mfileDid, keywords)`** (owner-only);  
- **`AddRead(mfileDid, memoDid)`** (paid access, open to anyone);  
- **`GrantRead(mfileDid, memoDid)`** (free access, owner-only);  
- **`DeactivateRead(mfileDid, memoDid)`** (owner-only, revokes free access only).  

**Transferring Ownership**  
Invoking `changeController(mfileDid, controller)` requires owner verification.  

**Granting Read Access**  
- **Paid Access**: Users pay the owner to be added to `read`.  
- **Free Access**: The owner grants access without payment.  

### Deleting an MFILE DID  

Deletion marks the DID document as deprecated.  

## File Permissions  

| Field                          | Read Access | Full Control |  
| ----------------------------- | ----------- | ------------ |  
| `read` (file DID)             | ✔️          | ❌           |  
| `controller` (file DID)       | ✔️          | ✔️           |  
| `capabilityDelegation` (user DID) | ✔️          | ❌           |