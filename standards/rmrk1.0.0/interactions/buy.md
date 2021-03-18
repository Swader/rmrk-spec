# BUY

The BUY interaction allows a user to immediately purchase an [NFT](../entities/nft.md) listed for
sale using the [LIST](list.md) interaction, as long as the listing hasn't been canceled.

You can only BUY an existing NFT (one that has not been [CONSUMEd](consume.md) yet).

## Standard

The BUY interaction is a
[batchAll call](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions) of a
system remark and a token transfer.

- the first message, A is: `0x{bytes(RMRK::BUY::{version}::{id})}`.
- the second transaction is a transfer of tokens matching the price advertised on the item ID's
  [LIST](list.md) entry.

The format of BUY is thus: `utility.batchAll(system.remark(A), balances.transfer({list_price}))`.

- `version` is the version of the standard used (e.g. `1.0.0`)
- `id` is the [nft](../entity/nft.md)'s ID [computed field](../entities/nft.md#computed-fields).
- `list_price` is the price advertised on the item's [LIST](list.md) entry.

A BUY that satisfies the [LIST](list.md) conditions of an item's sale immediately counts as a
transfer of ownership. No [SEND](send.md) is necessary.

## Effects

This interactions cancels any pending [LIST](list.md) on the NFT with this ID. It is equivalent to
having called LIST with a cancel on it.

## Examples

Suppose we have the following NFT minted in block 5105000:

```json
{
  "collection": "0aff6865bed3a66b-VALHELLO",
  "name": "POTION_HEAL",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

And this NFT has been listed with
`RMRK::LIST::1.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-0000000000000001::10000000000` for
0.01 KSM by user `CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp`.

To buy it, we format our BUY interaction like so:

```
RMRK::BUY::1.0.0::5105000-0aff6865bed3a66b-VALHELLO-POTION_HEAL-0000000000000001
```

In bytes, that's:

```
0x524d524b3a3a4255593a3a312e302e303a3a353130353030302d306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d30303030303030303030303030303031
```

To complete the purchase, we submit a `utility.batchAll` call comprised of two transactions:

```js
utility.batchAll([
  system.remark(
    0x524d524b3a3a4255593a3a312e302e303a3a353130353030302d306166663638363562656433613636622d56414c48454c4c4f2d504f54494f4e5f4845414c2d30303030303030303030303030303031
  ),
  tx.balances.transfer(CpjsLDC1JFyrhm3ftC9Gs4QoyrkHKhZKtK7YqGTRFtTafgp, 10000000000),
]);
```

It is an implementing library's responsibility to count BUYs without a sufficient transfer attached
as invalid. It is the buyer's responsibility to send the right amount or risk sending money to a
seller without actually getting the item.
