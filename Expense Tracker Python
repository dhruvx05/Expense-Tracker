'''
Expense Tracker: A Python Program for Managing Your Finances...
'''

# A class in Python is a blueprint for creating objects, 
# encapsulating data and functionality into a single unit.

import os
import datetime
import matplotlib.pyplot as plt

class Expense:
    
    def __init__(self, name, category, amount) -> None:
        self.name = name
        self.category = category
        self.amount = amount
        self.date = datetime.datetime.now()
        
    def __repr__(self):
        return f"<Expense: {self.name}, {self.category}, ₹{self.amount:.2f}, {self.date}>"

class User:
    
    def __init__(self, name, age) -> None:
        self.name = name
        self.age = age
        
    def __repr__(self):
        return f"<User: {self.name}, {self.age}>"

def get_user_details():
    print("👤 Enter User Details:")
    name = input("Name: ")
    age = input("Age: ")
    return User(name, age)

def set_budget():
    budget = float(input("💰 Set your monthly budget (in ₹): "))
    return budget

def get_user_expense():
    
    print("🎯 Getting User Expense...")
    expense_name = input("Enter expense name: ")
    expense_amount = float(input("Enter expense amount (in ₹): "))
    expense_categories = [
        "Food",
        "Home",
        "Work",
        "Fun",
        "Miscellaneous",
    ]
    
    while True:
        
        print("Select a category: ")
        for i, category_name in enumerate(expense_categories):
            print(f"  {i+1}. {category_name}")
        
        value_range = f"[1-{len(expense_categories)}]"
        selected_index = int(input(f"Enter a category number {value_range}: "))
        
        if selected_index in range(1, 6):
            selected_category = expense_categories[selected_index - 1]
            new_expense = Expense(
                name=expense_name, category=selected_category, amount=expense_amount
                )
            return new_expense  
        else:
            print("❌ Invalid category, Please try again!")

def save_expense_to_file(expense: Expense, expense_file_path):
    
    with open(expense_file_path, "a") as f:
        f.write(f"{expense.name},{expense.amount},{expense.category}\n")
    print(f"🎯 Saved User Expense: {expense}")

def display_expense_info(expense):
    
    print("\nExpense Details:")
    print(f"Name: {expense.name}")
    print(f"Category: {expense.category}")
    print(f"Amount: ₹{expense.amount:.2f}")

def summarize_expenses(expense_file_path, budget):
    
    print("🎯 Summarizing your Expenses...") 
    expenses = []
    with open(expense_file_path, "r") as f:
        lines = f.readlines()
        for line in lines:
            expense_name, expense_amount, category_name = line.strip().split(",")
            line_expense = Expense(
                name=expense_name, amount=float(expense_amount), category=category_name
                )
            expenses.append(line_expense)
    
    amount_by_category = {}
    for expense in expenses:
        key = expense.category
        if key in amount_by_category:
            amount_by_category[key] += expense.amount
        else:
            amount_by_category[key] = expense.amount
    
    total_spent = sum(expense.amount for expense in expenses)
    budget_left = budget - total_spent
    
    print("Expenses by category: ")
    for key, amount in amount_by_category.items():
        print(f"   {key}: ₹{amount:.2f}")
        
    print(f"\nTotal Spent: ₹{total_spent:.2f}")
    print(f"Budget Left: ₹{budget_left:.2f}")

    # Prompt user to display detailed information for individual expenses
    while True:
        choice = input("\nDo you want to display detailed information for individual expenses? (yes/no): ")
        if choice.lower() == "yes":
            
    # Display detailed information for each expense
            for expense in expenses:
                display_expense_info(expense)
            break
        elif choice.lower() == "no":
            break
        else:
            print("❌ Invalid choice. Please enter 'yes' or 'no'.")

def plot_expenses(expenses):
    categories = set(expense.category for expense in expenses)
    category_amounts = {category: sum(expense.amount for expense in expenses if expense.category == category) for category in categories}
    
    plt.figure(figsize=(10, 6))
    plt.bar(category_amounts.keys(), category_amounts.values(), color='skyblue')
    plt.xlabel('Categories')
    plt.ylabel('Amount Spent (₹)')
    plt.title('Expenses by Category')
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.show()

def main():
    print("🎯 Running Expense Tracker!")
    # Create a directory to store user data if it doesn't exist
    if not os.path.exists("user_data"):
        os.makedirs("user_data")
    
    user_data_path = "user_data"
    
    existing_users = []
    for filename in os.listdir(user_data_path):
        if filename.endswith("_expenses.csv"):
            user_name, user_age, _ = filename.split("_")
            existing_users.append((user_name, user_age))
    
    if existing_users:
        print("Existing Users:")
        for i, (name, age) in enumerate(existing_users, 1):
            print(f"{i}. {name}, Age: {age}")
        
        choice = input("Do you want to continue with an existing user? (yes/no): ")
        if choice.lower() == "yes":
            user_index = int(input("Enter the number corresponding to the user you want to continue with: "))
            if 1 <= user_index <= len(existing_users):
                selected_user = existing_users[user_index - 1]
                user = User(selected_user[0], selected_user[1])
            else:
                print("Invalid selection. Creating a new user.")
                user = get_user_details()
        else:
            user = get_user_details()
    else:
        user = get_user_details()
    
    # Set monthly budget
    budget = set_budget()
    
    print(f"\n📅 Welcome, {user.name}! Let's track your expenses for this month.")
    print(f"💰 Your monthly budget is: ₹{budget:.2f}")
    
    # Set user-specific expense file path
    expense_file_path = os.path.join("user_data", f"{user.name}_{user.age}_expenses.csv")
    
    # Get user expenses
    expenses = []
    total_expense = 0
    while total_expense <= budget:
        expense = get_user_expense()
        expenses.append(expense)
        total_expense += expense.amount
        if total_expense >= budget:
            print("You have reached your monthly budget.")
            break
        if input("Add another expense? (yes/no): ").lower() != "yes":
            break
    
    # Check if budget is exceeded
    if total_expense > budget:
        print("Warning: You have exceeded your monthly budget!")
    
    # Write expenses to file
    for expense in expenses:
        save_expense_to_file(expense, expense_file_path)
    
    # Summarize expenses
    summarize_expenses(expense_file_path, budget)
    
    # Plot expenses
    plot_expenses(expenses)

if __name__ == "__main__":
    main()
