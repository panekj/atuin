# Конфигурация

Autin использует два файла конфигурации. Они хранятся в `~/.config/atuin/`. Данные
хранятся в `~/.local/share/atuin` (если не определено другое в XDG\_\*).

Путь до катклога конфигурации может быть изменён установкой 
параметра `ATUIN_CONFIG_DIR`. Например

```
export ATUIN_CONFIG_DIR = /home/ellie/.atuin
```

## Пользовательская конфигурация

```
~/.config/atuin/config.toml
```

Этот файл используется когда клиент работает на локальной машине (не сервере).

See [config.toml](../atuin-client/config.toml) for an example

### `dialect`

Этот параметр контролирует как [stats](stats.md) команда обрабатывает данные.
Может принимать одно из двух допустимых значений:

```
dialect = "uk"
```

или

```
dialect = "us"
```

По умолчанию - "us".

### `auto_sync`

Синхронизироваться ли автоматически если выполнен вход. По умолчанию - да (true)
```
auto_sync = true/false
```

### `sync_address`

Адрес сервера для синхронизации. По умолчанию `https://api.atuin.sh`.

```
sync_address = "https://api.atuin.sh"
```

### `sync_frequency`

Как часто клиент синхронизируется с сервером. Может быть указано в 
понятном для человека формате. Например, `10s`, `20m`, `1h`, и т.д.
По умолчанию `1h`

Если стоит значение 0, Autin будет синхронизироваться после каждой выполненной команды.
Помните, что сервера могут иметь ограничение на количество отправленных запросов.

```
sync_frequency = "1h"
```

### `db_path`

Путь до базы данных SQlite. По умолчанию это
`~/.local/share/atuin/history.db`.

```
db_path = "~/.history.db"
```

### `key_path`

Путь до ключа шифрования Autin. По умолчанию,
`~/.local/share/atuin/key`.

```
key = "~/.atuin-key"
```

### `session_path`

Путь до серверного файла сессии Autin. По умолчанию,
`~/.local/share/atuin/session`. На самом деле это просто API токен.

```
key = "~/.atuin-session"
```

### `search_mode`

Определяет, какой режим поиска будет использоваться. Autin поддерживает "prefix",
текст целиком (fulltext) и неточный ("fuzzy") поиск. Режим "prefix" производит
поиск по "запрос\*", "fulltext" по "\*запрос\*", и "fuzzy" использует 
[вот такой](#fuzzy-search-syntax) синтаксис.

По умолчанию стоит значение "prefix"

### `filter_mode`

Фильтр, по-умолчанию использующийся для поиска

| Столбец 1        | Столбец 2	                                               |
|------------------|----------------------------------------------------------|
| global (default) | Искать историю команд со всех хостов, сессий и каталогов |
| host             | Искать историю команд с этого хоста                      |
| session          | Искать историю команд этой сессии                        |
| directory        | Искать историю команд, выполненных в текущей папке       |

Режимы поиска могут быть изменены через ctrl-r


```
search_mode = "fulltext"
```

#### fuzzy search syntax

Режим поиска "fuzzy" основан на
[fzf search syntax](https://github.com/junegunn/fzf#search-syntax).

| Токен     | Тип совпадений             | Описание                            |
|-----------|----------------------------|-------------------------------------|
| `sbtrkt`  | fuzzy-match                | Всё, что совпадает с `sbtrkt`       |
| `'wild`   | exact-match (В кавычках)   | Всё, что включает в себя `wild`     |
| `^music`  | prefix-exact-match         | Всё, что начинается с `music`       |
| `.mp3$`   | suffix-exact-match         | Всё, что заканчивается на `.mp3`    |
| `!fire`   | inverse-exact-match        | Всё, что не включает в себя `fire`  |
| `!^music` | inverse-prefix-exact-match | Всё, что не начинается с `music`    |
| `!.mp3$`  | inverse-suffix-exact-match | Всё, что не заканчивается на `.mp3` |

Знак вертикальной черты означает логическое ИЛИ. Например, запрос ниже вернет
всё, что начинается с `core` и заканчивается либо на `go`, либо на `rb`, либо на `py`.

```
^core go$ | rb$ | py$
```

## Серверная конфигурация

`// TODO`
