# Формат ответа

Верни результат строго в двух секциях. Текст вне секций не допускается.

## Секция 1: Mermaid

Fenced-блок: ```mermaid

Формат: `flowchart LR`

Таблицы — трапеции с префиксом `tbl_`:

    tbl_user[/"User (entity)"/]

Функции — скруглённые прямоугольники с префиксом `fn_`:

    fn_register_user(["RegisterUser"])

Допустимые типы рёбер (указывать в подписи на стрелке):
- `consumes`: `table → function`
- `produces`: `function → table`
- `reads`: `table → function`
- `writes`: `function → table`
- `triggers`: `function → function`

Запрещено: `table → table`, `function → function` без `triggers`.

## Секция 2: JSON

Fenced-блок: ```json

Один объект с двумя ключами: `functions` и `tables`.

### functions

Ключ — имя функции (`PascalCase`, глагол + существительное).  
Значение — объект:

```json
{
  "purpose": "одно предложение",
  "processing": ["шаг 1", "шаг 2", "..."],
  "input": {
    "consumes": { "ИмяТаблицы": "короткое описание" },
    "reads": { "ИмяТаблицы": "короткое описание" }
  },
  "output": {
    "produces": { "ИмяТаблицы": "короткое описание" },
    "writes": { "ИмяТаблицы": "короткое описание" }
  },
  "errors": [
    { "condition": "условие", "result": "результат" }
  ],
  "triggers": [
    { "function": "ИмяФункции", "condition": "условие" }
  ],
  "contract": {
    "Cin": ["таблицы"],
    "Cout": ["таблицы"],
    "R": ["таблицы"],
    "W": ["таблицы"],
    "Tin": ["функции"],
    "Tout": ["функции"]
  }
}
```

Правила:
- `consumes` / `reads` / `produces` / `writes`: значение — только короткая строка, не объект
- `processing`: каждый элемент — одно действие, одна строка
- `contract`: массивы трактуются как множества, порядок незначим, дубликаты запрещены
- пустые поля: `{}` для объектов, `[]` для массивов

### tables

Ключ — имя таблицы (`PascalCase`, единственное число).  
Значение — объект:

```json
{
  "x-kind": "entity | event | reference | log | projection | error",
  "x-pk": "имя_поля",
  "properties": {
    "имя_поля": { "type": "тип", "format": "формат" }
  },
  "required": ["обязательные поля"]
}
```

Расширения на уровне поля (внутри `properties`):
- `"x-fk": "Таблица.поле"` — внешний ключ
- `"x-unique": true` — уникальность значения

`x-pk` и `x-fk` — только `single-field`.

Типы полей:
- `text` → `string`
- `number` → `number` (`integer` допустим)
- `bool` → `boolean`
- `date` → `string` + `format: date-time`
- `enum` → `string` + `enum: [значения]`
- `ref` → тип целевого PK + `x-fk`

Имена полей и enum-значений: `snake_case`.

## Приоритет секций

Если Mermaid и JSON расходятся, источником истины считается JSON.

## Согласованность

Секции должны быть согласованы:
- каждый узел в Mermaid = запись в `functions` или `tables`
- каждое ребро в Mermaid = элемент `contract`
- каждый элемент `contract` = ребро в Mermaid
- `kind` в подписи узла Mermaid = `x-kind` в `tables`
- ключи `consumes` / `reads` / `produces` / `writes` = соответствующие массивы `contract`
- значения `triggers[*].function` = массив `Tout` в `contract`
