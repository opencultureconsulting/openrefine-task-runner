# OpenRefine Task Runner (💎+🤖)

[![Codacy Badge](https://app.codacy.com/project/badge/Grade/888dbf663fdd409e8d8fcf8472114194)](https://www.codacy.com/gh/opencultureconsulting/openrefine-task-runner/dashboard) [![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/felixlohmeier/openrefineder/master)

Templates for OpenRefine batch processing (import, transform, export) using the task runner [go-task](https://github.com/go-task/task) and the [openrefine-client](https://github.com/opencultureconsulting/openrefine-client) to control OpenRefine via [its HTTP API](https://docs.openrefine.org/technical-reference/openrefine-api). 

## Features

* basic error handling by monitoring the OpenRefine server log
* dedicated OpenRefine instance with temporary workspace (your existing OpenRefine data will not be touched)
* prevent unnecessary work by fingerprinting generated files and their sources
* the [openrefine-client](https://github.com/opencultureconsulting/openrefine-client) used here supports many core features of OpenRefine:
  * import CSV, TSV, line-based TXT, fixed-width TXT, JSON or XML (and specify input options)
  * apply [undo/redo history](https://docs.openrefine.org/manual/running/#reusing-operations) from given JSON file(s)
  * export to CSV, TSV, HTML, XLS, XLSX, ODS
  * [templating export](https://github.com/opencultureconsulting/openrefine-client#templating) to additional formats like JSON or XML
  * works with OpenRefine 2.7, 2.8, 3.0, 3.1, 3.2, 3.3, 3.4 and 3.5
* tasks are easy to extend with additional commands (e.g. to download input data or validate results)

## Typical workflow

**Step 1**: Do some experiments with your data (or parts of it) in the graphical user interface of OpenRefine. If you are fine with all transformation rules, [extract the json code](http://kb.refinepro.com/2012/06/google-refine-json-and-my-notepad-or.html) and save it as file (e.g. dedup.json).

**Step 2**: Configure a task to automate importing your data set, applying the json file and exporting to the required output format.

**Possible automation benefits:**

* When you receive updated data (in the same structure), you just need to drop the input file and start the task like this:

  ```sh
  task
  ```

* The entire data processing (including options during import) becomes reproducible. The task configuration file can also be used for documentation through source code comments.

* Metadata experts can use OpenRefine's graphical interface and IT staff can incorporate the created transformation rules into regular data processing flows.

## Requirements

* GNU/Linux (tested with Fedora 34)
* JAVA 8+ (for OpenRefine)

## Demo via binder

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/felixlohmeier/openrefineder/master)

- free to use on-demand server with Jupyterlab and Bash Kernel
- OpenRefine, openrefine-client and go-task [preinstalled](binder/postBuild)
- no registration needed, will start within a few minutes
- [restricted](https://mybinder.readthedocs.io/en/latest/about/about.html#how-much-memory-am-i-given-when-using-binder) to 2 GB RAM and server will be deleted after 10 minutes of inactivity

## Install

1. Clone this git repository

    ```sh
    git clone https://github.com/opencultureconsulting/openrefine-task-runner.git
    cd openrefine-task-runner
    ```

2. Install [Task 3.10.0](https://github.com/go-task/task/releases/tag/v3.10.0)+

    a) RPM-based (Fedora, CentOS, SLES, etc.)

    ```sh
    wget https://github.com/go-task/task/releases/download/v3.10.0/task_linux_amd64.rpm
    sudo dnf install ./task_linux_amd64.rpm && rm task_linux_amd64.rpm
    ```

    b) DEB-based (Debian, Ubuntu etc.)

    ```sh
    wget https://github.com/go-task/task/releases/download/v3.10.0/task_linux_amd64.deb
    sudo apt install ./task_linux_amd64.deb && rm task_linux_amd64.deb
    ```

3. Run install task to download [OpenRefine 3.5.2](https://github.com/OpenRefine/OpenRefine/releases/tag/3.5.2) and [openrefine-client 0.3.10](https://github.com/opencultureconsulting/openrefine-client/releases/tag/v0.3.10)

   ```sh
   task install
   ```

## Usage

* Run default task (start, import, transform, export, stats, check, kill and cleanup)

    ```sh
    task default
    ```

* Override settings with environment variables

    ```sh
    OPENREFINE_MEMORY=2000M OPENREFINE_PORT=3334 task default
    ```

* Force run a task even when the task is up-to-date

    ```sh
    task default --force
    ```

* Dry-run in verbose mode for debugging

    ```sh
    task default --dry --verbose --force
    ```

* List available tasks

    ```sh
    task --list
    ```

### Examples

* [noah-biejournals](https://github.com/opencultureconsulting/noah-biejournals): Harvesting des Zeitschriftenservers BieJournals der UB Bielefeld und Transformation in METS/MODS für das Portal noah.nrw 

### Getting help

Please file an [issue](https://github.com/opencultureconsulting/openrefine-task-runner/issues) if you miss some features or if you have tracked a bug. And you are welcome to ask any questions!
