# Шпаргалка — команды, которые нужны чаще всего

## ШАГ 0 — без этого ничего не заработает

Открой консоль и **сначала** перейди в корень репозитория — туда, где лежит папка
`bountiful_wizards_REMAKE`. Это та же папка, в которую ты клонировал проект.

🔑 **Не запоминай путь и не копируй его отсюда.** Машин две (десктоп и ноутбук), и путь на них разный.
Правильный путь всегда печатает сам проект — из корня репозитория:

```
python bountiful_wizards_REMAKE/tools/paths.py
```

Он выведет строку `machine` (desktop или laptop) и актуальные `MASTER`, `REPO`, `SERVER`, `CLIENT`.
**Если эта команда отработала — ты стоишь в правильной папке.** Если написала, что файла нет, —
поднимись на уровень выше и повтори.

Все команды ниже запускаются **именно из корня репозитория**, а не из папки `tools`.

⚠️ **Две типовые ошибки, обе легко словить:**

**1. Зайти в `tools` и запускать оттуда.** Путь `tools/что-то.py` отсчитывается от папки выше,
поэтому изнутри `tools` получится `tools\tools\...` и ошибка `No such file or directory`.

**2. Набрать просто `python` и нажать Enter.** Это бросает внутрь интерпретатора Python — видно
по значку `>>>` в начале строки. Там команды консоли не работают и дают `SyntaxError`. Выйти:

```
exit()
```

Правильная строка всегда целиком, одной командой: `python`, пробел, и сразу путь к скрипту:

```
python tools/price_engine_2026_08_01.py --explain irons_spellbooks:upgrade_orb
```

Пути к серверу и клиенту машина определяет сама — десктоп и ноут различаются, но `tools/paths.py`
это уже решает, руками ничего писать не надо. Проверить, что он видит:

```
python tools/paths.py
```

## Ноут: синк клиента и репозитория

```bash
cd bountiful_wizards_REMAKE
python tools/sync_laptop_client.py --check          # что уедет, ничего не трогая
python tools/sync_laptop_client.py --match-branch   # синк + перевести ноут на ветку этой машины
```

Читать **последние три строки**. Строка `branch … on both sides · same commit …` — единственное
доказательство, что `claudework/` (чеклист, реестры, отчёты) реально доехал: файловое зеркало копирует
только `mods/ config/ kubejs/ defaultconfigs/`, всё остальное едет через git. **Некоммиченное и
незапушенное на ноут не попадает никогда**, сколько бы файлов ни скопировалось.

Если `laptop offline` — на ноуте должны быть шары `MAIN` и `ClaudeCode`, входящий 445 с
`100.64.0.0/10`, а на этой машине один раз сохранённый доступ:
`cmdkey /add:100.70.109.109 /user:ANDRO /pass` (набирает владелец, не агент).

Подробности и разбор всех отказов — скилл `laptop-sync`.

## Orca: закрыть терминалы отработавших воркеров

```bash
python tools/orca_orchestrator.py reap-workers --check   # кого закрыл бы
python tools/orca_orchestrator.py reap-workers           # закрыть
```

Закрывает только те терминалы, которые Orca считает `reclaimable` — то есть Dispatch уже завершён.
Вывод воркера перед закрытием архивируется, `worker-read` продолжает работать. Вкладки, которые ты
взял себе руками, и `retained` не трогаются никогда.

## Orca — запустить диспетчерскую агентов

Для владельца — один двойной клик в корне репозитория:

```powershell
ORCA_GUARDIAN.cmd
```

Он спрашивает основной и резервный маршрут из активных Claude/Codex, ставит автозапуск Windows,
поднимает обычное окно Orca и одного Producer. Сейчас Kiro временно выключен из Guardian, поэтому
порядок только **Claude Opus 5 → Codex Terra**. Повторный запуск меняет порядок и печатает пульт.

Альтернативный режим без desktop GUI:

```powershell
ORCA_GUARDIAN.cmd --headless
ORCA_GUARDIAN.cmd --pair-mobile
```

Он использует Tailscale; обычный режим использует окно Orca и Relay.

**Второй вход — сказать «будь оркестратором» прямо в открытом чате** Claude Code или Codex. Тогда
Producer — эта самая сессия, на своей модели, без терминала в Orca. Быстро и без возни, но нет
watchdog и нет авто-переключения: сессия закрылась — координатора нет. Полный разбор двух входов —
`agents/orca/OWNER_GUIDE_RU.md` §«Два входа».

Проверить связку без изменений:

