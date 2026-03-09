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