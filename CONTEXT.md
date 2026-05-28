# XO Crew — контекст проекта (обновлено 2026-05-17)

## Сайт и деплой
- **URL**: https://xocrew.ru
- **Хостинг**: Amvera PaaS — автоматически деплоит из GitHub при каждом пуше в `main`
- **GitHub**: `https://github.com/strenekaterina/xocrew.git`
- **Токен**: `[TOKEN_REDACTED]` (пользователь `strenekaterina`)
- **Основной файл**: `/Users/egorkurakin/Documents/Claude/Projects/Дизайнер/xocrew/index.html`

## Как пушить на GitHub (через /tmp, т.к. FUSE-mount блокирует git)
```bash
# Если /tmp/xocrew_push уже есть:
cp /sessions/serene-magical-keller/mnt/Дизайнер/xocrew/index.html /tmp/xocrew_push/index.html
cd /tmp/xocrew_push
git config user.email "m82753827@gmail.com"
git config user.name "Мишка"
git add index.html
git commit -m "описание"
git push origin main

# Если /tmp/xocrew_push не существует — сначала клонировать:
git clone https://strenekaterina:[TOKEN_REDACTED]@github.com/strenekaterina/xocrew.git /tmp/xocrew_push
```

## CSS-переменные
```css
--orange: #FF5500;
--dark:   #0D0D0D;
--white:  #FFFFFF;
--blue:   #1929C2;
--f-head: 'Bebas Neue', sans-serif;
--f-body: 'Inter', sans-serif;
```

## Секции (в порядке на странице)
- `#hero` — главный экран
- `#why` `.why-sec` — ПОЧЕМУ? | `.what-photo` = 6 карточек flex-column (01–06); `.what-content` с цитатой
- `#for-whom` — ДЛЯ КОГО? | `.what-photo` = 2×2 grid (01–04)
- `#how` — КАК?
- `#program` — ПРОГРАММА | SVG viewBox="0 0 1000 420", стрелки в (500,258)
- `#pricing` — СТОИМОСТЬ | кнопки `<button onclick="openModal()">` (НЕ ссылки на VK!)
- `.pair-sec` — ГДЕ? + КОГДА? | два `.pair-col` на тёмном фоне
- `#footer` — подвал

## Блок ГДЕ?/КОГДА? — текущее состояние

**Левая колонка (ГДЕ?):**
- `padding-top:10vh; padding-bottom:10vh;`
- Нет иконки VK и замочка (удалены)
- Лейбл "Формат" → `pair-item-head` → текст "Онлайн. Закрытый канал VK с последующим<br>доступом ко всем материалам."
- Лейбл "Доступность" → "Участвовать можно из любой точки мира."

**Правая колонка (КОГДА?):**
- `padding-top:10vh; padding-bottom:0;`
- Текст "1 месяц..." имеет `padding-top:28px` (выравнивание с "Онлайн." слева)
- Даты в 2-колонночном гриде внизу: "1 Июля" / "1 Августа"

## Модальное окно
```css
.modal-box { padding:48px 48px 44px; max-width:760px; border-radius:20px; }
```
- `.m-title` и `.m-sub` — `text-align:center`
- Рассрочка строчными: `<p style="text-align:center;font-size:13px;...">Можно оформить в рассрочку</p>`
- JS: `openModal()`, `closeModal()`, `goToBot()` → VK-бот

## Последний коммит
`456da0f` — "fix: align КОГДА text with ГДЕ body text, increase block top padding"

## Все выполненные правки
1. Восстановлены секции из браузерного кэша (sections_cached.txt)
2. #why — 6 карточек вместо списка
3. #for-whom — 2×2 грид вместо списка
4. #program — новый SVG с реалистичной фигурой
5. Кнопки стоимости → openModal() вместо ссылки на VK
6. Модальное окно расширено (760px), увеличены отступы
7. Текст модалки выровнен по центру, рассрочка строчными
8. КОГДА? — даты в 2-колонночном гриде
9. Удалены иконка VK и замочек из ГДЕ?
10. Добавлены <br> в текстах ГДЕ? и КОГДА?
11. Выравнивание правой колонки: padding-top:10vh + padding-top:28px на тексте
