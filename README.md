
import json
from datetime import datetime, timedelta

# Function to load tasks from a file
def load_tasks():
    try:
        with open("tasks.json", "r") as file:
            tasks = json.load(file)
    except FileNotFoundError:
        tasks = []
    return tasks

# Function to save tasks to a file
def save_tasks(tasks):
    with open("tasks.json", "w") as file:
        json.dump(tasks, file)

# Function to add a new task
def add_task(tasks, title, priority, due_date):
    task = {
        "title": title,
        "priority": priority,
        "due_date": due_date.strftime("%Y-%m-%d") if due_date else None,
        "completed": False,
    }
    tasks.append(task)
    save_tasks(tasks)

# Function to remove a task
def remove_task(tasks, index):
    if 0 <= index < len(tasks):
        del tasks[index]
        save_tasks(tasks)

# Function to mark a task as completed
def complete_task(tasks, index):
    if 0 <= index < len(tasks):
        tasks[index]["completed"] = True
        save_tasks(tasks)

# Function to display tasks in a list
def display_tasks(tasks):
    if not tasks:
        print("No tasks found.")
    else:
        for index, task in enumerate(tasks):
            status = "[X]" if task["completed"] else "[ ]"
            print(f"{index + 1}. {status} {task['title']} - Priority: {task['priority']}, Due Date: {task['due_date']}")

# Main function to run the application
def main():
    tasks = load_tasks()

    while True:
        print("\n===== To-Do List =====")
        print("1. Add Task")
        print("2. Remove Task")
        print("3. Mark Task as Completed")
        print("4. Display Tasks")
        print("5. Exit")

        choice = input("Enter your choice (1-5): ")

        if choice == "1":
            title = input("Enter task title: ")
            priority = input("Enter task priority (high/medium/low): ")
            due_date_str = input("Enter due date (YYYY-MM-DD) or leave blank: ")
            due_date = datetime.strptime(due_date_str, "%Y-%m-%d") if due_date_str else None
            add_task(tasks, title, priority, due_date)

        elif choice == "2":
            index = int(input("Enter the task number to remove: ")) - 1
            remove_task(tasks, index)

        elif choice == "3":
            index = int(input("Enter the task number to mark as completed: ")) - 1
            complete_task(tasks, index)

        elif choice == "4":
            display_tasks(tasks)

        elif choice == "5":
            print("Exiting application.")
            break

        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
