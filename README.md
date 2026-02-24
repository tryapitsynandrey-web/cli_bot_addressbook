# Assistant Bot (CLI Address Book)

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?logo=python&logoColor=white)
![prompt_toolkit](https://img.shields.io/badge/prompt__toolkit-CLI-blue)
![Rich](https://img.shields.io/badge/Rich-Terminal-green)

A resilient, high-performance Command Line Interface (CLI) application for secure contact and note management.

## Architecture Overview

The project adheres to Domain-Driven Design (DDD) principles to strictly separate business rules, application orchestration, presentation logic, and persistence metadata.

- `domain/`: Contains pure business entities (`Models`, `Fields`, `Records`, `Contacts`, `Notes`) and custom exceptions (`exceptions.py`). This strictly isolates business rules from presentation or storage mechanisms.
- `services/`: Orchestrates the core application logic via `AddressBookService.py`. Acts as the intermediary layer connecting domain models with UI inputs.
- `ui/`: Manages external user interactions. `commands.py` routes user inputs, while `console.py` handles terminal output formatting via `rich`.
- `storage.py` & `import_export.py`: Infrastructure layer responsible for serialization and persistence. Manages dual-format storage (Pickle and JSON) and data migration mechanisms.
- `app.py`: The presentation event loop, integrating `prompt_toolkit` for interactive CLI features.

## Features

- **Interactive CLI Environment**: Features a persistent command prompt with context-aware tab autocompletion via `prompt_toolkit`.
- **Rich Terminal UI**: Offers colorized and structured terminal outputs utilizing the `rich` library.
- **Dual-Format Resilient Storage**: Automatically saves data via Python Pickle (`.pkl`) with a synchronized synchronous `.json` fallback to prevent data loss.
- **Tag Management System**: Allows users to attach, filter, and remove tags on individual contacts and notes.
- **Universal Import/Export Pipeline**: Built-in adapter functions to natively parse and dump records across CSV, JSON, and Pickle formats.
- **Automated Mock Data Generation**: `generate_data.py` allows rapid seeding of realistic mock contacts for testing and demonstration.

## Project Structure

```text
assistant-bot/
├── main.py                     # CLI application entry point
├── generate_data.py            # Development data seeder script
├── user_address_book/          # Default persistence directory (auto-created)
└── assistant_bot/
    ├── app.py                  # CLI event loop & smart completer
    ├── config.py               # Shared constants & application thresholds
    ├── import_export.py        # I/O adapters (CSV/JSON/Pickle pipelines)
    ├── storage.py              # Persistence manager (Dual-format saves)
    ├── domain/
    │   ├── exceptions.py       # Custom validation/logic exceptions
    │   └── models.py           # Core DDD entities
    ├── services/
    │   └── address_book_service.py # Core operations orchestrator
    ├── ui/
    │   ├── commands.py         # User input routing registry
    │   └── console.py          # Rich formatting wrappers
    └── utils/
        └── ux_messages.py      # UX strings and dialogue constants
```

## Installation

### Local Environment

1. Clone the repository:
   ```bash
   git clone <repository_url>
   cd cli_bot_addressbook_v9
   ```

2. Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

### Docker
*Docker setup is not currently provided or supported within the repository base.*

## Usage

### CLI Usage

Start the application by executing the main script:
```bash
python assistant-bot/main.py
```

Upon launch, you will enter the `bot>` interactive prompt.
- Press `Tab` at any time to utilize context-aware command and argument autocompletion.
- Type commands such as `add`, `add-note`, or `filter_by_tag` to manage records.
- Type `exit` or `close` to cleanly terminate the session and ensure pending data writes are flushed to disk.

*(Optional) Seed the database with mock records:*
```bash
python assistant-bot/generate_data.py
```

### Web Usage
*This project does not contain a web service component. It executes strictly as a local CLI application.*

## Data Storage

The application leverages a dual-serialization strategy:
- **Formats**: The primary storage medium is Python Pickle (`.pkl`), paired with automated synchronized `.json` backups.
- **Location**: By default, data files are written to the `assistant-bot/user_address_book/` directory relative to the repository root. This relative path is configurable globally.

## Configuration

Application constants and execution heuristics—such as the number of consecutive input errors required to trigger automated help interactions—can be adjusted globally by modifying constants within `assistant-bot/assistant_bot/config.py`.

## Development Notes

- **Extensibility**: The command dictionary mapped in `ui/commands.py` enables developers to map new CLI text commands directly to methods built inside `services.AddressBookService`, bypassing boilerplate routing logic.
- **Memory Footprint**: The application prioritizes I/O execution speed by loading the comprehensive dataset completely into memory during `main.py` bootstrap, writing back purely upon specific actions or termination.

## Limitations & Assumptions

- **Concurrency**: The tool currently operates entirely on a single thread and has no implemented file-locking mechanisms. Executing multiple concurrent instances mapped to the exact same disk storage target may yield race conditions or data corruption.
- **Memory Constraints**: The aggressive in-memory operational model assumes the address book dataset will naturally remain well within the bounds of standard hardware RAM availability.
- **Tests**: The repository contains an isolated `test_addressbook/` directory holding mock state files (`.pkl`, `.json`, `.csv`), but a strict Pytest/Unittest suite is not visibly integrated or executed via the main application flow. 

## License
*Not specified within the current repository structure.*
