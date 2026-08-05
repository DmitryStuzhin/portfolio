# DESIGN.md — Stuzhin Portfolio

Спецификация визуального языка одностраничного сайта-визитки.
Сгенерирована по SKILL `web-design` (Phase B). Код в `index.html` обязан следовать этому документу.

Направление: **Dark Editorial Tech** · интерактивный уровень **L2** · языки RU / EN.

---

## 1. Visual Theme & Atmosphere

**Философия.** Сайт разработчика — это витрина, а не декларация. Работы клиента цветные, тёплые и шумные
(яхты на закате, дерево и глина, золото стоматологии). Значит интерфейс обязан быть **чёрной галерейной рамой**:
почти монохромный, с единственным тёплым акцентом, который родствен скриншотам, но не спорит с ними.

**Ключевые слова атмосферы:** ночная студия · крупный набор · янтарный свет · инженерная точность · тишина между блоками.

**Одна фраза:** «Тёмный зал, в котором подсвечены только работы и одна янтарная линия».

**Что это означает на практике**
- Цвет живёт в скриншотах, а не в UI. Интерфейс имеет ровно один хроматический акцент.
- Иерархия строится размером и воздухом, а не обводками и заливками.
- Каждое движение мотивировано: подсветка курсора, вход блока, лента-манифест. Ничего «просто чтобы дёргалось».

---

## 2. Color Palette & Roles

```css
:root{
  /* поверхности */
  --void:#08090c;        --void-rgb:8,9,12;         /* фон страницы */
  --base:#0b0d11;        --base-rgb:11,13,17;       /* вторичная полоса, футер */
  --panel:#111318;       --panel-rgb:17,19,24;      /* карточки */
  --panel-2:#171a20;     --panel-2-rgb:23,26,32;    /* карточка в hover, теги */

  /* линии */
  --line:rgba(255,255,255,.09);
  --line-2:rgba(255,255,255,.16);   /* hover / focus граница */

  /* текст */
  --ink:#f5f6f8;         --ink-rgb:245,246,248;     /* заголовки */
  --body:#d3d7de;        --body-rgb:211,215,222;    /* основной текст карточек */
  --muted:#a2a8b4;       --muted-rgb:162,168,180;   /* лиды, подписи */
  --dim:#6c727e;         --dim-rgb:108,114,126;     /* метки, номера, футер */

  /* акцент — единственный хроматический цвет системы */
  --amber:#ffb84d;       --amber-rgb:255,184,77;
  --amber-hi:#ffc970;                                /* hover-состояние акцента */
  --amber-dim:rgba(255,184,77,.14);                  /* заливки, glow */

  /* семантика */
  --ok:#7ddba3;          --ok-rgb:125,219,163;       /* «в продакшене» */
  --wip:#8fb4ff;         --wip-rgb:143,180,255;      /* «в разработке» */
}
```

**Роли**
| Роль | Токен |
|---|---|
| Фон страницы | `--void` |
| Карточка кейса / стека | `--panel`, граница `--line` |
| Заголовки H1–H3 | `--ink` |
| Проза, лиды | `--muted`; текст внутри карточек `--body` |
| Метки, номера секций, футер | `--dim` |
| CTA, активная навигация, ссылки на живые сайты, hover-границы | `--amber` |
| Статус «в продакшене» / «в разработке» | `--ok` / `--wip` |

**Правила**
- Ровно один хроматический акцент. Второй цвет вводить запрещено — исключение только статусные точки `--ok` / `--wip`, они не используются как декор.
- Ни одного hex в разметке. Только `var(--token)` и `rgba(var(--token-rgb), a)`.
- Контраст: `--ink` на `--void` = 17.4:1, `--muted` на `--void` = 8.1:1, `--dim` на `--void` = 4.6:1 (только для ≥12px uppercase-меток), `--amber` на `--void` = 11.2:1.

---

## 3. Typography Rules

```
@import url('https://fonts.googleapis.com/css2?family=Unbounded:wght@600;700&family=Onest:wght@400;500;600&family=JetBrains+Mono:wght@400;500&display=swap');
```

