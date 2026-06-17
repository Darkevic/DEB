# Лекция и практическая работа №10
# Лицензирование, SPDX и open-source compliance

Тема 2.4 — Защита программ и данных от несанкционированного копирования
Дисциплина: Программно-аппаратная защита информации
Длительность: лекция ~35 минут + практика 90 минут
Форма сдачи: GitHub, папка pr10/ с отчётом, скриншотами и добавленным LICENSE

---

# ЧАСТЬ 1. ЛЕКЦИЯ

---

## 1. Зачем вообще думать о лицензиях если "код лежит на GitHub"

Классическое заблуждение: если код выложен в открытый доступ — значит его можно использовать как угодно. Нельзя.

Во всех нормальных странах авторское право возникает автоматически в момент создания произведения. Не нужно нигде регистрироваться, платить пошлины или ставить копирайт. Написал код — он твой. Все права защищены. По умолчанию никто не может его использовать, копировать, изменять или распространять без разрешения автора.

Выложить код на GitHub без лицензии — это не "сделать открытым". Это значит: "смотреть можно, делать с этим что-либо нельзя". Технически любой кто скачал твой код без лицензии и использует его в своём проекте — нарушает авторское право.

Лицензия — это юридический документ в котором автор говорит: "вот что вам разрешено делать с моим кодом". Без неё разрешено ничего.

**Почему это важно для безопасности и защиты информации:**

- использование кода под неподходящей лицензией может принести юридические проблемы всей организации
- некоторые лицензии требуют раскрывать исходный код коммерческого продукта — это критично для защиты интеллектуальной собственности
- проверка лицензий зависимостей — часть supply chain security (безопасности цепочки поставок)
- регуляторы и корпоративные покупатели всё чаще требуют документацию об используемых компонентах (SBOM)

---

## 2. Виды лицензий и чем они отличаются

Лицензии делятся на два больших лагеря: разрешительные (permissive) и копилефтные (copyleft).

### 2.1 Разрешительные лицензии (permissive)

Разрешают почти всё. Бери, используй, модифицируй, включай в коммерческий продукт. Единственное требование обычно — сохранить копирайт и текст лицензии.

**MIT License**

Самая популярная лицензия в мире. Текст занимает меньше страницы.

Разрешает: использовать, копировать, изменять, объединять, публиковать, распространять, сублицензировать, продавать.
Требует: сохранить текст лицензии и уведомление об авторских правах.
Не требует: раскрывать исходный код, использовать ту же лицензию в своём продукте.

Используют: React, Rails, jQuery, большинство npm-пакетов.

**Apache 2.0**

Похожа на MIT, но добавляет защиту от патентных претензий. Если компания лицензировала код под Apache 2.0, она автоматически предоставляет права на свои патенты которые покрывает этот код. Важно для корпоративного использования.

Требует дополнительно: указывать изменения внесённые в файлы.

Используют: Android (частично), большинство проектов Apache Foundation, Kubernetes.

**BSD (2-clause, 3-clause)**

Старая лицензия, исторически первая open-source лицензия. Практически то же что MIT, с небольшими вариациями в формулировках.

### 2.2 Копилефтные лицензии (copyleft)

Ключевая идея: если используешь код под копилефтной лицензией — твой продукт тоже должен быть под той же лицензией. "Вирусный" эффект.

**GPL (GNU General Public License), версии 2 и 3**

Самая известная копилефтная лицензия. Если ты включаешь GPL-код в свой продукт и распространяешь его — весь продукт должен быть под GPL, исходный код должен быть доступен.

Для коммерческих компаний это часто критично: нельзя просто взять GPL-библиотеку, вставить в закрытый продукт и продавать. Придётся либо открыть весь исходный код, либо не использовать GPL-код.

Используют: Linux kernel (GPL v2), Git, WordPress.

**LGPL (Lesser GPL)**

Ослабленная версия GPL. LGPL-библиотеку можно использовать в закрытом продукте как динамически подключаемую библиотеку, не раскрывая исходный код продукта. Но если изменяешь саму LGPL-библиотеку — изменения надо открыть.

**AGPL (Affero GPL)**

Усиленная версия GPL. Закрывает "облачную лазейку": если ты запускаешь GPL-программу как сервис (SaaS) и не распространяешь её — по GPL тебе не надо открывать код. AGPL требует открытости и в этом случае.

