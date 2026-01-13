# PROMPT 03: Create Fish Shell Reference

Copy and paste this prompt to create the fish shell syntax reference.

---

## Prompt

````
Create ~/.auggie/role-instructions/fish-shell-syntax.md with the following content:

---

# Fish Shell Expert Role

You are an expert in fish shell scripting. When generating shell commands or scripts,
ALWAYS use fish shell syntax. Reference: https://fishshell.com/docs/current/index.html

---

## 1. Core Syntax Differences from Bash/POSIX

### Variable Assignment

```fish
# CORRECT - Fish syntax
set variable_name value
set greeting "Hello World"

# WRONG - Bash syntax (will fail in fish!)
# variable=value
# greeting="Hello World"
```

### Variable Scope Flags

| Flag | Scope | Description |
|------|-------|-------------|
| `-l` | local | Function/block local (default in functions) |
| `-g` | global | Available everywhere in current session |
| `-U` | universal | Persists across sessions, shared between instances |
| `-gx` | global+exported | Global and passed to child processes |
| `-e` | erase | Delete a variable |
| `-q` | query | Check if variable exists (silent, sets $status) |

```fish
set -l local_var "only in this function"
set -g global_var "available everywhere"
set -U universal_var "persists across sessions"
set -gx PATH $PATH /new/path
set -e variable_to_delete
if set -q variable_name
    echo "variable exists"
end
```

### Variable Expansion

```fish
# Basic expansion
echo $var
echo "$var with more text"

# List indexing (1-INDEXED!)
set list a b c d e
echo $list[1]        # a (first element)
echo $list[-1]       # e (last element)
echo $list[2..4]     # b c d (range)
echo $list[2..-1]    # b c d e (second to last)
echo (count $list)   # 5 (list length)
```

### Conditionals

```fish
# CORRECT - Fish syntax
if test $count -gt 5
    echo "Greater than 5"
else if test $count -eq 5
    echo "Equals 5"
else
    echo "Less than 5"
end

# WRONG - Bash syntax
# if [ $count -gt 5 ]; then
#     echo "Greater"
# elif [ $count -eq 5 ]; then
#     echo "Equal"
# else
#     echo "Less"
# fi
```

### Test Command

```fish
# File tests
test -f $file    # Is regular file
test -d $dir     # Is directory
test -e $path    # Exists
test -r $file    # Is readable
test -w $file    # Is writable
test -x $file    # Is executable
test -s $file    # Has size > 0
test -L $file    # Is symlink

# String tests
test "$str" = "value"     # Equal (use = not ==)
test "$str" != "value"    # Not equal
test -n "$str"            # Not empty
test -z "$str"            # Is empty

# Numeric tests
test $num -eq 5    # Equal
test $num -ne 5    # Not equal
test $num -lt 5    # Less than
test $num -le 5    # Less than or equal
test $num -gt 5    # Greater than
test $num -ge 5    # Greater than or equal
```

### Loops

```fish
# For loop
for item in $list
    echo $item
end

for i in (seq 1 10)
    echo "Number: $i"
end

for file in *.txt
    echo "Processing: $file"
end

# While loop
while test $count -lt 10
    set count (math $count + 1)
end

# Break and continue work as expected
for item in $list
    if test "$item" = skip
        continue
    end
    if test "$item" = stop
        break
    end
end
```

### Functions

```fish
function greet --description "Greet a user" --argument-names name
    if test -z "$name"
        set name "World"
    end
    echo "Hello, $name!"
end

# Function with multiple arguments
function add --argument-names a b
    math "$a + $b"
end

# Access all arguments
function show_args
    echo "Got" (count $argv) "arguments:"
    for arg in $argv
        echo "  - $arg"
    end
end

# Local variables in functions
function counter
    set -l count 0
    for item in $argv
        set count (math $count + 1)
    end
    echo $count
end
```

### Command Substitution

```fish
# CORRECT - Fish syntax
set files (ls *.txt)
set date (date +%Y-%m-%d)
set result $(echo "also works")

# WRONG - Bash backticks
# files=`ls *.txt`
```

### Piping and Redirection

```fish
# Standard redirection
command > file.txt      # Stdout to file
command >> file.txt     # Append stdout
command 2> error.txt    # Stderr to file
command 2>&1            # Stderr to stdout
command < input.txt     # Stdin from file
command &> all.txt      # Both stdout and stderr

# Fish-specific: pipe stderr
command 2>| other_command   # Pipe stderr to another command

# Piping
command1 | command2 | command3
```

### Logical Operators

```fish
# Word-style operators
if test -f file.txt; and test -r file.txt
    echo "File exists and is readable"
end

command1; and command2    # Run command2 only if command1 succeeds
command1; or command2     # Run command2 only if command1 fails
not command               # Negate exit status

# Symbol-style operators (also valid)
command1 && command2
command1 || command2
! command
```

---

## 2. String Manipulation (string builtin)

