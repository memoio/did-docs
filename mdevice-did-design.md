# 1. MDevice DID Design Specification

## 1.1 DID Format

MDevice DID represents Memolabs' decentralized identifier (DID) standard for IoT devices (e.g., wearables, cameras), providing unique identity for secure communication and transactions in decentralized networks. Key considerations include uniqueness, verifiability, privacy protection, and interoperability.

**MDevice DID Structure:**
```
did:mdevice:<device-specific-id>
```
Where `<device-specific-id>` = `<address>` (Ethereum-style hexadecimal address derived from Keccak256 hash of public key, without '0x' prefix)

**Example:**
```
did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5
```

## 1.2 DID URL Format

**Structure:**
```
did:mdevice:<device-specific-id> path [? <query>] [# <fragment>]
```

**Supported Fragments:**
- `#masterKey`: Designates primary public key
- `#key-<n>`: Designates secondary public keys

**Example URLs:**
1. `did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#masterKey`
2. `did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5#key-1`

## 1.3 Core DID Document Attributes

### DID Document Structure

| Attribute | Required | Description | 
|-----------|----------|-------------|
| `@context` | Yes | JSON-LD context (fixed as "https://www.w3.org/ns/did/v1") |
| `id` | Yes | MDevice DID string |
| `type` | Yes | Device category (e.g., "camera", "wearable") |
| `serialNumber` | Yes | Manufacturer-assigned device identifier |
| `controller` | Yes | Owner's MEMO DID or MDevice DID |
| `verificationMethods` | Yes | Array of verification methods |
| `authentication` | No | Array of authentication references |

### Verification Method Structure

| Attribute | Required | Description |
|-----------|----------|-------------|
| `id` | Yes | DID URL identifying the method |
| `type` | Yes | Cryptographic algorithm type |
| `controller` | Yes | Owner DID (or null for document controller) |
| `publicKeyHex` | Yes | Compressed public key in hex |

**Special Notes:**
- Each document must contain exactly one `#masterKey` verification method
- When `controller` is all zeros (`did:mdevice:0...0`), it refers to the document's controller

## 1.4 Functional Design

### 1.4.1 DID Creation

**Process Flow:**
1. Generate ECDSA keypair (secp256k1)
2. Derive Ethereum-style address from public key
3. Construct DID string
4. Call `Register(did, type, serialNumber, controller, publicKey)`

**Initial DID Document Example:**
```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:mdevice:7614aF3f21Aa0238bB930be81b618Ed5f79D33F5",
  "type": "nas",
  "serialNumber": "AB202410X1230001",
  "controller": "did:memo:ce5ac89f...",
  "verificationMethods": [
    {
      "id": "did:mdevice:7614aF3f...#masterKey",
      "type": "EcdsaSecp256k1VerificationKey2019",
      "controller": "did:mdevice:000...000",
      "publicKeyHex": "031c9cc1c36d53ff1feae79bbd32854a05b3cb4bbf4032383ed1eac79af9a918e7"
    }
  ]
}
```

### 1.4.2 DID Resolution

**Methods:**
1. `Resolve(did)` - Retrieves complete DID document
2. `Dereference(didUrl)` - Resolves specific verification method

### 1.4.3 Document Updates

**Controlled Operations:**
1. `changeType(did, newType)` - Owner-only
2. `changeController(did, newController)` - Owner-only  
3. `addVerificationMethod(did, type, controller, publicKeyHex)` - MasterKey-only
4. `updateVerificationMethod(methodID, type, publicKeyHex)` - Method controller
5. `deactivateVerificationMethod(methodID)` - MasterKey-only
6. `addAuthentication(did, methodID)` - MasterKey-only
7. `deactivateAuthentication(did, methodID)` - MasterKey-only

**Update Example (Adding Method):**
```go
addVerificationMethod(
  "did:mdevice:7614aF3f...", 
  "EcdsaSecp256k1VerificationKey2019",
  "did:memo:b69f63fe...",
  "02948b109c8298057c1d798d9a14ed30f33689d8397e2e60f89ef408b08d7f48a1"
)
```

### 1.4.4 DID Deletion

Marks DID document as deprecated, indicating device decommissioning.

## Security Considerations

1. **Key Rotation**: Supported through verification method updates
2. **Controller Changes**: Strictly controlled via masterKey authorization
3. **Authentication**: Supports multiple authentication methods for flexible access control
4. **Verification**: All operations require cryptographic proof of authorization

This specification enables secure device identity management while maintaining flexibility for various IoT use cases. The hierarchical control structure (device owner → masterKey → verification methods) provides granular permission management suitable for enterprise deployments.