Используют: MongoDB (старые версии), Nextcloud, многие self-hosted сервисы.

**MPL (Mozilla Public License)**

Файловый копилефт: только изменённые файлы должны быть открыты под MPL, остальной код может быть закрытым. Компромисс между MIT и GPL.

Используют: Firefox, Thunderbird.

### 2.3 Специальные случаи

**CC (Creative Commons)** — для контента, не для кода. Документация, картинки, тексты.

**Unlicense / CC0** — отказ от авторских прав. Публичное достояние. Делай что хочешь.

**SSPL, BSL (Business Source License)** — условно-открытые лицензии. Код открыт, но использование в конкурирующих коммерческих продуктах запрещено. Придуманы MongoDB и HashiCorp чтобы облачные провайдеры не наживались на их коде.

**Проприетарные лицензии** — "все права защищены", покупаешь право использования, исходного кода нет.

---

## 3. SPDX — стандартный язык лицензий

### 3.1 Проблема без стандарта

До SPDX каждый указывал лицензию как хотел:
- "MIT License"
- "The MIT License (MIT)"
- "MIT license, see LICENSE file"
- "Licensed under MIT"
- "MIT/X11 License"

Это одно и то же, но машина не понимает. Проверять автоматически невозможно.

### 3.2 Что такое SPDX

**SPDX (Software Package Data Exchange)** — стандарт Linux Foundation для описания лицензий программного обеспечения. Включает:

- список идентификаторов для всех распространённых лицензий
- формат файла для описания компонентов пакета (SBOM)
- синтаксис для сложных выражений (несколько лицензий, исключения)

Официальный список: spdx.org/licenses

### 3.3 Основные SPDX-идентификаторы

| SPDX ID | Полное название | Тип |
|---------|----------------|-----|
| MIT | MIT License | Разрешительная |
| Apache-2.0 | Apache License 2.0 | Разрешительная |
| BSD-2-Clause | BSD 2-Clause "Simplified" | Разрешительная |
| BSD-3-Clause | BSD 3-Clause "New" or "Revised" | Разрешительная |
| GPL-2.0-only | GNU GPL v2.0 только | Копилефт |
| GPL-2.0-or-later | GNU GPL v2.0 или новее | Копилефт |
| GPL-3.0-only | GNU GPL v3.0 только | Копилефт |
| LGPL-2.1-only | GNU LGPL v2.1 только | Слабый копилефт |
| LGPL-3.0-or-later | GNU LGPL v3.0 или новее | Слабый копилефт |
| AGPL-3.0-only | GNU AGPL v3.0 | Сетевой копилефт |
| MPL-2.0 | Mozilla Public License 2.0 | Файловый копилефт |
| ISC | ISC License | Разрешительная |
| Unlicense | The Unlicense | Публичное достояние |
| CC0-1.0 | CC0 1.0 Universal | Публичное достояние |
| CC-BY-4.0 | CC Attribution 4.0 | Для контента |
| CC-BY-SA-4.0 | CC Attribution-ShareAlike 4.0 | Для контента |
| SSPL-1.0 | Server Side Public License | Ограниченная |
| BUSL-1.1 | Business Source License 1.1 | Ограниченная |

### 3.4 SPDX в коде

SPDX-идентификаторы можно добавлять прямо в заголовок файлов вместо длинного текста лицензии:

```python
# SPDX-FileCopyrightText: 2024 Иванов Иван <ivan@example.com>
# SPDX-License-Identifier: MIT
```

```javascript
// SPDX-FileCopyrightText: 2024 ООО Рога и Копыта
// SPDX-License-Identifier: Apache-2.0
```

Это машиночитаемо, коротко и понятно.

### 3.5 SPDX-выражения для сложных случаев

```
MIT AND Apache-2.0          -- под обеими лицензиями одновременно
MIT OR Apache-2.0           -- на выбор пользователя
GPL-2.0-only WITH Classpath-exception-2.0  -- с исключением
LicenseRef-custom           -- нестандартная лицензия
```

---

## 4. Open-source compliance — что это и зачем

**Open-source compliance** (соблюдение требований открытых лицензий) — процесс обеспечения того что организация использует open-source компоненты в соответствии с условиями их лицензий.

### 4.1 Почему это не просто формальность

Нарушение лицензий — реальный юридический риск. Несколько известных случаев:

