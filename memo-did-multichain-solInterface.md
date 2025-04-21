# Multichain Interface

1. Create DID

   ```solidity
   function createDID(
   		string memory did,
           string memory methodType,
           bytes memory pubKeyData,
           bytes memory signature
   )
   ```

2. Delete DID

   ```solidity
   function deactivateDID(
           string memory did,
           bool deactivate,
           bytes memory signature
       )
   ```

3. Add VerificationMethod

   ```solidity
   function addVeri(
           string memory did,
           IAccountDid.PublicKey memory pubKey,
           bytes memory signature
       )
   ```

4. Delete VerificationMethod

   ```solidity
   function deactivateVeri(
           string memory did,
           uint index,
           bool deactivate,
           bytes memory signature
       )
   ```

5. Update VerificationMethod

   ```solidity
   function updateVeri(
           string memory did,
           uint index,
           string memory methodType,
           bytes memory pubKeyData,
           bytes memory signature
       )
   ```

6. Add Authtication

   ```solidity
   function addAuth(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

7. Delete Authtication

   ```solidity
   function removeAuth(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

8. Add Assertion

   ```solidity
   function addAssertion(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

9. Delete Assertion

   ```solidity
   function removeAssertion(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

10. Add Delegation

    ```solidity
    function addDelegation(
            string memory did,
            string memory id,
            uint expiration,
            bytes memory signature
        )
    ```

11. Delete Delegation

    ```solidity
    function removeDelegation(
            string memory did,
            string memory id,
            bytes memory signature
        )
    ```

12. Add Recovery

    ```solidity
    function addRecovery(
            string memory did,
            string memory id,
            bytes memory signature
        )
    ```

13. Delete Recovery

    ```solidity
    function removeRecovery(
            string memory did,
            string memory id,
            bytes memory signature
        )
    ```
