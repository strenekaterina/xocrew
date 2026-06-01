# XO Crew — контекст проекта (обновлено 2026-05-29)

## Сайт и деплой
- **URL**: https://xocrew.ru
- **Хостинг**: Amvera PaaS — автоматически деплоит из GitHub при каждом пуше в `main`
- **GitHub**: `https://github.com/strenekaterina/xocrew.git`
- **Токен**: хранится только локально, НЕ коммитить в репо (GitHub Push Protection заблокирует пуш)
- **Основной файл**: `/Users/egorkurakin/Documents/Claude/Projects/Дизайнер/xocrew/index.html`
- **Бэкап**: `/Users/egorkurakin/Documents/Claude/Projects/Дизайнер/xocrew-backup-2026-05-28/`

## Как пушить на GitHub (через /tmp — ОБЯЗАТЕЛЬНО)
FUSE-mount блокирует git lock-файлы в примонтированной папке, поэтому git работает только из /tmp.
**Рабочий клон для git**: `/tmp/xo_push3` (нужно клонировать заново при каждом старте сессии — /tmp не персистентен)

```bash
# Клонировать (токен хранится только локально, не коммитить!):
git clone https://strenekaterina:[TOKEN]@github.com/strenekaterina/xocrew.git /tmp/xo_push3

# Пуш изменений:
cp "/Users/egorkurakin/Documents/Claude/Projects/Дизайнер/xocrew/index.html" /tmp/xo_push3/index.html
cd /tmp/xo_push3
git add index.html
git commit -m "описание"
git push origin main
```

⚠️ **ВАЖНО**: При пуше новых медиафайлов копировать ВСЕ типы — SVG, WebP, HTML. В прошлом забыли SVG → логотип и заголовок не загрузились на сайте.

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

## Мобильный стандарт заголовков секций (≤ 820px)
```css
/* Большинство секций: */
font-size: clamp(52px, 14vw, 80px) !important;
/* Если базовый CSS стоит ПОСЛЕ media query — обязательно !important */
```

## Изображения — правило вставки (КРИТИЧНО)

### Плейсхолдеры без фиксированной высоты (`.what-photo`, flex-колонки)
**ВСЕГДА использовать `.ph` div с `background-image`** — НЕ тег `<img>`!
```html
<div class="ph" style="background:#1a1a2e url('photo.webp') center/cover no-repeat;"></div>
```
На мобилке `.what-photo` нужен явный `height` (не `min-height`!):
- Стандарт: `height: 60vw` для большинства секций, `height: 52vw` для #where/#when

### Контейнеры с `aspect-ratio` (`.curator-photo`)
```html
<div class="curator-photo" style="background:url('kirill-photo.webp') top center/cover no-repeat;"></div>
```

## Сжатие изображений перед вставкой
```bash
convert "$SRC" -resize 1920x -quality 82 "$DST.webp"   # полноширинные
convert "$SRC" -resize 960x  -quality 82 "$DST.webp"   # полуширинные
convert "$SRC" -resize 480x  -quality 82 "$DST.webp"   # миниатюры/кураторы
```

## Текущие WebP-файлы в репозитории
| Файл | Секция |
|------|--------|
| `what-photo.webp` | ЧТО? |
| `why-photo.webp` | ПОЧЕМУ? |
| `for-whom-photo.webp` | ДЛЯ КОГО? |
| `per-topic-photo.webp` | ПО КАЖДОЙ ТЕМЕ |
| `how-photo.webp` | 30-40 МИН В ДЕНЬ |
| `why-join-photo.webp` | ЗАЧЕМ? |
| `chto-chto-photo.webp` | ЧТОБЫ ЧТО? |
| `where-photo.webp` | ГДЕ? |
| `when-photo.webp` | КОГДА? |
| `kirill-photo.webp` | Куратор Кирилл |
| `artem-photo.webp` | Куратор Артём |
| `team-photo.webp` | Организаторы |

## SVG-файлы в репозитории
- `zagolovok.svg` — заголовок hero (БРЕЙК-РОДИТЕЛЬ). Цвет `.cls-1 { fill: #FF5200; }`
- `logo-f.svg` — логотип в навбаре (десктоп)
- `logo-mobile.svg` — логотип мобильный (не используется, hero использует `logo-new.svg`)
- `logo-new.svg` — hero-заголовок на мобилке (1003×547), `max-width: 640px; width: 100%`
- `Line.svg` — декоративная линия "КАК ПОНЯТЬ:" (viewBox 0 0 701.44 104.1, соотношение 6.74:1)
- `star.svg` — звёздочка (background в CSS)