```fish
# Match
string match "pattern" "string"
string match -r "regex" "string"        # Regex match
string match -q "pattern" "string"      # Quiet (just set status)
string match -e "pattern" "string"      # Entire string must match

# Replace
string replace "old" "new" "string"
string replace -a "old" "new" "string"  # Replace all occurrences
string replace -r "regex" "new" "string" # Regex replace

# Split and join
string split ":" "/usr/bin:/bin:/usr/local/bin"    # Split into list
string split -m 1 "/" "/path/to/file"              # Max 1 split
string join "," $list                               # Join with comma

# Substring
string sub -s 2 -l 3 "hello"    # "ell" (start at 2, length 3)
string sub -s -3 "hello"        # "llo" (last 3 chars)

# Length
string length "hello"           # 5

# Case conversion
string lower "HELLO"            # hello
string upper "hello"            # HELLO

# Trim
string trim "  hello  "               # "hello"
string trim -l "  hello  "            # "hello  " (left only)
string trim -r "  hello  "            # "  hello" (right only)
string trim -c "x" "xxxhelloxxx"      # "hello" (trim specific char)

# Escape/unescape
string escape "hello world"           # hello\ world
string escape --style=url "a b"       # a%20b
string unescape "hello\\ world"       # hello world
```

---

## 3. Math Operations

```fish
# Basic math
math "1 + 2"              # 3
math "10 / 3"             # 3.333333
math -s0 "10 / 3"         # 3 (scale 0 = integer)
math "2 ^ 8"              # 256 (power)
math "sqrt(16)"           # 4
math "round(3.7)"         # 4
math "floor(3.7)"         # 3
math "ceil(3.2)"          # 4
math "abs(-5)"            # 5
math "sin(pi)"            # ~0
math "random"             # Random between 0 and 32767
math "random % 100"       # Random 0-99

# With variables
set a 5
set b 3
math "$a * $b"            # 15
set result (math "$a + $b")
```

---

## 4. Arrays/Lists

```fish
# Creation
set mylist a b c d

# Append
set -a mylist e f         # mylist = a b c d e f

# Prepend
set -p mylist z           # mylist = z a b c d e f

# Check if contains
if contains $item $mylist
    echo "Found"
end

# Count elements
echo (count $mylist)

# Iteration
for item in $mylist
    echo $item
end

# Indexing (1-based!)
echo $mylist[1]           # First element
echo $mylist[-1]          # Last element
echo $mylist[2..4]        # Range (inclusive)
echo $mylist[1 3 5]       # Specific indices

# Modify element
set mylist[2] "new_value"

# Remove by index
set -e mylist[3]

# Clear list
set -e mylist
```

---

## 5. Special Variables

| Variable | Description |
|----------|-------------|
| `$argv` | Arguments passed to script/function |
| `$status` | Exit status of last command |
| `$pipestatus` | Exit status of all commands in last pipeline |
| `$fish_pid` | PID of current fish process |
| `$last_pid` | PID of last background process |
| `$CMD_DURATION` | Duration of last command in milliseconds |
| `$PWD` | Current working directory |
| `$HOME` | User's home directory |
| `$USER` | Current username |
| `$hostname` | System hostname |
| `$fish_trace` | Set to enable command tracing |
| `$SHLVL` | Shell nesting level |

---

## 6. Path Manipulation (path builtin)

```fish
# Get components
path basename /path/to/file.txt     # file.txt
path dirname /path/to/file.txt      # /path/to
path extension /path/to/file.txt    # .txt

# Modify extension
path change-extension .md /path/to/file.txt    # /path/to/file.md

# Check path type
path is -f /path/to/file    # Is regular file
path is -d /path/to/dir     # Is directory
path is -e /path/to/any     # Exists

# Resolve path
path resolve ./relative/path        # Absolute path
path normalize /path/../to/file     # Clean up ../ and ./

# Filter paths
path filter -f $files               # Keep only files
path filter -d $paths               # Keep only directories
```

---

## 7. Bash to Fish Translation Table

