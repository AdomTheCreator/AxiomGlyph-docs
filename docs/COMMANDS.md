# Commands and permissions

| Command | Purpose | Permission |
| --- | --- | --- |
| `/ag language` | Open the player language selector | `axiomglyph.use` |
| `/ag language <code>` | Choose your language | `axiomglyph.use` |
| `/ag language <player> <code>` | Set another player's language | `axiomglyph.others` |
| `/ag language reload` | Reload configuration and translations | `axiomglyph.reload` |
| `/ag editor` | Create an administrator editor session | `axiomglyph.editor` |

`axiomglyph.use` is granted to players by default. Other permissions default to
operators. Opening a language selector does not require editor permission.

`/axiomglyph` and `/glyph` are aliases of `/ag`. The standalone commands
`/language`, `/lang` and `/lingua` remain for compatibility; use the canonical
`/ag language` form in new configurations and documentation.

An editor link grants editing access for 30 minutes. Treat it as a credential
and use **Save and exit** to end a session after saving.
