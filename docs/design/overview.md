# atproto-profile-alt: design overview

`app.nearhorizon.actor.profileAlt` stores descriptions (alt text) of an
account's profile avatar and banner, as a record in the account's own ATProto
repository.

| Document | Covers |
| --- | --- |
| [overview.md](overview.md) | Why the record exists and the principles behind it. |
| [record.md](record.md) | The record's fields, limits, writing rules and examples. |
| [display.md](display.md) | How clients show descriptions, and when they don't. |

## Why this exists

The Bluesky profile record (`app.bsky.actor.profile`) has an avatar and a
banner but no way to describe either. A screen reader can say "avatar" and
nothing more.

This record holds a description for each image, written by the account
itself. Any client can read it, and clients that don't know the lexicon
ignore it.

## Principles

- **Tied to one image.** Each description names the CID of the image it
  describes. When the image changes, in any client, the old description is
  no longer shown, so it never describes the wrong picture.
- **Self-sovereign.** One record (`rkey: self`) in the account's repo, read
  and written with standard `getRecord` and `putRecord`. No external service.
- **Plain text.** A description is text, stored as typed.
- **Additive.** It describes the profile; it never changes it. A client that
  ignores it loses nothing else.

## Repository layout

```
atproto-profile-alt/
  lexicons/app/nearhorizon/actor/profileAlt.json   the lexicon
  docs/design/                                     this spec
```
