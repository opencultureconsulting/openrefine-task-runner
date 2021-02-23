# OpenRefine Task Runner (💎+🤖)

[![Codacy Badge](https://app.codacy.com/project/badge/Grade/888dbf663fdd409e8d8fcf8472114194)](https://www.codacy.com/gh/opencultureconsulting/openrefine-task-runner/dashboard)

Templates for OpenRefine batch processing (import, transform, export) using the task runner [go-task](https://github.com/go-task/task) and the [openrefine-client](https://github.com/opencultureconsulting/openrefine-client) to control OpenRefine via [its HTTP API](https://docs.openrefine.org/technical-reference/openrefine-api). 

## Features

* run tasks in parallel
* basic error handling by monitoring the OpenRefine server log
* dedicated OpenRefine instances for each task (your existing OpenRefine data will not be touched)
* prevent unnecessary work by fingerprinting generated files and their sources
* the [openrefine-client](https://github.com/opencultureconsulting/openrefine-client) used here supports many core features of OpenRefine:
  * import CSV, TSV, line-based TXT, fixed-width TXT, JSON or XML (and specify input options)
  * apply [undo/redo history](https://docs.openrefine.org/manual/running/#reusing-operations) from given JSON file(s)
  * export to CSV, TSV, HTML, XLS, XLSX, ODS
  * [templating export](https://github.com/opencultureconsulting/openrefine-client#templating) to additional formats like JSON or XML
  * works with OpenRefine 2.7, 2.8, 3.0, 3.1, 3.2, 3.3, 3.4 and 3.4.1
* tasks are easy to extend with additional commands (e.g. to download input data or validate results)

## Typical workflow

**Step 1**: Do some experiments with your data (or parts of it) in the graphical user interface of OpenRefine. If you are fine with all transformation rules, [extract the json code](http://kb.refinepro.com/2012/06/google-refine-json-and-my-notepad-or.html) and save it as file (e.g. dedup.json).

**Step 2**: Configure a task to automate importing your data set, applying the json file and exporting to the required output format.

**Possible automation benefits:**

* When you receive updated data (in the same structure), you just need to drop the file and start the task like this:

  ```sh
  task example-doaj
  ```

* The entire data processing (including options during import) becomes reproducible. The task configuration file can also be used for documentation through source code comments.

* Metadata experts can use OpenRefine's graphical interface and IT staff can incorporate the created transformation rules into regular data processing flows.

## Requirements

* GNU/Linux (tested with Fedora 32)
* JAVA 8+ (for OpenRefine)

## Install

1. Clone this git repository

    ```sh
    git clone https://github.com/opencultureconsulting/openrefine-task-runner.git
    cd openrefine-task-runner
    ```

2. Install [Task 3.2.2](https://github.com/go-task/task/releases/tag/v3.2.2)

    a) RPM-based (Fedora, CentOS, SLES, etc.)

    ```sh
    wget https://github.com/go-task/task/releases/download/v3.2.2/task_linux_amd64.rpm
    sudo dnf install ./task_linux_amd64.rpm && rm task_linux_amd64.rpm
    ```

    b) DEB-based (Debian, Ubuntu etc.)

    ```sh
    wget https://github.com/go-task/task/releases/download/v3.2.2/task_linux_amd64.deb
    sudo apt install ./task_linux_amd64.deb && rm task_linux_amd64.deb
    ```

3. Run install task to download [OpenRefine 3.4.1](https://github.com/OpenRefine/OpenRefine/releases/tag/3.4.1) and [openrefine-client 0.3.10](https://github.com/opencultureconsulting/openrefine-client/releases/tag/v0.3.10)

   ```sh
   task install
   ```

## Usage

* Run all tasks in parallel

    ```sh
    task
    ```

* Run a specific task

    ```sh
    task example-duplicates:main
    ```

* Run some tasks in parallel

    ```sh
    task --parallel example-duplicates:main example-doaj:main
    ```

* Force run a task even when the task is up-to-date

    ```sh
    task example-duplicates:main --force
    ```

* Dry-run in verbose mode for debugging

    ```sh
    task example-duplicates:main --dry --verbose --force
    ```

* List available tasks

    ```sh
    task --list
    ```

### How to develop your own tasks

(first draft, will be elaborated later)

1. create a new folder
2. copy an example Taskfile.yml
3. provide input data in subdirectory input
4. provide OpenRefine transformation history files in subdirectory config
5. add commands to specific Taskfile (check openrefine-client help screen for available options: `openrefine/client --help`)
6. add project to general Taskfile
7. check memory load and increase RAM if needed

### Getting help

Please file an [issue](https://github.com/opencultureconsulting/openrefine-task-runner/issues) if you miss some features or if you have tracked a bug. And you are welcome to ask any questions!

## To do

- [ ] differentiate examples
  - [ ] example for loading multiple input files by providing a zip archive
  - [ ] example for download "fresh" input data as a dependent task and generating archives/diffs
  - [ ] example for applying multiple json files
  - [ ] example for templating xml and validation with xmllint
- [ ] describe example datasets (and differences) with source code examples
- [ ] elaborate how-to for developing tasks
  - [ ] document openrefine-client options and defaults (tables for input and output with file-format-specific defaults) including templating
  - [ ] how-to for extracting input options from OpenRefine GUI (via metadata in open project)
  - [ ] document known issues, e.g. [import xls, xlsx, ods](https://github.com/opencultureconsulting/openrefine-client/issues/4)
- [ ] add Binder files and badge
- [ ] add example notebooks (links to nbviewer and Binder)
