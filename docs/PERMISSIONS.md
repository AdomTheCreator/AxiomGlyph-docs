# Permissions

AxiomGlyph uses four permission nodes. Assign them through your server's
permissions manager, such as LuckPerms.

| Permission | Default | Allows |
| --- | --- | --- |
| `axiomglyph.use` | All players | Open `/ag language` and change your own language with `/ag language <code>` |
| `axiomglyph.others` | Operators | Set another online player's language with `/ag language <player> <code>` |
| `axiomglyph.reload` | Operators | Reload configuration and translations with `/ag language reload` |
| `axiomglyph.editor` | Operators | Generate a web editor session with `/ag editor` |

**All players** means the node defaults to `true`. **Operators** means it
defaults to `op`. Explicit assignments from a permissions manager can change
the effective result. AxiomGlyph does not change group membership or grant OP.

Receiving translated messages and detecting the client language require no
additional permission. `axiomglyph.use` controls opening the selector and
manually choosing a language.

## Assign permissions with LuckPerms

These examples use an existing `default` or `admin` group. Replace group and
player names with those on your server. In the server console, omit the leading
slash. Running LuckPerms commands requires the appropriate LuckPerms permissions.

### Let players choose their language

This is already enabled by AxiomGlyph's default, but can be assigned explicitly:

```text
/lp group default permission set axiomglyph.use true
```

### Allow an administrator to manage AxiomGlyph

```text
/lp group admin permission set axiomglyph.use true
/lp group admin permission set axiomglyph.others true
/lp group admin permission set axiomglyph.reload true
/lp group admin permission set axiomglyph.editor true
```

The administrative nodes are independent. Giving someone editor access does
not also grant the ability to change another player's language or run the
reload command.

### Give one person access to the editor

```text
/lp user PlayerName permission set axiomglyph.editor true
```

An editor session grants full catalog access: translations, groups, languages
and backup restoration. There are currently no separate read-only, per-group
or per-language editor permissions.

### Deny manual language selection

```text
/lp user PlayerName permission set axiomglyph.use false
```

To remove that explicit assignment and return to inherited/default behavior:

```text
/lp user PlayerName permission unset axiomglyph.use
```

An explicit `false` and removing an assignment have different meanings. See
[LuckPerms usage](https://luckperms.net/wiki/Usage) and
[permission command reference](https://luckperms.net/wiki/Permission-Commands).

## Editor sessions and permission changes

The editor permission is checked when a session link is generated. The browser
then authenticates with that session token. Removing a player's permission does
not revoke an already issued session.

Keep editor links private. **Save and exit** saves the draft and revokes that
session after verification; sessions also expire after 30 minutes or a server
restart. A session holder can use the editor's reload and catalog operations
without a separate in-game `axiomglyph.reload` check.

## Check a permission problem

Confirm the exact node, the player's inherited groups and any explicit denial
in your permissions manager. Test the actual player account; an operator's
default administrative permissions can differ from a regular player's.
Command aliases use the same checks as the canonical `/ag` commands.
