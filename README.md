# mac-awake
Designed specifically for use on an Apple system, like a MacBook.  This
command-line tool will keep your Mac awake and connected to the network for
a specified time, it's essentially a wrapper around Mac's CLI tool `caffeinate`
to make it easier to specify upcoming times to stay awake for, or durations
without needing to mental math exact seconds.

Designed in `BASH` version 5.3.3, the only backwards compatibility issue I've
found is my '|' usage in case statements, but check out the 'back_compat'
branch if version issues arise, that branch was designed for use in `BASH`
3.2.57

## Installation
From a personal style, I store my own 'additional' local scripts and binaries in
`$HOME/.local/bin`, with my additional man pages in `$HOME/.local/share/man/man1`.
That's how I do it, but if you don't want that style, you can edit the variables in
the `globals` script in `src/scripts` to change the paths and final script name, here's
a rundown of the variables to change and what happens:
| Global Variable    | Use                                                              | Default (change to your need) |
|--------------------|------------------------------------------------------------------|-------------------------------|
| `INSTALL_DIR`      | Directory where the final script is stored (should be on `PATH`) | `$HOME/.local/bin`            |
| `SCRIPT_NAME`      | Name you want for the final script you can call                  | `mac_awake`                   |
| `MAN_DIR`          | Directory where to locally store the man file                    | `$HOME/.local/share/man/man1` |

From clone to setup:
```bash
git clone https://github.com/Vladimir-Herdman/mac-awake
cd mac-awake
./scripts/install
```

If you don't want to use the install script, you can also just copy the file to
somewhere on your system PATH:
```bash
cp ./src/mac_awake <wherever you store scripts on PATH>
```

## CLI Options
| Option                    | Description                                           |
|---------------------------|-------------------------------------------------------|
| `-h, --help`              | Show usage information for script                     |
| `-d, --displayoff`        | Turn off display to save power, still awake           |
| `-s, --sleepafter`        | Once command finishes, enter sleep mode to save power |
| `-f, --for TIME`          | Stay awake for TIME duration                          |
| `-u, --until TIME [DATE]` | Stay awake until TIME and DATE occurs (DATE optional) |

## Option Values
As a wrapper around Mac's caffeinate to simplify duration and specified time's instead
of doing mental seconds math,you can pass *TIME* and optional *DATE* values to the `-f`
and `-u` options.
- **TIME**
  - for `-f`, time follows colon seperated format, so 24 is 24 seconds, 2:30 is 2 minutes and 30 seconds, 1:14:20 is 1 hour, 14 minutes, and 20 seconds
  - for `-u`, uses military format to specify time, or append pm/am to simplfy, so: `13:40:00` will be 1:40pm, or you can just pass `1:40pm` for same result.  Pretty flexible!
- **DATE**
  - Should be in YYYY/MM/DD, DD/MM/YYYY, or MM/DD/YYYY format, can use '-' instead of '/'.  If unspecified, command uses current day

## Example Use
Stay awake until 1:14pm on August 24th:

      mac_awake -u 13:14:21 2025-08-24

Stay awake for 2 hours, 15 minutes, and 10 seconds

      mac_awake -f 2:15:10

Stay awake for 20 minutes, but immediately dim the screen on command start,
and enter sleep mode once finished:
  
      mac_awake -d -s -f 20:0

It's 3:14pm, stay awake until 3:30pm, then sleep on finish:

      mac_awake -s -u 3:30pm

Same as just above, but instantly dim screen on start (still awake):

      mac_awake -d -s -u 3:30pm
