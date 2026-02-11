# Minishell Project Context

## Project Overview
**Minishell** is a custom implementation of a simple shell, inspired by Bash. It is written in C and adheres to the 42 School coding standards (Norminette). The project handles command parsing, expansion, and execution, including features like pipes, redirections, and environment variable management.

## Architecture
The project is modularized into several key components:

- **Core (`minishell.c`):**
  - Manages the main REPL (Read-Eval-Print Loop).
  - Handles signal configuration (`ft_set_sig`) and history.
  - Initializes and loads the environment.

- **Parsing (`parsing/`):**
  - **Lexing:** Tokenizes input strings (`lexing.c`).
  - **Expansion:** Expands environment variables and wildcards (`expansion.c`, `wildcards.c`).
  - **Parser:** Constructs the `t_cli` linked list (AST equivalent) from tokens (`parsing.c`). Handles syntax validation.

- **Execution (`exec/`):**
  - **Orchestrator:** `ft_execute.c` decides how to run commands (builtin vs. external).
  - **Builtins:** Custom implementations of `cd`, `echo`, `env`, `exit`, `export`, `pwd`, `unset` located in `exec/builtins/`.
  - **Pipes & Redirections:** `exec_pipe.c` and `exec/aux_exec/apply_redirs.c` manage file descriptors and process forking.

- **Utilities (`libft/`):**
  - A comprehensive library of standard C function reimplementations and helpers.

## Build and Run

### Compilation
The project uses a `Makefile` for compilation.

- **Build:** `make` (compiles `libft` and `minishell`)
- **Clean Objects:** `make clean`
- **Full Clean:** `make fclean`
- **Rebuild:** `make re`

### Usage
```bash
./minishell
```

## Development Conventions

- **Language:** C (POSIX standard).
- **Style:** 42 Norminette (strict formatting, variable naming, and function length rules).
- **Memory Management:** Manual management using `malloc` and `free`.
- **External Libraries:** Uses `readline` for input handling.

### Key Data Structures (`minishell.h`)
- **`t_cli`:** Represents a command node in the execution list. Contains arguments, redirection files (`infile`, `outfile`), heredoc info, and pipe/operator status.
- **`t_shenv`:** Linked list for environment variables.
- **`t_builtin`:** Maps builtin command names to their corresponding functions.

## Workflow
- **Debugging:** When debugging, pass the arguments to minishell when you execute the binary, so it runs in non interactive mode.

## Testing
- **Main Script:** `tests/tests.sh`
  - Intended to run integration and lexing tests.
- **Modus Operandi:**
  - **Structure:** Each specific test (e.g., `lexing`) has its own directory in `tests/` (e.g., `tests/test_lexing/`).
  - **Components:**
    - `Makefile`: Compiles 3 distinct executables with separate object files:
      - `msh`: Normal build (`-g3`).
      - `san_msh`: Sanitizer build (`-g3 -fsanitize=address,undefined`).
      - `val_msh`: Valgrind build (`-g3`).
    - `test_*.txt`: Contains test cases.
    - `mock_files/`: Contains adapted source files (e.g., with specific printers) and mock functions.
  - **Execution:** All tests are orchestrated by `tests/tests.sh`, which handles logging and comparison.
- **Static Analysis:** The test script also includes calls to `norminette` to verify code style adherence.