Новые Codex/Claude Producer запускаются без permission-пауз: Codex с `approval=never` и полным
доступом, Claude с bypass permissions. Это относится к терминалам, созданным watchdog; уже открытый
Producer получает новые флаги только после штатной ротации.

```powershell
python tools/orca_orchestrator.py usage       # модели: доступны / резерв / owner-only / недоступны
python tools/orca_orchestrator.py guardian --status
python tools/orca_orchestrator.py guardian --repair --no-prompt
python tools/orca_orchestrator.py status
python tools/orca_orchestrator.py health
python tools/orca_orchestrator.py doctor
python tools/orca_orchestrator.py supervisor-status
python tools/orca_orchestrator.py failover --to claude --dry-run
```

В чате с Producer достаточно написать **`usage`**: он обязан сам запустить эту read-only команду и
коротко ответить по актуальному shortlist и времени сброса лимитов. Run и Worker при этом не создаются.

Guardian работает вне Orca и поднимает её обычное окно даже после reboot. Он идёт по выбранной
тройке, пропускает reserve/unavailable и не закрывает Workers/не создаёт второй Run.
При включённом «автономно» он также следит за Run/DAG: 10 минут без живой раздачи или принятия
Delivery — напоминание Producer; ещё 5 минут без движения — checkpoint и безопасная ротация.

`health` — быстрый read-only контроль после раздачи: показывает, действительно ли Worker начал
работать, и считает 45 сохранённых старых деревьев как `completed+quiet`, а не как тревогу.

Когда Producer уже создал Run и Task, подключить внешнего Worker:

```powershell
python tools/orca_orchestrator.py claude-worker --task <task_id> --model opus --effort high --worktree current --run <run_id>
python tools/orca_orchestrator.py go-worker --task <task_id> --model deepseek-v4-flash --worktree current --run <run_id>
python tools/orca_orchestrator.py opencode-worker --task <task_id> --model alibaba-qwen/qwen3.8-max --worktree current --run <run_id>
python tools/orca_orchestrator.py opencode-worker --task <task_id> --model kilo/kilo-auto/free --worktree current --run <run_id>
python tools/orca_orchestrator.py gemini-worker --task <task_id> --model gemini-3.7-flash --worktree current --run <run_id>
```

Claude Worker запускается только через `claude-worker`: прямой `worker-start --agent claude` может
принять Task, показать bypass-баннер и тихо выйти до первого действия.

Сами флаги баннер НЕ убирают — они его и вызывают. Убирает только ключ
`"skipDangerousModePermissionPrompt": true` в настройках Claude; он стоит в `~/.claude/settings.json`
и в `.claude/settings.local.json` репозитория с 2026-08-19. Проверить перед волной Claude-воркеров:

```powershell
python -c "import json;print(json.load(open(r'C:/Users/daedr/.claude/settings.json'))['skipDangerousModePermissionPrompt'])"
```

Это соответственно OpenCode Go, Alibaba, бесплатный маршрутизатор Kilo и прямой Gemini CLI.
API-ключи остаются в глобальных настройках и никогда не попадают в Task. Gemini через API-ключ
реально выполнил файловый инструмент; подписочная авторизация Google AI Pro для CLI пока отклоняется.

Дальше пиши координатору обычным русским языком. Короткая инструкция:
[`agents/orca/OWNER_GUIDE_RU.md`](agents/orca/OWNER_GUIDE_RU.md).

---

## В игре

Применить изменения пулов, декретов и рецептов:

```
/reload
```

⚠️ `config/bountiful/bountiful.json` через `/reload` **не** подхватывается — нужен рестарт сервера.
И доски держат старые баунти сквозь `/reload`: чтобы прокрутить свежие, сломай и поставь доску
заново (это заодно сбросит её репутацию в 0). Новые баунти появляются через ~45 с тика доски.

Узнать id предмета в руке (CraftTweaker убран, живёт только это):

```
/kubejs hand
```

### Магия — Iron's Restrictions

Выдать себе ступень редкости (нужно после смены `startingRarity`, он применяется только к новым
персонажам):

```
/rarity set
```

Узнать id заклинания:

```
/getSpellID
```

Забыть одно заклинание — удобно, чтобы перепроверять цепочку обучения:

```
/forgetSpell
```

Список спелл-конфигов Iron's (даёт точные id заклинаний, их десятки):

```
/ironsSpellbooks config list
```

### Проверки, которые висят в чеклисте

Странствующий торговец должен быть заблокирован InControl. Тест — **яйцо призыва** из креатива:
поставь его, торговец появиться не должен.

