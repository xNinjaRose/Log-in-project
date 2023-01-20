import hashlib
import sqlite3

def register():
    username = input("Enter your username: ")
    password = input("Enter your password: ")

    # Generate a random salt
    salt = ''.join(random.choice(string.ascii_letters) for i in range(10))

    # Hash the password with the salt using SHA-256
    hashed_password = hashlib.sha256((password + salt).encode()).hexdigest()

    # Connect to the database
    conn = sqlite3.connect("users.db")
    c = conn.cursor()

    # Insert the new user into the database
    c.execute("INSERT INTO users (username, salt, hashed_password) VALUES (?, ?, ?)", (username, salt, hashed_password))

    # Commit the changes and close the connection
    conn.commit()
    conn.close()

    print("You have been registered successfully!")

def login():
    username = input("Enter your username: ")
    password = input("Enter your password: ")

    # Connect to the database
    conn = sqlite3.connect("users.db")
    c = conn.cursor()

    # Retrieve the salt and hashed password for the input username
    c.execute("SELECT salt, hashed_password FROM users WHERE username = ?", (username,))
    result = c.fetchone()

    if result:
        # Unpack the salt and hashed password from the query result
        salt, hashed_password = result

        # Hash the input password with the retrieved salt using SHA-256
        input_hashed_password = hashlib.sha256((password + salt).encode()).hexdigest()

        if input_hashed_password == hashed_password:
            print("Welcome, " + username + "!")
        else:
            print("Invalid username or password. Please try again.")
    else:
        print("Invalid username or password. Please try again.")

    # Close the connection
    conn.close()

# Create the users table if it doesn't already exist
conn = sqlite3.connect("users.db")
c = conn.cursor()
c.execute('''CREATE TABLE IF NOT EXISTS users
             (username text, salt text, hashed_password text)''')
conn.commit()
conn.close()

while True:
    print("1. Register")
    print("2. Login")
    print("3. Exit")

    choice = input()

    if choice == "1":
        register()
    elif choice == "2":
        login()
    elif choice == "3":
        break
    else:
        print("Invalid choice. Please try again.")
