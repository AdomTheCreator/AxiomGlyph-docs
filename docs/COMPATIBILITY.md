# Compatibility and local test results

Test date: **2026-11-05**. Current private version: **1.0.0**.
The checks below were run before the version-only rename; no runtime behavior
was changed by assigning version 1.0.0.
PacketEvents **2.14.0** and PlaceholderAPI **2.12.3** were installed in each
isolated server. Minecraft listeners and editor HTTP listeners were bound to
loopback. Each server used a new disposable world; the production server was
not changed. All eight runs used the same corrected plugin jar.

## Tested combinations

| Platform | Minecraft | Build | Java | Server/editor checks | Player packet checks |
| --- | --- | --- | --- | --- | --- |
| paper | 1.21 | 130 | 21 | Passed | Passed, two clients |
| paper | 1.21.11 | 132 | 21 | Passed | Passed, two clients |
| paper | 26.1.2 | 74 | 25 | Passed | Passed, two clients |
| paper | 26.3 | 242 | 25 | Passed | Not tested |
| folia | 1.21.11 | 14 | 21 | Passed | Passed, two clients |
| folia | 26.3 | 6 (Alpha) | 25 | Passed | Not tested |
| purpur | 1.21.11 | 2568 | 21 | Passed | Passed, two clients |
| purpur | 26.3 | 2655 | 25 | Passed | Not tested |

Folia 26.3 is an upstream **Alpha** build. Passing a local smoke test does not
make that upstream build production-stable. The table records tested builds,
not a certification of every Minecraft 1.21.x or 26.x release.

## What passed

Every listed server passed startup with dependency hooks, command routing,
authenticated editor requests, YAML saves, complete backup restore, console
placeholder resolution, save-and-exit session revocation and graceful shutdown
with exit code 0.

On Paper 1.21 and 1.21.11, Paper 26.1.2, Purpur 1.21.11 and Folia 1.21.11,
two simulated clients received their own configured language. Captured packets
verified system chat, actionbars, titles/subtitles, bossbars, menu titles,
inventory item names/lore, signed-book fields and front/back sign updates.
The fixtures checked that server-side item keys remained unchanged.

Client locale detection, a manual language choice overriding later client
settings, background persistence, editor saves preserving the manual choice and
the choice surviving reconnect all passed. The Folia players were teleported
to X=10000 and X=30000; entity-scheduled operations and simultaneous editor
changes completed without thread-access errors.

An additional Folia 1.21.11 run verified opening the selector through
the real player command, clicking the English option and the scheduled
inventory close, followed by the same packet and reconnect checks.

Paper 26.1.2 was exercised through the compatible 26.1 protocol supported by
the client test library. Native 26.3 clients were not simulated because that
library does not yet include the 26.3 protocol.

## Additional checks

Assertion-based checks passed for console rendering and log metadata, session
expiry/revocation, actual file saves, rollback on injected disk failure, locale
matching, preference persistence through close/reopen, language deletion,
group management, catalog restoration, retention, path rejection, editor HTTP
authentication and lock-independent translation reads.

Local HTTPS checks passed certificate/hostname validation, session authentication
and origin protection. Browser DOM checks passed language/group management,
theme persistence, context deletion, failed save-and-exit recovery and backup
restore success/failure states. These DOM checks are not a visual browser review.

## Issue found and corrected

The initial Paper 1.21 run exposed a sign serialization mismatch: old sign block
data stores JSON text strings, while newer versions use NBT components. The
translator now selects the sign encoding at the 1.21.5 boundary. The corrected
jar passed the complete eight-server matrix, including Paper 1.21 sign updates.

An older persistence check was updated to wait for the documented asynchronous
save rather than assuming the disk write completes inside the player command.

## Remaining boundaries

- No native-player packet verification on 26.3, and no test of every minor build.
- No long-running load test or performance percentage claim.
- Initial-chunk sign resends, hanging signs, equipment/cursor/metadata variants,
  filtered book variants and cross-plugin shared-packet isolation need broader
  checks beyond the tested update/slot paths.
- Creative item localization and writable books remain excluded by design.
- Language changes do not yet force all visible content to refresh.
- The 1.21 client library emitted nonfatal decoder diagnostics on unrelated
  incoming content; targeted translated packets were captured and asserted.
  Full rendering and interaction with the official game client remain useful.
- Local HTTPS does not establish that a particular VPS firewall, domain or
  reverse proxy is configured correctly.

Read [Text coverage](TEXT-SURFACES.md) for feature-specific limits and
[Maintenance](MAINTENANCE.md) for persistence and restore behavior.