- Компании выплачивали миллионы долларов за использование GPL-кода в закрытых продуктах без раскрытия исходников.
- OpenWRT и BusyBox регулярно судятся с производителями роутеров которые используют их GPL-код и не открывают исходники прошивок.
- В России ст. 1270 ГК РФ защищает исключительные права автора. Нарушение = убытки, компенсация до 5 млн рублей за произведение.

### 4.2 Что входит в compliance-процесс

1. **Инвентаризация компонентов** — знать что именно используется. Составляется SBOM (Software Bill of Materials).
2. **Проверка лицензий** — выяснить под какой лицензией каждый компонент.
3. **Анализ совместимости** — не конфликтуют ли лицензии между собой и с политикой компании.
4. **Выполнение обязательств** — сохранить notices, опубликовать исходники если требуется, указать авторство.
5. **Документирование** — зафиксировать всё для аудита.

### 4.3 SBOM — Software Bill of Materials

SBOM — это список всех компонентов программного продукта с указанием версий и лицензий. Аналог состава на упаковке продукта в магазине.

Зачем нужен SBOM:
- compliance: знаешь лицензии всех зависимостей
- безопасность: можно быстро проверить затронут ли продукт новой уязвимостью (пришёл CVE-2024-XXXX в библиотеке X — сразу видишь все продукты которые её используют)
- в США с 2021 года SBOM обязателен для ПО поставляемого федеральному правительству

Форматы SBOM: SPDX, CycloneDX, SWID.

---

## 5. Инструменты проверки лицензий

### 5.1 licensee

**licensee** — Ruby-гем (используется GitHub для определения лицензии репозитория). Сравнивает LICENSE-файл с базой известных лицензий.

```bash
# установить
gem install licensee

# проверить репозиторий
licensee detect /path/to/repo

# проверить конкретный файл
licensee detect --json LICENSE

# вывод:
# License:        MIT
# Match:          96%
# Confidence:     High
```

### 5.2 reuse (от FSFE)

**reuse** — инструмент от Free Software Foundation Europe. Проверяет соответствие спецификации REUSE которая требует SPDX-заголовки в каждом файле.

```bash
# установить
pip install reuse

# проверить репозиторий
reuse lint

# добавить заголовок к файлу
reuse addheader --year 2024 --copyright "Иванов Иван" --license MIT src/main.py

# скачать текст лицензии в LICENSES/
reuse download MIT
```

### 5.3 trivy (сканер уязвимостей и лицензий)

**trivy** — уже знакомый инструмент. Умеет не только уязвимости но и лицензии.

```bash
# сканировать файловую систему на лицензии
trivy fs --scanners license .

# сканировать Docker-образ
trivy image --scanners license nginx:alpine

# вывод покажет все найденные лицензии и компоненты
```

### 5.4 FOSSA

**FOSSA** — коммерческий SaaS-сервис для compliance, есть бесплатный tier для открытых проектов. Анализирует зависимости, строит граф лицензий, интегрируется с CI.

```bash
# установить CLI
curl -H 'Cache-Control: no-cache' https://raw.githubusercontent.com/fossas/fossa-cli/master/install-latest.sh | bash

# инициализировать проект
fossa init

# запустить анализ
fossa analyze

# проверить политику
fossa test
```

В CI (GitHub Actions) выглядит так:

```yaml
- name: FOSSA license check
  uses: fossas/fossa-action@main
  with:
    api-key: ${{ secrets.FOSSA_API_KEY }}
```

### 5.5 licensee в GitHub Actions (через licensee-action)

```yaml
name: License Check
on: [push, pull_request]

jobs:
  license:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Check license
        uses: actions/setup-ruby@v1
        with:
          ruby-version: '3.1'

      - run: gem install licensee
      - run: licensee detect --json
```

---

## 6. Совместимость лицензий — краткая шпаргалка

Не все лицензии можно смешивать. Самая частая проблема: нельзя взять GPL-код и включить его в MIT-проект который планируется закрыть.

```
Использую в проекте с лицензией X код под лицензией Y — можно?

         MIT  Apache-2.0  LGPL  GPL-2  GPL-3  AGPL
MIT       да     да        да    да     да     да
Apache-2  да     да        да    нет*   да     да
LGPL      да     да        да    да     да     да
GPL-2     да     нет*      да    да     нет*   нет
GPL-3     да     да        да    нет*   да     нет*
AGPL      да     да        да    нет*   да     да

* — технически несовместимы, нужна коммерческая лицензия или замена компонента
```

