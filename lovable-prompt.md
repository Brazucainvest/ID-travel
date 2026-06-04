# Lovable Prompt – Fight Club 12-Week Fitness App

## Project overview
Build a **personal fitness tracking PWA** for a single user (Deivide, 40yo, 171cm) following a 12-week strength + fat-loss program called "Fight Club". The app is mobile-first, dark theme, glassmorphism design. All data is stored in **localStorage** — no auth, no backend needed.

Attach the reference file `deivide-app.html` when prompting Lovable — it contains all workout data, food database, meal plans, and the complete existing logic. Use it as the source of truth for all constants and data.

---

## Design system
- Background: `#060c18` (very dark navy)
- Glass cards: `background: rgba(255,255,255,0.05); backdrop-filter: blur(16px); border: 1px solid rgba(255,255,255,0.1)`
- Accent gradient: `#60a5fa → #a78bfa` (blue to purple)
- Font: `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`
- All inputs: `font-size: 16px` (prevents iOS Safari zoom)
- Bottom navigation: fixed, frosted glass, `blur(28px)`
- Animations: `slideUp`, `pop`, `fadeIn` with spring curves `cubic-bezier(0.34, 1.4, 0.64, 1)`
- Viewport: `width=device-width, initial-scale=1.0, viewport-fit=cover`
- Safe area: `padding-bottom: env(safe-area-inset-bottom)` on nav and bottom-sticky buttons
- No scrollbars: `::-webkit-scrollbar { display: none }`

---

## User profile (hardcoded constants)
```
name: Deivide
age: 40, height: 171cm
startWeight: 70.2kg, goalWeight: 67kg
targetProtein: 140g/day
kcalTrain: 2150, kcalRest: 1950
stepGoal: 10000
```

---

## Navigation
5 bottom tabs:
1. 🏠 I dag (Today)
2. 💪 Trening (Workout)
3. 🥗 Mat (Food)
4. 📊 Fremgang (Progress)
5. 📋 Plan

Active tab: gradient text (blue→purple) + scale(1.1) icon + small dot indicator below.

---

## Screen 1 – Today (I dag)
Dashboard with today's date in header.

**Header** (gradient `#0d1f3c → #060c18`):
- Date + greeting "Hei, Deivide 👋"
- Week badge "Uke X/12" + streak badge (🔥) if ≥2 weeks

**Daily score card** (glass):
- Big % number (0–100) calculated from: protein goal (30pts), steps (20pts), water 8 glasses (15pts), workout if training day (25pts), energy logged (10pts)
- Score label: "Perfekt dag! 🔥" / "Solid dag 💪" / "Halvveis 🙂" / "Start dagen ☀️"
- Progress bar colored by score (green/yellow/red)
- Checklist: ✓ Protein / ✓ Steg / ✓ Vann / ✓ Trening / ✓ Energi

**4 ring indicators** (SVG rings with glow filter when > 15% filled):
- Protein (green), Calories (blue), Sessions this week /3 (purple), Steps (amber, tappable to log)

**2-column progress cards** (glass):
- Program: week progress bar (purple→pink gradient)
- Weight goal: kg progress bar (green→lime), shows estimated weeks to goal

**3-column body stats** (glass): current weight, waist, arm measurement

**Phase banner**: colored by current phase (weeks 1-4 blue, 5-8 purple, 9-12 red)

**Today's plan card**: tappable if it's a workout day → navigates to that workout

**Running tip** (teal card): shown on Mon/Wed/Sat with week-specific run plan

**Smart reminders** (glass card "📌 Viktig nå"):
- Workout reminder if training day
- Warning if 4+ days since last session
- Protein low warning if < 50% by noon
- Weight not logged this week
- Water reminder if < 4 glasses after 10am
- Monday: waist measurement reminder
- Even weeks: progress photo reminder

**Food quick action button** (glass): shows protein progress bar, tap → Food tab

**Water tracker**: 8 glass buttons, +/- controls, progress bar

**Energy log**: 5 emoji buttons (😴😐🙂💪🔥)

**Weekly rapport card**: sessions/3, avg protein, avg weight + trend vs last week, tips for next week

---

## Screen 2 – Workout (Trening)
Header: "💪 Trening" + today's date.
Two sub-tabs: **Økter** | **🏆 Rekorder**