| Роль | Стек |
|---|---|
| Display (H1, H2, крупные числа) | `'Unbounded', 'Onest', ui-sans-serif, system-ui, sans-serif` |
| Body (всё остальное) | `'Onest', ui-sans-serif, system-ui, -apple-system, 'Helvetica Neue', sans-serif` |
| Mono (метки, теги, номера, ссылки) | `'JetBrains Mono', ui-monospace, SFMono-Regular, Menlo, monospace` |

**Все три шрифта обязаны иметь кириллицу.** Это жёсткое требование: сайт двуязычный RU/EN,
и подмена системным шрифтом на русском тексте — брак. `Space Grotesk`, `Sora`, `Plus Jakarta Sans`,
`DM Sans`, `Playfair Display`, `Instrument Serif` **запрещены** — у них нет кириллицы.

| Уровень | Размер | Вес | Line-height | Letter-spacing |
|---|---|---|---|---|
| H1 | `clamp(40px, 7.4vw, 92px)` | 700 | 0.96 | −0.04em |
| H2 | `clamp(28px, 4.2vw, 46px)` | 700 | 1.04 | −0.03em |
| H3 (имя кейса) | `clamp(22px, 2.6vw, 30px)` | 600 | 1.1 | −0.02em |
| Lead | `clamp(16px, 1.5vw, 19px)` | 400 | 1.6 | 0 |
| Body | 15–16px | 400 | 1.62 | 0 |
| Label / mono | 11–13px | 500 | 1.4 | 0.14em, uppercase |
| Marquee | `clamp(38px, 7vw, 84px)` | 700 | 1 | −0.03em |

**Правила**
- H1 не длиннее 14ch на строку; переносы задаются явно, а не случайно.
- Mono только для меток, тегов, номеров, доменов и статусов — никогда для прозы.
- Русский текст: `line-height ≥ 1.6` в прозе, минимальный размер основного текста 15px.

---

## 4. Component Stylings

Каждый интерактивный компонент обязан иметь **default / hover / active / focus-visible / disabled**.

### Кнопка — primary
```css
.btn-p{background:var(--amber);color:var(--void);border:1px solid var(--amber);
  border-radius:999px;padding:14px 26px;font-weight:600;font-size:15px;
  transition:background .2s, transform .2s, box-shadow .2s}
.btn-p:hover{background:var(--amber-hi);transform:translateY(-2px);
  box-shadow:0 14px 34px -14px rgba(var(--amber-rgb),.7)}
.btn-p:active{transform:translateY(0) scale(.985);box-shadow:none}
.btn-p:focus-visible{outline:2px solid var(--amber);outline-offset:3px}
.btn-p[disabled]{opacity:.45;pointer-events:none}
```

### Кнопка — secondary
```css
.btn-s{background:transparent;color:var(--ink);border:1px solid var(--line);border-radius:999px;padding:14px 26px}
.btn-s:hover{border-color:var(--amber);color:var(--amber);background:var(--amber-dim)}
.btn-s:active{transform:scale(.985)}
.btn-s:focus-visible{outline:2px solid var(--amber);outline-offset:3px}
```

### Карточка кейса (SpotlightCard)
```css
.case{background:var(--panel);border:1px solid var(--line);border-radius:20px;overflow:hidden;
  transition:border-color .3s, transform .3s, background .3s}
.case::before{content:"";position:absolute;inset:0;opacity:0;transition:opacity .35s;pointer-events:none;
  background:radial-gradient(420px circle at var(--mx) var(--my), rgba(var(--amber-rgb),.10), transparent 62%)}
.case:hover{border-color:var(--line-2);background:var(--panel-2);transform:translateY(-3px)}
.case:hover::before{opacity:1}
.case:focus-within{border-color:var(--amber)}
```
Курсорная подсветка — только при `(hover:hover)` и с rAF-троттлингом `pointermove`.

