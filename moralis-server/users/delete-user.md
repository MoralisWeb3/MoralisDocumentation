# Delete User

### Manually Deleting Users

During testing, if you manually delete a user, then make sure to delete the corresponding rows in the following collections (where "xxx" is the chain prefix). Not doing so may cause unexpected behavior when authenticating via Web3 from the address that was just deleted.

* Session
* \_xxxAddress
