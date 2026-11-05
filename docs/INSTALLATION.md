# Installation

There is currently no public download. These instructions apply to an authorized
development copy supplied by the project owner.

1. Check [Compatibility](COMPATIBILITY.md) for the exact tested platforms and
   versions. Use the Java runtime required by the Minecraft server.
2. Stop the server normally and back up the plugin data directory if it exists.
3. Put the supplied AxiomGlyph jar in `plugins/`. Install a compatible
   PacketEvents build for outgoing text localization. Install PlaceholderAPI
   when other plugins use AxiomGlyph's PAPI placeholders.
4. Start the server and confirm AxiomGlyph enabled without errors.
5. Configure `plugins/AxiomGlyph/config.yml` and the YAML files under
   `plugins/AxiomGlyph/languages/`.
6. Run `/ag language reload`, then `/ag editor` to open the translation editor.

## Language defaults

```yaml
default-language: en_US
detect-client-language: true
console:
  enabled: true
  language: en_US
```

Client detection selects an existing translation language. It does not create
translations. A player's saved manual choice takes precedence. Language IDs
come from filenames, such as `en_US.yml`.

The editor binds to `127.0.0.1:8765` by default. Open the session link on the
server computer, or configure [HTTPS access](REMOTE-EDITOR.md) for another PC.
Listener and certificate changes require a full server restart.
