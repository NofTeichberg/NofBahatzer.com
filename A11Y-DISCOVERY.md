# A11Y-DISCOVERY.md — נוף בחצר | בדיקת נגישות בסיסית

## מיפוי קבצים

```
site/
├── index.html          ← עמוד ראשי יחיד (791 שורות, כולל CSS ו-JS inline)
├── hero-video.mp4      ← וידאו רקע ה-Hero
└── images/
    ├── logo.png
    ├── hero-poster.jpg
    ├── about.jpg
    ├── wine.jpg
    ├── food-1.jpg, food-2.jpg, food-3.jpg
    └── gallery-1.jpg … gallery-7.jpg
```

אין קובצי CSS/JS חיצוניים — הכל inline ב-`index.html`.

---

## מיפוי אלמנטים

| רכיב | Selector / ID | הערות |
|---|---|---|
| וידאו Hero | `video.hero-media` (source: hero-video.mp4) | autoplay muted loop playsinline, poster="images/hero-poster.jpg" |
| פופ-אפ גיוס | `#hiring-popup` | display:flex position:fixed — נפתח בטעינה, נסגר בלחיצת ✕ |
| קרוסלת גלריה | `#carouselTrack` + `.carousel-btn` | 7 שקופיות, 3 גלויות בבת אחת (desktop), 1 במובייל |
| תפריט עליון | `nav.top-nav` בתוך `header.top-bar` | נעלם ב-<900px (display:none), אין המבורגר |
| פוטר | `footer` | קישורי רשתות חברתיות, טלפון, וואטסאפ, Waze |
| לוגו (nav) | `a.logo[href="#"]` | מצביע על "#" — בעיית נגישות |

---

## טבלת צבעים ובדיקת ניגודיות

| משתנה | HEX | שימוש |
|---|---|---|
| `--beige-light` | `#faf6ee` | רקע ראשי (about, body) |
| `--beige` | `#f4ede0` | רקע גלריה, tags |
| `--beige-dark` | `#e8dcc4` | פוטר רקע, border-dark |
| `--white` | `#ffffff` | רקע wine-kitchen, info |
| `--green` | `#4a5d3f` | כפתורים, section-labels |
| `--green-dark` | `#3a4a32` | כותרות h3, tag text |
| `--text` | `#2d2d2a` | טקסט ראשי |
| `--text-soft` | `#5a5a55` | טקסט משני |
| `--line` | `#d8cdb4` | קווי הפרדה (דקורטיבי) |

### בדיקות ניגודיות (WebAIM WCAG 2.0 AA)

| צמד | יחס ניגודיות | דרישה | תוצאה |
|---|---|---|---|
| `--text #2d2d2a` על `--beige-light #faf6ee` | ~12.5:1 | ≥4.5:1 | ✅ עובר |
| `--text-soft #5a5a55` על `--beige-light #faf6ee` | ~6.4:1 | ≥4.5:1 | ✅ עובר |
| `--text-soft #5a5a55` על `--white #ffffff` | ~7.0:1 | ≥4.5:1 | ✅ עובר |
| `--green #4a5d3f` על `--beige-light #faf6ee` | ~6.4:1 | ≥4.5:1 | ✅ עובר (section-labels) |
| `--green-dark #3a4a32` על `--beige #f4ede0` | ~9.1:1 | ≥4.5:1 | ✅ עובר (tag) |
| `--white #fff` על `--green #4a5d3f` | ~6.4:1 | ≥4.5:1 | ✅ עובר (כפתורים) |
| `--white #fff` על `--green-dark #3a4a32` | ~9.1:1 | ≥4.5:1 | ✅ עובר (final-cta) |
| טקסט Hero (לבן) על וידאו | תלוי פריים | ≥4.5:1 | ⚠️ overlay rgba(0,0,0,.4) — גבולי, הוגדל ל-.55 |
| `--line #d8cdb4` (גבולות) | — | דקורטיבי | ✅ פטור |
| פוקוס outline `#8a6d0b` על `--beige-light #faf6ee` | ~4.6:1 | ≥3:1 | ✅ עובר |

**אין "זהב על קרם" בפלטה הנוכחית** — הסיכון היה בפלטות קודמות; הפלטה הנוכחית עוברת AA בכל הצמדים.

---

## ליקויים שנמצאו (Baseline)