## Секции (в порядке на странице)
- `#hero` — главный экран, таймер обратного отсчёта
- `#what` — ЧТО?
- `#why` — ПОЧЕМУ? | 6 карточек flex-column (01–06) | `.why-label-wrap` с `Line.svg`
- `#for-whom` — ДЛЯ КОГО? | 2×2 grid
- `#program` + `.topics-sec` — ПРОГРАММА + темы (оба синие, единый фон `28px` на мобилке)
- `#how` — 30–40 МИНУТ В ДЕНЬ | `.orange-left` секция, заголовок `#how-title` вынесен из карточки
- `#per-topic` — ПО КАЖДОЙ ТЕМЕ
- `#why-join` — ЗАЧЕМ?
- `#chto-chto` — ЧТОБЫ ЧТО?
- `#pricing` — СКОЛЬКО? | swipe-слайдер на мобилке
- `.pair-sec` — ГДЕ? + КОГДА?
- `#curators` — КУРАТОРЫ | swipe-слайдер на мобилке
- `#organizers` — ОРГАНИЗАТОРЫ
- `#s-statement` — синий блок со стейтментом + ДЛЯ ЮРЛИЦ + ЗАБОТА
- `#footer` — подвал

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
- JS: `openModal()`, `closeModal()`, `goToBot()` → VK-бот

## Мобильный адаптив — паттерны и решения (≤ 820px)

### КРИТИЧНО: базовый CSS после media query
Некоторые классы (`.topics-grid`, `.topic-desc`, `.topic-item`, `.wli-text` и др.) объявлены в базовом CSS ПОСЛЕ закрывающей скобки `@media` → перебивают мобильные стили. Правило: **всегда добавлять `!important`** для таких селекторов в mobile CSS.

### Swipe-слайдер (паттерн)
Применён в `#curators` и `#pricing`:
```css
/* Контейнер */
display: flex;
overflow-x: scroll;
overflow-y: hidden;
scroll-snap-type: x mandatory;
-webkit-overflow-scrolling: touch;
scrollbar-width: none;
padding: 0 calc(9vw - 6px) 4px;
scroll-padding-left: calc(9vw - 6px);   /* ← КЛЮЧЕВОЕ: без этого snap съедает отступ */
gap: 12px;
align-items: stretch;   /* равная высота карточек */

/* Карточка */
flex: 0 0 82vw;
scroll-snap-align: start;
```
Формула симметричного peek: карта 82vw → остаток 18vw − 12px gap → каждая сторона = `(18vw − 12px)/2 = 9vw − 6px`.
НЕ ставить `overflow: hidden` на родительскую секцию — мешает iOS Safari скроллить.

### Мобильные переносы текста (`.mob-br`)
```css
/* Базовый CSS (десктоп): */
.mob-br { display: none; }

/* @media (max-width: 820px): */
.mob-br { display: inline; }
```
⚠️ После `<br class="mob-br">` ВСЕГДА ставить пробел ` `, иначе на десктопе (где br скрыт) слова склеятся без пробела:
```html
<strong>3 дня</strong><br class="mob-br"> <strong>погружения,</strong>
```

### Line.svg (КАК ПОНЯТЬ:) на мобилке
```css
.why-label-line { left: -38px; height: 44px; top: 2px; }
```
SVG имеет встроенный прозрачный отступ слева → `left` должен быть заметно отрицательным.

### Заголовок #how на мобилке
`#how-title` вынесен ИЗ `.what-card`, теперь первый child `.orange-left`:
```html
<div class="orange-left">
  <h2 class="what-title what-title-md" id="how-title">30–40 МИНУТ<br>В ДЕНЬ</h2>
  <div class="what-card">...</div>
</div>
```
```css
#how-title { font-size: clamp(52px, 14vw, 80px) !important; text-align: center !important; color: var(--dark) !important; }
```

### Сетки на мобилке
Стандарт 28px для всех секций:
```css
background-size: 28px 28px !important;
```
Применено к: `#how`, `#program`, `.topics-sec`, `#s-statement` (через id).

### Ценовые карточки (#pricing) на мобилке
- Карточка: `flex: 0 0 82vw`
- `.pc-body`: `padding: 24px 16px 36px` (36px снизу — воздух перед кнопкой)
- Текст 3-го пункта:
  - БАЗА: `Доступ к записям<br class="mob-br"> на 6 месяцев`
  - РАСШИРЕННЫЙ: `Доступ к записям<br class="mob-br"> на 12 месяцев`

### Футер на мобилке
Всё отцентровано, Sprite Graffiti для описания и nav-ссылок:
```css
.ft-top { flex-direction: column; align-items: center; text-align: center; }
.ft-desc { font-family: var(--f-head) !important; font-size: clamp(18px, 5.5vw, 26px) !important; text-align: center; }
.ft-links { text-align: center !important; align-items: center !important; }
.ft-links a { font-family: var(--f-head) !important; font-size: clamp(18px, 5.5vw, 26px) !important; }
```

