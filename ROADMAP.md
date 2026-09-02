# ДОРОЖНАЯ КАРТА СБОРКИ

> Это сводка шапок `claudework/systems/*.md`, не второй источник процентов, решений или задач.
> Если строка расходится с системой, верна система; ROADMAP надо пересобрать.

## Ветки

| ветка | готовность | главный текущий разрыв | источник |
|---|---:|---|---|
| Волны и Factory Siege | 34 % | active Factory Siege jar и полный runtime-контур отсутствуют | [ВОЛНЫ_И_FACTORY_SIEGE.md](systems/ВОЛНЫ_И_FACTORY_SIEGE.md) |
| Еда | 50 % | у Aquaculture нет ни квестов, ни пресноводного источника; баланс эффектов/напитков и игровая приёмка не закрыты | [ЕДА.md](systems/ЕДА.md) |
| Лут и награды | 46 % | правило «риск → награда» и regression gate не завершены | [ЛУТ_И_НАГРАДЫ.md](systems/ЛУТ_И_НАГРАДЫ.md) |
| Магия | 61 % | не сведён окончательный маршрут изучения/редкости | [МАГИЯ.md](systems/МАГИЯ.md) |
| Матерлод | 59 % | RNS B доставлен, но fresh-chunk/runtime не принят | [МАТЕРЛОД.md](systems/МАТЕРЛОД.md) |
| Металлургия | 50 % | поздний контур и игровая приёмка неполны | [МЕТАЛЛУРГИЯ.md](systems/МЕТАЛЛУРГИЯ.md) |
| Наёмники | 9 % | релизный gameplay-контур почти не собран | [НАЁМНИКИ.md](systems/НАЁМНИКИ.md) |
| Навыки и перки | 27 % | постоянная архитектура дерева не выбрана | [НАВЫКИ_И_ПЕРКИ.md](systems/НАВЫКИ_И_ПЕРКИ.md) |
| Опт | 37 % | Wares contract surface и выпускная приёмка неполны | [ОПТ.md](systems/ОПТ.md) |
| Ореген | 38 % | нет принятой глобальной лестницы и fresh-world verdict | [ОРЕГЕН.md](systems/ОРЕГЕН.md) |
| Прогрессия | 36 % | нет end-to-end серверного vertical slice | [ПРОГРЕССИЯ.md](systems/ПРОГРЕССИЯ.md) |
| Структуры | 37 % | состав/плотность и посадка ещё не приняты | [СТРУКТУРЫ.md](systems/СТРУКТУРЫ.md) |
| Схематики | 19 % | доставка/library/security acceptance не завершены | [СХЕМАТИКИ.md](systems/СХЕМАТИКИ.md) |
| Технологии | ⚠️ спорно, см. `OPEN.md` §1.361 | страница системы печатает 50 %, кросс-ревью Opus рекомендует держать 42 % — строка «master inputs + workbook» оценена авансом; число не копируется сюда, пока он не выберет | [ТЕХНОЛОГИИ.md](systems/ТЕХНОЛОГИИ.md) |
| Торговля | 50 % | NPC-ассортимент и полный PRT-проход отсутствуют | [ТОРГОВЛЯ.md](systems/ТОРГОВЛЯ.md) |
| Экономика | 50 % | LEAF/printer/cash-out и NPC boundary не закрыты | [ЭКОНОМИКА.md](systems/ЭКОНОМИКА.md) |
| Энергия | 41 % | workbook не воспроизводится из master-owned inputs | [ЭНЕРГИЯ.md](systems/ЭНЕРГИЯ.md) |

## Как читать

- Процент берётся только из воспроизводимой weighted table соответствующей systems page.
- Решения остаются только в `agents/registers/OPEN.md`; работа — только в `TODO.md`; игровые проверки — только в `ИГРОВОЙ_ЧЕКЛИСТ.md`.
- Закрытый sync не повышает готовность автоматически: если нужна игровая приёмка, доля остаётся `0,5`.

## Чем проверить

```powershell
Get-ChildItem claudework/systems -File -Filter *.md | ForEach-Object {
  $header=Select-String $_.FullName -Pattern 'Готовность:\s*([0-9]+)\s*%' | Select-Object -First 1
  "{0}`t{1}" -f $_.BaseName,$header.Matches[0].Groups[1].Value
}
```

Норма: 17 строк; каждое имя и процент совпадают с таблицей выше.

Второй проход — по **закоммиченному** дереву: каждая ссылка таблицы должна указывать на файл,
который уже в индексе Git, иначе ROADMAP ссылается на страницу, которой в коммите нет.

```powershell
$linked = (Select-String claudework/ROADMAP.md -Pattern '\]\(systems/(.+?\.md)\)' -AllMatches).Matches |
  ForEach-Object { $_.Groups[1].Value } | Sort-Object -Unique
$tracked = git -c core.quotepath=false ls-tree --name-only HEAD claudework/systems/ |
  ForEach-Object { $_ -replace '^claudework/systems/','' }
"linked=$($linked.Count) tracked=$($tracked.Count)"
$linked | Where-Object { $_ -notin $tracked } | ForEach-Object { "DANGLING $_" }
```

Норма: `linked` равен `tracked` и ни одной строки `DANGLING`.