### Скриншот проекта
```css
.shot img{width:100%;aspect-ratio:16/10;object-fit:cover;object-position:top center;
  border:1px solid var(--line);border-radius:14px;filter:saturate(.94);transition:transform .5s, filter .5s}
.case:hover .shot img{transform:scale(1.02);filter:saturate(1.06)}
```
Все скриншоты — `loading="lazy"`, `decoding="async"`, с явными `width`/`height` против CLS.

### Навигация
```css
nav{position:sticky;top:0;background:rgba(var(--void-rgb),.72);backdrop-filter:blur(12px);
  border-bottom:1px solid transparent;transition:border-color .3s, background .3s}
nav.scrolled{border-bottom-color:var(--line);background:rgba(var(--void-rgb),.9)}
.navlinks a{color:var(--muted);position:relative}
.navlinks a::after{content:"";position:absolute;left:0;bottom:-5px;height:1px;width:0;background:var(--amber);transition:width .25s}
.navlinks a:hover{color:var(--ink)} .navlinks a:hover::after{width:100%}
.navlinks a:focus-visible{outline:2px solid var(--amber);outline-offset:4px}
```

### Тег стека / метка статуса
```css
.tag{font-family:var(--mono);font-size:12px;padding:5px 11px;border:1px solid var(--line);
  border-radius:999px;color:var(--muted);background:var(--base)}
.status{display:inline-flex;gap:7px;align-items:center;font-family:var(--mono);font-size:12px}
.status i{width:6px;height:6px;border-radius:50%;background:currentColor;box-shadow:0 0 10px currentColor}
.status.live{color:var(--ok)} .status.wip{color:var(--wip)} .status.nda{color:var(--dim)}
```

### Ссылка на живой сайт
```css
.visit{font-family:var(--mono);font-size:13px;color:var(--amber)}
.visit:hover{text-decoration:underline;text-underline-offset:4px}
.visit .arrow{display:inline-block;transition:transform .2s}
.visit:hover .arrow{transform:translate(3px,-3px)}
```

---

## 5. Layout Principles

- Контейнер: `max-width:1180px`, боковые поля 28px (мобильные 20px).
- Сетка кейсов: чередующиеся ряды `1.05fr / 1fr` (медиа ↔ текст), направление меняется на чётных карточках.
- Шкала отступов (8-based): `8 · 12 · 16 · 22 · 32 · 48 · 72 · 104 · 140`.
- Вертикальный ритм секции: `padding: clamp(72px, 9vw, 128px) 0`.
- Разделители секций — `1px solid var(--line)`, без теней и без градиентных полос.
- Секции пронумерованы моно-меткой `01 / 02 / 03` — это часть «инженерного» характера.

---

## 6. Depth & Elevation

Тёмная тема не использует классические тени для подъёма — только границу, фон и свет.

| Уровень | Применение | Реализация |
|---|---|---|
| E0 | фон страницы | `--void`, без тени |
| E1 | карточка в покое | `--panel` + `1px --line` |
| E2 | карточка в hover | `--panel-2` + `1px --line-2` + `translateY(-3px)` |
| E3 | акцентная кнопка в hover | `0 14px 34px -14px rgba(var(--amber-rgb),.7)` |
| E4 | ambient-свет hero | статичный `radial-gradient`, без `filter: blur()` |

`filter: blur()` на движущихся элементах запрещён. `backdrop-filter` — только в навигации, значение ≤ 12px.

---

## 7. Animation & Interaction — уровень L2

| # | Категория | Эффект | Реализация |
|---|---|---|---|
| 1 | Text — Hero H1 | построчный mask reveal | CSS `clip-path` + `translateY`, один раз при загрузке, stagger 90ms |
| 2 | Text — Section H2 | ScrollFloat: подъём + проявление | IntersectionObserver, `translateY(18px)→0`, 600ms |
| 3 | Text — Body / Label | stagger-проявление строк кейса | IntersectionObserver + `transition-delay` по индексу |
| 4 | Animation — элемент | магнитная CTA + подъём кнопок | `pointermove` (rAF) → `translate` до 6px, возврат на `pointerleave` |
| 5 | Component | SpotlightCard: подсветка карточки под курсором | CSS-переменные `--mx/--my` + `radial-gradient` |
| 6 | Background | ambient-градиент hero + курсорное световое пятно | одна `radial-gradient`-подложка, позиция от `--mx/--my` |
| + | Signature | лента-манифест бесконечным горизонтальным скроллом | чистый CSS `translateX` keyframes, дублированный трек |
| + | Микро-деталь | клик по e-mail копирует адрес, показывает «скопировано» и искру | `navigator.clipboard` + CSS-анимация 900ms |

