# grep/ripgrep

## Regex

| Regex    | Meaning                                                  | Example          | Matches          |
| -------- | -------------------------------------------------------- | ---------------- | ---------------- |
| `.`      | Any single character (except newline by default)         | `c.t`            | cat, cut, cot    |
| `*`      | Zero or more repetitions of the previous character/group | `ab*`            | a, ab, abb       |
| `+`      | One or more repetitions                                  | `ab+`            | ab, abb          |
| `?`      | Zero or one repetition                                   | `colou?r`        | color, colour    |
| `^`      | Beginning of a line                                      | `^import`        | import os        |
| `$`      | End of a line                                            | `;$`             | print("Hi");     |
| `[abc]`  | Any one character inside brackets                        | `[abc]at`        | cat, bat, aat    |
| `[a-z]`  | Any lowercase letter in range                            | `[a-z]+`         | hello            |
| `[0-9]`  | Any digit from 0 to 9                                    | `[0-9]+`         | 123, 42          |
| `[^abc]` | Any character except those listed                        | `[^0-9]`         | letters, symbols |
| `\d`     | Digit (ripgrep default regex)                            | `\d+`            | 123              |
| `\w`     | Word character                                           | `\w+`            | hello_123        |
| `\s`     | Whitespace character                                     | `\s+`            | spaces, tabs     |
| `()`     | Group expressions                                        | `(cat            | dog)s?`          | cat, cats, dog, dogs |
| `        | `                                                        | OR (alternation) | `cat             | dog`                 | cat, dog |
| `{n}`    | Exactly n repetitions                                    | `a{3}`           | aaa              |
| `{n,m}`  | Between n and m repetitions                              | `a{2,4}`         | aa, aaa, aaaa    |
| `\`      | Escape a special character                               | `\.`             | A literal dot    |

## Common grep / ripgrep flags

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

## Useful ripgrep-specific options

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

## Common find expressions

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

## Common fd flags

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

## Comparison

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
