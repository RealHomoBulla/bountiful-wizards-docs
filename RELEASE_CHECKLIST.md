# Before this pack goes public

Opened 2026-08-03, during the config pass. Things that are harmless on the author's own machine and
wrong the moment somebody else installs the pack. Nothing here is urgent for private play, which is
exactly why it needs a list — none of it will announce itself.

## ▶️ Run this first — most of section 1 is now a command

```
python tools/prepare_release.py            # report only
python tools/prepare_release.py --apply    # clear what is unambiguously personal residue
python tools/audit_config_drift.py  # after client + mirror are current
```

The config audit is a release gate as well as a sync check: it verifies the curated master config
set against the client and `server_mirror/`, and ensures each intentionally carried config is named
in `reference/tuned_config_manifest.json`. Its detailed UTF-8 report is
`bountiful_wizards_REMAKE/reference/_config_drift_report.md`; a missing copy fails loudly.

Added 2026-08-06, because a checklist was the wrong shape for this: **several entries grow back**.
`config/inventoryprofilesnext/` was swept clean on 2026-08-04 and by 08-06 already carried a public
routable IPv6 address again — the folder is recreated on every server join. And `oculus.properties`
is rewritten by the client on exit, so `enableShaders` is true again the moment you play. A one-off
tidy cannot hold either; a step in the packaging routine can.

It clears personal ids by **blanking** them, so each install regenerates its own rather than
inheriting a different arbitrary value. Judgement calls — heap, the Forge-owned early window, the
deliberate `betterfpsdist` tune, machine-tuned shader profiles — are reported and never touched.
The boxes below stay: they are the record of *why* each item is on the list.

## 1. Personal machine state that would ship with the pack

- [ ] **`cubes_without_borders.json` — `preferredMonitor = "0,0,3840,2160"`.** That is this machine's
      4K monitor. Every player would get those coordinates as their default. Reset before packaging.
- [ ] **`cyclopscore-common.toml` — `anonymousAnalyticsID = "6897330b-49d0-4ed0-806a-a257c91d2605"`
      with `analytics = true`.** The id is *yours*. Shipped as-is, every player's telemetry reports
      under one identifier. Either clear the id so each install generates its own, or set
      `analytics = false`.
- [ ] **`betterfpsdist.json` — `verticalScaling = 0.5`** against a default of 2.0. A deliberate and
      good tune for this machine, but it is a *client* render decision; decide whether the pack
      should impose it on everyone or ship the default.
- [ ] **`config/CSC/config.toml` — `UUID = "332eca2e-107e-458d-a51c-87bef74ef406"`.** A per-install
      identifier for the schematic checker, and `[online] UpdateInfo = true` means it talks to the
      CSC service. Blank the UUID before packaging so each install generates its own.
- [ ] Sweep the rest of `config/` for absolute paths, monitor geometry, personal ids and Patreon or
      account tokens. `xaeropatreon.txt` was already deleted as an orphan; check for siblings —
      three have turned up already (monitor, cyclopscore analytics, CSC UUID).
- [ ] **Scrub Ponderer's temporary AI authoring state.** Before packaging — and immediately after
      any supervised AI-assisted Ponder scene session — verify the client
      `config/ponderer-client.toml` is back at `configSource = "custom"`, `apiKey = ""`,
      `proxy = ""`, and `trustAllSsl = false`. Do not ship or commit copied Codex/Claude auth,
      tokens, proxy credentials, generated credential caches, or a `codex`/`claude_code` source.
      The finished Ponder scenes may ship; the authoring credentials and transport state may not.

## 2. The world is generated here; a public release starts fresh

Worldgen settings only touch chunks generated after the change, so every one of these is inert in
the current save and fully live in a new one.

- [ ] **BetterNether's four ruby ore features are off** (2026-08-03). In a fresh world that is the
      whole point: ruby drops from ~73 a chunk to ~4.5. Confirm it reads correctly on a new seed.
- [ ] **`betterdungeons`: Small Nether Dungeons are on, spawner skull and rod drops off.** Content
      without the wither-skull-to-nether-star channel. Only a fresh world will show it.
- [ ] The blockswap rule `betternether:nether_ruby_ore -> minecraft:netherrack` exists to clean the
      *existing* world. On a fresh release it does nothing and could be dropped from the shipped
      config — decide which.
- [ ] Apotheosis worldgen is whitelisted to the overworld. Rogue spawners and boss dungeons do not
      generate in the Nether or the End; the bosses themselves do. Deliberate or not, decide it
      before release rather than after.

## 3. Things that only bite other people

- [ ] **Carry On's blacklist** gained `ae2:*`, `sophisticatedstorage:*`, `sophisticatedbackpacks:*`
      and `numismatics:*` on 2026-08-03. On a private world a carried drive bay is your own problem;
      on a public server it is a dupe and grief surface. Re-check the list against whatever mods are
      added between now and release.
