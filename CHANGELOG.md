## RMRK v1.0.0 - 05.02.2021.

> ⚠ Because 0.1 was so broken it would have required a very large custom processor for a very small
> number of published RMRKs, we decided to deprecate it fully, CONSUME all published NFTs, and
> re-issue them, re-sending to their owners (this was done in agreement with both the current owners
> and the original minter).

### RIPs

- [RIP #001](https://github.com/Swader/rmrk-spec/issues/2): the NFT entity's computed ID field has
  been updated to include minting block number.
- [RIP #002](https://github.com/Swader/rmrk-spec/issues/3): Embedded data - ability to directly add
  data to an NFT and a collection, avoiding reliance on third party metadata. Accompanying metadata
  is still recommended.
- [RIP #005](https://github.com/Swader/rmrk-spec/issues/6): Changing recommendation to use
  `batchAll` instead of `batch`
- [RIP #006](https://github.com/Swader/rmrk-spec/issues/10): Implemented EMOTE interaction
- [RIP #007](https://github.com/Swader/rmrk-spec/issues/13): Removed version from collection payload

### Fixes

- Fixed some dead links and typos.
- Standard versions have been added to MINT and MINTNFT where previously there were none. Tools
  should consider these interactions as 0.1 where non-specified.
- NFT standard used "name" as "instance" and as missing "instance". This has been fixed and
  additionally clarified.
