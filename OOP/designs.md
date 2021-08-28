### OOP

#### Library management

https://www.educative.io/courses/grokking-the-object-oriented-design-interview/RMlM3NgjAyR

###### Actors: 
* Librarian: Mainly responsible for adding and modifying books, book items, and users. The Librarian can also issue, reserve, and return book items.
* Member: All members can search the catalog, as well as check-out, reserve, renew, and return a book.
* System: Mainly responsible for sending notifications for overdue books, canceled reservations, etc.

###### Classes


Library: The central part of the organization for which this software has been designed. It has attributes like ‘Name’ to distinguish it from any other libraries and ‘Address’ to describe its location.

Book: The basic building block of the system. Every book will have ISBN, Title, Subject, Publishers, etc.
BookItem: Any book can have multiple copies, each copy will be considered a book item in our system. Each book item will have a unique barcode.
Author: This class will encapsulate a book author.
Catalog: Catalogs contain list of books sorted on certain criteria. Our system will support searching through four catalogs: Title, Author, Subject, and Publish-date.
Rack: Books will be placed on racks. Each rack will be identified by a rack number and will have a location identifier to describe the physical location of the rack in the library.

Account: We will have two types of accounts in the system, one will be a general member, and the other will be a librarian.
LibraryCard: Each library user will be issued a library card, which will be used to identify users while issuing or returning books.

BookReservation: Responsible for managing reservations against book items.
BookLending: Manage the checking-out of book items.

Fine: This class will be responsible for calculating and collecting fines from library members.
Notification: This class will take care of sending notifications to library members.