⚠️ **`/summon` для этого не годится и никогда не годился.** Команда оператора кладёт сущность в мир
напрямую, мимо всех четырёх событий, на которых InControl умеет висеть (кроме `onjoin`, а он заодно
убил бы пленного торговца в Evoker Fort). Торговец, вызванный командой, не говорит о блокировке
ничего — ни за, ни против. Строка с этим тестом стояла здесь до 06.08 и стоила одного ложного
«подтверждено» по вопросу Q7 (закрыт — `OPEN.md` §5; вывод целиком в
`archive/OPEN_full_register_to_2026_08_09.md`) и одного отката правила.

Яйцо годится потому, что идёт тем же путём (`FinalizeSpawn`), что и сам `WanderingTraderSpawner`.

Прогрессия Twilight Forest живёт в **геймруле**, а не в конфиге — если однажды понадобится,
искать надо здесь:

```
/gamerule tfEnforcedProgression
```

⚠️ **Не выключай без причины.** По умолчанию `true`, и это замысел мода: Нага → Лич →
Миношрум → Гидра → Рыцари → Ур-Гаст, каждый следующий биом за трофеем предыдущего.
`false` схлопывает всю ветку в «иди куда хочешь» — и заодно открывает поздние TF-сундуки
на раннем этапе, а лут-скрипт кладёт в них магические материалы в расчёте на прогрессию.

---

## Пайплайн — после ЛЮБОЙ правки пулов или декретов

Строго в этом порядке:

```
python tools/rep_ladder.py
```
```
python tools/reverse_spread.py
```
```
python tools/sanity_check.py
```
```
python tools/validate_all_ids.py
```
```
python tools/make_spreadsheet.py
```
```
python tools/make_emi_trading_info.py
```

⚠️ Последние две — **не опциональные**. Это дисплей-слой: без них игра показывает старые цены, и
05.08 EMI неделю врал про blank rune, пока ты сам не заметил. Обе требуют **закрытого пака** —
пишут в живые копии.

Только если добавлял или переименовывал торговцев:

```
python tools/decree_lang.py --apply
```

⚠️ **`--apply` появился 2026-08-17 и он обязателен у 13 инструментов, которые правят пулы и
декреты** (`decree_lang`, `wholesale_pass`, `rune_fix`, `unbrick_negative_rep`, `earlygame_polish`,
`add_logistics_bundle`, `apply_backlog`, `apply_homework`, `capacity_patch`, `new_traders`,
`recipe_repricing`, `restore_expensive`, `spread_patch`). Без флага они печатают «DRY RUN — nothing
was written», говорят, что изменили бы, и выходят с кодом 2 — то есть запустить «посмотреть» теперь
безопасно. Генераторы (`index_all_recipes`, `make_hidden_tags`, `kubejs_recipes`, `tiers`,
`make_emi_trading_info`) флага не требуют: их перезапуск и есть рабочий процесс.
```
python tools/sync_avatars.py
```

---

## Синхронизация

Пулы и декреты, master → сервер (пак должен быть закрыт):

```
cp config/bountiful/bounty_pools/*.json "$(python tools/paths.py --server)/config/bountiful/bounty_pools/"
```

⚠️ **Путь к серверу не пиши руками.** Он разный на десктопе и на ноуте, поэтому здесь `paths.py
--server` — он подставит правильный. До 18.08 в этой строке стоял литерал `C:/Documents/server`,
которого на десктопе нет вообще.

⚠️ Никогда не сноси весь `config/bountiful/` на сервере: рядом лежит `bountiful.json`, конфиг самого
мода. С 02.08 он **есть и в master** (все четыре копии совпадают побайтово, проверено 18.08), но
сносить папку всё равно незачем — `sync_client.py` его уже возит, а `/reload` его не подхватывает,
нужен рестарт сервера.

KubeJS-скрипты и датапак, master ↔ сервер:

```
python tools/sync_kubejs.py
```

> 🛑 **`--help` у `sync_kubejs.py` и `sync_client.py` тоже ловушка.** Argparse у них нет: флаг читается
> как `"--check" in sys.argv`, поэтому **любой нераспознанный аргумент, включая `--help`, выполняет
> настоящий синк с записью.** Проверено 20.08 — `sync_kubejs.py --help` отчитался о выполненном синке
> master → сервер. Сухой прогон — ровно `--check`, ничего другого.

Если он ругнулся `CONFLICT` — **не форси сразу**. Сначала посмотри, есть ли на сервере строки,
которых нет в master:

```
diff <(tr -d '\r' < kubejs/server_scripts/08_Magic.js) <(tr -d '\r' < "$(python tools/paths.py --server)/kubejs/server_scripts/08_Magic.js") | grep '^>'
```

Пусто — значит master строго впереди и форс безопасен:

```
python tools/sync_kubejs.py --force-master
```

Сервер → клиент (проверить, не применяя):

```
python tools/sync_client.py --check
```
```
python tools/sync_client.py
```

Комп → **ноут** (мод-сет + конфиги клиента; если ноут выключен — просто пометка, без ругани):

```
python tools/sync_laptop_client.py --check
```
```
python tools/sync_laptop_client.py
```

Куда слать, задаётся в `reference/laptop_sync.json` (образец рядом, `.example`). Лишний джарник на
ноуте не удаляется, а переименовывается в `.jar.disabled`. Состояние — в
`reference/laptop_sync_state.json`.

Обновить зеркало сервера в репозитории:

```
python tools/mirror_server_for_git.py
```

После синка клиента и обновления зеркала — обязательная тройная сверка конфигов:

```
python tools/audit_config_drift.py
```

Подробности в UTF-8 `reference/_config_drift_report.md`; отсутствие client/mirror — ошибка, не
«чистый» отчёт. Если вручную принимаем конфиг из инстанса, сначала сверяем с сервером, затем кладём
утверждённый файл в master **и** добавляем строку с причиной в `reference/tuned_config_manifest.json`.

---

## 🔴 После ЛЮБОЙ правки рецепта — одна команда

```
python tools/rebuild_caches.py
```

Пересобирает **все четыре** кеша рецептов в единственном рабочем порядке. Без этого экономические
инструменты отвечают про прошлое, причём молча.

> 🛑 **У этой команды нет `--help`, и это ловушка.** `python tools/rebuild_caches.py --help` не
> печатает справку — он **запускает конвейер**, начиная с шага 1/5 `sync_kubejs`, который пишет в
> SERVER-дерево. Проверено 2026-08-20: попытка «просто посмотреть аргументы» реально стартовала
> синхронизацию. Она остановилась сама на конфликте и ничего не скопировала, так что обошлось — но
> запускать её, пока какой-нибудь воркер правит `kubejs/`, значит утащить на сервер недописанную
> работу. Аргументы смотри в исходнике, а не флагом.

⚠️ **Зачем это отдельным пунктом.** Кешей четыре, собираются четырьмя разными скриптами из **двух
разных деревьев**, и протухают независимо. 06.08 три из четырёх отстали на четыре дня, и это дало
три разных неверных ответа подряд: аудит несколько сессий показывал худшим маршрутом пака мёртвый
**2.91×**; он же продолжал показывать принтер **7.76×** уже после починки, потому что читает
**серверное** дерево, а правка лежала в мастере; а движок считал `item_vault` и `gantry_shaft` по
джарниковым рецептам, которые пак давно удалил.

Порядок внутри неочевиден и потому зашит в скрипт: **`sync_kubejs` первым** (один из кешей читает
сервер), и **overlay обязателен** — это единственный, который знает, какие рецепты пак *удалил*.

Если ругнулся `CONFLICT` — **не форси**, сначала diff по процедуре ниже, и только потом:

```
python tools/rebuild_caches.py --force
```

---

## Спросить экономику

Откуда взялась цена предмета и по какому маршруту:

```
python tools/price_engine_2026_08_01.py --explain irons_spellbooks:upgrade_orb
```

Вся лестница разом:

```
python tools/price_engine_2026_08_01.py --report
```

Что было бы при других полиси-переключателях:

```
python tools/price_engine_2026_08_01.py --scenario
```

Арбитражные маршруты (с 06.08 считает FE по 1.35/kFE и жидкости по их якорям):

```
python tools/audit_recipe_spreads.py
```

Машинные принтеры:

```
python tools/audit_machine_printers_2026_08_01.py
```

Перегенерировать `agents/RULINGS.md` — он **генерируемый**, руками не править:

```
python tools/rulings_register.py
```

---

## Планнер

Пересобрать блоб цен (после любой правки пулов, иначе страница цитирует старое):

```
node reference/_build_market_data.js
```

Проверить дрейф жёстко вписанных цен на странице:

```
node reference/_verify_prices.js
```

Починить найденный дрейф:

```
python tools/sync_planner_prices.py --apply
```

---

## Разовые скрипты 06.08