- [ ] **Spawner silk needs Silk Touch III** and **the Wandering Trader is blocked through InControl**
      — both worth a line in the pack description, because both differ from what players expect.
- [ ] **Confirm CSC's NBT whitelist does what it says.** It was rebuilt on 2026-08-03 from 14 stock
      entries to 73 — every installed Create, AE2 and Immersive Engineering mod. Copy an addon block
      with settings via schematic and check the settings survive; then copy something outside the
      list and check its NBT is still stripped. Both halves matter: the first is the feature, the
      second is the protection.
- [ ] Decide whether `itemsHidingJeiRei` actually works behind the EMI/TooManyRecipeViewers shim.
      If it does nothing, take it back out rather than shipping a setting that lies.

## 4. Hygiene

- [ ] **Orphaned configs.** 21 deleted on 2026-08-03; `tools/audit_orphan_configs_2026_08_03.py`
      reports what is left, including a "mentioned by name only" tier that needs a human. Re-run
      before packaging — every mod removal leaves more.
- [ ] **Path policy is closed — verify it, do not quote it.** `tools/` no longer carries this
      machine's hardcoded paths; everything resolves through `tools/paths.py`. Re-run the current path
      audit before shipping instead of trusting any number written on this page.
- [ ] Run the current in-game checklist (`ИГРОВОЙ_ЧЕКЛИСТ.md`) on a **fresh world**, not just
      the live save. Several items behave differently on first generation.

- [ ] **`integrated_villages-forge-1_20.toml` → `"Activate Create Contraptions"`** (found 2026-08-04).
      Stock `true`, and the mod's own comment says turning it off "could prevent some lag". It fires
      **during world generation**, so the cost lands on whoever generates the world, on their
      machine. Same class as `chunk_builder_threads = 16`: fine here, a question for a player on four
      cores. Decide before shipping; it changes nothing for chunks that already exist.

- [ ] **`config/inventoryprofilesnext/` carries per-server folders** (found 2026-08-04): `New World`,
      `Engineers_and_Wizards`, `test`, a link-local IPv6 address with a port, and
      `resources-establishing.gl.joinmc.link` — another server's hostname. Four of the five are empty
      and `enable_lock_slots_per_server` is `false`, so the mod uses none of them. Strip them before
      packaging; they regenerate. Same class as the other personal ids on this list.

---

## Done 2026-08-04 — three release items closed, one caveat

- [x] **`fml.toml` early window** — was **1920×1080** on the server and **1536×960** on the client,
      i.e. two different people's screens. Both set to Forge's own default **854×480**
      (`FMLConfig$ConfigValue`, `sipush 854 / sipush 480`). ⚠️ **Re-check at packaging time:** this
      file is written by Forge itself and is genuinely machine-local, which is exactly why master
      does **not** own it — forcing one machine's geometry onto the other is the bug, not the fix.
- [x] **`embeddium-options.json` `chunk_builder_threads` 16 → 0 (auto).** 16 was pinned to a
      24-thread machine and would have landed on players with four cores. `0` means auto.
      `use_block_face_culling` was restored to the jar default `true` in the same edit — free
      culling that had been turned off. `use_quad_normals_for_shading` left at `true` on the user's
      call: shader taste, and problems there will be reported rather than guessed at.
- [x] **`config/inventoryprofilesnext/` per-server folders stripped** — and there were more on the
      client than the server showed: `127.0.0.1`, `localhost`, four IPv6 addresses including a
      **public routable one** (`2a05:…`), and two `*.gl.joinmc.link` hostnames. All were **empty**,
      and `enable_lock_slots_per_server` is `false`, so the mod used none of them. `New World/` and
      `integrationHints/` kept — they hold real files.


---

## Added 2026-08-04 — ship with shaders off, and pointed at a pack that exists

- [ ] **`config/oculus.properties`: `enableShaders=false` and `shaderPack=` naming an installed
      folder.** It read `ComplementaryUnbound_r5.7.1 + EuphoriaPatches_1.8.6` — a name that is **not
      on disk any more**; the 5.7.1 folders were renamed `Outdated …` when 5.8.1 went in. So the
      selection pointed at nothing and turning shaders on would have found no pack. Set to
      **`ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3`**, shaders still **off**.
      ⚠️ **This file is rewritten by the client on exit.** The user turns shaders on to play, so by
      packaging time it will say `enableShaders=true` again — set it back to `false` at packaging,
      and check the pack name still matches a folder in `shaderpacks/`.
- [ ] **Heap numbers are machine-local — do not ship the laptop's.** `user_jvm_args.txt` and
      `variables.txt` are now **`-Xmx6G -Xms6G`**, which is right for a 15.6 GB laptop that runs the
      client and the server at once, and wrong for a dedicated box. The desktop has 32 GB and can
      keep 10G. Decide what the released server pack should carry rather than shipping whichever
      machine last touched it.
