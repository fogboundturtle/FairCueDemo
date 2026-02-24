# FairCue

FairCue is a desktop pool tournament manager built with PyQt6 and SQLite.

It supports:
- Player database management
- Chip and bracket tournament formats
- Matchups, round announcements, and history tracking
- Purse distribution management
- PDF export
- English/French translations
- Theme editing and modern UI styling

## Tech Stack

- Python
- PyQt6
- SQLite
- PyInstaller (for EXE builds)

Optional modules:
- `fpdf2` for PDF export
- `openpyxl` and `xlrd` for spreadsheet import compatibility

## Project Structure

- `main.py`: app entry point
- `main_window.py`: main UI shell
- `tabs/`: tab implementations (Players, Tournament, Purse, Matchups, History)
- `dialogs/`: modal dialogs
- `widgets/`: custom UI widgets
- `database.py`: SQLite data layer
- `models.py`: tournament/player business logic
- `activation.py`: full-license activation checks
- `demo.py`: demo mode timeline + embedded feature gating
- `activation_support_tool.py`: support CLI for unlock/reset token workflow
- `FairCue.spec`: full app EXE spec
- `FairCueDemo.spec`: demo app EXE spec

## Running From Source

From `src`:

```powershell
python main.py
```

## Install Dependencies

From `src`:

```powershell
python -m pip install PyQt6 pyinstaller fpdf2 openpyxl xlrd
```

## Build Executables

From `src`:

1. Full application EXE

```powershell
python -m PyInstaller --noconfirm FairCue.spec
```

Output:
- `dist/FairCue.exe`

2. Demo application EXE

```powershell
python -m PyInstaller --noconfirm FairCueDemo.spec
```

Output:
- `dist/FairCueDemo.exe`

## Demo Mode

Demo mode is triggered by:
- EXE name containing `demo` (for example `FairCueDemo.exe`), or
- `FAIRCUE_DEMO_MODE=1`

Current demo policy is embedded in executable code (not controlled by registry values):
- 14-day trial
- Max 16 tournament players
- Disabled features:
  - Theme Editor
  - PDF export
  - Player XLS import/export
  - Custom PDF logo

The demo timeline is machine-bound and signed in:
- `HKCU\Software\FairCue\Demo`

## Full License Activation

Full builds use machine-bound activation and integrity checks.

Registry path:
- `HKCU\Software\FairCue\Activation`

Activation policy:
- Maximum activations: `5`
- Over-limit users receive a support request code and can enter a one-time unlock code.

Support utility:

```powershell
python activation_support_tool.py decode --code "<support_request_code>"
python activation_support_tool.py make-token --code "<support_request_code>" --new-count 1 --valid-hours 168
```

## Security Notes

- Do not hardcode seller API tokens in source.
- Use environment variables for secrets:
  - `FAIRCUE_ACTIVATION_SECRET`
  - `FAIRCUE_DEMO_SECRET`
  - `FAIRCUE_GUMROAD_PRODUCT_ID`
  - `FAIRCUE_GUMROAD_VERIFY_URL`

## Data and Settings

Runtime files are stored next to the executable/source, including:
- `tournament.db`
- `theme.json`
- `translations_en.json`
- `translations_fr.json`
- `display_settings.json`
- `language_pref.json`

## Version

Current app version in installer flow: `1.0.1`