Правило большого пальца: разрешительные (MIT, Apache, BSD) совместимы со всем. GPL несовместима с Apache-2.0 v2 (из-за патентных условий). GPL-2 и GPL-3 несовместимы между собой.

---

## 7. Как это связано с нормативкой

Тема 2.4 программы прямо называется "Защита программ и данных от несанкционированного копирования". Лицензирование — это легальный инструмент этой защиты.

| Аспект | Нормативная база | Инструмент |
|--------|-----------------|-----------|
| Авторское право на ПО | Часть IV ГК РФ, ст. 1259 | Лицензия в репозитории |
| Защита от копирования | ст. 1270 ГК РФ | Проприетарная лицензия, DRM |
| Использование open-source | ст. 1286 ГК РФ (лицензионный договор) | SPDX, SBOM, compliance |
| SBOM для госзакупок | ФСТЭК — требования к составу СЗИ | SPDX-файл в составе документации |

---

# ЧАСТЬ 2. ПРАКТИЧЕСКАЯ РАБОТА

---

## Цель

Разобраться с лицензированием на практике: добавить LICENSE в репозиторий, проставить SPDX-заголовки в файлы, установить и запустить инструменты проверки лицензий, настроить проверку в GitHub Actions CI.

---

## Подготовка (5 мин)

Создайте папку практики:

```bash
cd ~/security-course
mkdir -p pr10/screens
touch pr10/report.md
```

Установите нужные инструменты:

```bash
# pip для Python-инструментов
sudo apt-get install -y python3-pip ruby rubygems

# reuse от FSFE
pip3 install reuse --user
export PATH="$HOME/.local/bin:$PATH"

# licensee
sudo gem install licensee

# проверьте установку
reuse --version
licensee version
```

Убедитесь что находитесь в репозитории:

```bash
cd ~/security-course
git status
```

---

## Часть 1. Добавляем LICENSE в репозиторий (15 мин)

### 1.1 Выбираем лицензию

Сначала решите под какой лицензией будет ваш учебный репозиторий. Для учебных проектов обычно выбирают MIT или Apache-2.0.

В отчёте ответьте на вопросы:
- Какую лицензию вы выбираете и почему?
- Если кто-то возьмёт ваш код и встроит в коммерческий продукт — что требует выбранная лицензия?
- Может ли кто-то взять ваш код, ничего не менять и продавать?

### 1.2 Добавляем файл LICENSE

Самый простой способ — через GitHub. Но мы сделаем руками чтобы понять что происходит.

Шаг 1. Создайте файл LICENSE в корне репозитория. Текст MIT лицензии (замените год и имя):

```bash
cat > ~/security-course/LICENSE << 'EOF'
MIT License

Copyright (c) 2024 ВАШЕ ИМЯ

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
EOF
```

Если хотите Apache-2.0 — скачайте с официального сайта:

```bash
curl -s https://www.apache.org/licenses/LICENSE-2.0.txt > ~/security-course/LICENSE
```

Шаг 2. Проверьте что licensee распознаёт вашу лицензию:

```bash
cd ~/security-course
licensee detect .
```

Сделайте скриншот. В отчёте: какой процент совпадения (Match) показывает licensee? Что означает этот процент?

Шаг 3. Закоммитьте:

```bash
git add LICENSE
git commit -m "license: add MIT license"
git push origin main
```

Шаг 4. Откройте репозиторий на GitHub. На главной странице должен появиться бейдж с названием лицензии рядом с количеством звёзд. Сделайте скриншот.

В отчёте: откуда GitHub знает какая лицензия у репозитория?

---

## Часть 2. SPDX-заголовки в файлах (20 мин)

### 2.1 Изучаем спецификацию REUSE

REUSE — спецификация которая требует чтобы каждый файл содержал информацию о лицензии и авторских правах. Это решает проблему: LICENSE в корне говорит о лицензии репозитория в целом, но не о каждом конкретном файле. Что если один файл под MIT а другой под Apache?

Три правила REUSE:
1. Каждый файл с исходным кодом должен содержать SPDX-заголовок
2. Полные тексты всех используемых лицензий лежат в папке LICENSES/
3. Все copyright notices указаны в заголовках файлов

### 2.2 Добавляем SPDX-заголовки вручную

