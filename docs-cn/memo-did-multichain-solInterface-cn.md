1. 创建DID

   ```solidity
   function createDID(
   		string memory did,
           string memory methodType,
           bytes memory pubKeyData,
           bytes memory signature
   )
   ```

2. 删除DID

   ```solidity
   function deactivateDID(
           string memory did,
           bool deactivate,
           bytes memory signature
       )
   ```

3. 添加验证方法

   ```solidity
   function addVeri(
           string memory did,
           IAccountDid.PublicKey memory pubKey,
           bytes memory signature
       )
   ```

4. 删除验证方法

   ```solidity
   function deactivateVeri(
           string memory did,
           uint index,
           bool deactivate,
           bytes memory signature
       )
   ```

5. 修改验证方法

   ```solidity
   function updateVeri(
           string memory did,
           uint index,
           string memory methodType,
           bytes memory pubKeyData,
           bytes memory signature
       )
   ```

6. 添加Authtication

   ```solidity
   function addAuth(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

7. 删除Authtication

   ```solidity
   function removeAuth(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

8. 添加Assertion

   ```solidity
   function addAssertion(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

9. 删除Assertion

   ```solidity
   function removeAssertion(
           string memory did,
           string memory id,
           bytes memory signature
       )
   ```

10. 添加Delegation

    ```solidity
    function addDelegation(
            string memory did,
            string memory id,
            uint expiration,
            bytes memory signature
        )
    ```

11. 删除Delegation

    ```solidity
    function removeDelegation(
            string memory did,
            string memory id,
            bytes memory signature
        )
    ```

12. 添加Recovery

    ```solidity
    function addRecovery(
            string memory did,
            string memory id,
            bytes memory signature
        )
    ```

13. 删除Recovery

    ```solidity
    function removeRecovery(
            string memory did,
            string memory id,
            bytes memory signature
        )
    ```
