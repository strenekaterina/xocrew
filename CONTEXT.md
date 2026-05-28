# XO Crew — контекст проекта (обновлено 2026-05-28)

## Сайт и деплой
- **URL**: https://xocrew.ru
- **Хостинг**: Amvera PaaS — автоматически деплоит из GitHub при каждом пуше в `main`
- **GitHub**: `https://github.com/strenekaterina/xocrew.git`
- **Токен**: хранится только локально, НЕ коммитить в репо (GitHub Push Protection заблокирует пуш)
- **Основной файл**: `/Users/egorkurakin/Documents/Claude/Projects/Дизайнер/xocrew/index.html`
- **Бэкап**: `/Users/egorkurakin/Documents/Claude/Projects/Дизайнер/xocrew-backup-2026-05-28/`

## Как пушить на GitHub (через /tmp — ОБЯЗАТЕЛЬНО)
FUSE-mount блокирует git lock-файлы в примонтированной папке, поэтому git работает только из /tmp.

```bash
# Путь в bash к рабочей папке:
# /Users/egorkurakin/Documents/Claude/Projects/Дизайнер/xocrew/
# = /sessions/<session-name>/mnt/Дизайнер/xocrew/   (имя сессии меняется каждый раз!)

# Шаг 1 — клонировать (если /tmp/xocrew_push не существует):
git clone https://strenekaterina:[TOKEN]@github.com/strenekaterina/xocrew.git /tmp/xocrew_push

# Шаг 2 — скопировать изменённые файлы:
cp /sessions/СЕССИЯ/mnt/Дизайнер/xocrew/index.html /tmp/xocrew_push/
cp /sessions/СЕССИЯ/mnt/Дизайнер/xocrew/*.webp /tmp/xocrew_push/
cp /sessions/СЕССИЯ/mnt/Дизайнер/xocrew/*.svg /tmp/xocrew_push/   # НЕ ЗАБЫВАТЬ SVG!

# Шаг 3 — коммит и пуш:
cd /tmp/xocrew_push
git config user.email "m82753827@gmail.com"
git config user.name "Мишка"
git add -A
git commit -m "описание"
git push origin main
```

⚠️ **ВАЖНО**: Всегда копировать ВСЕ типы файлов — SVG, WebP, HTML. В прошлом забыли SVG → логотип и заголовок не загрузились на сайте.

## CSS-переменные и шрифты
```css
--orange: #FF5200;   /* точный hex — не #FF5500 и не #f60 */
--dark:   #0D0D0D;
--white:  #FFFFFF;
--blue:   #1929C2;
--f-head: 'Sprite Graffiti', sans-serif;   /* граффити-шрифт для заголовков */
--f-body: 'Neutral Face', 'Inter', sans-serif;
```

## Классы заголовков секций
```css
.what-title     { font-size: clamp(95px, 14vw, 182px); }   /* большой, 1 строка */
.what-title-md  { font-size: clamp(68px, 9.5vw, 120px); }  /* средний, 2 строки */
```
Если заголовок в 2 строки — использовать `what-title what-title-md` вместе.
JS-функция `fitHowTitle()` удалена — больше не используется.

## Изображения — правило вставки (КРИТИЧНО)

### Плейсхолдеры без фиксированной высоты (`.what-photo`, flex-колонки)
**ВСЕГДА использовать `.ph` div с `background-image`** — НЕ тег `<img>`!
- `<img height:100%>` растягивает flex-контейнер по своей натуральной высоте → появляется лишний отступ
- `background-image` на div без intrinsic-размера → заполняет контейнер без изменения его размера

```html
<div class="ph" style="background:#1a1a2e url('photo.webp') center/cover no-repeat;"></div>
```

### Контейнеры с `aspect-ratio` (`.curator-photo`)
Можно background-image — предпочтительно для единообразия:
```html
<div class="curator-photo" style="background:url('kirill-photo.webp') top center/cover no-repeat;"></div>
```

### Контейнеры с `aspect-ratio: 16/7` (блок организаторов — team photo)
Здесь `<img>` допустим:
```html
<img src="team-photo.webp" alt="Команда XO Crew" style="width:100%;height:100%;object-fit:cover;object-position:center;display:block;">
```

## Сжатие изображений перед вставкой
Всегда сжимать через ImageMagick перед embed:
```bash
# Полноширинные (hero, секции на всю ширину):
convert "$SRC" -resize 1920x -quality 82 "$DST.webp"

# Полуширинные (плейсхолдеры рядом с текстом):
convert "$SRC" -resize 960x -quality 82 "$DST.webp"

# Миниатюры / кураторы:
convert "$SRC" -resize 480x -quality 82 "$DST.webp"
```

