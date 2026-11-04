# Project: SoulGraves

## Overview
SoulGraves is a Minecraft plugin built exclusively against the **Paper API** (no Spigot/Bukkit-only support). It is a grave system: when a player dies, a gravestone is placed and the player's inventory, XP, and "soul" are stored in a soul orb bound to that grave. It works standalone and knows nothing about limbo or rituals — it exposes an API and events for other plugins.

## Core Requirements
- Intercept `PlayerDeathEvent`: clear vanilla drops, store inventory (main + armor + offhand) and XP in a `GraveData` object.
- Place a gravestone block (configurable: vanilla block or custom Nexo/ItemsAdder/Oraxen block) at the death location or inside an optional graveyard region.
- Create a **soul orb** custom item with the `graveId` stored in its PersistentDataContainer (PDC).
- Right-click on the gravestone extracts the soul orb (permission-gated).
- Optional **graveyard regions**: configurable regions where graves are automatically placed instead of the death location. Supports internal region storage (BoundingBox) and optional WorldGuard regions. Placement strategies: spiral scan from a spawn point, random attempts, grid, first-free-slot. Configurable mode: `IN_REGION_ONLY`, `ALWAYS`, `DISABLED`.
- Everything fully configurable via `config.yml` (blocks, items, regions, placement strategy, permissions, database).
- Persistence: YAML or SQLite/MySQL (graves + regions survive restarts).

## Public API (for other plugins)
- `SoulGraveAPI` interface registered via `ServicesManager`:
  - `getGrave(UUID graveId)`, `getGraveByPlayer(UUID playerUUID)`, `hasGrave(UUID)`
  - `releaseGrave(UUID graveId, Player receiver)` — returns items, removes gravestone + orb
  - `getGraveIdFromOrb(ItemStack)` — reads `graveId` from the orb's PDC
- `SoulGraveCreatedEvent` fired after a gravestone is placed (contains player, graveId, grave location, death location).

## Dependencies (server-installed, all optional)
| Plugin | Required? | Purpose |
|---|---|---|
| Nexo / ItemsAdder / Oraxen | Optional (one of them) | Custom gravestone blocks & soul orb items |
| WorldGuard | Optional | Graveyard regions via WorldGuard regions |
| Database (local SQLite / external MySQL) | Optional | Persistence (SQLite requires no installation) |

`plugin.yml`: `softdepend: [Nexo, ItemsAdder, Oraxen, WorldGuard]`

## Build Dependencies
- Paper API 1.21.x (`compileOnly`)
- Adventure MiniMessage (`compileOnly`)
- ItemUtils — unified Nexo/ItemsAdder/Oraxen API (`implementation`, shaded)
- WorldGuard API (`compileOnly`)
- BoostedYAML (`implementation`, shaded)
- CommandAPI for Paper (`implementation`, shaded)
- Jackson / JacksonPaper (`implementation`, shaded)
- HikariCP (`implementation`, shaded)
- MockBukkit + JUnit 5 (`testImplementation`)

## Permissions (`plugin.yml`)
- `soulgraves.command.use` (default: true)
- `soulgraves.graveyard.admin` (default: op)
- `soulgraves.grave.teleport` (default: op)
- `soulgraves.grave.bypass` (default: op)
- `soulgraves.admin` (default: op, children of the above)

## CI/CD
GitHub Actions workflow (`.github/workflows/release.yml`):
- Trigger: `push` on tags matching `v[0-9]+.[0-9]+.0` (i.e. only `vX.X.0` releases, patch versions like `v1.0.1` are ignored)
- JDK 21 (Temurin), `chmod +x ./gradlew`, `./gradlew build` (or `shadowJar`)
- Create GitHub Release via `softprops/action-gh-release@v2`
- `generate_release_notes: true` — GitHub automatically generates a changelog summary from commits and PRs since the last release
- Upload `build/libs/*.jar` as release asset
- `permissions: contents: write` required for release creation

## License
GNU General Public License v3.0 (GPL-3.0). `LICENSE` file in repo root, copyright header in every `.java` file.

## Repo
https://github.com/QuacksALotLabs/SoulGraves