### Økter tab
List of 5 workout cards (glass, with colored top bar):
- A: Push (blue→purple) – Monday
- B: Pull (green→teal) – Wednesday  
- C: Legs (orange→amber) – Saturday
- NØD: Emergency 15min (red)
- KB: Kettlebell home (teal)

Each card: emoji icon, title, subtitle, day label (📅 Mandag/Onsdag/Lørdag), exercise count, last session date.

Tap → **Workout Detail** screen:
- Gradient header with title + day label
- List of exercises (glass cards with thumbnail image, name, sets×reps, PR badge)
- "🚀 Start økt" button (gradient)
- "📝 Etter-registrer økt" button (glass) — retroactive logging

### Rekorder tab
- **Strength history**: per-workout accordion showing each exercise with PR badge, mini bar chart of last 8 sessions, trend arrows
- **Session history list**: all logged sessions newest first

### Active Workout screen (full screen)
Header (workout gradient):
- ← Avbryt button
- **"X/7 øvelser ▾" tappable** → opens exercise jump sheet (list of all exercises, tap to jump, shows completion per exercise)
- "Lagre" button (top right)
- Progress bar = total sets done / total sets
- Date picker shown only in retro/edit mode

Exercise view:
- Full-width exercise image (220px)
- Exercise name + English name + sets×reps
- Tags: focus muscle, equipment, PR badge
- Tip card
- Previous session reference (weights used last time)
- Set rows: set number | kg input | reps input | ✓ checkbox
  - When ✓ checked: row turns green
  - Shows estimated 1RM below row if reps > 1 (Epley formula: `weight × (1 + reps/30)`)
- Rest timer button (⏱ Hvile) — manual trigger only

Sticky bottom nav (never hidden):
- ← Forrige | Neste → 
- Last exercise: "🏆 Fullfør og lagre"

**Rest timer** (modal overlay): 90s default, presets 60/90/120/180s, pause/reset, bar changes color (blue→yellow→red)

**Exercise jump overlay** (bottom sheet): all exercises listed with completion badge, current highlighted, tap to jump, "Fullfør og lagre" button

**Finish modal**: sets done count, "✅ Lagre økt" saves to localStorage, updates PRs automatically

**Rules:**
- Rest timer does NOT auto-trigger in retro/edit mode
- "Neste →" never saves — only "Lagre økt" in finish modal saves
- Retro mode: date picker in header, saves with chosen date

---

## Screen 3 – Food (Mat)
Header (gradient `#0a2218 → #060c18`): "🥗 Mat" + today's date + "Treningsdag/Hviledag · mål X kcal" + protein streak badge.

**Header stats** (2 cards with colored glass borders):
- Protein: green border, progress bar
- Calories: blue border, shows remaining or over-budget

**Macro bar** (P/C/F colored segments, shown when food logged)

Sub-tabs: **📋 Logger** | **📚 Kostguide**

### Logger tab
5 meal sections (glass cards): 🌅 Frokost / ☀️ Lunsj / 🍌 Mellom / 🍽 Middag / 🌙 Kveld

Each meal: header with protein+kcal total, "+ Legg til" button, list of logged items with × remove button.

**Add meal modal** (dark glass overlay, 88vh max):
4 tabs:
1. **🔍 Søk** — search food database (60+ items), select → choose portion or enter grams → add to basket → add basket as one meal entry
2. **⚡ Hurtig** — suggested meals per meal type (from MEALS constant)
3. **📸 Foto** — AI photo analysis:
   - First time: API key entry (sk-ant-... stored in localStorage)
   - Camera button → capture → resize to 900px max → send to claude-haiku-4-5-20251001 via Anthropic API with header `anthropic-dangerous-direct-browser-access: true`
   - Prompt: analyze food photo, return JSON array with name/portion/protein/carbs/fat/kcal per item (Norwegian)
   - Show results with individual "+ Legg til" or "✅ Legg til alle" button
4. **✏️ Manuell** — free-form entry: name, protein, carbs, fat, kcal

### Kostguide tab
Nutrition guide cards for Protein/Karbs/Fett with expandable info, food examples, and tips. Also Supplements section and NutritionGuide.

---

## Screen 4 – Progress (Fremgang)
Header (gradient `#130a2e → #060c18`): "📊 Fremgang" + today's date + start/goal weight.

**Week selector**: 12 buttons (1-12), green dot if sessions exist, current week ring highlight.

**Weight trend chart** (SVG line chart): shows weight across weeks, goal line (green dashed), start line (grey dashed), colored dots.