### JS — автозамена пробелов на &nbsp;
Скрипт внизу страницы обрабатывает `.team-desc-text` и другие классы, добавляя неразрывные пробелы после коротких предлогов ("и", "в", "к", "как" и т.д.). Это блокирует естественный перенос строк. Решение: использовать явные `<br class="mob-br">` теги.

## Известные ошибки и решения

### 1. `<img>` в flex-контейнере без фиксированной высоты
**Решение**: `.ph` div с `background-image`.

### 2. FUSE-mount блокирует git
**Решение**: клонировать репо в `/tmp/xocrew_push2` и работать оттуда.

### 3. GitHub Push Protection блокирует пуш
**Решение**: убрать токен из коммитируемых файлов, хранить ТОЛЬКО локально.

### 4. SVG-файлы не загружаются на сайте
**Решение**: при пуше копировать все типы файлов (`*.svg`, `*.webp`, `index.html`).

### 5. scroll-snap съедает padding-left в swipe-слайдере
**Решение**: `scroll-padding-left: calc(9vw - 6px)` на контейнере.

### 6. `<br class="mob-br">` склеивает слова на десктопе
**Решение**: ставить пробел после каждого mob-br тега.

### 7. Базовый CSS после media query перебивает мобильные стили
**Решение**: добавлять `!important` в mobile CSS для затронутых селекторов.

## Последний коммит
`96b201b` — "fix: orange links in legal section font-weight 900" (2026-06-01)
*(следующий пуш: накопленные правки сессии 2026-06-01)*

## Навбар — средний брейкпоинт (13-14" ноутбуки)
```css
@media (max-width: 1550px) and (min-width: 821px) {
  nav { grid-template-columns: auto 1fr 160px; padding: 0 0 0 12px; }
  .nav-links { gap: 20px; }
  .nav-links a { letter-spacing: 0.03em; }
  .nav-cta { font-size: 13px; letter-spacing: 0.04em; }
}
```
MacBook Air 13" = 1280px. НЕ использовать clamp — трогает все экраны.

## Мобильные fit-функции заголовков
Паттерн: бинарный поиск + `setProperty('font-size', X, 'important')` + НЕ сбрасывать `white-space`.
Добавлены в JS (все с guard > 820 / cleanup при десктопе):
- `repeatWhatTitle()` — "ЧТО? ЧТО?" до края (#what)
- `fitWhyMobileTitle()` — "ПОЧЕМУ?" до края (#why)
- `fitForWhomMobileTitle()` — "ДЛЯ КОГО?" до края (#for-whom) — nowrap обязателен (пробел в слове!)
- `fitPerTopicMobileTitle()` — "ПО КАЖДОЙ ТЕМЕ" / "ВЫ ПОЛУЧИТЕ:" 2 строки (#per-topic)
- `fitWhyJoinMobileTitle()` — "ЗАЧЕМ?" до края (#why-join)
- `fitChtoChtoMobileTitle()` — "ЧТОБЫ ЧТО?" до края (#chto-chto)
- `repeatWhereMobileTitle()` — "ГДЕ? ГДЕ?" до края (#where)
- `repeatWhenMobileTitle()` — "КОГДА? КОГДА?" до края (#when)

⚠️ `fitChtoTitle()` (десктопная) имеет guard `if (window.innerWidth <= 820) return` — без него перезаписывала мобильный размер.

## Все выполненные правки (хронологически)
1–25. [предыдущие сессии — см. git log]
26. #legal `.pair-item-text` → `font-size: 15px !important`
27. `.why-label-line` мобилка: `left: -38px; height: 44px; top: 2px`
28. `.topic-item` → `font-size: 15px !important` (добавлен `!important`, был без него)
29. `#format .fmt-desc` — мобильные переносы через `.mob-br`, JS-блокировка убрана
30. Ценовые карточки: `<br class="mob-br">` в 3-м пункте + "1 год" → "12 месяцев" + `padding-bottom: 36px`
31. `#s-statement` (синий блок стейтмент+юрлица+забота) → `background-size: 28px 28px` на мобилке
32. Футер на мобилке: всё по центру, Sprite Graffiti для описания и ссылок
33. fix: пробелы после mob-br чтобы слова не склеивались на десктопе
34. Open Graph + Twitter Card мета-теги добавлены в `<head>`
35. Навбар: средний брейкпоинт 821–1550px для MacBook Air 13"
36. `#per-topic` HTML: `ПО КАЖДОЙ ТЕМЕ ВЫ<br>ПОЛУЧИТЕ:` (ВЫ на той же строке что ТЕМЕ на 1280px)
37. Мобильные fit-функции для 8 заголовков секций (см. список выше)
