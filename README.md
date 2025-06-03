# Capstone Meeting Tracker

A bilingual **Streamlit** web‑app that lets capstone teams capture daily meeting notes, auto‑sync them to a shared Google Sheet, and instantly download a tidy markdown copy for their own repos.

---

## ⭐ Why you’ll love it

* **One form, two audiences** – students type once; instructors get live analytics while teams keep a local record.
* **English & 日本語 toggle** – switch language from the sidebar; labels, tooltips, and hints update on the fly.
* **Day‑by‑day workflow** – four tabs mirror the sprint cadence (Kick‑off → MVP Build → Soft Freeze → Final Prep).
* **Guided prompts everywhere** – hover over ❓ to see stakeholder‑style coaching questions.
* **Safe Google Sheets backend** – rows append to an "All\_Submissions" worksheet using a service‑account key; headers auto‑heal if the schema changes.
* **Instant offline backup** – after a successful submit, download `<Capstone_Notes_GroupX_DayN_YYYY‑MM‑DD>.md` with the same content.

---

## 🚀 Quick start

```bash
# 1  clone the repo
$ git clone https://github.com/<your‑org>/capstone‑meeting‑tracker.git
$ cd capstone‑meeting‑tracker

# 2  install deps (Python 3.10+)
$ pip install -r requirements.txt

# 3  add Google credentials
$ mkdir -p .streamlit && nano .streamlit/secrets.toml
```

```toml
# .streamlit/secrets.toml
[gcp_service_account]
# full JSON for your service account …

[gcp_spreadsheet]
key = "<SPREADSHEET_ID>"
```

```bash
# 4  run the app
$ streamlit run app.py
```

Then open [http://localhost:8501](http://localhost:8501) in your browser.

---

## 🖥️ Using the app

1. Pick **English** or **日本語** in the sidebar.
2. Fill the meeting meta‑data (Group No., Time‑slot, Date…).
3. Click the tab for today’s sprint day and complete the guided form.
4. Hit **Submit to Instructor & Download**.

   * ✅ A green toast = your row reached Google Sheets.
   * 📥 A download button appears = grab your markdown copy.

---

## 🔐 Secrets & environment

| Key                   | Purpose                                                         |
| --------------------- | --------------------------------------------------------------- |
| `gcp_service_account` | Full service‑account JSON that owns *edit* rights on the sheet. |
| `gcp_spreadsheet.key` | Spreadsheet ID (the long string between `/d/` and `/edit`).     |

> No other environment variables are required.

---

## 🗂️ Code tour

| File     | Role                                     |
| -------- | ---------------------------------------- |
| `app.py` | Main Streamlit application (UI + logic). |

### Key functions

| Function                         | What it does                                                        |
| -------------------------------- | ------------------------------------------------------------------- |
| `initialize_session_state()`     | Seeds every form field & checkbox with sane defaults.               |
| `render_day_inputs()`            | Builds dynamic inputs for each sprint day via the translation dict. |
| `save_to_gsheets_new_row()`      | Appends form data, auto‑creating header row if missing.             |
| `get_student_download_content()` | Generates the markdown file offered to users.                       |

### Extend / customize

* **Add a language** – copy the `"en"` block in `TRANSLATIONS`, translate, and list it in `language_options`.
* **Add a sprint day** – follow the `day_5_` naming convention; no Python refactor needed.
* **Tweak Google targets** – change `SHEET_NAME` or workbook in `save_to_gsheets_new_row()`.

---

## 🛠️ Troubleshooting

| Symptom                         | Fix                                                                               |
| ------------------------------- | --------------------------------------------------------------------------------- |
| *“GSheets Connect Error”*       | Check that the service account JSON is valid **and** the sheet is shared with it. |
| No download button after submit | Write failed – see error toast & network connection.                              |
| Japanese text shows as squares  | Install a CJK‑capable font on your Streamlit host.                                |

---

## 🤝 Contributing

Pull requests are welcome! Open an issue first to discuss substantial changes.

---

## 📝 License

MIT © 2025 Capstone Team
