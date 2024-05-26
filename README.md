import tkinter as tk
from tkinter import Listbox, END, BOTH

class todo:
    def _init_(self, root):
        self.root = root
        self.root.title("To-Do List")
        self.root.geometry('650x410+300+150')

        self.label = tk.Label(self.root, text='To-Do-List-App',
                              font='ariel, 25 bold', width=10, bd=5, bg='orange', fg='black')
        self.label.pack(side='top', fill=BOTH)

        self.label2 = tk.Label(self.root, text='Add Task',
                               font='ariel, 18 bold', width=10, bd=5, bg='orange', fg='black')
        self.label2.place(x=30, y=54)

        self.label3 = tk.Label(self.root, text='Tasks',
                               font='ariel, 18 bold', width=10, bd=5, bg='orange', fg='black')
        self.label3.place(x=320, y=54)

        self.main_text = Listbox(self.root, height=9, bd=5, width=23, font="ariel, 20 italic bold")
        self.main_text.place(x=280, y=100)

        self.text = tk.Text(self.root, bd=5, height=2, width=30, font='ariel, 10 bold')
        self.text.place(x=20, y=120)

        self.button = tk.Button(self.root, text="Add", font='sarif, 20 bold italic',
                                width=10, bd=5, bg='orange', fg='black', command=self.add)
        self.button.place(x=30, y=100)

        self.button2 = tk.Button(self.root, text="Delete", font='sarif, 20 bold italic',
                                 width=10, bd=5, bg='orange', fg='black', command=self.delete)
        self.button2.place(x=30, y=280)

        self.load_tasks()

    def add(self):
        content = self.text.get(1.0, END)
        self.main_text.insert(END, content)
        with open('data.txt', 'a') as file:
            file.write(content)
        self.text.delete(1.0, END)

    def delete(self):
        delete_ = self.main_text.curselection()
        look = self.main_text.get(delete_)
        with open('data.txt', 'r') as f:
            new_f = f.readlines()
        with open('data.txt', 'w') as f:
            for line in new_f:
                item = str(look)
                if item not in line:
                    f.write(line)
        self.main_text.delete(delete_)

        self.main_text.delete(0, END)
        self.load_tasks()

    def load_tasks(self):
        try:
            with open('data.txt', 'r') as file:
                tasks = file.readlines()
                for task in tasks:
                    self.main_text.insert(END, task.strip())
        except FileNotFoundError:
            pass

def main():
    root = tk.Tk()
    ui = todo(root)
    root.mainloop()

if _name_ == "_main_":
    main()
