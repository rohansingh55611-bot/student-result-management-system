import sqlite3
import os

# --- DATABASE SETUP ---
def initialize_database():
    conn = sqlite3.connect('sms_project.db')
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS students (
            roll_no INTEGER PRIMARY KEY,
            name TEXT NOT NULL,
            age INTEGER,
            course TEXT,
            grade TEXT
        )
    ''')
    conn.commit()
    conn.close()

# --- FUNCTIONS ---
def add_student():
    try:
        roll = int(input("Enter Roll Number (Integer): "))
        name = input("Enter Student Name: ")
        age = int(input("Enter Age: "))
        course = input("Enter Course: ")
        grade = input("Enter Grade: ")

        conn = sqlite3.connect('sms_project.db')
        cursor = conn.cursor()
        cursor.execute("INSERT INTO students VALUES (?, ?, ?, ?, ?)", (roll, name, age, course, grade))
        conn.commit()
        conn.close()
        print("\n✅ Student added successfully!")
    except sqlite3.IntegrityError:
        print("\n❌ Error: Roll Number already exists!")
    except ValueError:
        print("\n❌ Error: Invalid input! Please enter numbers where required.")

def view_all():
    conn = sqlite3.connect('sms_project.db')
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM students")
    data = cursor.fetchall()
    conn.close()

    if data:
        print("\n--- ALL STUDENT RECORDS ---")
        print(f"{'Roll No':<10} {'Name':<20} {'Age':<5} {'Course':<15} {'Grade':<5}")
        print("-" * 55)
        for s in data:
            print(f"{s[0]:<10} {s[1]:<20} {s[2]:<5} {s[3]:<15} {s[4]:<5}")
    else:
        print("\n📂 Database is empty.")

def delete_student():
    roll = int(input("Enter Roll Number to delete: "))
    conn = sqlite3.connect('sms_project.db')
    cursor = conn.cursor()
    cursor.execute("DELETE FROM students WHERE roll_no = ?", (roll,))
    if cursor.rowcount > 0:
        print(f"\n🗑️ Student with Roll No {roll} deleted.")
    else:
        print("\n❌ Student not found.")
    conn.commit()
    conn.close()

# --- MAIN MENU ---
def main():
    initialize_database()
    while True:
        print("\n--- STUDENT MANAGEMENT SYSTEM ---")
        print("1. Add New Student")
        print("2. View All Students")
        print("3. Delete Student")
        print("4. Exit")
        
        choice = input("\nSelect Option (1-4): ")
        
        if choice == '1':
            add_student()
        elif choice == '2':
            view_all()
        elif choice == '3':
            delete_student()
        elif choice == '4':
            print("Exiting... Thank you!")
            break
        else:
            print("Invalid Choice! Please try again.")

if __name__ == "__main__":
    main()# student-result-management-system
student result management system
