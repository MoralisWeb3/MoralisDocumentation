# 🤝 Resolve Transfer

### Resolving the results

`Moralis.transfer()` returns a [transaction response](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse). This object contains all data about the transaction. If you need data about the **result** of the transaction, then you need to wait for the transaction to be [**confirmed**](https://ethereum.org/en/developers/docs/transactions/#transaction-lifecycle).

You can do this via `transaction.wait()` to wait for 1 confirmation (or `transaction.wait(5)` to wait for 5 confirmations)

**Example -**&#x20;

```javascript
const transaction = await Moralis.transfer(options);
const result = await transaction.wait();
```
