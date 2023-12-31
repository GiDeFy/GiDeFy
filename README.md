
import tkinter as tk 
from tkinter import ttk 
import sqlite3


class Main(tk.Frame):
    def _init_(self, root): 
        super().__init__(root) self.init_main()
        self.db db
        self.view_records()
    
    def init_main(self):
        toolbar = tk.Frame (bg="#d7d8e0", bd=2) 
        toolbar.pack(side=tk. TOP, fill=tk.X
        self.add_img = tk.PhotoImage(file="./img/add.png")
        btn_open_dialog = tk.Button(
            toolbar, bg="#d7d8e0", bd-0, image=self.add_img, command-self.open_dialog)
        btn_open_dialog.pack(side=tk.LEFT)
        
        self.tree ttk. Treeview(
            self, columns=("ID", "name", "tel", "email", "zp"), height-45, show="headings")
        
        self.tree.column ("ID", width=30, anchor=tk.CENTER) 
        self.tree.column ("name", width=300, anchor=tk.CENTER) 
        self.tree.column("tel", width=150, anchor-tk.CENTER) 
        self.tree.column ("email", width=150, anchor-tk.CENTER) 
        self.tree.column ("zp", width=150, anchor=tk.CENTER)
        
        self.tree.heading("ID", text="ID") self.tree.heading("name", text="NO")
        self.tree.heading("tel", text="Teлedoн")
        self.tree.heading("email", text="E-mail")
        self.tree.heading("zp", text="3аработная плата")
        self.tree.pack(side=tk.LEFT)
        self.update_img=tk.PhotoImage(file="./img/update.png")
        btn_edit_dialog = tk.Button(
            toolbar,
            bg="#d7d8e0",
            bd=0,
            image=self.update_img,
            command=self.open_update_dialog,
        )
        btn_edit_dialog.pack (side-tk.LEFT)
        
        self.delete_img = tk. PhotoImage(file="./img/delete.png")
        btn_delete = tk.Button(
            toolbar,
            bg="#d7d8e0", bd=0,
            image=self.delete_img,
            command=self.delete_records,
        )
        btn_delete.pack(side=tk. LEFT)
        self.search_img = tk. PhotoImage(file="./img/search.png") btn_search= tk.Button(
            toolbar,
            bg="#d7d8e0",
            bd=0,
            image=self.search_img,
            command-self.open_search_dialog,
        )
        btn_search.pack(side=tk.LEFT)
        self.refresh_img = tk. PhotoImage(file="./img/refresh.png") 
        btn_refresh= tk.Button(
            toolbar,
            bg="#d7d8e0",
            bd=0,
            image=self.refresh_img,
            command=self.open_search_dialog,
        )
        btn_refresh.pack(side=tk.LEFT)
    def open_dialog(self):
        Child()
        
    def records (self, name, tel, email, zp): 
        self.db.insert_data(name, tel, email, zp) 
        self.view_records()
    def view_records (self):
        self.db.cursor.execute("SELECT * FROM db")
        [self.tree.delete(i) for i in self.tree.get_children()]
        [self.tree.insert("", "end", values-row) for row in self.db.cursor.fetchall()]
    def open_update_dialog(self):
        Update()
    def update_records (self, name, tel, email, zp):
        self.db.cursor.execute(
            """UPDATE db SET name=?, tel-?, email=?, zp=? WHERE id=?""",
            (name, tel, email, zp, self.four.set(self.four.selection()[0], "#1")),
        )
        self.db.conn.commit()
        self.view_records()
    def delete_records (self):
        for selection_items in self.tree.selection():
            self.db.cursor.execute(
                "DELETE FROM db WHERE id=?", (self.four.set(selection_items, "#1"))
            )
            self.db.conn.commit()
            self.view_records()
    def open_search_dialog(self):
        Search()
        
    def search_records (self, name):
        name = "" + name + ""
        self.db.cursor.execute("SELECT * FROM db WHERE name LIKE ?", (name,))
        
        [self.tree.delete(i) for i in self.tree.get_children()]
class Update (Child): 
    def _init_(self): 
        super().__init__() 
        self.init_edit() 
        self.view = app
        self.db = db
        self.default_data()
    def init_edit(self):
        self.title("Редактирование контакта")
        btn_edit = ttk.Button(self, text="Редактировать") 
        btn_edit.place(x=205, y=170)
        btn_edit.bind(
            "<Button-1>",
            lambda event: self.view.update_records(
                self.entry_name.get(), self.entry_email.get(), self.entry_tel.get()
            ),
        btn_edit.bind("<Button-1>", lambda event: self.destroy(), add="+") 
        self.btn_ok.destroy()
    def default_data(self):
        self.db.cursor.execute(
            "SELECT * FROM db WHERE id=?",
            self.view.four.set(self.view.four.selection()[0], "#1"),
        )
        row = self.db.cursor.fetchone()
        self.entry_name.insert(0, row[1])
        self.entry_email.insert(0, row[2])
        self.entry_tel.insert(0, row[3])
        self.entry_zp.insert(0, row[4])
class Search (tk. Toplevel):
    def _init_(self):
        super().__init__() 
        self.init_search() 
        self.view app
    def init_search(self):
        self.title("Поиск контакта")
        self.geometry("300x100")
        self.resizable(False, False)
        
        label_search=tk.Label(self, text="Имя:")
        label_search.place (x=50, y=20)
        self.entry_search= ttk. Entry (self)
        self.entry_search.place(x=100, y=20, width=150)
        btn_cancel = ttk.Button(self, text="3aкpытb", command=self.destroy) 
        btn_cancel.place(x=185, y=50)
        search_btn = ttk.Button(self, text="Haйти")
        search_btn.place(x=105, y=50) search_btn.bind(
            "<Button-1>",
            lambda event: self.view.search_records (self.entry_search.get())
    def open_search_dialog(self):
        Search()
    def search_records (self, name):
        name = "" + name + "%"
        self.db.cursor.execute("SELECT * FROM db WHERE name LIKE?", (name,))
        
        [self.tree.delete(i) for i in self.tree.get_children()]
        [self.tree.insert("", "end", values-row) for row in self.db.cursor.fetchall()]
class Child(tk. Toplevel):
    def __init__(self):
        super().__init__(root) 
        self.init_child()
        self.view app
    def init_child(self):
        self.title("Добавить")
        self.geometry("400x220")
        self.resizable(False, False)
        self.grab_set()
        self.focus_set()
        
        label_name = tk. Label (self, text="OMO:")
        label_name.place(x=60, y=25)
        label_select= tk.Label(self, text="Teлeдон:")
        label_select.place(x=60, y=55)
        label_sum = tk.Label(self, text="E-mail:")
        label_sum.place(x=60, y=85)
        label_zp = tk.Label (self, text="3аработная плата:") 
        label_zp.place(x=60, y=115)
        self.entry_name = ttk. Entry (self)
        self.entry_name.place (x=200, y=25)
        self.entry_email=ttk.Entry(self)
        self.entry_email.place (x=200, y=85) 
        self.entry_tel = ttk.Entry(self) 
        self.entry_tel.place(x=200, y=55) 
        self.entry_zp = ttk.Entry(self) 
        self.entry_zp.place (x=200, y=115)
        
        self.btn_cancel = ttk.Button(self, text="3aкpыть", command=self.destroy)
        self.btn_cancel.place (x=220, y=170)
        self.btn_ok = ttk.Button(self, text="Добавить")
        self.btn_ok.place(x=300, y=170)
        self.btn_ok.bind(
            "<Button-1>",
            lambda event: self.view.records (
                self.entry_name.get(), self.entry_email.get(), self.entry_tel.get(), self.entry_zp.get()
            ),
            )
        self.geometry("300x100") 
        self.resizable(False, False)
        label_search= tk. Label(self, text="ИMA:") 
        label_search.place(x=50, y=20)
        self.entry_search= ttk. Entry (self)
        self.entry_search.place(x=100, y=20, width=150)
        btn_cancel = ttk.Button(self, text="3akpытb", command=self.destroy) 
        btn_cancel.place(x=185, y=50)
        search_btn = ttk.Button(self, text="нaйти") 
        search_btn.place (x=105, y=50) 
        search_btn.bind(
            "<Button-1>",
            lambda event: self.view.search_records (self.entry_search.get()),
            )
            search_btn.bind("<Button-1>",lambda event: self.destroy(), add="+")
class DB:
    def __init__(self):
        self.conn = sqlite3.connect("db.db")
        self.cursor = self.conn.cursor()
        self.cursor.execute(
             """CREATE TABLE IF NOT EXISTS db (
                id INTEGER PRIMARY KEY,
                name TEXT,
                tel TEXT,
                email TEXT,
                zp TEXT
            )"""
        self.conn.commit()
    def insert_data(self, name, tel, email, zp):
        self.cursor.execute(
            """INSERT INTO db (name, tel, email, zp) VALUES (?, ?, ?, ?)""", (name, tel, email, zp)
        )
            self.conn.commit()
if  __name__ == "__main__":
    root = tk.Tk()
    db = DB()
    app = Main(root)
    app.pack()
    root.title("Список сотрудников компании")
    root.geometry("665x450")
    root.resizable (False, False)
    root.mainloop()