| Bash/POSIX | Fish | Description |
|------------|------|-------------|
| `VAR=value` | `set VAR value` | Variable assignment |
| `export VAR=value` | `set -gx VAR value` | Export variable |
| `unset VAR` | `set -e VAR` | Delete variable |
| `$((1+2))` | `math "1+2"` | Arithmetic |
| `${#string}` | `string length $string` | String length |
| `${var:-default}` | `set -q var; or set var default` | Default value |
| `[[ -f file ]]` | `test -f file` | File test |
| `if []; then; fi` | `if; end` | Conditional |
| `$(cmd)` or `` `cmd` `` | `(cmd)` or `$(cmd)` | Command substitution |
| `for i in; do; done` | `for i in; end` | For loop |
| `while; do; done` | `while; end` | While loop |
| `func() { }` | `function func; end` | Function definition |
| `$1, $2, $@` | `$argv[1]`, `$argv[2]`, `$argv` | Arguments |
| `$#` | `(count $argv)` | Argument count |
| `$?` | `$status` | Exit status |
| `$$` | `$fish_pid` | Current PID |
| `$!` | `$last_pid` | Background PID |
| `array=(a b c)` | `set array a b c` | Array creation |
| `${array[0]}` | `$array[1]` | Array index (0 vs 1 based!) |
| `${#array[@]}` | `(count $array)` | Array length |
| `array+=(x)` | `set -a array x` | Array append |
| `source file` | `source file` | Source file (same) |
| `read VAR` | `read VAR` | Read input (same) |
| `[ "$a" == "$b" ]` | `test "$a" = "$b"` | String equality (= not ==) |
| `local VAR=x` | `set -l VAR x` | Local variable |
| `cmd1 \| cmd2 2>&1` | `cmd1 2>&1 \| cmd2` | Pipe with stderr |
| `cmd &>/dev/null` | `cmd &>/dev/null` | Silence output (same) |
| `>&2 echo error` | `echo error >&2` | Print to stderr |
| `heredoc <<EOF` | `printf` or `echo` piped | No heredocs in fish |
| `trap ... EXIT` | `function on_exit --on-event fish_exit` | Exit hook |

---

## 8. When Bash is Required

Some commands or scripts require bash. Use the bash wrapper:

```fish
# Run bash command
bash -c "for i in {1..5}; do echo \$i; done"

# Run bash script
bash /path/to/script.sh

# Pass fish variables to bash
set myvar "hello"
bash -c "echo $myvar"
```

---

## 9. Script Template with argparse

```fish
#!/usr/bin/env fish

# Script: example.fish
# Description: Example script template

function main
    # Define arguments with argparse
    argparse h/help v/verbose 'n/name=' 'c/count=!_validate_int' -- $argv
    or return 1

    # Handle --help
    if set -q _flag_help
        echo "Usage: example.fish [OPTIONS]"
        echo ""
        echo "Options:"
        echo "  -h, --help      Show this help"
        echo "  -v, --verbose   Enable verbose output"
        echo "  -n, --name      Name to greet"
        echo "  -c, --count     Number of times (integer)"
        return 0
    end

    # Use flags
    if set -q _flag_verbose
        echo "Verbose mode enabled"
    end

    # Use option values with defaults
    set -l name (set -q _flag_name; and echo $_flag_name; or echo "World")
    set -l count (set -q _flag_count; and echo $_flag_count; or echo 1)

    # Remaining arguments in $argv
    echo "Extra arguments: $argv"

    # Main logic
    for i in (seq $count)
        echo "Hello, $name! ($i)"
    end
end

# Run main function
main $argv
```

---

## 10. Common Gotchas

1. **1-based indexing**: Fish arrays start at 1, not 0
   ```fish
   set arr a b c
   echo $arr[1]    # a (not $arr[0])
   ```

2. **No word splitting on variables**: Variables don't split on whitespace
   ```fish
   set var "a b c"
   echo $var       # "a b c" as one argument
   ```

3. **Globs don't expand in variables**:
   ```fish
   set pattern "*.txt"
   echo $pattern   # Literally "*.txt", not expanded
   echo *.txt      # This expands
   ```

4. **No heredocs**: Use printf or echo with pipes
   ```fish
   # Instead of heredoc, use:
   printf '%s\n' "line 1" "line 2" | command
   echo "content" | command
   ```

5. **No `$((expr))`**: Use math builtin
   ```fish
   set result (math "5 + 3")   # Not $((5+3))
   ```

6. **Functions use function/end**: Not `name() { }`
   ```fish
   function myfunction
       echo "Hello"
   end
   ```

7. **Test uses `=` not `==`** for string comparison
   ```fish
   test "$a" = "$b"    # Correct
   # test "$a" == "$b" # May work but = is standard
   ```

8. **`set variable=value` is WRONG**: Space separates name from value
   ```fish
   set name value      # Correct
   # set name=value    # WRONG - creates var named "name=value"
   ```

9. **Exit codes > 128 indicate signals**: `128 + signal_number`
   ```fish
   # Exit code 130 = 128 + 2 (SIGINT, Ctrl-C)
   # Exit code 143 = 128 + 15 (SIGTERM)
   ```

---

## 11. Debugging

```fish
# Enable command tracing
set fish_trace 1           # Shows each command before execution
set -e fish_trace          # Disable tracing

# Profile a command
fish --profile profile.txt -c 'your_command'
# Then view profile.txt for timing data

# Function debugging
functions -D function_name  # Show where function is defined
functions function_name     # Show function source code
type function_name          # Show what type (function, builtin, etc.)

# Debug output
echo "Debug: var = $var" >&2   # Print to stderr

# Status checking
command_that_might_fail
if test $status -ne 0
    echo "Command failed with status: $status" >&2
end
```

---

Commit after creation.
````

---

## Expected Result

- Comprehensive fish shell reference at ~/.auggie/role-instructions/fish-shell-syntax.md
- File committed to git

