# reminders-cli

A Swift CLI tool for interacting with macOS Reminders via EventKit.

## Project Structure

```
Sources/
├── reminders/
│   └── main.swift              # Entry point
└── RemindersLibrary/
    ├── CLI.swift               # Command definitions (ArgumentParser)
    ├── Reminders.swift         # Core reminder operations (EventKit)
    ├── EKReminder+Encodable.swift  # JSON encoding for reminders
    ├── NaturalLanguage.swift   # Date parsing ("tomorrow", "next week")
    ├── Sort.swift              # Sorting options
    └── CollectionType+Extension.swift  # Helper extensions

Tests/
└── RemindersTests/
    └── NaturalLanguageTests.swift  # Date parsing tests
```

## Building

```bash
swift build           # Debug build
swift build -c release  # Release build
swift test            # Run tests
```

## Key Components

### CLI.swift
Defines commands using Swift ArgumentParser:
- `Add` - Create reminders with optional due date, priority, notes
- `Edit` - Modify existing reminders (text, notes, due date, priority)
- `Complete` / `Uncomplete` - Toggle completion status
- `Delete` - Remove reminders
- `Show` / `ShowAll` - Display reminders with filtering options
- `ShowLists` / `NewList` - Manage reminder lists

### Reminders.swift
Core EventKit operations:
- `Priority` enum maps to EKReminderPriority values (none/low/medium/high/urgent)
- `addReminder()` - Creates new reminder with all properties
- `edit()` - Updates existing reminder properties
- `setComplete()` - Marks reminders done/undone
- `delete()` - Removes reminders

### Priority Mapping
```
urgent = 1 (highest)
high   = 2-4 (EKReminderPriority.high)
medium = 5   (EKReminderPriority.medium)
low    = 6-9 (EKReminderPriority.low)
none   = 0
```

## Testing Notes

The NaturalLanguageTests have 3 known failures due to DateComponents comparison including `dayOfYear` in actual but not expected values. These are test bugs, not code bugs.

## Fork Changes (from keith/reminders-cli)

1. Added `--due-date` and `--priority` options to `edit` command
2. Added `urgent` priority level (priority 1, highest)
3. Updated Priority enum to use `intValue` for proper int mapping
4. Updated display to show "urgent" for priority 1 reminders

## Homebrew Distribution

This fork is distributed via:
- Tap: `danielhopkins/formulae`
- Formula: `reminders-cli`
- Install: `brew install danielhopkins/formulae/reminders-cli`

To release a new version:
1. Tag the release: `git tag X.Y.Z && git push fork X.Y.Z`
2. Build release: `swift build -c release`
3. Create tarball: `tar -czvf reminders.tar.gz reminders`
4. Create GitHub release with tarball
5. Update formula SHA256 in homebrew-formulae repo
