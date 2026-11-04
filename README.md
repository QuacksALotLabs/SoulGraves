<img src="icon.svg" width="150" alt="Icon" align="left">

### SoulGraves

![Paper Plugin](https://img.shields.io/badge/api-Paper-00c8d4?labelColor=008a9c&style=flat-square&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZD0iTTIgMTJMMjIgMkwxMiAyMkwxMCAxM0wyIDEyWiIgZmlsbD0iI2ZmZmZmZiIvPjxwYXRoIGQ9Ik0xMCAxM0wyMiAyIiBzdHJva2U9IiMwMDAwMDAiIHN0cm9rZS13aWR0aD0iMC44IiBvcGFjaXR5PSIwLjMiLz48L3N2Zz4=)
![SoulGraves Version](https://img.shields.io/github/v/tag/QuacksALotLabs/SoulGraves?label=Version&style=flat-square&color=00c8d4&labelColor=008a9c)

is the grave system of your project: it intercepts player deaths, places a gravestone (optionally automatically inside a configurable graveyard region), and stores the player's inventory, XP, and soul in a soul orb that is bound to that grave. It works standalone and knows nothing about limbo or rituals — it simply exposes an API and events that other plugins can use to access graves.

<br clear="left">

## Plugin Dependencies

| Plugin | Required? | Purpose |
|---|---|---|
| **Nexo** / **ItemsAdder** / **Oraxen** | Optional (one of them) | Custom gravestone blocks & soul orb items |
| **WorldGuard** | Optional | Manage graveyard regions via WorldGuard regions |
| **Database** (local SQLite / external MySQL) | Optional | Persistence of graves & regions (SQLite requires no installation) |