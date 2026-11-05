# Updates and rollback

No public plugin release or automatic update service is available yet. Updates
are supplied manually to authorized development installations.

1. Read the supplied change notes and tested compatibility information.
2. Stop the server normally and back up the plugin data directory and old jar.
3. Replace only the AxiomGlyph jar. Keep language files, groups.yml, config.yml
   and players.yml. Never run two versions simultaneously.
4. Add any newly documented settings, then start the server and inspect the log.
5. If an update fails, stop the server and restore both the old jar and its
   matching data backup. Avoid plugin hot-reload tools.

Missing new configuration options use built-in defaults. Existing configuration
files are not replaced automatically. See [Languages and backups](MAINTENANCE.md)
for editor restore, retention and the scope of a catalog backup.
