# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [1.0] — 2026-10-26

Initial release: `app.nearhorizon.actor.profileAlt`.

- Writer rules: save the profile first, keep the other slot and unknown properties, don't write after a failed read, pass `swapRecord`, trim `alt` and treat whitespace-only text as no description.
- Display: show a description only while its image's CID matches, as plain text, and add it to a button's label rather than replacing it.