### קריטי
- [ ] אין `<main>` landmark — קורא מסך לא יכול לנווט לתוכן
- [ ] אין קישור "דלג לתוכן" — משתמשי מקלדת חייבים לעבור כל ה-nav בכל טעינה
- [ ] פופ-אפ גיוס: אין `role="dialog"`, אין `aria-modal`, אין מלכודת פוקוס, אין סגירה ב-Esc
- [ ] וידאו רץ ללא כפתור עצירה — WCAG 2.2.2 (Pause, Stop, Hide)

### חמור
- [ ] `a.logo[href="#"]` — קישור ריק ללא יעד משמעותי
- [ ] אין הצהרת נגישות (חובה חוקית ת"י 5568)
- [ ] אין רכיב נגישות
- [ ] מלכודת פוקוס חסרה במודאל
- [ ] כותרת H2 בפופ-אפ מופיעה לפני H1 ב-DOM (מבלבל קורא מסך)

### בינוני
- [ ] `outline:none` / `outline:0` לא נמצא, אך אין `focus-visible` מוגדר ברמת הגלובל
- [ ] כפתורי קרוסלה: יש `aria-label` בסיסי אך מיקום RTL לא מוסבר
- [ ] תמונות בגלריה: יש alt אבל חלקן ריקות או גנריות

---

## ציון Baseline (Lighthouse Accessibility)
*משוער 55-65 בהתבסס על הליקויים שנמצאו — נדרש ריצה ידנית לאישור*

---

## ✅ תיקונים שבוצעו (feature/accessibility-5568)

| שלב | פריט | קובץ |
|---|---|---|
| P0 | תיעוד גילוי, טבלת צבעים | A11Y-DISCOVERY.md |
| P0 §3.1 | עמוד הצהרת נגישות | accessibility.html |
| P1 §4.1 | `<main id="main-content">`, landmarks מלאים | index.html |
| P1 §4.2 | קישור "דלג לתוכן הראשי" | index.html |
| P1 §4.3 | `:focus-visible` גלובלי, צבע focus עם ניגודיות 4.6:1 | index.html |
| P1 §4.4 | H1 יחיד, section-labels הפכו ל-`<p aria-hidden="true">` | index.html |
| P3.2 | רכיב נגישות מובנה (גודל, ניגודיות, קישורים, גופן, תנועה) | index.html |
| P5.1 | כפתור עצור/הפעל וידאו (44×44px, ARIA) | index.html |
| P5.1 | prefers-reduced-motion — וידאו מוחלף ב-poster | index.html |
| P5.2 | מודאל גיוס: role=dialog, aria-modal, focus trap, Esc, sessionStorage | index.html |
| P5.3 | קרוסלה: aria-roledescription, aria-label לכפתורים, inert על slides מוסתרות | index.html |
| P5.4 | קישור לוגו: מ-`href="#"` ל-`href="#main-content"` | index.html |
| P5.4 | aria-label לכל קישורי הפוטר | index.html |
| P5.5 | alt="" לתמונות דקורטיביות; alt משמעותי לתמונות נושאות מידע | index.html |
| P6 | overlay Hero הוכהה (0.35→0.65) לניגודיות טקסט לבן מעל וידאו | index.html |
| P7 | אחדות קישור תפריט: `hopa.tech` בלבד | index.html |
| P7 | min-height 44px לכפתורים וקישורי nav/footer | index.html |
| P7 | viewport ללא user-scalable=no | index.html |
| §3.1 | קישור "הצהרת נגישות" בפוטר | index.html |

---

## נותר למילוי ידני ע"י צוות נוף בחצר

1. **פרטי ממונה נגישות** בקובץ `accessibility.html`: שם, טלפון, דוא"ל
2. **פרטי נגישות פיזית**: חנייה, כניסה, שירותים, מסלולי תנועה
3. **תאריך עדכון ההצהרה**
4. **מגבלות ידועות** (אם קיימות)
5. **אישור מול מורשה נגישות מוסמך** — ממולץ בהתאם לתקנות

---

## הבהרה: רכיב הנגישות אינו תחליף לתיקוני הקוד

רכיב הנגישות (כפתור ♿) הוא **תוספת** שמאפשרת התאמות שכבתיות (גודל, ניגודיות, תנועה).
כל תיקוני הנגישות הבסיסיים בוצעו ישירות בקוד HTML/CSS/JS — מבנה landmarks, alt text,
focus management, modal dialog, ARIA — ואינם תלויים ברכיב.
הדף נגיש גם ללא הפעלת הרכיב.

---

## ציון סופי (לאחר תיקונים)
*להריץ Lighthouse / axe אחרי פריסה לסביבה עם HTTPS — משוער ≥95*
