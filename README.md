# University Tracker

## Phone-ready version (recommended)

`mobile_pwa/` is the new responsive **Study Flow** version. It is a private,
offline-first web app: its data stays in the browser on the device and it has
JSON export/import backups. It uses the requested navy, teal, aqua, and mint
palette.

For a quick local preview, run:

```powershell
python run_mobile.py
```

Then open the phone address printed by the script while your computer and phone
are on the same Wi-Fi. For installation and offline PWA support on a real phone,
publish the contents of `mobile_pwa/` to any private HTTPS static host; browsers
only enable service workers (the offline/install feature) on HTTPS or localhost.

## Android APK

The same phone-first PWA is also packaged as a native Android app with
Capacitor. It keeps the same local-only data model, offline behavior, backup
export/import, dark mode, and all subjects/tasks/grades features. The web
install prompt is not shown inside the APK because Android installs the app
directly.

### Install the ready-built app

The signed release APK is created at:

```text
android/app/build/outputs/apk/release/app-release.apk
```

Copy it to an Android phone, open it, and allow **Install unknown apps** for the
app you used to open the file (for example Files or Drive). Android will then
install **Study Flow** as a normal app. Data starts empty and stays on that
phone until you export a JSON backup.

### Build it again

Prerequisites: Node.js, pnpm, Android SDK Platform 36, and Java 21. Android
Studio's bundled Java runtime is suitable on Windows:

```powershell
$env:JAVA_HOME = "C:\\Program Files\\Android\\Android Studio\\jbr"
$env:PATH = "$env:JAVA_HOME\\bin;$env:PATH"
pnpm install
pnpm android:build:debug
```

The debug APK is written to
`android/app/build/outputs/apk/debug/app-debug.apk`. To rebuild the signed
release APK, first create the local signing configuration described below, then
run:

```powershell
pnpm android:build:release
```

### Release signing and backup

`android/keystore.properties` and Android keystore files are intentionally
ignored by Git. Copy `android/keystore.properties.example` to
`android/keystore.properties`, create a keystore at the configured path with
`keytool`, and put your own passwords in that local file. The same keystore is
required to publish future updates over an installed release, so keep an
encrypted backup outside this repository (for example, encrypted cloud storage
and an offline copy). Never commit the keystore or its passwords.

The prior desktop UI is preserved in `backups/desktop-ui-20260709-195841/` and
the original desktop source is still in `university_tracker/`.

---

## Legacy desktop version

A local-only desktop app for tracking university subjects, topics, homework, grades, and deadlines.

The app uses:

- Python
- PySide6 for the desktop interface
- SQLite for local storage

No server, login, or online sync is required. Your data is stored in `data/university_tracker.sqlite`.

## Features

- Signatures with name, semester, professor, teacher email, and schedule
- Signatures view with grading, topics, and homeworks managed inside each signature
- Topics linked to signatures with pending, studying, or seen status
- Editable grading categories per signature, each with its own percentage weight
- Homework linked to signatures and grading categories, with due dates, status, optional grade, and notes/link references
- Category-based grade calculations with a prominent overall grade per signature
- Colombian grading scale from 1.0 to 5.0, where 1.0-2.9 is Lost and 3.0-5.0 is Pass
- Dashboard with upcoming homework, calculated grades, and important deadlines
- Main navigation for Home, Dashboard, and Signatures
- Full-screen home menu with large buttons for Dashboard, Homeworks, Signatures, and Grades
- Sample data created automatically on first launch

## Project Structure

```text
university_tracker/
  app.py          # PySide6 desktop interface
  database.py     # SQLite schema, seed data, and database functions
  __init__.py
data/
  university_tracker.sqlite  # Created automatically when you run the app
run.py
requirements.txt
README.md
```

## Setup

Install Python 3.11 or newer from https://www.python.org/downloads/.

Then open PowerShell in this folder and run:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python run.py
```

If Windows says `python` was not found, install Python from python.org and make sure the installer option **Add python.exe to PATH** is enabled.

## Reset Sample Data

To start fresh, close the app and delete:

```text
data/university_tracker.sqlite
```

The app will create a new database with sample data the next time it starts.
