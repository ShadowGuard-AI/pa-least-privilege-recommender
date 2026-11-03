# pa-least-privilege-recommender
Monitors a named process with lsof and suggests the least privileged set of permissions for the files that process touches, based on recorded activity. The recommendations describe the usage of the monitored process itself: for each file, the recommendation is the union of the access modes lsof reported for that process's descriptors during monitoring (read, write, or both). They do not describe the operator running the tool, and they are not computed from the operator's own access to the files. Uses `psutil` to check the monitored process and `rich` for the results table. - Focused on Tools for analyzing and assessing file system permissions

The tool exits 1 when no file access was observed during monitoring, and in that case it writes no output file. It also exits 1 when the output path is the same file as the baseline path, which it refuses to overwrite, and when a named baseline file cannot be read at all. Baseline lines are parsed on their last colon, so paths may contain colons; malformed baseline lines are skipped and the number skipped is reported.

## Install
`git clone https://github.com/ShadowGuard-AI/pa-least-privilege-recommender`
`pip install -r requirements.txt`

## Usage
`python3 main.py --user USER --pid PID [--group GROUP] [--duration SECONDS] [--output FILE] [--baseline FILE]`

## Parameters
- `-h`: Show help message and exit
- `--user`: The username to analyze. Required.
- `--group`: The groupname to analyze. Optional.
- `--pid`: The process ID to monitor. Required.
- `--duration`: The duration (in seconds) to monitor the process activity (default: 60).
- `--output`: The file to write recommended permissions to. Optional.
- `--baseline`: Path to a file containing a baseline ACL to compare against. Optional.

## Verified behavior
- `python3 -m py_compile main.py` passes, but `python3 main.py --help` fails in this sandbox at import: `psutil` is not installed, and installs are forbidden in this wave. The parameter list above is taken from the argument parser in `main.py`, and no live monitoring is claimed here. A live run also needs the `lsof` command, which is present here.
- Limits, read from the code: monitoring is a fresh `lsof` poll every second, not a continuous watch, so a file opened and closed between polls never appears. The --user and --group arguments are accepted but barely used: only the user emptiness check reads one of them, and nothing filters monitoring by owner. The PID alone decides whose files are watched.

## License
See LICENSE.
