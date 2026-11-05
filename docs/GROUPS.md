# Groups, themes and manual YAML editing

Use **Groups** in the editor to create an empty group or delete a group and all translations it contains. Deletion requires confirmation and keeps original files in the normal editor backup. The `general` group is reserved for root translation keys and cannot be removed.

Empty groups are stored in `plugins/AxiomGlyph/groups.yml`:

```yaml
groups:
  - sunlight
  - menus
```

Groups are also inferred from the first segment of the translation keys, even if they are absent from this registry. Language files remain editable by hand:

```yaml
display-name: Italiano
messages:
  sunlight:
    welcome: 'Benvenuto'
```

This creates group `sunlight`, key `welcome`, with `{{loc:sunlight.welcome}}` and `%axiomglyph_sunlight.welcome%`. Root keys such as `messages.welcome` appear under `general`. The translation editor uses a select dropdown containing all registered and inferred groups.

After editing language files or `groups.yml`, use **Reload files** in the editor or `/ag language reload` in Minecraft. Main `config.yml` changes should use `/ag language reload`. To remove a group manually, remove it from the registry and remove its translation section from the language files. Removing only its translations leaves a registered empty group.

Right-click a translation row to delete it without opening its form. The theme button switches between light and dark; the preference is saved in the browser and does not change server files.
