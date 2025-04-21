# MEMO Data Wallet

## Non-Fungible Tokens (NFTs)

Non-Fungible Tokens (NFTs) represent a distinct category of cryptocurrency that differs fundamentally from traditional cryptocurrencies like Bitcoin. While Bitcoin tokens are fungible (each unit is interchangeable and holds equal value), NFTs are unique digital assets with individualized worth. They are commonly used to represent real-world assets such as artwork, music, or films. The value of an NFT is intrinsically tied to the asset it represents - for instance, an NFT representing the literary classic "Journey to the West" holds completely different value from one representing Van Gogh's "Starry Night." Even among multiple NFTs representing the same underlying asset, variations in condition, provenance, or annotations can create significant value differences.

The ERC-721 standard establishes the framework for NFT implementation. Recognizing each NFT's unique value, ERC-721 utilizes uint256 variables to differentiate between tokens. Additionally, it employs a linking mechanism to associate NFTs with their underlying assets, allowing NFT owners flexibility in choosing storage systems. However, this approach requires owners to bear the long-term storage costs of the associated content.

### Challenges and Solutions in NFT Implementation

The fundamental challenge facing NFT owners is the ongoing responsibility for storage costs. Failure to maintain these payments results in the underlying content becoming inaccessible, effectively nullifying the NFT's value. Current industry practice relies on generating sufficient NFT revenue to offset storage expenses, which works well for high-value, low-size assets like digital art or game items (as evidenced by platforms like OpenSea where small image files command significant prices). However, this model proves problematic for data assets where value varies dramatically - from highly valuable corporate data to low-value personal information.

Projects like Ocean Protocol have attempted solutions by introducing:

1. Data NFTs representing copyright ownership

2. Data tokens serving as access credentials

3. An encryption layer managed by specialized nodes

While innovative, this approach introduces multiple failure points (blockchain, storage system, and protocol nodes) and still suffers from the fundamental disconnection between on-chain NFTs and off-chain content status.

### Proposed Solution Architecture

Our solution addresses these limitations through:

1. **On-Chain Status Tracking**: A dedicated storage contract maintains real-time data status (including validity periods)
2. **Proof Mechanisms**: Storage systems provide periodic cryptographic proofs of content preservation
3. **Integrated Access Control**: Permission management tied directly to storage system authentication

### Data Asset Composition

The system organizes data assets into three components:

| Component         | Description                                  | Location              |
|-------------------|----------------------------------------------|-----------------------|
| Storage Metadata  | Hash, size, expiration, uploader address    | On-chain storage contract |
| Permission Metadata | Ownership, access rights, modification rules | On-chain permission contract |
| Data Content      | Actual file content                         | Off-chain storage nodes |

### Permission Contract Specification

The permission contract implements the following key functions:

```solidity
interface dataPermission {
    // Management Functions
    function safeTransferFrom(address from, address to, uint256 tokenId, bytes data) external;
    function setReader(address reader, uint256 tokenId) external;
    function approve(address approved, uint256 tokenId) external;
    function setApprovalForAll(address operator, bool approved) external;
    
    // View Functions
    function ownerOf(uint256 tokenId) external view returns (address);
    function isReader(uint256 tokenId, address reader) external view returns (bool);
    function getApproved(uint256 tokenId) external view returns (address);
    function isApprovedForAll(address owner, address operator) external view returns (bool);
}
```

Key operational rules include:
- All transactions verify content expiration status
- Transfer operations validate ownership chain
- Contract addresses must implement ERC721 receiver interface
- Permission changes require proper authorization

### Storage System Requirements

The integrated storage system must provide:

1. **Metadata Access**: Retrieve storage metadata via content hash
2. **Proof Verification**: Validate storage proofs submitted by nodes
3. **Two-Phase Upload**:
   - On-chain: Storage contract interaction for node selection and metadata recording
   - Off-chain: Actual content transfer to selected nodes
4. **Persistent Validation**: Regular proof-of-storage submissions during content lifetime
5. **Permissioned Access**: Download restricted to authorized users based on on-chain permissions
6. **Economic Model**:
   - Bandwidth fees: Pay-per-use for upload/download
   - Storage fees: Time-based pricing for content persistence

### Practical Examples

**Scenario 1: Basic Transfer**

1. Alice uploads file to storage node
2. Alice grants Bob read access
3. Alice transfers ownership to Carl
4. Result: Bob retains read access, Carl gains ownership - both can access content

**Scenario 2: Expiration**

1. Alice uploads file and grants Bob access
2. Alice transfers ownership to Carl
3. File expires
4. Result: All access disabled (both on-chain functions and content retrieval)

This architecture provides a robust framework for managing data NFTs while maintaining clear linkage between on-chain representations and off-chain content status, solving critical problems in current implementations.