Выберите несколько файлов из предыдущих практик и добавьте заголовки:

```bash
# посмотрим что есть в репозитории
find ~/security-course -name "*.md" -o -name "*.sh" -o -name "*.py" | head -10
```

Для Markdown-файла (HTML-комментарий в начале):

```bash
# открываем файл и добавляем в самое начало
head -3 ~/security-course/pr09/report.md
```

Для shell-скрипта добавьте после shebang-строки:

```bash
cat > /tmp/example-with-spdx.sh << 'EOF'
#!/bin/bash
# SPDX-FileCopyrightText: 2024 Иванов Иван <ivan@example.com>
# SPDX-License-Identifier: MIT

echo "Hello from a properly licensed script"
EOF
cat /tmp/example-with-spdx.sh
```

### 2.3 Используем reuse для добавления заголовков

Инструмент reuse умеет добавлять заголовки автоматически:

```bash
cd ~/security-course

# скачать текст лицензии в папку LICENSES/ (требование спецификации)
reuse download MIT

# посмотрите что появилось
ls LICENSES/

# добавить заголовок к конкретному файлу
reuse addheader \
  --year 2024 \
  --copyright "Ваше Имя <your@email.com>" \
  --license MIT \
  pr10/report.md

# посмотрите начало файла
head -5 pr10/report.md
```

Добавьте заголовки ещё к нескольким файлам:

```bash
# добавить к нескольким файлам из pr09
reuse addheader \
  --year 2024 \
  --copyright "Ваше Имя <your@email.com>" \
  --license MIT \
  pr09/report.md
```

Сделайте скриншот файла с заголовком.

### 2.4 Проверяем соответствие спецификации REUSE

```bash
cd ~/security-course
reuse lint
```

Скорее всего получите список файлов без заголовков — это нормально, у нас большой репозиторий и не все файлы охвачены.

В отчёте: сколько файлов без заголовков нашёл reuse? Все ли из них реально нуждаются в заголовке (подсказка: бинарники, картинки, автогенерированные файлы)?

Исправьте ошибки для файлов которые создавали на этой практике:

```bash
# добавить заголовки ко всем md-файлам в pr10
for f in pr10/*.md; do
  reuse addheader --year 2024 --copyright "Ваше Имя <your@email.com>" --license MIT "$f"
done

# запустить lint снова
reuse lint
```

Сделайте скриншот вывода reuse lint. В отчёте: что означает сообщение "Congratulations! Your project is compliant with version X of the REUSE Specification"?

---

## Часть 3. Проверяем лицензии зависимостей (15 мин)

В реальном проекте важно знать не только свою лицензию но и лицензии всего что используешь.

### 3.1 Создаём тестовый Python-проект

```bash
mkdir -p ~/security-course/pr10/demo-project
cd ~/security-course/pr10/demo-project

# создаём файл зависимостей
cat > requirements.txt << 'EOF'
requests==2.31.0
flask==3.0.0
numpy==1.26.0
EOF

# создаём простой скрипт
cat > app.py << 'EOF'
# SPDX-FileCopyrightText: 2024 Ваше Имя
# SPDX-License-Identifier: MIT

import requests
print("demo app")
EOF
```

### 3.2 Проверяем лицензии через pip-licenses

```bash
# устанавливаем инструмент
pip3 install pip-licenses --user

# устанавливаем зависимости (в виртуальном окружении)
python3 -m venv /tmp/demo-venv
source /tmp/demo-venv/bin/activate
pip install requests flask numpy
pip-licenses --format=markdown
pip-licenses --format=markdown --with-urls
deactivate
```

Сделайте скриншот таблицы лицензий. В отчёте:
- Под какими лицензиями зависимости?
- Есть ли среди них GPL или AGPL? Что это означало бы для коммерческого закрытого проекта?

### 3.3 Проверяем лицензии в Docker-образе через trivy

```bash
# сканируем образ на лицензии
docker run --rm aquasec/trivy:latest image --scanners license --severity UNKNOWN,HIGH python:3.11-slim 2>/dev/null | head -50
```

Примечание: загрузка триви занимает минуту. Если Docker не установлен — пропустите этот шаг.

Сделайте скриншот. В отчёте: какие лицензии нашёл trivy в базовом образе Python?

### 3.4 Проверяем лицензию в репозитории через licensee

