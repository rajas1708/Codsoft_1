import os
import json
from datetime import datetime

class ToDoList:
    def __init__(self, filename='todo_list.json'):
        self.filename = filename
        self.tasks = []
        self.load_tasks()

    def load_tasks(self):
        if os.path.exists(self.filename):
            with open(self.filename, 'r') as file:
                self.tasks = json.load(file)

    def save_tasks(self):
        with open(self.filename, 'w') as file:
            json.dump(self.tasks, file, indent=2)

    def show_tasks(self):
        if not self.tasks:
            print("No tasks found.")
        else:
            print("Tasks:")
            for index, task in enumerate(self.tasks, start=1):
                print(f"{index}. {task['title']} - {task['date']}")

    def add_task(self, title):
        date = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.tasks.append({'title': title, 'date': date})
        self.save_tasks()
        print(f"Task '{title}' added successfully.")

    def update_task(self, index, title):
        if 1 <= index <= len(self.tasks):
            self.tasks[index - 1]['title'] = title
            self.save_tasks()
            print(f"Task {index} updated successfully.")
        else:
            print(f"Invalid task index. Please provide a valid index.")

    def remove_task(self, index):
        if 1 <= index <= len(self.tasks):
            removed_task = self.tasks.pop(index - 1)
            self.save_tasks()
            print(f"Task {index} removed: {removed_task['title']}")
        else:
            print(f"Invalid task index. Please provide a valid index.")

# Example usage
if __name__ == "__main__":
    todo_list = ToDoList()

    while True:
        print("\n=== To-Do List ===")
        print("1. Show Tasks")
        print("2. Add Task")
        print("3. Update Task")
        print("4. Remove Task")
        print("5. Quit")

        choice = input("Enter your choice (1-5): ")

        if choice == '1':
            todo_list.show_tasks()
        elif choice == '2':
            title = input("Enter task title: ")
            todo_list.add_task(title)
        elif choice == '3':
            index = int(input("Enter task index to update: "))
            title = input("Enter new task title: ")
            todo_list.update_task(index, title)
        elif choice == '4':
            index = int(input("Enter task index to remove: "))
            todo_list.remove_task(index)
        elif choice == '5':
            break
        else:
            print("Invalid choice. Please enter a number between 1 and 5.")