## Текущие WebP-файлы в репозитории
| Файл | Секция | Размер |
|------|--------|--------|
| `what-photo.webp` | ЧТО? | 126 KB |
| `why-photo.webp` | ПОЧЕМУ? | 177 KB |
| `for-whom-photo.webp` | ДЛЯ КОГО? | 151 KB |
| `per-topic-photo.webp` | ПО КАЖДОЙ ТЕМЕ | 84 KB |
| `how-photo.webp` | 30-40 МИН В ДЕНЬ | 109 KB |
| `why-join-photo.webp` | ЗАЧЕМ? | 200 KB |
| `chto-chto-photo.webp` | ЧТОБЫ ЧТО? | 82 KB |
| `where-photo.webp` | ГДЕ? | 34 KB |
| `when-photo.webp` | КОГДА? | 147 KB |
| `kirill-photo.webp` | Куратор Кирилл | 79 KB |
| `artem-photo.webp` | Куратор Артём | 30 KB |
| `team-photo.webp` | Организаторы (команда) | 258 KB |

## SVG-файлы в репозитории
- `zagolovok.svg` — заголовок hero (БРЕЙК-РОДИТЕЛЬ). Цвет `.cls-1 { fill: #FF5200; }`
- `logo-f.svg` — логотип в навбаре
- `logo-mobile.svg` — логотип мобильный
- `Line.svg` — декоративная линия в секциях
- `star.svg` — звёздочка (background в CSS)
- `брейк-родитель.svg` — альтернативный вариант (не используется)

## Секции (в порядке на странице)
- `#hero` — главный экран, таймер обратного отсчёта
- `#what` `.why-sec` — ЧТО? | `.what-photo` = плейсхолдер + `.what-content`
- `#why` `.why-sec` — ПОЧЕМУ? | `.what-photo` = 6 карточек flex-column (01–06)
- `#for-whom` — ДЛЯ КОГО? | `.what-photo` = 2×2 grid (01–04)
- `#program` — ПРОГРАММА | SVG viewBox="0 0 1000 420"
- `#how` — 30–40 МИНУТ В ДЕНЬ | `.orange-left` секция, заголовок оранжевый
- `#per-topic` — ПО КАЖДОЙ ТЕМЕ | тёмная секция
- `#why-join` — ЗАЧЕМ?
- `#chto-chto` — ЧТОБЫ ЧТО?
- `#pricing` — СКОЛЬКО? | кнопки `<button onclick="openModal()">` (НЕ ссылки на VK!)
- `.pair-sec` — ГДЕ? + КОГДА? | два `.pair-col` на тёмном фоне
- `#curators` — КУРАТОРЫ
- `#organizers` — ОРГАНИЗАТОРЫ
- `#footer` — подвал

## Блок ГДЕ?/КОГДА?
**Левая колонка (ГДЕ?):** `padding-top:10vh; padding-bottom:10vh;`
- Иконка VK и замочек удалены
- Лейбл "Формат" → "Онлайн. Закрытый канал VK с последующим\<br\>доступом ко всем материалам."
- Лейбл "Доступность" → "Участвовать можно из любой точки мира."

**Правая колонка (КОГДА?):** `padding-top:10vh; padding-bottom:0;`
- Текст "1 месяц..." имеет `padding-top:28px` (выравнивание с "Онлайн." слева)
- Даты в 2-колонночном гриде внизу: "1 Июля" / "1 Августа"

## Модальное окно (тёмный стиль)
```css
.modal-box {
  background: var(--dark);
  background-image: linear-gradient(rgba(255,255,255,.07) 1px, transparent 1px),
                    linear-gradient(90deg, rgba(255,255,255,.07) 1px, transparent 1px);
  background-size: 40px 40px;
  border: 1px solid rgba(255,255,255,.12);
  border-radius: 20px;
  padding: 48px 48px 44px;
  max-width: 760px;
  box-shadow: 0 24px 80px rgba(0,0,0,.7);
}
```
- Карточки с ценами (`.m-opt`) — сетка убрана, фон сплошной `#1a1a1a`
- `.m-title`, `.m-sub` — `text-align:center`
- "Можно оформить в рассрочку" — `color:rgba(255,255,255,.5)`, строчными
- JS: `openModal()`, `closeModal()`, `goToBot()` → VK-бот

## Сетки на тёмных секциях
```css
background-image: linear-gradient(rgba(255,255,255,.08) 2px, transparent 2px),
                  linear-gradient(90deg, rgba(255,255,255,.08) 2px, transparent 2px);
background-size: 60px 60px;
```

## Известные ошибки и решения

### 1. `<img>` в flex-контейнере без фиксированной высоты
**Симптом**: появляется лишний отступ снизу от карточки/плейсхолдера.
**Причина**: `<img height:100%>` требует родителя с явно заданной высотой; flex растягивается по натуральному размеру картинки.
**Решение**: `.ph` div с `background-image: url(...) center/cover no-repeat`.

