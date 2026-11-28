# Regex

| Regex    | Meaning                                                  | Example    | Matches          |
| -------- | -------------------------------------------------------- | ---------- | ---------------- |
| `.`      | Any single character (except newline by default)         | `c.t`      | cat, cut, cot    |
| `*`      | Zero or more repetitions of the previous character/group | `ab*`      | a, ab, abb       |
| `+`      | One or more repetitions                                  | `ab+`      | ab, abb          |
| `?`      | Zero or one repetition                                   | `colou?r`  | color, colour    |
| `^`      | Beginning of a line                                      | `^import`  | import os        |
| `$`      | End of a line                                            | `;$`       | print("Hi");     |
| `[abc]`  | Any one character inside brackets                        | `[abc]at`  | cat, bat, aat    |
| `[a-z]`  | Any lowercase letter in range                            | `[a-z]+`   | hello            |
| `[0-9]`  | Any digit from 0 to 9                                    | `[0-9]+`   | 123, 42          |
| `[^abc]` | Any character except those listed                        | `[^0-9]`   | letters, symbols |
| `\d`     | Digit (ripgrep default regex)                            | `\d+`      | 123              |
| `\w`     | Word character                                           | `\w+`      | hello_123        |
| `\s`     | Whitespace character                                     | `\s+`      | spaces, tabs     |
| `()`     | Group expressions together, treating them as one unit    | `(ab)+`    | ab, abab, ababab |
| `\|`     | OR (alternation)                                         | `cat\|dog` | cat, dog         |
| `{n}`    | Exactly n repetitions                                    | `a{3}`     | aaa              |
| `{n,m}`  | Between n and m repetitions                              | `a{2,4}`   | aa, aaa, aaaa    |
| `\`      | Escape a special character                               | `\.`       | A literal dot    |

# Bash Wildcards (Globbing)

| Pattern        | Meaning                                                            | Example               | Matches                                                    |
| -------------- | ------------------------------------------------------------------ | --------------------- | ---------------------------------------------------------- |
| `*`            | Zero or more characters                                            | `*.txt`               | file.txt, notes.txt                                        |
| `?`            | Exactly one character                                              | `file?.txt`           | file1.txt, fileA.txt                                       |
| `[abc]`        | Any one character listed                                           | `file[123].txt`       | file1.txt, file2.txt                                       |
| `[a-z]`        | Any lowercase letter in range                                      | `file[a-z].txt`       | filea.txt, fileb.txt                                       |
| `[A-Z]`        | Any uppercase letter in range                                      | `file[A-Z].txt`       | fileA.txt, fileB.txt                                       |
| `[0-9]`        | Any digit from 0 to 9                                              | `file[0-9].txt`       | file1.txt, file2.txt                                       |
| `[!abc]`       | Any one character except those listed                              | `file[!0].txt`        | file1.txt, file2.txt                                       |
| `[!0-9]`       | Any one character except digits                                    | `file[!0-9].txt`      | filea.txt, fileb.txt                                       |
| `[[:digit:]]`  | Any single digit                                                   | `file[[:digit:]].txt` | file1.txt, file2.txt                                       |
| `[[:digit:]]*` | Any digit followed by zero or more characters                      | `[[:digit:]]*`        | 123.txt, 1file                                             |
| `[[:alpha:]]`  | Any single alphabetic character                                    | `[[:alpha:]]`         | a, B, z                                                    |
| `[[:alpha:]]*` | Any alphabetic character followed by zero or more characters       | `[[:alpha:]]*`        | apple, banana, hello123                                    |
| `[[:lower:]]`  | Any single lowercase letter                                        | `[[:lower:]]`         | a, b, z                                                    |
| `[[:lower:]]*` | Any lowercase letter followed by zero or more characters           | `[[:lower:]]*`        | apple, banana, hello123                                    |
| `[[:upper:]]`  | Any single uppercase letter                                        | `[[:upper:]]`         | A, B, Z                                                    |
| `[[:upper:]]*` | Any uppercase letter followed by zero or more characters           | `[[:upper:]]*`        | Apple, README.md                                           |
| `[[:alnum:]]`  | Any single alphanumeric character                                  | `[[:alnum:]]`         | a, 1, B                                                    |
| `[[:alnum:]]*` | Any alphanumeric character followed by zero or more characters     | `[[:alnum:]]*`        | file1, abc123                                              |
| `[[:space:]]`  | Any single whitespace character                                    | `*[[:space:]]*`       | my file.txt                                                |
| `*.txt`        | `.txt` files in the current directory                              | `*.txt`               | notes.txt, todo.txt                                        |
| `*/*.txt`      | `.txt` files exactly one directory deep                            | `*/*.txt`             | python/lesson.txt, linux/commands.txt                      |
| `**/*.txt`     | `.txt` files at any directory depth (requires `shopt -s globstar`) | `**/*.txt`            | notes.txt, python/lesson.txt, linux/scripts/cheatsheet.txt |
| `{a,b}`        | Brace expansion: generates alternatives                            | `{cat,dog}.txt`       | cat.txt, dog.txt                                           |
| `{1..5}`       | Brace expansion: generates a numeric sequence                      | `file{1..5}.txt`      | file1.txt through file5.txt                                |
| `{a..e}`       | Brace expansion: generates a letter sequence                       | `file{a..e}.txt`      | filea.txt through filee.txt                                |
| `\*`           | Matches a literal asterisk                                         | `file\*.txt`          | file*.txt                                                  |

# grep / ripgrep flags

| Flag   | Meaning                               | grep Example             | ripgrep Example        |
| ------ | ------------------------------------- | ------------------------ | ---------------------- |
| `-i`   | Case-insensitive search               | `grep -i "hello" file`   | `rg -i "hello" file`   |
| `-n`   | Show line numbers                     | `grep -n "hello" file`   | `rg -n "hello" file`   |
| `-r`   | Search recursively                    | `grep -r "hello" .`      | `rg -r "hello" .`      |
| `-w`   | Match whole words only                | `grep -w "cat" file`     | `rg -w "cat" file`     |
| `-v`   | Invert match (exclude matching lines) | `grep -v "error" file`   | `rg -v "error" file`   |
| `-c`   | Count matching lines                  | `grep -c "error" file`   | `rg -c "error" file`   |
| `-l`   | Show filenames with matches           | `grep -rl "error" .`     | `rg -l "error" .`      |
| `-o`   | Show only matching parts              | `grep -o "cat" file`     | `rg -o "cat" file`     |
| `-E`   | Use extended regular expressions      | `grep -E "cat            | dog" file`             | Not needed (default) |
| `-F`   | Search fixed strings (no regex)       | `grep -F "a.b" file`     | `rg -F "a.b" file`     |
| `-A n` | Show n lines after match              | `grep -A 2 "error" file` | `rg -A 2 "error" file` |
| `-B n` | Show n lines before match             | `grep -B 2 "error" file` | `rg -B 2 "error" file` |
| `-C n` | Show n lines before and after         | `grep -C 2 "error" file` | `rg -C 2 "error" file` |
| `-m n` | Stop after n matching lines           | `grep -m 5 "error" file` | `rg -m 5 "error" file` |

## ripgrep-specific options

| Flag                    | Meaning                                                  | Example                              |
| ----------------------- | -------------------------------------------------------- | ------------------------------------ |
| `-t`                    | Search a specific file type                              | `rg -t py "import" .`                |
| `-g`                    | Include/exclude files using glob patterns                | `rg -g "*.py" "import" .`            |
| `--hidden`              | Include hidden files                                     | `rg --hidden "config" .`             |
| `--no-ignore`           | Search files ignored by gitignore and other ignore rules | `rg --no-ignore "test" .`            |
| `-uu`                   | Disable hidden-file and ignore filtering                 | `rg -uu "test" .`                    |
| `--stats`               | Display search statistics                                | `rg --stats "error" .`               |
| `--files`               | List files that ripgrep can search                       | `rg --files`                         |
| `--files-with-matches`  | List files containing matches                            | `rg -l "error" .`                    |
| `--files-without-match` | List files without matches                               | `rg --files-without-match -g "*.py"` |
| `-e`                    | Specify a search pattern explicitly                      | `rg -e "cat" -e "dog" .`             |

# find/fd

```bash
find [PATH] [OPTIONS] [EXPRESSION]

