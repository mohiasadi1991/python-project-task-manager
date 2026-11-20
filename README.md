import json
import os

FILE = "tasks.json"

def load_tasks():
    if os.path.exists(FILE):
        try:
            with open(FILE, "r", encoding="utf-8") as f:
                return json.load(f)
        except (json.JSONDecodeError, OSErro):
            return []
    return []

def save_tasks(tasks):
    with open(FILE, "w", encoding="utf-8") as f:
        json.dump(tasks, f, indent=4, ensure_ascii=False)

def add_task(tasks):
    title = input("Task title: ").strip()
    project = input("Project name: ").strip()
    if not title or not project:
        print("Both fields are required.")
        return
    tasks.append({"title": title, "project": project, "status": "Pending"})
    save_tasks(tasks)
    print("Task added successfully!")

def show_tasks(tasks):
    if not tasks:
        print("No tasks found.")
        return
    for i, task in enumerate(tasks, 1):
        print(f"{i}. {task['title']} | Project: {task['project']} | Status: {task['status']}")

def update_task(tasks):
    show_tasks(tasks)
    if not tasks:
        return
    try:
        number = int(input("Task number to update: "))
        if not 1 <= number <= len(tasks):
            print("Invalid task number.")
            return
        print("1. Pending")
        print("2. In Progress")
        print("3. Completed")
        choice = input("New status: ")
        statuses = {"1": "Pending", "2": "In Progress", "3": "Completed"}
        if choice in statuses:
            tasks[number - 1]["status"] = statuses[choice]
            save_tasks(tasks)
            print("Task updated!")
        else:
            print("Invalid status.")
    except ValueError:
        print("Enter a valid number.")

def search_tasks(tasks):
    keyword = input("Search task or project: ").lower().strip()
    results = [task for task in tasks if keyword in task["title"].lower() or keyword in task["project"].lower()]
    show_tasks(results)

def delete_task(tasks):
    show_tasks(tasks)
    if not tasks:
        return
    try:
        number = int(input("Task number to delete: "))
        if 1 <= number <= len(tasks):
            removed = tasks.pop(number - 1)
            save_tasks(tasks)
            print(f"Deleted: {removed['title']}")
        else:
            print("Invalid task number.")
    except ValueError:
        print("Enter a valid number.")

def main():
    tasks = load_tasks()
    while True:
        print("\n===== PROJECT TASK MANAGER =====")
        print("1. Add task")
        print("2. Show tasks")
        print("3. Update task")
        print("4. Search tasks")
        print("5. Delete task")
        print("6. Exit")
        choice = input("Choose an option: ")
        if choice == "1":
            add_task(tasks)
        elif choice == "2":
            show_tasks(tasks)
        elif choice == "3":
            update_task(tasks)
        elif choice == "4":
            search_tasks(tasks)
        elif choice == "5":
            delete_task(tasks)
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
