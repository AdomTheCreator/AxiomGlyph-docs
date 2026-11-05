# Troubleshooting

## A placeholder is shown literally

Check that the key exists under `messages:` and that its group matches the key.
Run `/ag language reload` after editing YAML files. For `{{loc:...}}`, confirm
PacketEvents is enabled and the text field is covered by
[Text coverage](TEXT-SURFACES.md). For `%axiomglyph_...%`, confirm the producing
plugin processes PlaceholderAPI and that PlaceholderAPI is installed.

## A player sees a different language

A saved manual language choice takes precedence over the client locale. Confirm
the player's choice with `/ag language`. Missing text follows the language's
fallback and then the configured default. There must be a configured translation
file for the desired language.

## Existing text does not change immediately

Translations are applied when packets are sent. Reopen the menu or resend the
affected content. A language change does not yet force a refresh of all visible
signs, bossbars, items or open menus.

## Item text stays untranslated in Creative

This is intentional: Creative actions can send the displayed item back to the
server. Use Survival for the currently supported item-localization flow.
Writable book pages are also excluded; signed books are supported.

## The editor link does not work

Generate a fresh link with `/ag editor`. Links expire after 30 minutes and on
restart. A localhost link must be opened on the server computer. For access
from another machine configure [Remote editor](REMOTE-EDITOR.md), including
HTTPS, the allocated port and relevant firewall rules.

## Save or restore fails

Keep the open draft, read the editor error and check the server log. Verify file
permissions, free disk space and valid YAML. Do not delete existing files to
make an error disappear. Use a complete recovery backup if needed.
**Save and exit** leaves the session available when verification fails.

## Report a problem

Include the plugin version, server platform/build, Java version, dependency
versions and minimal reproduction steps. Remove tokens, session links, passwords,
certificate keys and private player data from logs and screenshots.
