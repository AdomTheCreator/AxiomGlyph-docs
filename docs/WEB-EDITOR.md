# Web editor

## Open a session

Run `/ag editor` with `axiomglyph.editor` permission (operators by default).
Open the generated link. It grants editing access for 30 minutes and is invalid
after a server restart. Do not share session links or include them in screenshots
and bug reports. Opening the base address alone does not authorize API access.

The editor binds to `127.0.0.1:8765` by default. Open the link on the computer
running the Minecraft server. To connect from elsewhere, follow
[External editor access](REMOTE-EDITOR.md). Listener changes require a restart.

## Edit translations

Search or filter by group, then open a translation. Select a group, enter the
placeholder key and fill in one field per configured language. Leave a language
field empty to use its fallback. **Save** writes the actual YAML files on the
server and displays the AxiomGlyph and PlaceholderAPI formats for that key.

Right-click a row to delete it without opening it. Deletion requires confirmation.
Use the theme control to switch between light and dark mode; the preference is
kept in the browser.

## Manage the catalog

- **Groups** creates or deletes groups. Removing a group also removes its
  translations. The reserved `general` group is protected.
- **Languages** adds a language with a name and fallback, or removes an existing
  one. The default language is protected. Removed languages are backed up and
  fallback references and affected player preferences are remapped.
- **Backups** restores the language and group catalog from before an editor
  change. A recovery copy of the current catalog is created first. Read
  [Maintenance](MAINTENANCE.md) before restoring.
- **Reload files** reads translations edited directly in YAML. Main configuration
  changes should use `/ag language reload`.

## Save and exit

**Save and exit** saves an open draft, verifies active language files on the
server, and revokes the session only after successful verification. A validation,
network or disk error keeps the interface available so you can recover your work.
Closing the browser tab does not save an unfinished draft.

Editor writes are staged and verified. Recoverable batch failures attempt to
restore the previous files. Backups remain necessary for a process crash or
machine failure; they are not an off-site copy of the server data.
