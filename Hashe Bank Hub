
import tkinter as tk
from tkinter import messagebox
from tkinter import ttk  
import sqlite3
import random
import hashlib


def create_db():
    conn = sqlite3.connect('banking_app.db', timeout=10)
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS accounts (
                        account_number INTEGER PRIMARY KEY, 
                        first_name TEXT, 
                        last_name TEXT,
                        phone_number TEXT,
                        id_number TEXT,
                        username TEXT,
                        password TEXT,
                        balance REAL)''')
    cursor.execute('''CREATE TABLE IF NOT EXISTS transactions (
                        transaction_id INTEGER PRIMARY KEY AUTOINCREMENT,
                        account_number INTEGER, 
                        transaction_type TEXT, 
                        amount REAL, 
                        timestamp DATETIME DEFAULT CURRENT_TIMESTAMP)''')
    conn.commit()
    conn.close()

def hash_password(password):
    return hashlib.sha256(password.encode()).hexdigest()

def generate_account_number():
    return random.randint(10000, 99999)


def create_account():
    global create_account_window, entry_first_name, entry_last_name, entry_phone_number, entry_id_number, entry_username, entry_password
    create_account_window = tk.Tk()
    create_account_window.title("Create Account")
    create_account_window.geometry('400x500')
    create_account_window.configure(bg="#333333")
    
    
    back_button = tk.Button(create_account_window, text="Back to Login", font=("Arial", 12), bg="#FF5722", fg="white", command=lambda: go_back(create_account_window))
    back_button.pack(pady=10)

    
    label_first_name = tk.Label(create_account_window, text="First Name", font=("Arial", 10), bg="#333333", fg="white")
    label_first_name.pack(pady=10)
    entry_first_name = tk.Entry(create_account_window, font=("Arial", 10))
    entry_first_name.pack(pady=10)

    label_last_name = tk.Label(create_account_window, text="Last Name", font=("Arial", 10), bg="#333333", fg="white")
    label_last_name.pack(pady=10)
    entry_last_name = tk.Entry(create_account_window, font=("Arial", 10))
    entry_last_name.pack(pady=10)

    label_phone_number = tk.Label(create_account_window, text="Phone Number", font=("Arial", 10), bg="#333333", fg="white")
    label_phone_number.pack(pady=10)
    entry_phone_number = tk.Entry(create_account_window, font=("Arial", 10))
    entry_phone_number.pack(pady=10)

    label_id_number = tk.Label(create_account_window, text="ID Number", font=("Arial", 10), bg="#333333", fg="white")
    label_id_number.pack(pady=10)
    entry_id_number = tk.Entry(create_account_window, font=("Arial", 10))
    entry_id_number.pack(pady=10)

    label_username = tk.Label(create_account_window, text="Username", font=("Arial", 10), bg="#333333", fg="white")
    label_username.pack(pady=10)
    entry_username = tk.Entry(create_account_window, font=("Arial", 10))
    entry_username.pack(pady=10)

    label_password = tk.Label(create_account_window, text="Password", font=("Arial", 10), bg="#333333", fg="white")
    label_password.pack(pady=10)
    entry_password = tk.Entry(create_account_window, font=("Arial", 10), show="*")
    entry_password.pack(pady=10)

    create_account_button = tk.Button(create_account_window, text="Create Account", font=("Arial", 12), bg="#4CAF50", fg="white", bd=0, relief="flat", command=create_account_db, padx=30, pady=8)
    create_account_button.pack(pady=20)

    create_account_window.mainloop()

def go_back(window):
    window.destroy()  

def create_account_db():
    first_name = entry_first_name.get().strip()
    last_name = entry_last_name.get().strip()
    phone_number = entry_phone_number.get().strip()
    id_number = entry_id_number.get().strip()
    username = entry_username.get().strip()
    password = entry_password.get().strip()

    
    if not first_name or not last_name or not phone_number or not id_number or not username or not password:
        messagebox.showerror("Input Error", "All fields must be filled!")
        return

    if not first_name.isalpha() or not last_name.isalpha():
        messagebox.showerror("Input Error", "First and Last names must contain only alphabets.")
        return

    if not phone_number.isdigit() or len(phone_number) != 10:
        messagebox.showerror("Input Error", "Phone number must be numeric and contain exactly 10 digits.")
        return

    if not id_number.isdigit() or len(id_number) != 13:
        messagebox.showerror("Input Error", "ID number must be numeric and contain exactly 13 digits.")
        return

    if username_exists(username):
        messagebox.showerror("Input Error", "Username already exists! Please choose a different username.")
        return

    account_number = generate_account_number()
    hashed_password = hash_password(password)

    conn = sqlite3.connect('banking_app.db', timeout=10)
    cursor = conn.cursor()
    cursor.execute('''INSERT INTO accounts (account_number, first_name, last_name, phone_number, 
                      id_number, username, password, balance) VALUES (?, ?, ?, ?, ?, ?, ?, ?)''', 
                   (account_number, first_name, last_name, phone_number, id_number, username, hashed_password, 0.0))
    conn.commit()
    conn.close()

    messagebox.showinfo("Account Created", f"Account created successfully!\nYour account number is {account_number}")
    create_account_window.destroy()
    login_screen()


def username_exists(username):
    conn = sqlite3.connect('banking_app.db', timeout=10)
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM accounts WHERE username=?", (username,))
    account = cursor.fetchone()
    conn.close()
    return account is not None


def login():
    username = entry_login_username.get().strip()
    password = entry_login_password.get().strip()

    if not username or not password:
        messagebox.showerror("Login Error", "Please fill both the username and password fields")
        return

    hashed_password = hash_password(password)

    conn = sqlite3.connect('banking_app.db', timeout=10)
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM accounts WHERE username=? AND password=?", (username, hashed_password))
    account = cursor.fetchone()
    conn.close()

    if account:
        messagebox.showinfo("Login Success", "Login successful!")
        open_dashboard(account)
    else:
        messagebox.showerror("Login Error", "Invalid username or password.")


def login_screen():
    global login_window, entry_login_username, entry_login_password
    login_window = tk.Tk()
    login_window.title("Login")
    login_window.geometry('400x300')
    login_window.configure(bg="#333333")

    label_login_username = tk.Label(login_window, text="Username", font=("Arial", 12), bg="#333333", fg="white")
    label_login_username.pack(pady=10)
    entry_login_username = tk.Entry(login_window, font=("Arial", 12))
    entry_login_username.pack(pady=10)

    label_login_password = tk.Label(login_window, text="Password", font=("Arial", 12), bg="#333333", fg="white")
    label_login_password.pack(pady=10)
    entry_login_password = tk.Entry(login_window, font=("Arial", 12), show="*")
    entry_login_password.pack(pady=10)

    login_button = tk.Button(login_window, text="Login", font=("Arial", 12), bg="#4CAF50", fg="white", command=login)
    login_button.pack(pady=20)

    create_account_button = tk.Button(login_window, text="Create Account", font=("Arial", 12), bg="#FF5722", fg="white", command=create_account)
    create_account_button.pack(pady=10)

    login_window.mainloop()


def open_dashboard(account):
    account_number = account[0]
    username = account[5]
    balance = account[7]
    
    login_window.destroy()

    dashboard_window = tk.Tk()
    dashboard_window.title(f"Dashboard - {username}")
    dashboard_window.geometry('500x600')
    dashboard_window.configure(bg="#333333")

    
    back_button = tk.Button(dashboard_window, text="Logout", font=("Arial", 12), bg="#FF5722", fg="white", command=lambda: go_back(dashboard_window))
    back_button.pack(pady=10)

    
    label_account_number = tk.Label(dashboard_window, text=f"Account Number: {account_number}", font=("Arial", 12, "bold"), bg="#333333", fg="yellow")
    label_account_number.pack(pady=10)

    label_balance = tk.Label(dashboard_window, text=f"Balance: R{balance:.2f}", font=("Arial", 16, "bold"), bg="#333333", fg="white")
    label_balance.pack(pady=20)

    
    def update_balance_display():
        conn = sqlite3.connect('banking_app.db', timeout=10)
        cursor = conn.cursor()
        cursor.execute("SELECT balance FROM accounts WHERE account_number=?", (account_number,))
        new_balance = cursor.fetchone()[0]
        conn.close()
        label_balance.config(text=f"Balance: R{new_balance:.2f}")

    def add_account_to_combobox():
        conn = sqlite3.connect('banking_app.db', timeout=10)
        cursor = conn.cursor()
        cursor.execute("SELECT account_number, username FROM accounts")
        existing_accounts = cursor.fetchall()
        conn.close()
        
        
        existing_accounts_list = [f"{account[1]} (Account: {account[0]})" for account in existing_accounts]
        account_combobox['values'] = existing_accounts_list  

    def deposit_funds():
        try:
            amount = float(entry_amount.get())
            if amount <= 0:
                raise ValueError
        except ValueError:
            messagebox.showerror("Invalid Amount", "Deposit amount must be a positive number.")
            return

        conn = sqlite3.connect('banking_app.db', timeout=10)
        cursor = conn.cursor()
        cursor.execute("UPDATE accounts SET balance = balance + ? WHERE account_number=?", (amount, account_number))
        cursor.execute("INSERT INTO transactions (account_number, transaction_type, amount) VALUES (?, ?, ?)", 
                       (account_number, 'Deposit', amount))
        conn.commit()
        conn.close()
        update_balance_display()
        messagebox.showinfo("Deposit Successful", f"R{amount:.2f} deposited successfully.")

    def withdraw_funds():
        try:
            amount = float(entry_amount.get())
            if amount <= 0:
                raise ValueError
        except ValueError:
            messagebox.showerror("Invalid Amount", "Withdrawal amount must be a positive number.")
            return

        conn = sqlite3.connect('banking_app.db', timeout=10)
        cursor = conn.cursor()
        cursor.execute("SELECT balance FROM accounts WHERE account_number=?", (account_number,))
        balance = cursor.fetchone()[0]
        if amount > balance:
            messagebox.showerror("Insufficient Funds", "You do not have enough balance.")
            return

        cursor.execute("UPDATE accounts SET balance = balance - ? WHERE account_number=?", (amount, account_number))
        cursor.execute("INSERT INTO transactions (account_number, transaction_type, amount) VALUES (?, ?, ?)", 
                       (account_number, 'Withdraw', amount))
        conn.commit()
        conn.close()
        update_balance_display()
        messagebox.showinfo("Withdrawal Successful", f"R{amount:.2f} withdrawn successfully.")

    def transfer_funds():
        try:
            amount = float(entry_amount.get())
            if amount <= 0:
                raise ValueError
        except ValueError:
            messagebox.showerror("Invalid Amount", "Transfer amount must be a positive number.")
            return

        selected_account_str = account_combobox.get()
        if not selected_account_str:
            messagebox.showerror("Invalid Account", "Please select an account from the list.")
            return

        
        transfer_to_account = selected_account_str.split(' ')[-1].strip("()Account:")

        conn = sqlite3.connect('banking_app.db', timeout=10)
        cursor = conn.cursor()
        cursor.execute("SELECT balance FROM accounts WHERE account_number=?", (account_number,))
        balance = cursor.fetchone()[0]
        if amount > balance:
            messagebox.showerror("Insufficient Funds", "You do not have enough balance.")
            return

        
        cursor.execute("UPDATE accounts SET balance = balance - ? WHERE account_number=?", (amount, account_number))
        cursor.execute("UPDATE accounts SET balance = balance + ? WHERE account_number=?", (amount, transfer_to_account))
        cursor.execute("INSERT INTO transactions (account_number, transaction_type, amount) VALUES (?, ?, ?)", 
                       (account_number, 'Transfer', amount))
        cursor.execute("INSERT INTO transactions (account_number, transaction_type, amount) VALUES (?, ?, ?)", 
                       (transfer_to_account, 'Transfer', amount))
        conn.commit()
        conn.close()
        
        update_balance_display()
        messagebox.showinfo("Transfer Successful", f"R{amount:.2f} transferred successfully.")

    
    def show_transaction_history():
        transaction_window = tk.Toplevel(dashboard_window)
        transaction_window.title("Transaction History")
        transaction_window.geometry('500x400')
        transaction_window.configure(bg="#333333")

        conn = sqlite3.connect('banking_app.db', timeout=10)
        cursor = conn.cursor()
        cursor.execute("SELECT * FROM transactions WHERE account_number=? ORDER BY timestamp DESC", (account_number,))
        transactions = cursor.fetchall()
        conn.close()

        listbox = tk.Listbox(transaction_window, font=("Arial", 12), bg="#333333", fg="white", width=60, height=15)
        listbox.pack(pady=10)

        for transaction in transactions:
            listbox.insert(tk.END, f"{transaction[4]} | {transaction[2]} | R{transaction[3]:.2f}")

    
    history_button = tk.Button(dashboard_window, text="Transaction History", font=("Arial", 12), bg="#4CAF50", fg="white", command=show_transaction_history)
    history_button.pack(pady=20)

    
    label_amount = tk.Label(dashboard_window, text="Amount", font=("Arial", 12), bg="#333333", fg="white")
    label_amount.pack(pady=10)
    entry_amount = tk.Entry(dashboard_window, font=("Arial", 12))
    entry_amount.pack(pady=10)

    
    label_transfer_to = tk.Label(dashboard_window, text="Transfer to (Account Number)", font=("Arial", 12), bg="#333333", fg="white")
    label_transfer_to.pack(pady=10)
    
    
    account_combobox = ttk.Combobox(dashboard_window, state="readonly", font=("Arial", 12))
    account_combobox.pack(pady=10)

    add_account_to_combobox()  

    
    deposit_button = tk.Button(dashboard_window, text="Deposit", font=("Arial", 12), bg="#4CAF50", fg="white", command=deposit_funds)
    deposit_button.pack(pady=5)

    withdraw_button = tk.Button(dashboard_window, text="Withdraw", font=("Arial", 12), bg="#FF5722", fg="white", command=withdraw_funds)
    withdraw_button.pack(pady=5)

    transfer_button = tk.Button(dashboard_window, text="Transfer", font=("Arial", 12), bg="#3F51B5", fg="white", command=transfer_funds)
    transfer_button.pack(pady=5)

    dashboard_window.mainloop()


create_db()
login_screen()