```bash
cd ~/security-course

# детальная проверка
licensee detect --json . | python3 -m json.tool

# только название
licensee detect .
```

В отчёте: что такое "confidence" в выводе licensee? Почему он не всегда 100%?

---

## Часть 4. CI-проверка лицензий в GitHub Actions (20 мин)

Настроим автоматическую проверку лицензий при каждом push.

### 4.1 Создаём workflow для licensee

```bash
cd ~/security-course
mkdir -p .github/workflows

cat > .github/workflows/license-check.yml << 'EOF'
name: License Check

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  licensee:
    name: Check repository license
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install licensee
        run: gem install licensee

      - name: Detect license
        run: |
          licensee detect .
          echo "License check passed"

  reuse:
    name: REUSE compliance check
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: REUSE compliance check
        uses: fsfe/reuse-action@v3
EOF
```

### 4.2 Создаём workflow для проверки лицензий зависимостей

```bash
cat > .github/workflows/dependency-licenses.yml << 'EOF'
name: Dependency License Check

on:
  push:
    paths:
      - '**/requirements.txt'
      - '**/package.json'
      - '**/*.lock'

jobs:
  check-licenses:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install pip-licenses
        run: pip install pip-licenses

      - name: Check if requirements.txt exists
        id: check_req
        run: |
          if [ -f "pr10/demo-project/requirements.txt" ]; then
            echo "exists=true" >> $GITHUB_OUTPUT
          fi

      - name: Install dependencies and check licenses
        if: steps.check_req.outputs.exists == 'true'
        run: |
          pip install -r pr10/demo-project/requirements.txt
          echo "## Dependency Licenses" >> $GITHUB_STEP_SUMMARY
          pip-licenses --format=markdown >> $GITHUB_STEP_SUMMARY

      - name: Check for GPL licenses in dependencies
        if: steps.check_req.outputs.exists == 'true'
        run: |
          if pip-licenses | grep -E "GNU General Public License|AGPL"; then
            echo "WARNING: GPL/AGPL licensed dependencies found!"
            echo "Review required for commercial use"
          else
            echo "No copyleft licenses found in dependencies"
          fi
EOF
```

### 4.3 Закоммитьте и проверьте что CI запустился

```bash
cd ~/security-course
git add .github/ LICENSES/ pr10/
git commit -m "pr10: add license check CI workflows and SPDX headers"
git push origin main
```

Откройте вкладку Actions на GitHub. Дождитесь завершения workflow.

Сделайте скриншот результата выполнения workflows. В отчёте:
- Какой статус у каждого job?
- Что произойдёт если кто-то добавит GPL-зависимость в requirements.txt?

---

## Часть 5. Анализ совместимости лицензий (10 мин)

Ответьте на вопросы в отчёте. Используйте таблицу совместимости из лекции:

Сценарий 1: Ваш проект под MIT. Вы хотите использовать библиотеку под GPL-3.0. Можно ли включить её в закрытый коммерческий продукт?

Сценарий 2: Ваш проект под Apache-2.0. Вы хотите использовать библиотеку под MIT. Можно?

Сценарий 3: Компания хочет взять ваш MIT-код и продавать его как часть закрытого продукта без указания авторства. Нарушает ли это MIT?

Сценарий 4: Вы нашли библиотеку под AGPL-3.0. Хотите использовать её как backend для SaaS-сервиса не раскрывая исходники своего сервиса. Можно?

Сценарий 5: В репозитории нет файла LICENSE вообще. Можно ли использовать этот код в своём проекте?

---

## Часть 6. Оформление отчёта и публикация (5 мин)

Заполните pr10/report.md по шаблону:

