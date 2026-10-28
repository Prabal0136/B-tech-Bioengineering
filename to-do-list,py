#!/usr/bin/env python3
"""A simple command-line to-do list manager that stores tasks in a JSON file."""

import argparse
import json
import sys
from datetime import datetime
from pathlib import Path

DATA_FILE = Path(__file__).resolve().parent / "tasks.json"


def load_tasks(path=DATA_FILE):
    """Load tasks from the JSON file. Returns an empty list if the file doesn't exist."""
    if not path.exists():
        return []
    with path.open("r", encoding="utf-8") as f:
        return json.load(f)


def save_tasks(tasks, path=DATA_FILE):
    """Save the list of tasks to the JSON file."""
    with path.open("w", encoding="utf-8") as f:
        json.dump(tasks, f, indent=2)


def find_task(tasks, task_id):
    """Return the task with the given ID, or None if not found."""
    for task in tasks:
        if task["id"] == task_id:
            return task
    return None


def add_task(title, path=DATA_FILE):
    """Create a new task and save it. Returns the created task."""
    tasks = load_tasks(path)
    new_id = max((t["id"] for t in tasks), default=0) + 1
    task = {
        "id": new_id,
        "title": title,
        "done": False,
        "created": datetime.now().isoformat(timespec="seconds"),
    }
    tasks.append(task)
    save_tasks(tasks, path)
    return task


def list_tasks(pending_only=False, path=DATA_FILE):
    """Print tasks. If pending_only is True, completed tasks are hidden."""
    tasks = load_tasks(path)
    if pending_only:
        tasks = [t for t in tasks if not t["done"]]

    if not tasks:
        print("No tasks found." if not pending_only else "No pending tasks. Nice work!")
        return

    for task in tasks:
        mark = "[x]" if task["done"] else "[ ]"
        print(f"{mark} #{task['id']}  {task['title']}")


def complete_task(task_id, path=DATA_FILE):
    """Mark a task as done. Returns True if successful, False if the task doesn't exist."""
    tasks = load_tasks(path)
    task = find_task(tasks, task_id)
    if task is None:
        return False
    task["done"] = True
    save_tasks(tasks, path)
    return True


def delete_task(task_id, path=DATA_FILE):
    """Delete a task by ID. Returns True if successful, False if the task doesn't exist."""
    tasks = load_tasks(path)
    task = find_task(tasks, task_id)
    if task is None:
        return False
    tasks.remove(task)
    save_tasks(tasks, path)
    return True


def build_parser():
    parser = argparse.ArgumentParser(description="A simple command-line to-do list.")
    subparsers = parser.add_subparsers(dest="command", required=True)

    add_parser = subparsers.add_parser("add", help="Add a new task")
    add_parser.add_argument("title", nargs="+", help="The task description")

    list_parser = subparsers.add_parser("list", help="List tasks")
    list_parser.add_argument("--pending", action="store_true", help="Show only pending tasks")

    done_parser = subparsers.add_parser("done", help="Mark a task as done")
    done_parser.add_argument("id", type=int, help="The task ID")

    delete_parser = subparsers.add_parser("delete", help="Delete a task")
    delete_parser.add_argument("id", type=int, help="The task ID")

    return parser


def main(argv=None):
    parser = build_parser()
    args = parser.parse_args(argv)

    if args.command == "add":
        title = " ".join(args.title).strip()
        if not title:
            print("Error: task title cannot be empty.", file=sys.stderr)
            return 1
        task = add_task(title)
        print(f"Added task #{task['id']}: {task['title']}")

    elif args.command == "list":
        list_tasks(pending_only=args.pending)

    elif args.command == "done":
        if complete_task(args.id):
            print(f"Marked task #{args.id} as done.")
        else:
            print(f"Error: no task with ID {args.id}.", file=sys.stderr)
            return 1

    elif args.command == "delete":
        if delete_task(args.id):
            print(f"Deleted task #{args.id}.")
        else:
            print(f"Error: no task with ID {args.id}.", file=sys.stderr)
            return 1

    return 0


if __name__ == "__main__":
    sys.exit(main())