**4 measurement tabs**: ⚖️ Vekt | 📏 Mål | 😴 Søvn | 📅 Historikk

- **Vekt**: log up to 3 daily weights (glass card), shows week average
- **Mål**: waist, chest, arm, shoulder inputs with previous week comparison arrows
- **Søvn**: 0-7 nights slider buttons + steps input + notes textarea
- **Historikk**: list of all completed sessions newest first (glass cards with colored top bar, sets done, protein/kcal for that day)
  - Tap any session → **DayDetailSheet** (full screen overlay):
    - Header: workout gradient, date, edit button
    - Quick stats: protein, kcal, water, energy emoji
    - Workout section: all exercises with logged weights×reps per set (✓/partial/skipped)
    - Food section: grouped by meal type with totals
    - "✏️ Rediger økt" → navigates to ActiveWorkout edit mode

**12-week overview table**: all weeks with avg weight, waist, arm, sessions (✓✓✓), sleep.

---

## Screen 5 – Plan (Plan)
Header: "📋 Plan" + today's date + week number.

5 sub-tabs: **Uke** | **🏃 Løp** | **🥗 Mat** | **🍳 Oppskrifter** | **💊 Tilskudd**

- **Uke**: phase banner, progression rules, weekly schedule (Mon-Sun with workout/rest labels)
- **Løp**: current week's run plan (Mon/Wed/Sat), full 12-week running progression table
- **Mat**: suggested meals for each meal type with protein/kcal
- **Oppskrifter**: recipe cards (expandable) — quick high-protein recipes under 20 min
- **Tilskudd**: supplement cards (creatine, whey, vitamin D, omega-3, magnesium)

Also has **Shopping list** with categories, checkboxes, reset button.

---

## localStorage data model
```
fc_start: "2025-01-06"           // program start date
fc_sessions: [                    // workout sessions
  { id: number, date: ISO string, workoutId: "A"|"B"|"C"|"NØD"|"KB",
    workoutTitle: string,
    log: { [exId]: [{w: string, r: string, done: boolean}] } }
]
fc_prs: { [exId]: {w, r, vol, date} }  // personal records
fc_food_log: { "2025-01-06": [          // food entries per day
  { id, name, desc, protein, carbs, fat, kcal, mealType, time }
] }
fc_progress: {                          // weekly body metrics
  w1: { weights: number[], waist, arm, chest, shoulder, sleep, steps, note }
}
fc_water: { "2025-01-06": 5 }          // glasses of water per day
fc_energy: { "2025-01-06": 3 }         // energy level 1-5 per day
fc_shopping: { [itemKey]: boolean }     // shopping list checkboxes
fc_api_key: "sk-ant-..."               // Anthropic API key for photo analysis
```

---

## Key business logic
- **Week number**: `Math.ceil((today - startDate) / 7)` clamped to 1-12
- **Phase**: weeks 1-4 = Teknikk (blue), 5-8 = Muskelbygging (purple), 9-12 = Definisjon (red)
- **1RM estimate**: `weight × (1 + reps / 30)` (Epley formula)
- **Daily score**: weighted sum of 5 goals (protein 30, steps 20, water 15, workout 25, energy 10)
- **Goal date estimate**: based on avg weekly weight delta over logged weeks
- **PR update**: on session save, compare volume (weight × reps) per exercise, update if higher
- **Protein streak**: count consecutive days with protein ≥ 140g
- **Week sessions**: filter fc_sessions by date range for current week

---

## Important UX rules
1. All modals: `max-height: 88vh`, content area `overflow-y: auto`, action buttons in sticky footer (`flex-shrink-0`) — never hidden behind keyboard or scroll
2. `font-size: 16px` on all inputs — prevents iOS Safari auto-zoom
3. Rest timer: auto-trigger on set completion ONLY during live workout (not retro/edit mode)
4. "Neste →" in workout never saves — only the explicit "Lagre økt" button saves
5. Retro registration: date picker in header, session saved with chosen date
6. Edit session: pre-populates all log data, saves over existing session
7. Exercise jump sheet: shows all exercises in workout, completion status, tap to jump directly

---

## Reference file
**Attach `deivide-app.html`** — this contains the complete exercise database (5 workouts × 7 exercises each), food database (60+ items with per-100g macros and portion presets), meal suggestions, recipes, supplements list, shopping list, and all Norwegian UI text. Use it as the exact source of truth for all data.
