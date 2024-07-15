# BASH COMMANDS

## BASIC COMMANDS

### Create file

`<filename>.sh`

### Run file

`bash filename.sh`

### Search command history

`Ctrl+r`

### Rerun last command

`!!`, e.g. `sudo !!`

### Fix last command

`fc`

### Find out type

`type ...`, e.g. `type time`, `type ping`  
Some commands are 'builtins': funcions inside the bash program, e.g. `type`, `alias`, `read`, `source`, `declare`, `printf`, `echo`, `cd`

### Find out which binary is being used

`which <command>`, e.g. `which ls`

### Beginning of line

`Ctrl+a`

### End of line

`Ctrl+e`

### Clear screen

`Ctrl+l`

### Suspend running program

`Ctrl+z`

### Start suspended program in background

`bg`

### Bring suspended/backgrounded program to the foreground

`fg`

## NAVIGATING

### Change to last dir

`cd -`

## DIRECTORIES

### Create directory

`mkdir <name>`

### Delete directory

`rmdir <name>`

## FILES

### List files in temporary folder

`ls /tmp`

### Create file

`touch <file>`
`touch file-$(date -I).txt` // file-2024-07-15.txt

## VARIABLES / ARGUMENTS

Always quote variables! `"$1"`

### Running a script

`./script.sh panda banana` --> `$1` is `panda`, `$2` is `banana`

### Get all arguments

`${@}`, e.g. `ls --color "${@}"`

### Loop over all arguments

`for i in "${@}" do ... done`

### Print date:

`$(date)` // Mo, 15. Jul 2024 17:37:53  
`$(date -I)` // 2024-07-15

## WORK WITH TEXT

### Replace text in file

`sed -i 's/<origin_text>/<new_text>/g' <path_to_file>`  
e.g. `sed -i 's/World/Andy/g' ./hello.txt`

## LOOPS

### For loops

`for i in *.png  
do  
    convert $i $i.jpg
done`

### `{}`

e.g. `convert file.{jpg, png}` expands to `convert file.jpg file png`  
`{1..5}` expands to `1 2 3 4 5`

## PROCESSES

### Process substitution (alternative to pipes)

`<(COMMAND)`, e.g. `diff <(ls) <(ls -a)`

### Kill

`kill -SIGNAL PID`, `SIGNAL` is name or number, e.g. `kill -7`  
`pgrep` prints PIDs of matching running programs.  
`pgrep fire` matches `firefox` and `firebird`, but not `bash firefox.sh`  
To search the whole command line (e.g. `bash firefox.sh`) use `pgrep -f`

### killall

`killall -SIGNAL NAME`, e.g. `killall firefox`  
flag `-w` waits for all signaled processes to die.  
flag `-i` ask before signalling.  
`pkill`, e.g. `pkill -f firefox`

## BRACKETS

### Run command in subshell

`(cd ~/-music; pwd)`

### Group commands (runs in same process)

`{cd ~/-music; pwd; }`

### Arithmetic

`x=$((2+2))`

### Evaluate statements

`if [ ... ]`  
`if [[ ... ]]` (is bash syntax, is more powerful than `[]`)

### Brace expansion

`a{.png, .svg}` --> expands to `a.png a.svg`

## OTHER USEFUL STUFF

### Alias

Set up shorthand commands with `alias`: `alias gc="git commit"` in `~/.bashrc` (runs when bash starts).

### Source

`bash script.sh` runs `script.sh` in a subprocess, so you can't use its variables and functions.  
`source script.sh` is like pasting the contents of `script.sh`.
