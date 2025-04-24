# python-internship-by-codesoft
# TASK-1
# A to-do list application is a useful project that helps users manage and organize their tasks efficiently.This project aims to create a command-line or GUI-based application using python, allowing users to create, update, and track to-do lists.

import tkinter as tk
from tkinter import messagebox
import json
import os

TASKS_FILE = 'tasks.json'

def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, 'r') as file:
            return json.load(file)
    return []

def save_tasks(tasks):
    with open(TASKS_FILE, 'w') as file:
        json.dump(tasks, file, indent=4)

class TodoApp:
    def __init__(self, root):
        self.root = root
        self.root.title("📝 To-Do List")

        self.tasks = load_tasks()

        # Entry to add task
        self.task_entry = tk.Entry(root, width=40)
        self.task_entry.pack(pady=10)

        # Add Task Button
        self.add_button = tk.Button(root, text="Add Task", command=self.add_task)
        self.add_button.pack()

        # Listbox to show tasks
        self.task_listbox = tk.Listbox(root, width=50, selectmode=tk.SINGLE)
        self.task_listbox.pack(pady=10)

        # Buttons for complete and delete
        self.complete_button = tk.Button(root, text="Mark as Complete", command=self.complete_task)
        self.complete_button.pack(pady=2)

        self.delete_button = tk.Button(root, text="Delete Task", command=self.delete_task)
        self.delete_button.pack(pady=2)

        self.load_task_list()

    def load_task_list(self):
        self.task_listbox.delete(0, tk.END)
        for task in self.tasks:
            display = f"[{'✓' if task['completed'] else ' '}] {task['description']}"
            self.task_listbox.insert(tk.END, display)

    def add_task(self):
        desc = self.task_entry.get()
        if desc.strip() == "":
            messagebox.showwarning("Warning", "Task cannot be empty.")
            return
        self.tasks.append({"description": desc, "completed": False})
        save_tasks(self.tasks)
        self.task_entry.delete(0, tk.END)
        self.load_task_list()

    def complete_task(self):
        selected = self.task_listbox.curselection()
        if selected:
            index = selected[0]
            self.tasks[index]['completed'] = True
            save_tasks(self.tasks)
            self.load_task_list()
        else:
            messagebox.showinfo("Info", "Please select a task to mark complete.")

    def delete_task(self):
        selected = self.task_listbox.curselection()
        if selected:
            index = selected[0]
            del self.tasks[index]
            save_tasks(self.tasks)
            self.load_task_list()
        else:
            messagebox.showinfo("Info", "Please select a task to delete.")

# Run the app
if __name__ == "__main__":
    root = tk.Tk()
    app = TodoApp(root)
    root.mainloop()