### 2. FUSE-mount блокирует git
**Симптом**: `error: cannot lock ref`, `unable to create .git/index.lock`.
**Причина**: sandbox монтирует папку проекта через FUSE, которая не поддерживает file locks.
**Решение**: клонировать репо в `/tmp/xocrew_push` и работать оттуда.

### 3. GitHub Push Protection блокирует пуш
**Симптом**: `remote: Push cannot contain secrets` — если в коммите есть GitHub PAT токен.
**Причина**: токен попал в CONTEXT.md (строки с `ghp_...`).
**Решение**: заменить токен на `[TOKEN_REDACTED]` в файле, новый коммит, пуш.
**Правило**: токен хранить ТОЛЬКО локально, никогда не коммитить.

### 4. SVG-файлы не загружаются на сайте
**Симптом**: логотип и заголовок hero показываются как broken image.
**Причина**: при пуше скопировали только `index.html` и `*.webp`, забыли `*.svg`.
**Решение**: всегда копировать все типы файлов (`*.svg`, `*.webp`, `index.html`).

### 5. Незавершённый git merge в /tmp
**Симптом**: `fatal: You have not concluded your merge (MERGE_HEAD exists)`.
**Решение**: `git merge --abort` или удалить `/tmp/xocrew_push` и клонировать заново.

## Мобильный адаптив — ключевые размеры (≤ 820px)

### Hero секция
- Логотип nav: `logo-f.svg`, высота `40px`
- Hero-заголовок (центр): `logo-new.svg` (1003×547 viewBox), `max-width: 640px; width: 100%`
- Описание `.hero-sub`: `font-size: clamp(14px, 4.2vw, 16px); letter-spacing: .025em`
- Сетка фона `.hero-top`: `background-size: 28px 28px`
- Кнопка nav `.nav-cta`: `grid-column: 3; padding: 0 20px; font-size: 14px`

### Таймер `.hero-bottom`
- Layout: `flex-direction: row; flex-wrap: wrap; padding: 16px 24px 24px`
- Метки "ДО СТАРТА" (left) и "СТАРТ 1 ИЮЛЯ" (right): `order: 1/2; font-size: 10px`
- Цифры `.cd-wrap`: `order: 3; flex: 0 0 100%`
- `.cd-row`: `width: 100%; justify-content: space-between`
- Шрифт `.cd-n` / `.cd-sep`: `clamp(42px, 13.5vw, 58px)`

### Секция ОРГАНИЗАТОРЫ `#team`
- Заголовок `.team-title`: `clamp(50px, 16vw, 82px)` — до края ширины (Sprite Graffiti ~0.43em/char × 12 chars = ~307px @ 375px)
- Описание `.team-desc-text`: `clamp(13px, 3.5vw, 15px) !important; word-wrap: break-word`

### Фото-плейсхолдеры `.what-photo`
- Нужен ЯВНЫЙ `height` (не `min-height`!), иначе `.ph { height:100% }` не работает
- Стандарт: `height: 60vw` для большинства секций, `height: 52vw` для #where/#when

## Последний коммит
`81ccc56` — "fix: mobile .what-photo needs explicit height"

## Все выполненные правки (хронологически)
1. Восстановлены секции из браузерного кэша
2. #why — 6 карточек вместо списка
3. #for-whom — 2×2 грид вместо списка
4. #program — SVG с реалистичной фигурой
5. Кнопки стоимости → openModal() вместо ссылки на VK
6. Модальное окно расширено (760px), увеличены отступы
7. Текст модалки выровнен по центру, рассрочка строчными
8. КОГДА? — даты в 2-колонночном гриде
9. Удалены иконка VK и замочек из ГДЕ?
10. Добавлены `<br>` в текстах ГДЕ? и КОГДА?
11. Выравнивание правой колонки: padding-top:10vh + padding-top:28px на тексте
12. `zagolovok.svg` — цвет исправлен на `#FF5200`
13. "30–40 МИНУТ В ДЕНЬ" → класс `what-title-md`, удалена JS-функция fitHowTitle
14. Модальное окно — тёмный стиль с мелкой сеткой
15. Карточки цен в модалке — убрана сетка, сплошной тёмный фон
16. "Рассрочку" — белый цвет текста
17. Добавлены фотографии во все плейсхолдеры (12 WebP-файлов, сжатые)
18. Все плейсхолдеры переведены на background-image паттерн
19. Созданы кураторские фото (Кирилл, Артём) через background на .curator-photo
20. Бэкап версии в xocrew-backup-2026-05-28/
21. Деплой на xocrew.ru (коммит 7024232)
22. Мобильный адаптив — полный рефакторинг @media (max-width: 820px)
23. Hero: новый логотип (logo-new.svg) как заголовок, таймер вертикальный стек, описание с mob-br
24. ОРГАНИЗАТОРЫ: заголовок 21vw до края, описание 3.5vw + word-wrap
25. Фото-плейсхолдеры: height вместо min-height для .what-photo