```
# ПР №10. Лицензирование и open-source compliance

## 1. Выбор лицензии

Выбранная лицензия: MIT / Apache-2.0 (нужное оставить)
Тип: разрешительная / копилефтная

Что разрешает: ...
Что требует от пользователей кода: ...
Может ли кто-то продавать ваш код: ...

## 2. Результат licensee detect

Вывод команды:
(вставить)

Что означает Match %: ...
Откуда GitHub знает какая у репозитория лицензия: ...

## 3. SPDX-заголовки

Пример заголовка который добавили в файл:
(вставить)

Что означает каждая строка:
- SPDX-FileCopyrightText: ...
- SPDX-License-Identifier: ...

Результат reuse lint:
(вставить или описать)

## 4. Лицензии зависимостей

Таблица из pip-licenses:
(вставить)

Есть ли GPL/AGPL зависимости: ...
Что это означало бы для коммерческого проекта: ...

## 5. CI-проверка

Ссылка на запущенный workflow: https://github.com/...
Статус licensee job: ...
Статус reuse job: ...

Что произойдёт если добавить GPL-зависимость: ...

## 6. Анализ совместимости

Сценарий 1 (MIT + GPL в коммерческом): ...
Сценарий 2 (Apache-2.0 + MIT): ...
Сценарий 3 (MIT без указания авторства): ...
Сценарий 4 (AGPL в SaaS): ...
Сценарий 5 (нет LICENSE): ...

## 7. Связь с нормативкой

| Аспект | Статья ГК РФ | Как это реализуется |
|--------|-------------|-------------------|
| Авторское право на ПО | ст. 1259 | ... |
| Защита от копирования | ст. 1270 | ... |
| Использование open-source | ст. 1286 | ... |

## Выводы

(что узнали, что было неожиданным)
```

Закоммитьте:

```bash
cd ~/security-course
git add pr10/
git commit -m "pr10: complete license compliance report"
git push origin main
```

---

## Контрольные вопросы

1. Чем разрешительная лицензия (MIT) отличается от копилефтной (GPL)? Что значит "вирусный эффект" GPL?

2. Что такое SPDX-идентификатор? Зачем нужна машиночитаемая форма лицензии?

3. Компания взяла open-source библиотеку под GPL-2.0 и встроила в закрытый коммерческий продукт. Что она нарушила и каковы последствия?

4. Что такое SBOM и зачем он нужен с точки зрения как compliance, так и безопасности?

5. Репозиторий на GitHub без LICENSE-файла. Можно ли использовать код из него в своём проекте? Обоснуйте.

6. Чем отличается AGPL от обычной GPL? Какую "лазейку" она закрывает?

7. Зачем проверять лицензии зависимостей в CI, а не один раз при добавлении библиотеки?

---

## Критерии оценки

| Критерий | Баллы | Комментарий |
|----------|-------|-------------|
| LICENSE добавлен в репозиторий, licensee detect показывает совпадение | 2 | Скриншот вывода licensee |
| На GitHub появился бейдж с лицензией, скриншот | 1 | Главная страница репозитория |
| SPDX-заголовки в минимум 3 файлах, скриншот | 2 | С FileCopyrightText и License-Identifier |
| LICENSES/ папка создана, reuse lint запущен | 2 | Скриншот вывода reuse lint |
| pip-licenses или trivy: таблица лицензий зависимостей | 2 | В отчёте с анализом |
| GitHub Actions workflow создан и запущен | 3 | Ссылка + скриншот результата |
| Пять сценариев совместимости разобраны в отчёте | 3 | С обоснованием каждого |
| Таблица нормативной базы в отчёте | 1 | Минимум 3 строки |
| Коммит с понятным сообщением, структура pr10/ | 1 | Скриншоты в pr10/screens/ |

Итого: 17 баллов. Оценка: 15-17 отлично, 11-14 хорошо, 7-10 удовлетворительно.

---

## Шпаргалка

| Команда | Что делает |
|---------|-----------|
| licensee detect . | Определить лицензию репозитория |
| licensee detect --json . | То же в JSON-формате |
| reuse lint | Проверить REUSE-соответствие проекта |
| reuse addheader --year 2024 --copyright "Имя" --license MIT файл | Добавить SPDX-заголовок в файл |
| reuse download MIT | Скачать текст лицензии в LICENSES/ |
| pip-licenses --format=markdown | Таблица лицензий установленных пакетов |
| pip-licenses --with-urls | То же с ссылками на лицензии |
| trivy image --scanners license IMAGE | Лицензии компонентов Docker-образа |
| trivy fs --scanners license . | Лицензии в файловой системе |

---

## Полезные ссылки

- spdx.org/licenses -- полный список SPDX-идентификаторов
- reuse.software -- спецификация REUSE и документация
- choosealicense.com -- помогает выбрать лицензию (от GitHub)
- tldrlegal.com -- лицензии простым языком, без юридического текста
- fossa.com -- коммерческий compliance-сервис
- github.com/fossas/fossa-cli -- FOSSA CLI
- spdx.github.io/spdx-spec -- спецификация SPDX
