# Placeholders and language files

## AxiomGlyph format

`{{loc:key}}` reads `messages.key` in the player's selected language.
Use `{{loc:group.key}}` for a nested key. PacketEvents must be installed for
supported outgoing player text. Explicit keys in console logs use the separately
configured console language. See [text coverage](TEXT-SURFACES.md) for the exact
surfaces and limitations; a key is not resolved in every arbitrary plugin field.

```yaml
messages:
  death:
    kill: "<white>{1}</white> defeated <white>{2}</white>."
  welcome: "<green>Welcome to the server!"
```

In another plugin's configurable message, use:

```yaml
messages:
  killed-player: "{{loc:death.kill|%attacker%|%victim%}}"
  welcome: "{{loc:welcome}}"
```

The other plugin must resolve its own `%attacker%` and `%victim%` tokens before
sending the message. Values separated by `|` become `{1}`, `{2}`, and so on.
Arguments are treated as plain text, with MiniMessage tags escaped.

Escape a literal separator as `\|` and a literal backslash as `\\` in the
placeholder text. YAML has its own escaping rules; single-quoted YAML strings
preserve backslashes as written.

## PlaceholderAPI format

Plugins that process PlaceholderAPI placeholders can use:

| Placeholder | Value |
| --- | --- |
| `%axiomglyph_welcome%` | Translation for `messages.welcome` |
| `%axiomglyph_death.kill%` | Translation for a dotted key |
| `%axiomglyph_language%` | Selected language ID |
| `%axiomglyph_language_name%` | Language display name |

This syntax does not accept positional arguments. Tab lists and scoreboards
work through the other plugin's PlaceholderAPI integration. Requests without
a player use the configured console language.

## Files and fallbacks

Language IDs come from filenames such as `languages/en_US.yml`. Set
`default-language` in `config.yml`. Each language file can set a `display-name`
and a `fallback` pointing to another configured language ID. Missing values use
the fallback chain and the configured default. Group names map to the first
key segment; [Groups](GROUPS.md) explains the reserved `general` group.

Files remain editable directly. After changing them, run `/ag language reload`
or use **Reload files** in the editor.

## Console

```yaml
console:
  enabled: true
  language: en_US
```

From AxiomGlyph's load phase, explicit `{{loc:key|argument}}` and
`%axiomglyph_key%` tokens passing through the root Log4j logger are resolved in
this language. Formatting becomes plain text, while log metadata and exceptions
are preserved. Unknown keys remain visible. Ordinary log text and exception
stack traces are not machine-translated. Messages emitted before AxiomGlyph loads are not rewritten.
The hook is installed before normal plugin enable messages, so other plugins'
startup keys can be resolved even if they enable before AxiomGlyph.

Run `/ag language reload` after changing console configuration. Editor changes
to translations update the console snapshot when saved.
