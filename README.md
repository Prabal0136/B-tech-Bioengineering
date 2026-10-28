# Simple CLI To-Do List

A lightweight command-line to-do list manager written in Python. Tasks are saved to a local `tasks.json` file, so your list persists between sessions.

## Features

- Add tasks
- List all tasks or only pending ones
- Mark tasks as done
- Delete tasks
- Data stored in plain JSON (no database required)

## Requirements

- Python 3.9 or newer
- No third-party packages (see `requirements.txt`)

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/YOUR-USERNAME/simple-todo-cli.git
   cd simple-todo-cli
   ```

2. (Optional) Create and activate a virtual environment:

   ```bash
   python -m venv venv
   source venv/bin/activate      # macOS/Linux
   venv\Scripts\activate         # Windows
   ```

3. Install requirements:

   ```bash
   pip install -r requirements.txt
   ```

## Usage

Add a task:

```bash
python todo.py add Buy milk and eggs
```

List all tasks:

```bash
python todo.py list
```

List only pending tasks:

```bash
python todo.py list --pending
```

Mark a task as done:

```bash
python todo.py done 1
```

Delete a task:

```bash
python todo.py delete 1
```

Show help:

```bash
python todo.py --help
```

## Example Output

```
$ python todo.py add Finish project report
Added task #1: Finish project report

$ python todo.py add Call the dentist
Added task #2: Call the dentist

$ python todo.py list
[ ] #1  Finish project report
[ ] #2  Call the dentist

$ python todo.py done 1
Marked task #1 as done.

$ python todo.py list
[x] #1  Finish project report
[ ] #2  Call the dentist
```

## Project Structure

```
simple-todo-cli/
├── README.md
├── requirements.txt
├── todo.py
└── tasks.json      # created automatically when you add your first task
```

## License

This project is licensed under the MIT License.
