# THEMEADD

The THEMEADD interaction allows the `issuer` of a [base](../entities/base.md) to add a new theme to
the base.

Themes are **named** objects of `variable` => `value` pairs which get interpolated into the base's
`themable` parts according to its type. E.g. on an SVG-type base, a `green` theme which defines the
`primary_color` as `0x00ff00` will make sure that every `{primary_color}` placeholder inside of all
`themable` SVG parts of this base (fixed or slots) during rendering get replaced by the `0x00ff00`
value, thus theming them.

A theme name must be unique in a base, and a theme with a key other than `default` CANNOT be added
before there is a theme with the `default` key.

```json
{
  "type": "svg",
  "parts": [
    {
      "id": "wing_1_front",
      "type": "fixed",
      "z": 3,
      "src": "ipfs://ipfs/hash2",
      "themable": true,
      "..."
    }
  ],
  "themes": {
    "default": {
      "primary1": "yellow",
      "secondary1": "darkyellow"
    },
    "sepia": {
      "primary1": "0x47822a",
      "secondary1": "0x331213"
    }
  }
}
```

## Format

The format of a `THEMEADD` interaction is:

```
0x{bytes(rmrk::THEMEADD::{version}::{base_id}::{name}::{html_encoded_json})}.
```