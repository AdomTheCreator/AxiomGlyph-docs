# Outgoing text localization

Use `{{loc:group.key}}` in the configurable text sent by another plugin. AxiomGlyph resolves the key using the receiving player's language and the language YAML fallback. It is not machine translation. PlaceholderAPI syntax still requires the producing plugin to parse PlaceholderAPI itself.

The outgoing packet listener now handles:

| Surface | Fields |
| --- | --- |
| System chat | Message component, including the overlay flag already supplied by the sender |
| Actionbar | Dedicated actionbar component |
| Titles | Title and subtitle; timing packets are unchanged |
| Bossbar | Title on creation and title updates; health, style and flags are unchanged |
| Inventory menu | Window title |
| Item stacks | Custom name, item name and individual lore lines |
| Signed books | Title, author and pages, including filtered variants |
| Signs and hanging signs | Front/back messages and filtered messages; both block-entity updates and initial chunk data |

Item text is handled in full inventory contents, individual slots, cursor stacks, newer player-inventory packets, equipment, and item-stack entity metadata. Only changed stacks, component lists and sign NBT are copied. Other item components (custom model data, attributes, enchantments, etc.) are preserved. Terrain, heightmaps and lighting are not deliberately modified.

## Examples

```yaml
messages:
  menus:
    fishing: '<aqua>Fishing shop'
  items:
    rod-name: '<gold>River rod'
    rod-description: '<gray>A light fishing rod.'
  books:
    welcome-page: '<gold>Welcome!</gold><newline><gray>Explore the world at your pace.'
  signs:
    market: '<blue>Market'
```

Use `{{loc:menus.fishing}}` for a menu title, `{{loc:items.rod-name}}` for an item name, `{{loc:items.rod-description}}` in a lore line, `{{loc:books.welcome-page}}` in a signed book page, and `{{loc:signs.market}}` on a sign line. The key and group remain editable through language YAML files or the web editor. Arguments such as `{{loc:key|value}}` remain supported.

## Current boundaries

- Local player tests cover the surfaces listed below on the tested builds. Consult [Compatibility](COMPATIBILITY.md) for exact versions and coverage; this does not certify every build.
- Localization is applied when a packet is sent. Changing language or editing a translation does not yet force a resend of open menus, visible signs, existing bossbars or previously received item stacks. Reopen/resend the affected content to see its updated translation. Menu titles require a newly opened window.
- Inventory/item text localization is skipped for recipients in Creative mode or whose game mode has not yet been observed. Creative inventory actions can submit the client-visible stack back to the server. This boundary remains until reversible creative-item handling is implemented. Menu titles, titles, bossbars and signs are independent of that item guard.
- Written, signed books are covered; writable book pages are deliberately left unchanged because edited pages are submitted back to the server. Signed-book titles are constrained to Minecraft's 32-character title limit when a translation changes them.
- The listener does not directly edit server inventories, books or sign blocks. A player or another plugin may still perform its normal item/sign editing operations. Editing localized sign text can replace its keys, so maintain those keys through the original server/plugin configuration.
- Only the 1.21.x–26.x packet family is targeted. Alternative protocol representations, creative interactions, client refresh behavior, initial-chunk sign updates and shared-packet isolation require broader checks than the current local matrix.
- Unsupported or malformed content is kept as the original packet, with warnings limited to one per minute. Unknown translation keys remain visible.

Implementation uses PacketEvents wrappers and data components, not direct NMS classes. No translation texts, player preferences or existing server worlds are migrated by this change.
