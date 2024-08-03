# dev
strongdev
import json
import os

class Book:
    def __init__(self, title, author, year):
        self.title = title
        self.author = author
        self.year = year

    def __str__(self):
        return f"Title: {self.title}\nAuthor: {self.author}\nYear: {self.year}"

class Library:
    def __init__(self, filename="library.json"):
        self.filename = filename
        self.books = []
        self.load_books()

    def load_books(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                books_data = json.load(file)
                self.books = [Book(**data) for data in books_data]
        else:
            self.books = []

    def save_books(self):
        with open(self.filename, "w") as file:
            json.dump([book.__dict__ for book in self.books], file, indent=4)

    def add_book(self, title, author, year):
        new_book = Book(title, author, year)
        self.books.append(new_book)
        self.save_books()

    def remove_book(self, title):
        self.books = [book for book in self.books if book.title != title]
        self.save_books()

    def edit_book(self, old_title, new_title, new_author, new_year):
        for book in self.books:
            if book.title == old_title:
                book.title = new_title
                book.author = new_author
                book.year = new_year
                self.save_books()
                break

    def display_books(self):
        if not self.books:
            print("No books found.")
        else:
            for book in self.books:
                print(book)
                print("-" * 30)

def main():
    library = Library()

    while True:
        print("\nLibrary Menu")
        print("1. Add book")
        print("2. Remove book")
        print("3. Edit book")
        print("4. View books")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            title = input("Enter book title: ")
            author = input("Enter book author: ")
            year = input("Enter book year: ")
            library.add_book(title, author, year)
        elif choice == "2":
            title = input("Enter the title of the book to remove: ")
            library.remove_book(title)
        elif choice == "3":
            old_title = input("Enter the title of the book to edit: ")
            new_title = input("Enter new book title: ")
            new_author = input("Enter new book author: ")
            new_year = input("Enter new book year: ")
            library.edit_book(old_title, new_title, new_author, new_year)
        elif choice == "4":
            library.display_books()
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
