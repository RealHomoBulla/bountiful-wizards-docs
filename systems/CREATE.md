# Create

> **Главный мод:** Create 6.0.8 + аддоны · **Готовность: 45 %**
> **Больше всего мешает:** Pneumatic Express уничтожает корзины Farmer's Delight (NPE жив на обеих сторонах), а цепь Create ни разу не прошла игровую приёмку
> **Ждёт твоего слова:** 0 вопросов (§1.388 решён 24.09 = a, мод остаётся) · **Проверить в игре:** 2 пункта (6.1 и 6.6 чеклиста)

## Куда идём — каким это будет на 100 %

Create — кинетический хребет пака. Игрок ставит источник вращения, тянет валы и ленты к машинам Create, собирает первые
фабрики на рецептах пака, потом переводит вращение в FE через Create Crafts & Additions и Create New Age и выходит в AE2 через
генератор C&A. Pneumatic Express даёт трубную логистику между PetrolParts и AE2. Каждая машина крутится так же после
перезахода и выгрузки чанка, а каждый аддон, который остался в паке, имеет роль, рецепт пака и конфиг под мастером.
Вращающиеся узлы требуют обслуживания: «износ до сервиса» и смазка (креозот), которую игрок автоматизирует, а не носит
руками каждые три минуты (`OPEN.md` §1.185 п.12). Поезда Steam 'n' Rails — своя ветка квестов в главе 3 (блиц #14 Q9 = a),
а спрос на них — строки у Хоббса (`OPEN.md` §1.971 D1 = b).

**Чем ограничиваем:** своими рецептами KubeJS (`kubejs/server_scripts/create_my/`, 23 скрипта: сначала `01_Removals.js`, потом
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
| Основа Create и слой рецептов KubeJS | 3 | 0,5 | 🔧 статически есть, игрой не принято | `create-1.20.1-6.0.8.jar` на обеих сторонах, один SHA; 23 скрипта в `create_my/`; строка 6.1 `TECH-GAME-01` не отмечена |
| Механика машин | 2 | 1,0 | ✅ доказана из байткода | дробилки, жернов, вентилятор, турель CDG, Henry, Sifter разобраны по джарникам; джарники с тех пор не менялись (SHA сверены 24.09) — `knowledge/create/CURRENT_STATE.md` |
| Конфиги аддонов под мастером | 2 | 0,5 | 🔧 частично | в мастере: C&A, New Age, Create Solar, Vintage, Wheels-upon-Chairs, Guardian Beam; **только в мире**: сам Create (server), Henry, серверная половина CDG, мост Applied Create — пропадут при сбросе мира |
| Pneumatic Express | 2 | 0 | ❌ не принят | NPE корзины FD при каждой постановке (см. «Что сломано») |
| Опись аддонов и их роли | 1 | 0,5 | 🔧 опись есть, разбора нет | 111 modId замерены по живым джарникам 24.09; роль/рецепт/конфиг разобраны примерно у двадцати |
| Обслуживание машин: смазка и износ | 1 | 0 | ❌ принято, не начато | `OPEN.md` §1.185 п.12; `TODO.md` `MACHINE-SERVICE-LUBRICANT` |

**Готовность раздела: 45 %** — Σ(вес × доля) = 5,0 (1,5 + 2,0 + 1,0 + 0 + 0,5 + 0); Σ вес = 11; 100 × 5,0 / 11 = **45 %**.
Вес по объёму работы: основа и рецепты — самое большое (3), механика, конфиги и Pneu — по 2, опись и смазка — по 1. Шкала долей та же, что у
технологий (1,0 / 0,85 / 0,5 / 0). Две строки — «основа Create» (3 × 0,5) и «Pneumatic Express» (2 × 0) — до 24.09 считались на
странице технологий; там их больше нет, так что одно и то же не считается дважды.

## Что нужно от тебя

Сейчас — ничего. Последний вопрос раздела, Wheels-upon-Chairs 1.0.4, закрыт 24.09 (`OPEN.md` §1.388 = a:
мод остаётся, `easy_belt` уже выключен в трёх копиях).

Уже отвечено, повторно не спрашиваем: где живёт Create (§1.387 = a — эта страница), Wheels-upon-Chairs (§1.388 = a — мод
остаётся, опции на дефолтах), фаза Pneumatic Regulator (§1.229 = a — после PetrolParts, до AE2), мотор Henry (§1.382), четыре квеста-счётчика Create через CRH (§1.566 = a, блиц #17 Q21 — места и числа в `КВЕСТЫ_TODO.md`). Политика энергии, реактора и нагрева (§1.390) касается New Age и Create Solar, но
живёт на [ЭНЕРГИЯ.md](ЭНЕРГИЯ.md).

| проверить в игре | где |
|---|---|
| Цепь Create крутится и не ломается после выгрузки чанка | `ИГРОВОЙ_ЧЕКЛИСТ.md` §6, 6.1 `TECH-GAME-01` |
| Пакет машинных рецептов (с 25.09 23:30 включён по умолчанию, AGENTS 13d): отметить «оставляем / нет» — для Create это A1 точный механизм с пружиной, A6 катапульта на пружинах, B1 электронная лампа через вакуум, D5 кинетический механизм | `ИГРОВОЙ_ЧЕКЛИСТ.md` 6.6 `TOGGLE-RECIPE-PACK-OWNER-01`; строки — `agents/recipes/2026_09_24_TOGGLE_RECIPE_PACK.md` |

## 🔴 Что сломано

| проблема | доказательство | последствие |
|---|---|---|
| Pneumatic Express ломает корзину Farmer's Delight при **любой** постановке, не только при загрузке чанка | `CreatePneumaticExpress.java:68` вызывает `getBlockState()` на любом block entity до проверки «это труба Create»; у `create_central_kitchen` этот вызов сам бросает NPE. Фильтра по классу в исходнике нет (`grep -c 'instanceof FluidPipeBlockEntity'` → 0, 24.09). Оба мода стоят на обеих сторонах | содержимое корзины уничтожается, сломанную корзину не убрать командами; Pneu нельзя считать принятой логистикой |

## В работе

Сейчас ни одна задача ветки не исполняется. Очередь — в `TODO.md`: `FD-BASKET-PNEUMATIC-EXPRESS-NPE-20260830` (патч на бумаге),
`PNEU-REGULATOR-PHASE-AND-LOOK-20260924`, `PNEU-TUBE-SOUND-MISSING-20260924`, `CRH-CREATE-PIN-AND-FTBQ-WIRING-20260912` (§1.566 = a,
счётчики приняты), `DIRECT-CHUTE-VERDICT`, `MACHINE-SERVICE-LUBRICANT` (сначала дизайн).

## На чём это стоит

- ⚖️ **Своя папка и страница Create** — `OPEN.md` §1.387 = a (блиц #14 Q23, 24.09). Технологии сужены до промышленной техники: [ТЕХНОЛОГИИ.md](ТЕХНОЛОГИИ.md).
- ⚖️ **Pneumatic Regulator** — после PetrolParts, до AE2; вид — перекрашенный насос или подключаемый электромотор (`OPEN.md` §1.229 = a).
- ⚖️ **Create Henry** — `kinetic_motor` вне выживания, `furnace_engine` остаётся (`OPEN.md` §1.382; `kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js`, одинаков на мастере, клиенте и сервере 24.09).
- ⚖️ **Ванадий Vintage снят** — E35-b (`OPEN.md` §1.915 №6; `create_my/96_Retire_Vanadium_Radiance.js`); остаток — строка 6.4 чеклиста на [ТЕХНОЛОГИИ.md](ТЕХНОЛОГИИ.md).
- ⚖️ **Пакет машинных рецептов** — все 10 из топа Henry/Vintage/Metallurgy одним пакетом-выключателем (`OPEN.md` §1.955 = a, §1.952 = c + выключатель, блиц #14); собран выключенным, `TODO.md` `TOGGLE-RECIPE-PACK-20260924`, `kubejs/server_scripts/99_commented_reference_toggle_recipe_pack.js`; с 25.09 включён по умолчанию (`TOGGLE_RECIPE_PACK_ON = true`, `ee956b9d5`, блиц #22 → AGENTS 13d: выключает он сам, если не понравится), на выложенной сборке машинно проверено 26.09 — `agents/tooling/2026_09_26_OPUS_RETEST_PLAYGROUND_V7.md` §2.4.
- ⚖️ **Смазка как расходник обслуживания** — *«Однозначно в todo!»*, но *«не сильно душно»* и с автоматизацией (`OPEN.md` §1.185 п.12). Охлаждение реактора — на [ЭНЕРГИЯ.md](ЭНЕРГИЯ.md).
- 🔴 **Пин Create:** `createmetallurgy` (и отдельно `createsolar`) требуют `create` **`[6.0.8,6.1.0)`**, обязательно, обе стороны. Create 6.1.0+ — это падение загрузки на обеих сторонах; не обновлять без отдельного пересмотра.
- Доказательства по модам, механике, швам и рискам — [knowledge/create/INDEX.md](../agents/knowledge/create/INDEX.md); запись переезда — `agents/docs_audit/2026_09_24_CREATE_HOME.md`.
- Соседи: энергия и её числа — [ЭНЕРГИЯ.md](ЭНЕРГИЯ.md); металлургическая цепочка Create Metallurgy/Vintage — [МЕТАЛЛУРГИЯ.md](МЕТАЛЛУРГИЯ.md); RNS — [МАТЕРЛОД.md](МАТЕРЛОД.md); схематики — [СХЕМАТИКИ.md](СХЕМАТИКИ.md); AE2 и IE — [ТЕХНОЛОГИИ.md](ТЕХНОЛОГИИ.md).

## Чем проверить

```bash
# 1. Пин и версия: норма — create [6.0.8,6.1.0) mandatory BOTH; установлен 6.0.8.
S=$(python3 bountiful_wizards_REMAKE/tools/paths.py --server); C=$(python3 bountiful_wizards_REMAKE/tools/paths.py --client)
unzip -p "$S"/mods/createmetallurgy-*.jar META-INF/mods.toml | grep -A4 'modId *= *"create"'
ls "$C"/mods/create-1.20.1-*.jar "$S"/mods/create-1.20.1-*.jar

# 2. Слой рецептов: норма сегодня — 23 файла.
ls bountiful_wizards_REMAKE/kubejs/server_scripts/create_my/*.js | wc -l

# 3. Потолок вращения мира: норма — maxRotationSpeed = 256, fanProcessingTime = 150.
grep -n 'maxRotationSpeed\|fanProcessingTime' "$S"/world/serverconfig/create-server.toml

# 4. Конфиги только в мире (строка «Конфиги аддонов»): норма сегодня — четыре файла, и ни одной копии Henry в мастере (find печатает только скрипт KubeJS).
ls "$S"/world/serverconfig/ | grep -E 'create-server|create_henry|createdieselgenerators-server|appliedcreate'
find bountiful_wizards_REMAKE server_mirror -iname '*henry*' -not -path '*/cloud/*'

# 5. Wheels-upon-Chairs: норма — easy_belt = false во всех трёх копиях.
grep -n '^\s*easy_belt' bountiful_wizards_REMAKE/config/createwheelsuponchairs-common.toml "$C"/config/createwheelsuponchairs-common.toml "$S"/config/createwheelsuponchairs-common.toml

# 6. NPE Pneu: норма сегодня — 0 (дефект жив); после починки — 1.
grep -c 'instanceof FluidPipeBlockEntity' custom/mods/create_pneumatic_express/src/main/java/com/battlecraft/pneumaticexpress/CreatePneumaticExpress.java

# 7. Мотор Henry снят: норма — один и тот же хэш на мастере, клиенте и сервере.
sha256sum bountiful_wizards_REMAKE/kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js "$C"/kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js "$S"/kubejs/server_scripts/99_Remove_Henry_Kinetic_Motor.js

# 8. Опись семейства Create: норма 24.09 — UNIQUE_IDS 111. Скрипт целиком — knowledge/create/MOD_SURFACE.md §Recheck.

# 9. Игровая строка: норма сегодня — `- [ ]` (не отмечена).
grep -n 'TECH-GAME-01' claudework/ИГРОВОЙ_ЧЕКЛИСТ.md
```
