# AxiomGlyph documentation

**[Read the AxiomGlyph Wiki](https://github.com/kwasadom/AxiomGlyph-docs/wiki)** — installation, commands, translations and server administration.

Write server translations in YAML or in a browser, then choose the language
shown to each player. AxiomGlyph is part of the **Axiom** suite and does not use
machine translation.

This public repository contains **documentation only**. Plugin source code and
jar files remain private. There is no public plugin download or release yet.

## Read the guides

- [Documentation home](docs/README.md)
- [Installation](docs/INSTALLATION.md)
- [Commands and permissions](docs/COMMANDS.md)
- [Permissions and LuckPerms examples](docs/PERMISSIONS.md)
- [Placeholders](docs/PLACEHOLDERS.md)
- [Web editor](docs/WEB-EDITOR.md)
- [Compatibility and local test results](docs/COMPATIBILITY.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)

## Main features

| Area | Capabilities |
| --- | --- |
| Catalog | YAML, MiniMessage, fallbacks and translation groups |
| Languages | Client locale detection and persistent manual choices |
| Editor | Translation fields per language, light/dark themes, groups, backups and safe exit |
| Text | Supported chat, actionbar, title, bossbar, menu, item, signed-book and sign fields |
| Integrations | PacketEvents for outgoing text, PlaceholderAPI for compatible plugins |
| Maintenance | Background preference saves, backup retention and restoration |

Use `/ag language` for the player selector and `/ag editor` for an administrator
editor session. The editor writes the actual language files. Existing YAML files
remain editable directly.

The plugin targets Paper, Purpur and Folia across Minecraft 1.21.x–26.x. Only
the combinations and checks explicitly listed in [Compatibility](docs/COMPATIBILITY.md)
have been verified. Folia 26.3 is an upstream Alpha build.

## Project status

Current private plugin version: **1.0.0**. No plugin jar is published here.

AxiomGlyph is under active development. This documentation describes the current
development snapshot, not a released product. Source and binary distribution
terms are separate from this documentation; proprietary release review remains
open. Public documentation does not grant access to the plugin.