Каждый несёт всю деривацию в докстринге — читать перед запуском.

```
python tools/reprice_deposit_containers_2026_08_06.py
```
```
python tools/cap_corvin_orb_amounts_2026_08_06.py
```
```
python tools/cap_corvin_rune_amounts_2026_08_06.py
```

⛔ `reprice_charged_capacitors_2026_08_06.py` — **не запускать**, отменён тем же днём законом DEP.

---

## Перед сборкой релиза

Посмотреть, что уедет игрокам (ничего не меняет):

```
python tools/prepare_release.py
```

Вычистить личные хвосты:

```
python tools/prepare_release.py --apply
```

⚠️ Гонять **каждый раз перед упаковкой**, а не однажды: пер-серверные папки возвращаются при каждом
заходе на сервер, а `enableShaders` клиент переписывает при выходе. Решения (heap, геометрия окна
Forge, тюн `betterfpsdist`, шейдерный профиль) инструмент только показывает и не трогает.

---

## Откат старого ценового прохода

Запускай из `bountiful_wizards_REMAKE/`:

```powershell
git checkout -- config/bountiful/bounty_pools/
python tools/reprice_shelves_recipe_true_2026_08_13.py --revert-all
```

После отката выполни обычный sync. Эта команда относится к обслуживанию файлов, а не к игровому
чек-листу.

---

## Шейдеры

Профили и объяснение — `shader_profiles/README.md`. Команды ниже написаны для **ноутбука**
(`ANDRO`) — профили `laptop-*` тюнились под его железо. На десктопе путь тот же, но с `daedr`,
а профиль выбирай осознанно: у RX 6650 XT свой запас, `laptop-safe` там не про тебя.

Применить сбалансированный:

```
cp "claudework/shader_profiles/laptop-balanced.txt" "C:/Users/ANDRO/curseforge/minecraft/Instances/MAIN/shaderpacks/ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3.txt"
```

Если драйвер начал срываться (чёрный экран, `nvlddmkm`):

```
cp "claudework/shader_profiles/laptop-safe.txt" "C:/Users/ANDRO/curseforge/minecraft/Instances/MAIN/shaderpacks/ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3.txt"
```

Откатиться к тому, что было до правок:

```
cp "claudework/shader_profiles/00_original_backup_2026-08-06.txt" "C:/Users/ANDRO/curseforge/minecraft/Instances/MAIN/shaderpacks/ComplementaryUnbound_r5.8.1 + EuphoriaPatches_1.9.3.txt"
```

После замены — F6 → Reload или перезаход. ⚠️ `config/oculus.properties` переписывается клиентом
при выходе, включая `enableShaders`.

---

## Копание в джарниках

Найти, где мод на самом деле держит рецепт (лечит ловушку неймспейса — id пишется по пути в
джарнике, а не по имени мода):

```
python -c "import zipfile,glob;[print(j,n) for j in glob.glob('*.jar') for n in zipfile.ZipFile(j).namelist() if 'upgrade_orb' in n]"
```

Прочитать дефолт из байт-кода, а не из комментария в конфиге (на ноутбуке — `ANDRO` вместо
`daedr`, jabba стоит на обеих машинах):

```
"C:/Users/daedr/.jabba/jdk/temurin@17.0.19/bin/javap" -p -c ИМЯ.class
```

---

## Что помнить рядом с этими командами

- **Дефолт берётся из джарника, не из комментария.** Комментарии в конфигах врут — так было у
  YungsMenuTweaks и у `pipeorgans` с опечаткой `idiIdleTimeout` в самом джарнике.
- **`event.remove` без чтения полного пути `data/` — молчаливый no-op.** Create Wizardry шлёт
  школьные орбы под `minecraft:`, а базовый — под `irons_spellbooks:`; CAA шлёт своё под `create:`.
- **`reverse_spread.py` не отчитывается, а переписывает пулы.** Любой want выше 0.80 от give он
  срежет. Исключение одно — `DEPOSIT_CONTAINERS` (конденсаторы).
- **want = 1.00 от give — это бесплатная ферма репутации**, а не нулевая прибыль: объектив
  закрывается покупкой у того же магазина, а награда рисуется под его worth.
- **Зелёная сводка инструмента говорит только про то, что он знает.** `rep_ladder` печатает
  «GATES ON | rewards=1829» и при этом подтверждает *отсутствующий* гейт.
- **Пер-мировые конфиги лежат в шести местах.** `world/serverconfig/` — живой,
  `defaultconfigs/` — только засев новых миров. Правка одного без другого не держится.
