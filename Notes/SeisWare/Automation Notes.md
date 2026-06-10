## If you can influence the app (quick dev hooks that help QA)

- Expose `ProjectSelectOneList::model()` or make it subclass `QAbstractItemView` with a public model — one line on your side unlocks robust data reading.
- Assign **`objectName`** or **`accessibleName`** to each item widget (or at least to the main content widget). TestComplete can then find and read them without OCR.
- Ensure **Qt Accessibility** is enabled for the control (Qt’s accessibility bridge); that often lights up UI Automation patterns, which we can read much more directly than OCR.
- Specific to project properties but could be applied to a lot of applications.
- Your widget:

```
QtClassName = ProjectSelectOneList
Name        = QtObject("projectsTree")
```

is a **custom Qt widget** that:

- ❌ does **not** expose `model()`
- ❌ does **not** provide usable Qt object introspection
- ❌ does **not** expose child standard Qt views (`QTableView/QListView/QTreeView`)
- ❌ does **not** expose UIA grid patterns
- ❌ does **not** render its items as separate child widgets (buttons/labels)
- ❌ does **not** support OCR (by your license)

This means the widget renders its items **entirely custom‑painted** on a `QScrollArea`'s viewport.  
To TestComplete, that viewport is just one rectangle, without any structure inside it.

This is **exactly the scenario** TestComplete documents as:

> _“Only coordinate-based interaction is possible with this control unless the application provides accessibility or exposes a model.”_

(Source: SmartBear Support Docs — “Testing Custom Controls With No Object Model or UIA Layer”)


---
## ✅ Solution (Short Summary)

**Goal:** Make custom Qt controls testable without OCR/coordinates by exposing structure and accessibility.

**Core approach (centralized where possible):**

1. **Create a shared base class/mix‑in** for custom controls
    
    - Enable **Qt Accessibility / UIA bridge** once.
    - Set default **`objectName` / `accessibleName`** policy.
    - Provide a virtual **`QAbstractItemModel* model() const`** (returns `nullptr` by default).
2. **Per‑widget minimal changes (only where needed):**
    
    - Override `model()` to return the control’s model (use `QStandardItemModel` or a light `QAbstractItemModel` wrapper if none exists).
    - (Optional) Refactor some controls to subclass **`QListView/QTreeView/QTableView`** so `model()` and accessibility come for free.

**Outcome:** TestComplete (and other tools) can enumerate items via model/UIA; QA gets stable selectors and reliable reads.

---

## 💵 Effort & Cost (Ballpark)

> Assumptions: medium–large codebase, 10–30 custom controls, experienced Qt dev, typical North American rates. Adjust to your context.

### Scenario A — Common base class already exists

- **Implement base accessibility + naming + `virtual model()`**: **1–2 days**
- **Per‑widget `model()` override / light wrapping (10–30 ctrls)**: **~30–60 min each**
- **Total duration:** **2–5 days**
- **Cost (example @ $120/hr):** **$1.9k – $4.8k**

### Scenario B — No shared base; ad‑hoc custom widgets

- **Introduce shared base & refactor inheritance:** **1–3 days**
- **Add accessibility + naming policies:** **~1–1.5 days**
- **Add `virtual model()` + shared helpers:** **~1 day**
- **Per‑widget adjustments:** **1–2 weeks** (depends on count/complexity)
- **Total duration:** **1–3 weeks**
- **Cost (example @ $120/hr):** **$4.8k – $14.4k**

---

## 🧩 What’s centralized vs. per‑control

- **Centralized (one time):** Qt Accessibility/UIA bridge setup, default `objectName` & `accessibleName`, `virtual model()` API, logging/diagnostics.
- **Per control (small, only if needed):** Return an existing model or add a thin model wrapper; optional migration to a standard Qt view.