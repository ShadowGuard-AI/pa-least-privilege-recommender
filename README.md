# pa-least-privilege-recommender
Monitors a named process with lsof and suggests the least privileged set of permissions for the files that process touches, based on recorded activity. The recommendations describe the usage of the monitored process itself: for each file, the recommendation is the union of the access modes lsof reported for that process's descriptors during monitoring (read, write, or both). They do not describe the operator running the tool, and they are not computed from the operator's own access to the files. Uses `psutil` to check the monitored process and `rich` for the results table. - Focused on Tools for analyzing and assessing file system permissions

The tool exits 1 when no file access was observed during monitoring, and in that case it writes no output file. It also exits 1 when the output path is the same file as the baseline path, which it refuses to overwrite, and when a named baseline file cannot be read at all. Baseline lines are parsed on their last colon, so paths may contain colons; malformed baseline lines are skipped and the number skipped is reported.

## Install
`git clone https://github.com/ShadowStrikeHQ/pa-least-privilege-recommender`

## Usage
`./pa-least-privilege-recommender [params]`

## Parameters
- `-h`: Show help message and exit
- `--user`: The username to analyze.
- `--group`: The groupname to analyze.
- `--pid`: The process ID to monitor.
- `--duration`: No description provided
- `--output`: The file to write recommended permissions to.
- `--baseline`: Path to a file containing a baseline ACL to compare against.

## License
Copyright (c) ShadowStrikeHQ
