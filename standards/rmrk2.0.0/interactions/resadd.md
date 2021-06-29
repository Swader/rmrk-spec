# RESADD

The RESADD interaction allows the `issuer` of a [class](../entities/nftclass.md) to suggest a new resource for an [NFT](../entities/nft.md) in that collection. If the `issuer` is also the `owner` of this NFT, this interaction also counts as a [RESACCEPT](resaccept.md) automatically.

This is useful for adding new rendering approaches to existing art, for adding alternative reproductions to a certain piece of media, and more. As an example, consider an NFT which is a book. In its raw form, it can just be a PDF. But we can also `RESADD` a resource which is a high-resolution cover, and one which is an MP3 file. Now the book can be loaded into a marketplace like Opensea, showing the cover, into Audible and play back the audio version, or into [Singular](https://singular.rmrk.app) to be read as a PDF.

## Implications

If the resource being added is the first resource of this NFT, and it is accepted, then this operations results in the NFT being given a `priority` property with the value of `[0]`, meaning the first resource of this NFT is highest priority.

## Pending

A resource that is added to an NFT via `RESADD` enters a pending state and MUST be accepted with a `RESACCEPT` unless the owner of the NFT is the same account as the issuer of the collection, in which case it is automatically accepted.

## Spam

It is possible to spam resources on an NFT that forever take up a resource pending queue. A way to permanently reject resources may be added later if this proves to be the case, but the official recommendation towards implementers right now is to ignore resources that have been pending for more than a month.

## Standard

The format of a RESADD interaction is
`0x{bytes(rmrk::RESADD::{version}::{id}::{html_encoded_json})}`.

- `version` is the version of the standard used (e.g. `2.0.0`)
- `id` is the [NFT](../entities/nft.md)'s ID
- `html_encoded_json` is the html encoded resource, formatted according to the standard in [NFT](../entities/nft.md) under the Resources section

## Examples

Suppose we have an NFT with the ID `5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001`.

Suppose it has no resources right now:

```json
{
  "nftclass": "0aff6865bed3a66b-DLEP",
  "symbol": "DL15",
  "transferable": 1,
  "sn": "0000000000000001",
  "metadata": "ipfs://ipfs/QmavoTVbVHnGEUztnBT2p3rif3qBPeCfyyUE5v4Z7oFvs4"
}
```

We want to add the following resource to it:

```json
{
    "media": "hash-of-metadata-guest-bird-art-with-jetpack",
    "metadata": "hash-of-metadata-with-credits"
}   
```

So we issue:

```txt
rmrk::RESADD::2.0.0::5105000-0aff6865bed3a66b-DLEP-DL15-0000000000000001::%7B%22media%22%3A%22hash-of-metadata-guest-bird-art-with-jetpack%22%2C%22metadata%22%3A%22hash-of-metadata-with-credits%22%7D
```