# Language selection and maintenance

## Client language

`detect-client-language: true` uses the locale reported by the client on join and
on locale changes. It does not translate text automatically: it selects one of
the configured language files. Matching is case-insensitive and treats `-` and
`_` equally. An exact locale wins, followed by a language-family match (prefer
the configured default, otherwise the first language ID in sorted order).
Unsupported locales use `default-language`.

A saved manual choice from `/ag language` or an administrator always takes
precedence. Client detection is session data and is cleared on disconnect;
manual choices are stored in `players.yml`. Disable detection to use the default
for players without a manual choice. No change forces already visible packet
content to refresh; see TEXT-SURFACES.md.

## Preference saves

Player choices take effect immediately in memory. A dedicated worker coalesces
changes and writes `players.yml` approximately every two seconds using a temporary
file and atomic replacement when supported. The player command does not wait
for disk I/O. Save errors are logged and retried. A normal shutdown requests a
final save and waits up to ten seconds; a timeout or interruption is logged.
An abrupt crash may lose the most recent unsaved choices.

Editor catalog reloads retain unsaved choices. `players.yml` is loaded at startup;
stop the server before editing that file manually. Translation YAML remains
editable directly: use `/ag language reload` or **Reload files** after editing it.

## Backups

```yaml
backups:
  max-count: 100
  max-age-days: 30
```

Either limit can be disabled with `0`. Retention runs after successful editor
writes. The newest complete recovery point is always kept. Incomplete or legacy
backups are not removed automatically. Disk failures are reported without
reporting a successful catalog save as failed solely because cleanup failed.

Every new editor write stores a complete copy of the language and group catalog
as it existed before the operation, alongside the existing per-file recovery
copies. These are local backups, not protection from loss of the server disk.

Open **Backups** in the editor and choose **Restore**. Confirming restores the
entire language and group catalog, including removing languages created after
that snapshot. A recovery backup of the current catalog is created first.
The configured default language must exist in the selected snapshot. YAML is
validated before writing. The active catalog reloads after restoration.
Server settings and player choices are not restored; an unavailable player
language falls back through normal selection rules.

Older backups and incomplete operations appear as **Manual recovery only**.
Their files remain available on disk. Restore them manually with the server
stopped. They do not contain a complete, unambiguous catalog snapshot.

## Verification status

See [Compatibility](COMPATIBILITY.md) for the exact local test coverage and remaining limits.
