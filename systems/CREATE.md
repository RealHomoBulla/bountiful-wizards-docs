# Create

> **Главный мод:** Create 6.0.8 + аддоны · **Готовность: 63 %**
> **Больше всего мешает:** фаза Pneumatic Regulator не сделана, а пакет машинных рецептов (Б6) ждёт твоего «оставляем / нет». Цепь Create принята 13.10 (13.2), корзина Farmer's Delight (13.4) и звуки труб (4.2) — тоже.
> **Ждёт твоего слова:** 1 вопрос — оставляем ли пакет машинных рецептов (Б6, плюс «включить VX3M после твоей проверки») · **Проверить в игре:** 1 пункт — 33.11 (пружины, лампа IE, купорос из пакета) *(Б6 — решение, не тест)*

Ставка миксера в модели цен — класс 129–256 (твой ответ «а» 27.09): миксер и дробилка в модели ~20 % быстрее, цены у торговцев пересчитаны, клиент синхронизирован. Андезитовый сплав и две пластины ждут разбора защиты от печатных станков.

## Куда идём — каким это будет на 100 %

Create — кинетический хребет пака. Игрок ставит источник вращения, тянет валы и ленты к машинам Create, собирает первые
фабрики на рецептах пака, потом переводит вращение в FE через Create Crafts & Additions и Create New Age и выходит в AE2 через
генератор C&A. Pneumatic Express даёт трубную логистику между PetrolParts и AE2. Каждая машина крутится так же после
перезахода и выгрузки чанка, а каждый аддон, который остался в паке, имеет роль, рецепт пака и конфиг под мастером.
Вращающиеся узлы требуют обслуживания: «износ до сервиса» и смазка (креозот), которую игрок автоматизирует, а не носит
руками каждые три минуты (`OPEN.md` §1.185 п.12). Поезда Steam 'n' Rails — своя ветка квестов в главе 3 (блиц #14 Q9 = a),
а спрос на них — строки у Хоббса (`OPEN.md` §1.971 D1 = b).

**Чем ограничиваем:** своими рецептами KubeJS (`kubejs/server_scripts/create_my/`, 25 скриптов на 08.10: сначала `01_Removals.js`, потом
замены), конфигами аддонов в мастере и пином версии Create `[6.0.8,6.1.0)`.

**Вырезано и почему:**
- бесплатный мотор `create_henry:kinetic_motor` (1 536 SU из ничего) — вне выживания; топливный `furnace_engine` оставлен (§1.382);
- ванадиевая семья Vintage — снята целиком (E35-b, та же причина: одна цепочка вместо параллельной дешёвой);
- бесплатная скорость лент Wheels-upon-Chairs (`easy_belt`, ±256 без SU) — выключена конфигом во всех трёх копиях; сам мод
  **оставлен** — решение §1.388 = a (блиц #16, 24.09), остальные опции живут на дефолтах мода;
- дешёвые параллельные маршруты рецептов — сняты в `01_Removals.js`.

⚠️ **Скоуп по остальным аддонам не решён поштучно.** Замер 24.09 нашёл 111 модов семейства Create; роль и допуск описаны
примерно у двадцати, остальные просто установлены. Это не вопрос к тебе, а наш недоделанный разбор (строка «Опись аддонов» ниже).

## Где мы сейчас

| подсистема | вес | доля | состояние | чем доказано |
|---|---:|---:|---|---|
| Основа Create и слой рецептов KubeJS | 3 | 0,5 | 🔧 цепь принята в игре 13.10 (13.2); хвосты рецептов открыты | `create-1.20.1-6.0.8.jar` на обеих сторонах, один SHA; 25 скриптов в `create_my/` (08.10); строка 13.2 `TECH-GAME-01` отмечена PASS 13.10 (ИГРОВОЙ_ЧЕКЛИСТ.md, «Куда ушло») |
| Механика машин | 2 | 1,0 | ✅ доказана из байткода | дробилки, жернов, вентилятор, турель CDG, Henry, Sifter разобраны по джарникам; джарники с тех пор не менялись (SHA сверены 24.09) — `knowledge/create/CURRENT_STATE.md` |
| Конфиги аддонов под мастером | 2 | 0,5 | 🔧 частично | в мастере: C&A, New Age, Create Solar, Vintage, Wheels-upon-Chairs, Guardian Beam; **только в мире** (нет в мастере): Henry; серверная половина CDG лежит в `server_mirror/defaultconfigs/createdieselgenerators-server.toml` (снимок живого сервера, 11.10), в мастере её нет; сам Create (server) и мост Applied Create уже в мастере (`defaultconfigs/`, 08.10) |
| Pneumatic Express | 2 | 0,5 | 🔧 NPE починен и выложен 24.09, звук трубы починен 24.09 временным ресурспаком; в игре принят: корзина 13.4 и звуки 4.2 — PASS; фаза регулятора не сделана · 🆕 19.10 пневморегулятор слит и выложен на клиент (BRQ `72b83b16e`): трубы везут только через регулятор с FE, 5 FE за предмет — чеклист Б232 | джарник A `6a93301d` на сервере, клиенте и `reference/cloud/mods` (`TODO.md` `FD-BASKET-PNEUMATIC-EXPRESS-NPE-20260830`, ✅); `grep -c 'instanceof FluidPipeBlockEntity'` в исходнике → 1; чеклист 13.4 `FD-BASKET-NPE-01` PASS 13.10, 4.2 `PNEU-SOUNDS-01` — PASS 12.10 («НО ВСЕ РАБОТАЕТ»; звук и тултип ушли в TODO); звук — `TODO.md` `PNEU-TUBE-SOUND-MISSING-20260924` ✅ (пак `zzz_pneu_sounds_1_20_1`); фаза регулятора — `PNEU-REGULATOR-PHASE-AND-LOOK-20260924` 🟡 |
| Опись аддонов и их роли | 1 | 1,0 | ✅ 17.10 полная опись: 102 джарника семейства Create — роль, сторона, есть ли конфиг в мастере и отличается ли он от дефолта; инвентаризации игра не нужна | `agents/mods/2026_10_19_FREE_CREATE_ADDON_ROLES.md` (AGY, `752a2b210`) · **17.10 MCHK машинно:** джарники `<клиент>/mods` с modId `create*` или зависимостью от `create` против описи → `client create-family jars: 102 missing from doc: 2` — оба наши (`bw_lode_noise` 1.5.5, `factory_siege` 0.0.19), в описи те же моды прошлых версий: 102/102 |
| Обслуживание машин: смазка и износ | 1 | 0 | ❌ принято, не начато | `OPEN.md` §1.185 п.12; `TODO.md` `MACHINE-SERVICE-LUBRICANT` |
| 🆕 Пакет машинных рецептов по блицу #22 (пружины, купорос, сера, VX, лазер, лампа) | 2 | 0,85 | 🔧 выложен на клиент, в игре не отмечен; сервер отстаёт (дерево сервера заморожено с 28.09, `TODO.md` `LINUX-SERVER-TREE-STALE-20261008`) | 26.09 09:0x `ca44cb50e`, **43 включённых блока** (08.10) в `99_commented_reference_toggle_recipe_pack.js` (`IE1`/`IE2` — пружины в механизмы IE, `AE1` — купорос в энергоячейку AE2, `SU6` — 9 скории → 1 сера IE, `APOK1` — самоцвет Momentum с лазером, `B1` — лампа через вакуум, `VX1`–`VX4`); md5 master = client `98bf6a0c…`, сервер `5d02175a…` — старая копия (08.10); 17.10 MCHK: sha256 мастер = клиент `4d98d3a3f27a`, лог клиента `Loaded 128/128 KubeJS server scripts … 0 errors`; в игре — пункт 33.11; копи-рут: 100/100 скриптов, 0 ошибок; 🔴 найденный и починенный хардлок B1 (лампа шла только через деплоеры, а деплоеру нужны 4 лампы — ручной рецепт вернули, машинный не дешевле прежнего). Хвост: `VX3M` (миксерный рецепт ламп IE) выключен по твоему «после теста»; отчёт `agents/recipes/2026_09_26_OPUS_RECIPE_TOGGLES_BATCH.md` |

**Готовность раздела: 63 %** — Σ(вес × доля) = 8,20 (1,5 + 2,0 + 1,0 + 1,0 + 1,0 + 0 + 1,7); Σ вес = 13; `round-half-up(100 × 8,20 / 13) = round-half-up(63,08) = 63 %`. 17.10 MCHK: «Опись аддонов» 0,85 → 1,0 — опись сверена с клиентом, 102/102 (было 62 % = 8,05 / 13). Шкала долей: 1,0 / 0,85 / 0,5 / 0.
⚠️ Строка «основа Create» оставлена на 0,5: цепь принята (13.2), но у слоя рецептов остались хвосты — `VX3M` выключен по твоему слову, `CL5` выключен «рецепты пока не финальные». Переоценка — кандидат Продюсеру отдельным замером.

## Что нужно от тебя

Один открытый вопрос — **чек-лист Б6**: пакет машинных рецептов включён по умолчанию (закон 13d, «выключает он сам»),
и 26.09 в него добавлены ещё шесть блоков по твоим ответам. Отметь «оставляем / нет», и отдельно скажи «включить
VX3M» после проверки, что вакуумная камера действительно делает лампу — миксерный рецепт уйдёт.

Три рецепта вернулись 26.09 и ждут только твоего взгляда в EMI: кинетический механизм Create Henry, пневматический
механизм Create Fishery Industry и механизм перегрева Protection Pixel. Они исчезали не сами: чистка `create:cogwheel`
и `create:precision_mechanism` сносила сборочные линии, где они были лишь побочным результатом. Правка — исключение
по id в `01_Removals.js`; на копии сервера счётчик удалений упал 1 185 → 1 182, все три рецепта живые, крафт шестерни
и точного механизма по-прежнему убран. На мастере и клиенте; на сервер не выложено (`TODO.md` `UNINTENDED-SEQ-ASSEMBLY-REMOVALS-20260926` 🟡 «ждёт выкладки + пересборка кэшей»), в EMI не проверено.

Уже отвечено, повторно не спрашиваем: где живёт Create (§1.387 = a — эта страница), Wheels-upon-Chairs (§1.388 = a — мод
остаётся, опции на дефолтах), фаза Pneumatic Regulator (§1.229 = a — после PetrolParts, до AE2), мотор Henry (§1.382), четыре квеста-счётчика Create через CRH (§1.566 = a, блиц #17 Q21 — места и числа в `КВЕСТЫ_TODO.md`). Политика энергии, реактора и нагрева (§1.390) касается New Age и Create Solar, но
живёт на [ЭНЕРГИЯ.md](ЭНЕРГИЯ.md).

| проверить в игре | где |
|---|---|
| Пакет машинных рецептов (с 25.09 23:30 включён по умолчанию, AGENTS 13d): отметить «оставляем / нет» — для Create это A1 точный механизм с пружиной, A6 катапульта на пружинах, B1 электронная лампа через вакуум, D5 кинетический механизм | `ИГРОВОЙ_ЧЕКЛИСТ.md` Б6 `TOGGLE-RECIPE-PACK-OWNER-01`; строки — `agents/recipes/2026_09_24_TOGGLE_RECIPE_PACK.md` |

## 🔴 Что сломано

| проблема | доказательство | последствие |
|---|---|---|
Поломок Create, подтверждённых в игре, нет. Корзину FD NPE больше не ломает (приёмка 13.4, PASS 13.10). | `TODO.md` `FD-BASKET-PNEUMATIC-EXPRESS-NPE-20260830` ✅ | — |

## В работе

Сейчас ни одна задача ветки не исполняется. Очередь — в `TODO.md`:
`PNEU-REGULATOR-PHASE-AND-LOOK-20260924`, `UNINTENDED-SEQ-ASSEMBLY-REMOVALS-20260926` (ждёт выкладки на сервер), `CRH-CREATE-PIN-AND-FTBQ-WIRING-20260912` (§1.566 = a,
счётчики приняты), `DIRECT-CHUTE-VERDICT`, `MACHINE-SERVICE-LUBRICANT` (сначала дизайн).

## На чём это стоит

- ⚖️ **Своя папка и страница Create** — `OPEN.md` §1.387 = a (блиц #14 Q23, 24.09). Технологии сужены до промышленной техники: [ТЕХНОЛОГИИ.md](ТЕХНОЛОГИИ.md).
- ⚖️ **Вентилятор и железные блоки (20.10):** мод-страж `bw_pack_guards` 0.1.1 на клиенте — Encased Fan не превращает блок железа (промышленный / выветренный) в другой кожух, а клик с Shift просто ставит блок рядом с вентилятором, вентилятор цел (чеклист Б245 `PACK-GUARDS-BREW-FAN-01`; версия 0.1.0 только отказывала, 0.1.1 ещё и ставит блок).
- ⚖️ **Pneumatic Regulator** — после PetrolParts, до AE2; вид — перекрашенный насос или подключаемый электромотор (`OPEN.md` §1.229 = a).
- ⚖️ **Create Henry** — `kinetic_motor` вне выживания, `furnace_engine` остаётся (`OPEN.md` §1.382; `kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js`, одинаков на мастере, клиенте и сервере 24.09).
- ⚖️ **Ванадий Vintage снят** — E35-b (`OPEN.md` §1.915 №6; `create_my/96_Retire_Vanadium_Radiance.js`); строка чеклиста `VANADIUM-RETIRE-EMI-01` снята 02.10 машинно (в архиве чеклиста: 13 форм Vintage скрыты).
- ⚖️ **Пакет машинных рецептов** — все 10 из топа Henry/Vintage/Metallurgy одним пакетом-выключателем (`OPEN.md` §1.955 = a, §1.952 = c + выключатель, блиц #14); собран выключенным, `TODO.md` `TOGGLE-RECIPE-PACK-20260924`, `kubejs/server_scripts/99_commented_reference_toggle_recipe_pack.js`; с 25.09 включён по умолчанию (`TOGGLE_RECIPE_PACK_ON = true`, `ee956b9d5`, блиц #22 → AGENTS 13d: выключает он сам, если не понравится), на выложенной сборке машинно проверено 26.09 — `agents/tooling/2026_09_26_OPUS_RETEST_PLAYGROUND_V7.md` §2.4.
- ⚖️ **26.09, шесть блоков по твоим ответам блица #22** (`ca44cb50e`, выложено): §4 «Рек» — пружины в железном и стальном
  механизме IE (и на верстаке, и в чертеже, чтобы не убрать ручной путь), §8 «d+ лазер» — самоцвет Momentum сборочной
  линией с лазером Vintage, §13 «B» — лампа через вакуум с поднятыми входами, §14 «А» — миксерный рецепт ламп IE снимается
  после твоей проверки (тумблер `VX3M`, выключен), §15 «b» — купорос во все три энергоячейки AE2, §16 — сера IE каноническая
  с гарантированным рецептом 9 скории → 1 сера. Отчёт `agents/recipes/2026_09_26_OPUS_RECIPE_TOGGLES_BATCH.md`.
- ⚖️ **26.09: три сборочные линии вернулись** — `event.remove({output: X})` в KubeJS бьёт по **любому** результату
  `results`, а не только основному, поэтому чистка шестерни и точного механизма сносила три рецепта как побочный
  эффект (`create_henry:sequenced_assembly/kinetic_mechanism`, `createfisheryindustry:sequenced_assembly/pneumatic_mechanism`,
  `protection_pixel:heatoverlockmechanismloot`). Правка — `not:`-исключение по id в `01_Removals.js:382-383, 484`
  (`a1b915d3c`), на мастере и клиенте, сервер ждёт выкладки; отчёт `agents/recipes/2026_09_26_SONNET_UNINTENDED_SEQ_REMOVALS.md`.
- ⚖️ **Смазка как расходник обслуживания** — *«Однозначно в todo!»*, но *«не сильно душно»* и с автоматизацией (`OPEN.md` §1.185 п.12). Охлаждение реактора — на [ЭНЕРГИЯ.md](ЭНЕРГИЯ.md).
- 🔴 **Пин Create:** `createmetallurgy` (и отдельно `createsolar`) требуют `create` **`[6.0.8,6.1.0)`**, обязательно, обе стороны. Create 6.1.0+ — это падение загрузки на обеих сторонах; не обновлять без отдельного пересмотра.
- Доказательства по модам, механике, швам и рискам — [knowledge/create/INDEX.md](../agents/knowledge/create/INDEX.md); запись переезда — `agents/docs_audit/2026_09_24_CREATE_HOME.md`.
- Соседи: энергия и её числа — [ЭНЕРГИЯ.md](ЭНЕРГИЯ.md); металлургическая цепочка Create Metallurgy/Vintage — [МЕТАЛЛУРГИЯ.md](МЕТАЛЛУРГИЯ.md); RNS — [МАТЕРЛОД.md](МАТЕРЛОД.md); схематики — [СХЕМАТИКИ.md](СХЕМАТИКИ.md); AE2 и IE — [ТЕХНОЛОГИИ.md](ТЕХНОЛОГИИ.md).

- **08.10 (§1.403 / §1.414):** `create_mobile_packages-server.toml` и `create-server.toml` приняты в мастер из клиента; в `create-server.toml` дальность ящика с инструментами 500 (у Create 10) — твоя настройка. Файлы мира сервера пересчитать при подключении SSD — строка `CREATE-SERVER-WORLD-CONFIG-RECOUNT-ON-SSD-20261008` в `TODO.md`.

## Чем проверить

```bash
# 1. Пин и версия: норма — create [6.0.8,6.1.0) mandatory BOTH; установлен 6.0.8.
S=$(python3 bountiful_wizards_REMAKE/tools/paths.py --server); C=$(python3 bountiful_wizards_REMAKE/tools/paths.py --client)
unzip -p "$S"/mods/createmetallurgy-*.jar META-INF/mods.toml | grep -A4 'modId *= *"create"'
ls "$C"/mods/create-1.20.1-*.jar "$S"/mods/create-1.20.1-*.jar

# 2. Слой рецептов: норма сегодня — 27 файлов.
ls bountiful_wizards_REMAKE/kubejs/server_scripts/create_my/*.js | wc -l

# 3. Потолок вращения мира: норма — maxRotationSpeed = 256, fanProcessingTime = 150.
grep -n 'maxRotationSpeed\|fanProcessingTime' "$S"/world/serverconfig/create-server.toml

# 4. Конфиги только в мире (строка «Конфиги аддонов»): норма сегодня (09.10) — world-копии на SSD не пересчитаны с дерева 28.09 (их здесь нет), но `create-server` и `appliedcreate-server` уже есть в мастере (`defaultconfigs/`, 08.10); без копии в мастере остаются Henry и серверная половина CDG (find печатает только скрипт KubeJS). Строки: `CREATE-ADDON-WORLD-ONLY-CONFIGS-20261008`, `CREATE-SERVER-WORLD-CONFIG-RECOUNT-ON-SSD-20261008`.
ls "$S"/world/serverconfig/ | grep -E 'create-server|create_henry|createdieselgenerators-server|appliedcreate'
find bountiful_wizards_REMAKE server_mirror -iname '*henry*' -not -path '*/cloud/*'

# 5. Wheels-upon-Chairs: норма — easy_belt = false во всех трёх копиях.
grep -n '^\s*easy_belt' bountiful_wizards_REMAKE/config/createwheelsuponchairs-common.toml "$C"/config/createwheelsuponchairs-common.toml "$S"/config/createwheelsuponchairs-common.toml

# 6. NPE Pneu: норма сегодня — 1 (починка в исходнике, выложена 24.09); 0 = регрессия.
grep -c 'instanceof FluidPipeBlockEntity' custom/mods/create_pneumatic_express/src/main/java/com/battlecraft/pneumaticexpress/CreatePneumaticExpress.java

# 7. Мотор Henry снят: норма — один и тот же хэш на мастере, клиенте и сервере.
sha256sum bountiful_wizards_REMAKE/kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js "$C"/kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js "$S"/kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js

# 8. Опись семейства Create: норма 24.09 — UNIQUE_IDS 111. Скрипт целиком — knowledge/create/MOD_SURFACE.md §Recheck.

# 9. Игровая строка: норма сегодня — строка в таблице «Куда ушло» (PASS 13.10), без `- [ ]`.
grep -n 'TECH-GAME-01' claudework/ИГРОВОЙ_ЧЕКЛИСТ.md

# 10. Пакет рецептов: норма — 43 включённых блока (08.10), master = client; сервер отстаёт до LINUX-SERVER-TREE-STALE-20261008.
grep -cE '^\s+[A-Z0-9]+: true' bountiful_wizards_REMAKE/kubejs/server_scripts/99_commented_reference_toggle_recipe_pack.js
md5sum bountiful_wizards_REMAKE/kubejs/server_scripts/99_commented_reference_toggle_recipe_pack.js \
  "$C"/kubejs/server_scripts/99_commented_reference_toggle_recipe_pack.js \
  "$S"/kubejs/server_scripts/99_commented_reference_toggle_recipe_pack.js
grep -n 'VX3M' bountiful_wizards_REMAKE/kubejs/server_scripts/99_commented_reference_toggle_recipe_pack.js   # норма — false, ждёт твоей проверки

# 11. Три возвращённые сборочные линии: в файле есть исключения по id; master = client, сервер ждёт выкладки (UNINTENDED-SEQ-ASSEMBLY-REMOVALS-20260926).
grep -n 'sequenced_assembly/kinetic_mechanism\|sequenced_assembly/pneumatic_mechanism\|heatoverlockmechanismloot' \
  bountiful_wizards_REMAKE/kubejs/server_scripts/create_my/01_Removals.js
md5sum bountiful_wizards_REMAKE/kubejs/server_scripts/create_my/01_Removals.js \
  "$C"/kubejs/server_scripts/create_my/01_Removals.js \
  "$S"/kubejs/server_scripts/create_my/01_Removals.js
```
