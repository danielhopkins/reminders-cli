# reminders-cli

A simple CLI for interacting with macOS Reminders.

> **Note:** This is a fork of [keith/reminders-cli](https://github.com/keith/reminders-cli) with additional features:
> - Edit due dates and priorities on existing reminders
> - New `urgent` priority level (highest priority)

## Usage

#### Show all lists

```
$ reminders show-lists
Soon
Eventually
```

#### Show reminders on a specific list

```
$ reminders show Soon
0 Write README
1 Ship reminders-cli
```

#### Complete an item on a list

```
$ reminders complete Soon 0
Completed 'Write README'
$ reminders show Soon
0 Ship reminders-cli
```

#### Undo a completed item

```
$ reminders show Soon --only-completed
0 Write README
$ reminders uncomplete Soon 0
Uncompleted 'Write README'
$ reminders show Soon
0 Write README
```

#### Add a reminder to a list

```
$ reminders add Soon Contribute to open source
$ reminders add Soon Go to the grocery store --due-date "tomorrow 9am"
$ reminders add Soon Something really important --priority high
$ reminders add Soon Drop everything --priority urgent
$ reminders show Soon
0: Ship reminders-cli
1: Contribute to open source
2: Go to the grocery store (in 10 hours)
3: Something really important (priority: high)
4: Drop everything (priority: urgent)
```

#### Edit an item on a list

Edit the text of a reminder:

```
$ reminders edit Soon 0 Some edited text
Updated reminder 'Some edited text'
```

Edit the due date:

```
$ reminders edit Soon 0 --due-date "tomorrow 9am"
Updated reminder 'Some edited text'
$ reminders show Soon
0: Some edited text (in 10 hours)
```

Edit the priority:

```
$ reminders edit Soon 0 --priority urgent
Updated reminder 'Some edited text'
$ reminders show Soon
0: Some edited text (in 10 hours) (priority: urgent)
```

Edit notes:

```
$ reminders edit Soon 0 --notes "Remember to bring the documents"
Updated reminder 'Some edited text'
```

You can combine multiple edits:

```
$ reminders edit Soon 0 --due-date 2025-12-25 --priority high --notes "Holiday task"
```

#### Delete an item on a list

```
$ reminders delete Soon 0
Deleted 'Write README'
$ reminders show Soon
0 Ship reminders-cli
```

#### Show reminders due on or by a date

```
$ reminders show-all --due-date today
1: Contribute to open source (in 3 hours)
$ reminders show-all --due-date today --include-overdue
0: Ship reminders-cli (2 days ago)
1: Contribute to open source (in 3 hours)
$ reminders show-all --due-date 2025-02-16
1: Contribute to open source (in 3 hours)
$ reminders show Soon --due-date today --include-overdue
0: Ship reminders-cli (2 days ago)
1: Contribute to open source (in 3 hours)
```

#### Priority levels

The following priority levels are available (from highest to lowest):

| Priority | Description |
|----------|-------------|
| `urgent` | Highest priority (priority 1) |
| `high`   | High priority (priority 2-4) |
| `medium` | Medium priority (priority 5) |
| `low`    | Low priority (priority 6-9) |
| `none`   | No priority (default) |

#### See help for more examples

```
$ reminders --help
$ reminders show -h
$ reminders edit -h
```

## Installation

#### With [Homebrew](http://brew.sh/)

```
$ brew install danielhopkins/formulae/reminders-cli
```

To install the original version (without edit enhancements):

```
$ brew install keith/formulae/reminders-cli
```

#### From GitHub releases

Download the latest release from
[here](https://github.com/danielhopkins/reminders-cli/releases)

```
$ tar -zxvf reminders.tar.gz
$ mv reminders /usr/local/bin
$ rm reminders.tar.gz
```

#### Building manually

This requires a recent Xcode installation.

```
$ cd reminders-cli
$ swift build -c release
$ cp .build/release/reminders /usr/local/bin/reminders
```

## License

MIT - See [LICENSE](LICENSE) file.
