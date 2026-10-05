# Intro to the Command Line
*by Pongsakorn Thawornpan,PhD / Department of Community Medical Technology, Factulty of Medical Technology, Mahidol University, 2026.*
*email: pongsakorn.tha@mahidol.ac.th*

> [!IMPORTANT]
> **🕐 Schedule**
>
> - 0:00–0:10 — Welcome and why the command line matters
> - 0:10–0:25 — Anatomy of a command
> - 0:25–0:50 — Navigating the file system
> - 0:50–1:15 — Creating, copying, moving and deleting files
> - 1:15–1:40 — Viewing and searching file contents
> - 1:40–1:55 — Pipes and redirection
> - 1:55–2:00 — Wrap-up and cheat sheet

> [!IMPORTANT]
> **❗ Requirements**
> - No prior experience needed
> - Access to a terminal:
>     - **Mac / Linux:** open the *Terminal* app
>     - **Windows:** install [WSL](https://example.com) or [Git Bash](https://example.com)[span_1](start_span)[span_1](end_span)


> [!IMPORTANT]
>**✅ Expected Outcomes**
>    - Move around the file system confidently
>    - Create, copy, move and remove files and folders
>    - Read and search text files
>    - Chain commands together with pipes

---

## Why the command line?

The command line lets you talk to your computer by typing instructions instead of clicking. It's faster for repetitive work, it's how you use remote servers and HPC clusters, and most scientific software only runs this way.

---

## Anatomy of a command

```
user@laptop:~/workshop$ ls -l data/
└──────── prompt ──────┘ └┬┘ └┬┘ └─┬─┘
                     command flag  argument
```

- **Prompt** — shows who and where you are. You never type it.
- **Command** — the program to run (`ls` = list).
- **Flag / option** — changes how the command behaves (`-l` = long format).
- **Argument** — what the command acts on (a file or folder).

!!! tip "Getting help"
    Almost every command explains itself:
    ```bash
    ls --help     # short summary of options
    man ls        # full manual (press q to quit)
    ```

!!! tip "Two habits that save hours"
    - Press **Tab** to auto-complete file and folder names.
    - Press **↑ (Up arrow)** to bring back previous commands.

---

## Setup: create practice files

Copy and paste this block into your terminal. It builds a small folder to play with.

```bash
mkdir -p ~/cli-workshop/data
cd ~/cli-workshop
printf "apple\nbanana\ncherry\napple\ndate\n" > data/fruits.txt
printf "id,name,score\n1,Ana,88\n2,Ben,92\n3,Cal,75\n4,Dee,92\n" > data/scores.csv
printf "INFO start\nERROR disk full\nINFO retry\nERROR timeout\nINFO done\n" > data/log.txt
```

---

## Navigating the file system

| Command | What it does |
|---|---|
| `pwd` | **P**rint **w**orking **d**irectory — where am I? |
| `ls` | List what's here |
| `ls -la` | List everything, with details and hidden files |
| `cd folder` | Change into a folder |
| `cd ..` | Go up one level |
| `cd ~` | Go to your home folder |

```bash
pwd
ls
cd data
ls -l
cd ..
```

!!! note "Absolute vs. relative paths"
    - **Absolute** paths start at the root: `/home/ana/cli-workshop/data`
    - **Relative** paths start where you are: `data/fruits.txt`
    - `.` means "here" and `..` means "one level up".

!!! warning "Spaces in paths are bad"
    `my data` will be read as two separate things. Use `my_data` or `my-data` instead.

---

## Working with files and folders

| Command | What it does |
|---|---|
| `mkdir results` | Make a new folder |
| `touch notes.txt` | Create an empty file |
| `cp a.txt b.txt` | Copy a file |
| `cp -r dir1 dir2` | Copy a folder (recursively) |
| `mv a.txt results/` | Move a file |
| `mv old.txt new.txt` | Rename a file |
| `rm file.txt` | Delete a file |
| `rm -r folder` | Delete a folder and its contents |

```bash
mkdir results
cp data/fruits.txt results/fruits_copy.txt
mv results/fruits_copy.txt results/backup.txt
ls results
```

!!! danger "There is no Recycle Bin"
    `rm` deletes immediately and permanently. Double-check before pressing Enter. Use `rm -i` to be asked for confirmation.

### Wildcards

`*` matches anything. `ls data/*.txt` lists every `.txt` file in `data/`.

---

## Viewing and searching files

| Command | What it does |
|---|---|
| `cat file` | Print the whole file |
| `less file` | Scroll through a file (q to quit) |
| `head -n 3 file` | First 3 lines |
| `tail -n 3 file` | Last 3 lines |
| `wc -l file` | Count lines |
| `grep word file` | Show lines containing *word* |
| `sort file` | Sort lines |
| `uniq` | Remove adjacent duplicates |

```bash
cat data/fruits.txt
head -n 2 data/scores.csv
wc -l data/log.txt
grep ERROR data/log.txt
```

---

## Pipes and redirection

- `>` sends output **into a file** (overwrites it)
- `>>` **appends** output to a file
- `|` (pipe) sends the output of one command **into the next**

```bash
grep ERROR data/log.txt > results/errors.txt     # save errors to a file
grep -c ERROR data/log.txt                       # count error lines
sort data/fruits.txt | uniq                      # unique fruits
sort data/fruits.txt | uniq -c | sort -nr        # how often each fruit appears
```

!!! note "Think of pipes as an assembly line"
    Each command does one small job and passes its result to the next. Combining simple tools is the core idea of the command line.

---

## Exercises

**1.** How many lines in `data/log.txt` contain `INFO`?

??? success "Solution"
    ```bash
    grep -c INFO data/log.txt
    ```
    Answer: 3

**2.** Show the scores file without its header line.

??? success "Solution"
    ```bash
    tail -n +2 data/scores.csv
    ```

**3.** Make a folder called `archive`, copy every `.txt` file from `data/` into it, then list it.

??? success "Solution"
    ```bash
    mkdir archive
    cp data/*.txt archive/
    ls archive
    ```

**4.** Save the list of unique fruits, sorted alphabetically, to `results/unique_fruits.txt`.

??? success "Solution"
    ```bash
    sort data/fruits.txt | uniq > results/unique_fruits.txt
    cat results/unique_fruits.txt
    ```

---

## Cheat sheet

| Goal | Command |
|---|---|
| Where am I? | `pwd` |
| What's here? | `ls -la` |
| Move around | `cd`, `cd ..`, `cd ~` |
| Make / delete | `mkdir`, `touch`, `rm`, `rm -r` |
| Copy / move | `cp`, `cp -r`, `mv` |
| Read files | `cat`, `less`, `head`, `tail` |
| Search & count | `grep`, `wc -l` |
| Combine | `|`, `>`, `>>` |
| Get help | `command --help`, `man command` |

## Further learning

- [The Software Carpentry: The Unix Shell](https://swcarpentry.github.io/shell-novice/)
- [explainshell.com](https://explainshell.com/) — paste any command to see what each part does

---

