# The profileAlt record

NSID: `app.nearhorizon.actor.profileAlt`
Lexicon: [`lexicons/app/nearhorizon/actor/profileAlt.json`](../../lexicons/app/nearhorizon/actor/profileAlt.json)

## Record key

`literal:self`: one record per account, the same pattern as
`app.bsky.actor.profile`.

## Fields

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `avatar` | `imageAlt` | no | Description of the profile avatar. |
| `banner` | `imageAlt` | no | Description of the profile banner. |
| `updatedAt` | datetime | yes | Set on every write. |

### `imageAlt`

| Field | Type | Required | Notes |
| --- | --- | --- | --- |
| `image` | string, `format: cid` | yes | The CID of the image blob described. |
| `alt` | string | yes | The description, in plain text. 1 to 2,000 graphemes, up to 20,000 bytes. |

`image` is the CID string itself (for example `bafkreib...`), the same
string as `ref.$link` in the profile record's `avatar` or `banner` blob. It's
a plain string, not a blob or a link, so the record never keeps an old image
alive in the repo.

## Matching

A description applies only while its `image` equals the CID of the profile's
current image in the same slot:

- `avatar.image` against the profile's `avatar` blob CID;
- `banner.image` against the profile's `banner` blob CID.

Compare the CID strings exactly. If the profile has no image in that slot, or
the CIDs differ, the description doesn't apply. Uploading the very same file
again gives the same CID, so its description still applies, which is right:
it's the same picture. A reader that only has a
profile view can take the CID from the image URL when its host puts the CID
in the URL; otherwise it fetches the profile record.

## Writing

- **Write after the profile.** Save the profile record first, so the new
  blob's CID is known, then write the description with that CID.
- **A new image drops its description.** When the user picks a new avatar
  or banner, the old description no longer matches. Offer to describe the
  new image instead of carrying the old text over.
- **Keep what you don't change.** Editing the avatar's description keeps the
  banner's, and the other way round.
- **Keep unknown properties.** A client that rewrites the record keeps
  properties it doesn't know. Read the record first, and if that read fails
  for any reason other than the record not existing, don't write.
- **Don't overwrite a newer write.** Pass the read record's CID as
  `swapRecord`, so a save from another client in between makes yours fail
  instead of silently replacing it.
- **Removing a description** means writing the record without that slot.
  An `alt` is never empty; leave the slot out instead. Trim the text before
  saving, and treat text that is only whitespace as no description.
- **Stale slots are harmless.** A description of an old image is simply
  unused. A client may drop it the next time it writes the record.

Clearing every slot and deleting the record mean the same thing: no
descriptions.

## Example

```json
{
  "$type": "app.nearhorizon.actor.profileAlt",
  "avatar": {
    "image": "bafkreihdwdcefgh4dqkjv67uzcmw7ojee6xedzdetojuzjevtenxquvyku",
    "alt": "A red fox curled up asleep, wearing a green knitted scarf."
  },
  "banner": {
    "image": "bafkreie5737gdxlw5i64vzichcalba3z2v5n6icifvx5xytvske7mr3hpm",
    "alt": "A pine forest at dusk under a pink and orange sky."
  },
  "updatedAt": "2026-10-26T12:00:00Z"
}
```

## Reading

No auth needed. Fetch the record from the account's PDS:

```bash
curl "https://bsky.social/xrpc/com.atproto.repo.getRecord?repo=did:plc:example&collection=app.nearhorizon.actor.profileAlt&rkey=self"
```

## Writing example

After saving the profile, with the new avatar's blob ref in hand:

```typescript
const existing = await agent.com.atproto.repo
  .getRecord({repo: userDid, collection: 'app.nearhorizon.actor.profileAlt', rkey: 'self'})
  .catch((err) => {
    // Only "no record yet" means start fresh. On any other error, stop:
    // writing anyway would drop the banner and unknown properties.
    if (err?.error === 'RecordNotFound') return undefined
    throw err
  })

await agent.com.atproto.repo.putRecord({
  repo: userDid,
  collection: 'app.nearhorizon.actor.profileAlt',
  rkey: 'self',
  swapRecord: existing?.data.cid, // fail instead of overwriting a newer write
  record: {
    ...existing?.data.value, // keep the banner and unknown properties
    $type: 'app.nearhorizon.actor.profileAlt',
    avatar: {image: avatarBlob.ref.toString(), alt: 'A red fox in a green scarf.'},
    updatedAt: new Date().toISOString(),
  },
})
```