**Тайминги.** Микро — 150–250ms; вход блока — 500–700ms; лента — 32s linear infinite.
**Easing.** Вход `cubic-bezier(.22,.61,.36,1)`; hover `cubic-bezier(.4,0,.2,1)`.

**Обязательные ограничители производительности**
- WebGL / 3D на странице: **0**. Уровень L2 достигается CSS-средствами.
- `pointermove` — всегда через `requestAnimationFrame`, один общий слушатель на секцию.
- Курсорные эффекты только под `matchMedia('(hover:hover) and (pointer:fine)')`.
- Изображения `loading="lazy"` + `aspect-ratio`, чтобы лента не прыгала.

```css
@media (prefers-reduced-motion: reduce){
  *,*::before,*::after{animation:none!important;transition:none!important;scroll-behavior:auto!important}
  .reveal{opacity:1!important;transform:none!important;clip-path:none!important}
  .marquee-track{animation:none!important}
}
```

---

## 8. Do's and Don'ts

**Do**
1. Держи ровно один хроматический акцент — янтарь. Всё остальное нейтрально.
2. Отдавай цвет скриншотам: интерфейс вокруг них должен быть тише их.
3. Формулируй кейс как «Задача → Решение → Результат» — это продающая часть, а не украшение.
4. Ставь статус проекта явно: живой сайт, в разработке, под NDA.
5. Пиши каждый цвет через CSS-переменную.
6. Держи фокус-состояние видимым на всём, что кликается.
7. Проверяй русский текст на реальном шрифте с кириллицей.
8. Любой эффект должен иметь путь отключения по `prefers-reduced-motion`.

**Don't**
1. ❌ Не вводи второй акцентный цвет и не делай радужных градиентов на тексте.
2. ❌ Не ставь `filter: blur()` на движущиеся элементы и не крась `backdrop-filter` больше 12px.
3. ❌ Не используй шрифты без кириллицы для русских заголовков.
4. ❌ Не ставь цветные плашки-заглушки вместо скриншотов — либо реальное изображение, либо честная типографическая карточка «под NDA».
5. ❌ Не добавляй эмодзи в интерфейс — тональность инженерная.
6. ❌ Не делай подложку карточек светлее `--panel-2`: тёмный зал не должен «выцветать».
7. ❌ Не вешай scroll-jacking, Lenis и pin-секции — это визитка, а не сайт-презентация.
8. ❌ Не пиши цифры достижений, которых нельзя подтвердить.
9. ❌ Не оставляй ссылку на чужой старый сайт как «мою работу», если редизайн ещё не выложен.

---

## 9. Responsive Behavior

| Брейкпоинт | Поведение |
|---|---|
| ≥ 1180px | Полная сетка, кейсы в две колонки с чередованием сторон |
| 900–1180px | Контейнер по ширине окна, кейсы остаются двухколоночными, шрифты по clamp |
| 640–900px | Кейс схлопывается в одну колонку: скриншот сверху, текст снизу. Стек — 2 колонки |
| < 640px | Одна колонка везде, навигационные ссылки скрыты (остаются логотип, язык, CTA), hero-паддинг 72px, лента-манифест ускоряется и уменьшается |

**Обязательно**
- Ни одного горизонтального переполнения при 320px.
- Тач-цели ≥ 44×44px, включая переключатель RU/EN.
- Курсорные эффекты полностью выключаются на `(hover:none)`.
- Скриншоты остаются в `aspect-ratio: 16/10` и не растягиваются.
