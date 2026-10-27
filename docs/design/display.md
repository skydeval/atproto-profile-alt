# Displaying descriptions

## When to show one

Show a description only when it [matches](record.md#matching) the profile's
current image in that slot. In every other case (no record, no slot, a
different CID, a blank `alt`, the record not loaded yet) show the image as
you would without this record. Never show a description that might be of
another picture.

A description is the account's own plain text, like its bio. Show it as
text: don't render markup or turn it into links.

## Screen readers

- **The image itself** (profile header, hover card, the full-size view): the
  description is the image's accessible text (`alt` on the web,
  `accessibilityLabel` or the platform's equivalent on native).
- **An avatar that's also a button or link** (on posts, in lists): add the
  description to the existing label, never in place of it, so the action is
  still announced. For example "Wren's avatar: a red fox in a green scarf".
- Long descriptions can make a list tedious. A client may use the
  description only where the avatar is the focus (the header, the
  full-size view) and keep its plain label in lists.

## Sighted readers

When the avatar or banner is opened full size, show an "ALT" badge that
reveals the description, the way clients do for post images. Show no badge
when no description applies.

## Editing

Put a "Describe this image" control beside the avatar and banner pickers,
using the same alt-text editor and limit as post images, if the client has
one. See [record.md](record.md#writing) for the save order and what to keep.
