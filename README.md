package project;

import java.io.*;
import java.util.ArrayList;
import java.util.Scanner;

class Book {
	private String title;
	private String author;
	private String isbn;

	public Book(String title, String author, String isbn) {
		this.title = title;
		this.author = author;
		this.isbn = isbn;
	}

	public String getTitle() {
		return title;
	}

	public String getAuthor() {
		return author;
	}

	public String getIsbn() {
		return isbn;
	}

	@Override
	public String toString() {
		return "Title: " + title + ", Author: " + author + ", ISBN: " + isbn;
	}
}

class Library {
	private ArrayList<Book> books = new ArrayList<>();

	public void addBook(Book book) {
		books.add(book);
	}

	public void removeBook(String isbn) {
		books.removeIf(book -> book.getIsbn().equals(isbn));
	}

	public Book findBookByTitle(String title) {
		for (Book book : books) {
			if (book.getTitle().equalsIgnoreCase(title)) {
				return book;
			}
		}
		return null;
	}

	public Book findBookByAuthor(String author) {
		for (Book book : books) {
			if (book.getAuthor().equalsIgnoreCase(author)) {
				return book;
			}
		}
		return null;
	}

	public void listAllBooks() {
		for (Book book : books) {
			System.out.println(book);
		}
	}

	public void saveLibraryToFile(String filename) {
		try (ObjectOutputStream obj = new ObjectOutputStream(new FileOutputStream(filename))) {
			obj.writeObject(books);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}

	public void loadLibraryFromFile(String filename) {
		try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filename))) {
			books = (ArrayList<Book>) ois.readObject();
		} catch (IOException | ClassNotFoundException e) {
			e.printStackTrace();
		}
	}
}

public class LibraryManagementSystem {
	public static void main(String[] args) {
		Library library = new Library();
		Scanner scanner = new Scanner(System.in);

		while (true) {
			System.out.println("\nLibrary Management System");
			System.out.println("1. Add a book");
			System.out.println("2. Remove a book");
			System.out.println("3. Find a book by title");
			System.out.println("4. Find a book by author");
			System.out.println("5. List all books");
			System.out.println("6. Save library to file");
			System.out.println("7. Load library from file");
			System.out.println("8. Exit");
			System.out.print("Enter your choice: ");

			int choice = scanner.nextInt();
			scanner.nextLine(); // Consume the newline character

			switch (choice) {
			case 1:
				System.out.print("Enter title: ");
				String title = scanner.nextLine();
				System.out.print("Enter author: ");
				String author = scanner.nextLine();
				System.out.print("Enter ISBN: ");
				String isbn = scanner.nextLine();
				Book newBook = new Book(title, author, isbn);
				library.addBook(newBook);
				break;
			case 2:
				System.out.print("Enter ISBN of the book to remove: ");
				String isbnToRemove = scanner.nextLine();
				library.removeBook(isbnToRemove);
				break;
			case 3:
				System.out.print("Enter title to search for: ");
				String titleToSearch = scanner.nextLine();
				Book foundByTitle = library.findBookByTitle(titleToSearch);
				if (foundByTitle != null) {
					System.out.println("Book found: " + foundByTitle);
				} else {
					System.out.println("Book not found.");
				}
				break;
			case 4:
				System.out.print("Enter author to search for: ");
				String authorToSearch = scanner.nextLine();
				Book foundByAuthor = library.findBookByAuthor(authorToSearch);
				if (foundByAuthor != null) {
					System.out.println("Book found: " + foundByAuthor);
				} else {
					System.out.println("Book not found.");
				}
				break;
			case 5:
				System.out.println("All books in the library:");
				library.listAllBooks();
				break;
			case 6:
				System.out.print("Enter filename to save the library: ");
				String saveFilename = scanner.nextLine();
				library.saveLibraryToFile(saveFilename);
				break;
			case 7:
				System.out.print("Enter filename to load the library: ");
				String loadFilename = scanner.nextLine();
				library.loadLibraryFromFile(loadFilename);
				break;
			case 8:
				System.out.println("Exiting Library Management System. Goodbye!");
				scanner.close();
				System.exit(0);
			default:
				System.out.println("Invalid choice. Please try again.");
			}
		}
	}
}