fd [OPTIONS] [PATTERN] [PATH]
```

## find-specific expressions

| Expression    | Meaning                           | Example                                   |
| ------------- | --------------------------------- | ----------------------------------------- |
| `-name`       | Match filename (case-sensitive)   | `find . -name "*.py"`                     |
| `-iname`      | Match filename (case-insensitive) | `find . -iname "readme*"`                 |
| `-type f`     | Find regular files                | `find . -type f`                          |
| `-type d`     | Find directories                  | `find . -type d`                          |
| `-type l`     | Find symbolic links               | `find . -type l`                          |
| `-path`       | Match the entire path             | `find . -path "*/config/*"`               |
| `-regex`      | Match path using regex            | `find . -regex ".*\.py"`                  |
| `-size`       | Filter by file size               | `find . -size +100M`                      |
| `-mtime`      | Filter by modification days       | `find . -mtime -1`                        |
| `-mmin`       | Filter by modification minutes    | `find . -mmin -10`                        |
| `-empty`      | Find empty files/directories      | `find . -empty`                           |
| `-executable` | Find executable files             | `find . -executable`                      |
| `-maxdepth`   | Maximum directory depth           | `find . -maxdepth 2`                      |
| `-mindepth`   | Minimum directory depth           | `find . -mindepth 2`                      |
| `-exec`       | Execute command on results        | `find . -name "*.py" -exec ls -lh {} \;`  |
| `-delete`     | Delete matching results           | `find . -name "*.tmp" -delete`            |
| `-prune`      | Skip specified directories        | `find . -path "./build" -prune -o -print` |
| `-print`      | Print matching paths              | `find . -name "*.py" -print`              |

## fd-specific flags

| Flag            | Meaning                              | Example                      |
| --------------- | ------------------------------------ | ---------------------------- |
| `-e`            | Filter by file extension             | `fd -e py`                   |
| `-t f`          | Find files only                      | `fd -t f "main"`             |
| `-t d`          | Find directories only                | `fd -t d "config"`           |
| `-t l`          | Find symbolic links                  | `fd -t l`                    |
| `-t x`          | Find executable files                | `fd -t x`                    |
| `-i`            | Case-insensitive search              | `fd -i "readme"`             |
| `-s`            | Case-sensitive search                | `fd -s "README"`             |
| `-g`            | Use glob patterns instead of regex   | `fd -g "*.py"`               |
| `-E`            | Exclude files/directories            | `fd -E node_modules`         |
| `-H`            | Include hidden files                 | `fd -H "config"`             |
| `-I`            | Include ignored files                | `fd -I "config"`             |
| `-u`            | Include hidden and ignored files     | `fd -u "config"`             |
| `-d`            | Limit maximum search depth           | `fd -d 2 "main"`             |
| `-x`            | Execute command for each result      | `fd -e py -x ls -lh`         |
| `-X`            | Execute command with all results     | `fd -e py -X ls -lh`         |
| `-a`            | Show absolute paths                  | `fd -a "main"`               |
| `-L`            | Follow symbolic links                | `fd -L "config"`             |
| `-p`            | Match against the full relative path | `fd -p "src/main"`           |
| `--max-results` | Limit number of results              | `fd --max-results 10 "main"` |

## find/fd comparison

| Task                          | find                                        | fd                   |
| ----------------------------- | ------------------------------------------- | -------------------- |
| Find `main.py`                | `find . -name "main.py"`                    | `fd "main.py"`       |
| Find Python files             | `find . -name "*.py"`                       | `fd -e py`           |
| Find directories              | `find . -type d`                            | `fd -t d`            |
| Find files only               | `find . -type f`                            | `fd -t f`            |
| Find symbolic links           | `find . -type l`                            | `fd -t l`            |
| Case-insensitive search       | `find . -iname "readme"`                    | `fd -i "readme"`     |
| Limit depth to 2              | `find . -maxdepth 2`                        | `fd -d 2`            |
| Exclude `build` directory     | `find . -path "./build" -prune -o -print`   | `fd -E build`        |
| Include hidden files          | Included by default                         | `fd -H`              |
| Include ignored files         | Included by default                         | `fd -I`              |
| Find empty files              | `find . -type f -empty`                     | No direct equivalent |
| Find files larger than 100 MB | `find . -type f -size +100M`                | `fd -t f -S +100m`   |
| Find recently modified files  | `find . -type f -mtime -1`                  | No direct equivalent |
| Execute command on results    | `find . -name "*.py" -exec ls -lh {} \;`    | `fd -e py -x ls -lh` |
| Find multiple extensions      | `find . \( -name "*.py" -o -name "*.js" \)` | `fd -e py -e js`     |
| Match filename using regex    | `find . -regex ".*\.py"`                    | `fd "\.py$"`         |
| Find empty directories        | `find . -type d -empty`                     | No direct equivalent |
| Follow symbolic links         | `find -L . -name "config"`                  | `fd -L "config"`     |

# mv/cp

## mv

| Flag     | Meaning                                                  | Example                             |
| -------- | -------------------------------------------------------- | ----------------------------------- |
| `-i`     | Interactive: prompt before overwriting                   | `mv -i file.txt backup/`            |
| `-n`     | No-clobber: do not overwrite existing files              | `mv -n file.txt backup/`            |
| `-v`     | Verbose: show what is being moved                        | `mv -v file.txt backup/`            |
| `-t DIR` | Specify destination directory                            | `mv -t backup/ file1.txt file2.txt` |
| `-b`     | Create a backup of the destination before overwriting it | `mv -b file.txt backup/`            | Existing destination saved as `file.txt~` |

## cp

| Flag     | Meaning                                                       | Example                             |
| -------- | ------------------------------------------------------------- | ----------------------------------- |
| `-i`     | Interactive: prompt before overwriting                        | `cp -i file.txt backup/`            |
| `-n`     | No-clobber: do not overwrite existing files                   | `cp -n file.txt backup/`            |
| `-v`     | Verbose: show what is being copied                            | `cp -v file.txt backup/`            |
| `-r`     | Recursive: copy directories and their contents                | `cp -r folder/ backup/`             |
| `-p`     | Preserve mode, ownership and timestamps                       | `cp -p file.txt backup/`            |
| `-t DIR` | Specify destination directory                                 | `cp -t backup/ file1.txt file2.txt` |
| `-b`     | Create a backup of an existing destination before overwriting | `cp -b file.txt backup/`            |

## Useful backup-related flags (`cp` and `mv`)

| Flag                | Meaning                                                       |
| ------------------- | ------------------------------------------------------------- |
| `-b`                | Create a backup before overwriting                            |
| `--backup`          | Same as `-b`                                                  |
| `--backup=numbered` | Create numbered backups (`file.~1~`, `file.~2~`)              |
| `--backup=existing` | Use numbered backups if they already exist; otherwise use `~` |
| `--suffix=.bak`     | Change the backup suffix from `~` to `.bak`                   |

# head/tail

## head (Display beginning of files)

| Flag    | Meaning                             | Example                       | Result                         |
| ------- | ----------------------------------- | ----------------------------- | ------------------------------ |
| `-n N`  | Display the first N lines           | `head -n 5 file.txt`          | First 5 lines                  |
| `-n -N` | Display all lines except the last N | `head -n -5 file.txt`         | Everything except last 5 lines |
| `-c N`  | Display the first N bytes           | `head -c 20 file.txt`         | First 20 bytes                 |
| `-q`    | Never display filename headers      | `head -q file1.txt file2.txt` | No filename headers            |
| `-v`    | Always display filename headers     | `head -v file.txt`            | Includes filename header       |

## tail (Display end of files)

| Flag        | Meaning                                                  | Example                       | Result                                  |
| ----------- | -------------------------------------------------------- | ----------------------------- | --------------------------------------- |
| `-n N`      | Display the last N lines                                 | `tail -n 5 file.txt`          | Last 5 lines                            |
| `-n +N`     | Display from line N to the end                           | `tail -n +5 file.txt`         | Everything from line 5                  |
| `-c N`      | Display the last N bytes                                 | `tail -c 20 file.txt`         | Last 20 bytes                           |
| `-f`        | Follow file as new lines are added                       | `tail -f app.log`             | Continuously displays new lines         |
| `-F`        | Follow file by name and retry if unavailable or replaced | `tail -F app.log`             | Keeps following a rotated/recreated log |
| `-q`        | Never display filename headers                           | `tail -q file1.txt file2.txt` | No filename headers                     |
| `-v`        | Always display filename headers                          | `tail -v file.txt`            | Includes filename header                |
| `--pid=PID` | Stop following when the specified process exits          | `tail -f --pid=1234 app.log`  | Stops when process 1234 exits           |

# cut

`cut` extracts specific sections (columns) from each line of a file based on delimiters or character positions.

| Flag                        | Meaning                                          | Example                                           | Result                                   |
| --------------------------- | ------------------------------------------------ | ------------------------------------------------- | ---------------------------------------- |
| `-f LIST`                   | Select fields (default delimiter: TAB)           | `cut -f1 file.txt`                                | Extracts the first field                 |
| `-b LIST`                   | Select specific bytes                            | `cut -b1-5 file.txt`                              | Extracts the first 5 bytes               |
| `-c LIST`                   | Select specific characters                       | `cut -c1-5 file.txt`                              | Extracts the first 5 characters          |
| `-d DELIM`                  | Specify a delimiter instead of TAB               | `cut -d',' -f1 file.csv`                          | Extracts the first comma-separated field |
| `--complement`              | Select everything except the specified positions | `cut -f2 --complement file.txt`                   | Extracts all fields except the second    |
| `-s`                        | Skip lines that do not contain the delimiter     | `cut -d',' -f1 -s file.csv`                       | Only processes lines containing commas   |
| `--output-delimiter=STRING` | Specify a different output delimiter             | `cut -d',' -f1,2 --output-delimiter=';' file.csv` | Separates output fields with semicolons  |

## LIST Syntax

| Syntax    | Meaning                                    | Example                  | Result                               |
| --------- | ------------------------------------------ | ------------------------ | ------------------------------------ |
| `N`       | Select the Nth field, byte, or character   | `cut -f2 file.txt`       | Selects the second field             |
| `N-`      | Select from N to the end                   | `cut -f2- file.txt`      | Selects from the second field onward |
| `N-M`     | Select from N to M (inclusive)             | `cut -f2-4 file.txt`     | Selects fields 2, 3, and 4           |
| `-M`      | Select from the beginning to M (inclusive) | `cut -f-3 file.txt`      | Selects the first 3 fields           |
| `N,M`     | Select multiple positions                  | `cut -f1,3 file.txt`     | Selects fields 1 and 3               |
| `N-M,P-Q` | Select multiple ranges                     | `cut -f1-3,5-7 file.txt` | Selects fields 1–3 and 5–7           |

# tr

| Flag                 | Meaning                                          | Example         | Result                               |
| -------------------- | ------------------------------------------------ | --------------- | ------------------------------------ |
| `-d`                 | Delete specified characters                      | `tr -d 'aeiou'` | Removes all vowels                   |
| `-s`                 | Squeeze repeated characters into one             | `tr -s ' '`     | Replaces consecutive spaces with one |
| `-c`, `--complement` | Select characters not specified in the first set | `tr -cd '0-9'`  | Keeps only digits                    |

## Character Sets

| Syntax      | Meaning                | Example                      | Result                          |
| ----------- | ---------------------- | ---------------------------- | ------------------------------- |
| `A-Z`       | Uppercase letters      | `tr 'A-Z' 'a-z'`             | Converts uppercase to lowercase |
| `a-z`       | Lowercase letters      | `tr 'a-z' 'A-Z'`             | Converts lowercase to uppercase |
| `0-9`       | Digits                 | `tr -d '0-9'`                | Removes all digits              |
| `[:upper:]` | All uppercase letters  | `tr '[:upper:]' '[:lower:]'` | Converts uppercase to lowercase |
| `[:lower:]` | All lowercase letters  | `tr '[:lower:]' '[:upper:]'` | Converts lowercase to uppercase |
| `[:digit:]` | All digits             | `tr -d '[:digit:]'`          | Removes all digits              |
| `[:space:]` | Whitespace characters  | `tr -d '[:space:]'`          | Removes whitespace              |
| `[:punct:]` | Punctuation characters | `tr -d '[:punct:]'`          | Removes punctuation             |

# sed

| Option     | Description                                 | Example                                        |
| ---------- | ------------------------------------------- | ---------------------------------------------- |
| `-i`       | In-place editing                            | `sed -i 's/old/new/' file.txt`                 |
| `-n`       | Suppress automatic printing                 | `sed -n '/pattern/p' file.txt`                 |
| `-e`       | Execute multiple commands                   | `sed -e 's/old/new/' -e '/pattern/d' file.txt` |
| `-f`       | Read commands from a file                   | `sed -f script.sed file.txt`                   |
| `-r`, `-E` | Use extended regular expressions            | `sed -E 's/old/new/' file.txt`                 |
| `-z`       | Separate lines with NUL character           | `sed -z 's/old/new/' file.txt`                 |
| `-l`       | Specify the line length for the `l` command | `sed -l 100 'l' file.txt`                      |
| `-b`       | Binary mode (do not strip CR characters)    | `sed -b 's/old/new/' file.txt`                 |

## Common `sed` Examples

| Command       | Meaning                                                           | Example                                 |
| ------------- | ----------------------------------------------------------------- | --------------------------------------- |
| `s/old/new/`  | Replace the first occurrence of `old` with `new` on each line     | `sed 's/Linux/Unix/' file1.txt`         |
| `s/old/new/g` | Replace all occurrences of `old` with `new` on each line          | `sed 's/Linux/Unix/g' file1.txt`        |
| `-i`          | Edit the file in-place instead of printing to stdout              | `sed -i 's/Linux/Unix/' file1.txt`      |
| `Nd`          | Delete line number N                                              | `sed '2d' file1.txt`                    |
| `/pattern/d`  | Delete lines matching a pattern                                   | `sed '/kernel/d' file1.txt`             |
| `-i.bak`      | Edit in-place and create a `.bak` backup file                     | `sed -i.bak 's/Unix/Linux/g' file1.txt` |
| `s/\t/    /g` | Replace all tab characters with four spaces                       | `sed 's/\t/    /g' file1.txt`           |
| `/^$/d`       | Delete empty lines                                                | `sed '/^$/d' file1.txt`                 |
| `-n`          | Suppress automatic printing; only print explicitly selected lines | `sed -n '/Ubuntu/p' file1.txt`          |
| `p`           | Print matching lines (commonly used with `-n`)                    | `sed -n '/Ubuntu/p' file1.txt`          |

# awk

- `-F` sets the field separator (for example, -F: for colon-delimited files).
- `-f` reads AWK rules from a script file instead of the command line.
- `-v` assigns a variable before processing begins.

```bash
# Print every line (no pattern, print is the default action)
awk '{ print }' file.txt

# Print only lines that match a pattern (no explicit action needed)
awk '/error/' file.txt
```

```bash
echo "Alice Engineering 95000" | awk '{ print $1, "works in", $2 }'
```
`$0` refers to the entire line.
