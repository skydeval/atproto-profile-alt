# ATProto Profile Alt Text Lexicon

**Namespace:** `app.nearhorizon.actor.profileAlt`
**Version:** 1.0
**License:** Apache 2.0

Descriptions (alt text) for a profile's avatar and banner, stored as a record
in the account's own repository. Each description is tied to its image by CID,
so changing the picture in any client never leaves a description of the wrong
one.

## The record

| NSID | Key | Holds |
| --- | --- | --- |
| `app.nearhorizon.actor.profileAlt` | `self` | An optional `avatar` and `banner`, each `{image, alt}`: the image's CID and up to 2,000 graphemes of plain text. Plus `updatedAt`. |

```json
{
  "$type": "app.nearhorizon.actor.profileAlt",
  "avatar": {
    "image": "bafkreihdwdcefgh4dqkjv67uzcmw7ojee6xedzdetojuzjevtenxquvyku",
    "alt": "A red fox curled up asleep, wearing a green knitted scarf."
  },
  "updatedAt": "2026-10-26T12:00:00Z"
}
```

## Adopting it

1. Use the lexicon: [`lexicons/app/nearhorizon/actor/profileAlt.json`](lexicons/app/nearhorizon/actor/profileAlt.json).
2. Show a description only when its `image` equals the CID of the profile's
   current avatar or banner.
3. Use it as the image's accessible text. Where the avatar is a button, add
   it to the label, never replace the label.
4. When writing, save the profile first, then the description with the new
   image's CID. Keep the other slot and any unknown properties.

## Docs

- [Overview](docs/design/overview.md): why it exists and the principles.
- [The record](docs/design/record.md): fields, limits, matching, writing rules, examples.
- [Display](docs/design/display.md): screen readers, the ALT badge, editing.
- [Changelog](CHANGELOG.md)

## Implementations

*None yet. Be the first!* Open an issue or PR to get listed.

## License

Apache 2.0
