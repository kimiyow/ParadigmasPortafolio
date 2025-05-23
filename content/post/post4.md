---
title: "Práctica 2"
date: 2025-03-15
draft: false
---

#  **Práctica #2.2: Sistema de Manejo de Biblioteca en Python**  

## **Montes Solis Kimberly 376169**  

## **── .✦ Introducción**  

En esta práctica se desarrolló un sistema de manejo de biblioteca utilizando programación orientada a objetos en **Python**. Se aplicaron conceptos como clases, herencia, encapsulamiento, polimorfismo y manejo de archivos JSON, además del uso de un módulo externo para gestionar memoria.  

---

## ᯓ★ **Estructura del Programa**  

El sistema está compuesto por múltiples clases que representan los elementos clave de una biblioteca: libros, libros digitales, miembros y la propia biblioteca. También se incluye un módulo adicional llamado `memory_management` para monitorear la memoria asignada y liberada.  

---
```python
'''Módulo para definir las clases de la biblioteca y sus operaciones'''

import json # manejo de datos
from memory_management import memory_management #guardan info de libros y miembros


class Genre:
    #enum
    '''Clase para definir los generos de los libros'''
    FICTION = "Ficcion" 
    NON_FICTION = "No Ficcion"
    SCIENCE = "Ciencia"
    HISTORY = "Historia"
    FANTASY = "Fantasia"
    BIOGRAPHY = "Biografia"
    OTHER = "Otro"

    @classmethod
    def all_genres(cls): # class
        return [cls.FICTION, cls.NON_FICTION, cls.SCIENCE, cls.HISTORY, cls.FANTASY, cls.BIOGRAPHY, cls.OTHER] # lista de los generos


class Book:
    '''Clase para definir los libros de la biblioteca'''
    def __init__(self, book_id, title, author, publication_year, genre, quantity): # constructor y sus argumentos/atributos
        self.id = book_id
        self.title = title
        self.author = author
        self.publication_year = publication_year
        self.genre = genre
        self.quantity = quantity
        memory_management.increment_heap_allocations(1)

    def __del__(self): # destructor de la clase
        memory_management.increment_heap_deallocations(1)

    def to_dict(self):  
        '''Método para convertir los datos del libro en un diccionario'''
        return {
            "id": self.id,
            "title": self.title,
            "author": self.author,
            "publication_year": self.publication_year,
            "genre": self.genre,
            "quantity": self.quantity
        } # accedemos por el nombre, en ligar de num 0,1,2...

    @staticmethod # de diccionario a tipo book
    def from_dict(data):
        '''Método para crear un objeto libro a partir de un diccionario''' # un diccionario es como un tipo de arreglo
        return Book(
            data["id"],
            data["title"],
            data["author"],
            data["publication_year"],
            data["genre"],
            data["quantity"]
        )


class DigitalBook(Book): # esta heredando de book metodos y atributos
    '''Clase para definir los libros digitales de la biblioteca'''
    def __init__(self, book_id, title, author, publication_year, genre, quantity, file_format):
        '''Constructor de la clase DigitalBook'''
        super().__init__(book_id, title, author, publication_year, genre, quantity) # manda a llamar a sus superclases
        self.file_format = file_format

    def to_dict(self): # POLIMORFISMO ---- sobreescribir y sobrecarga metodos
        '''Método para convertir los datos del libro digital en un diccionario'''
        data = super().to_dict()
        data["file_format"] = self.file_format
        return data

    @staticmethod
    def from_dict(data): 
        '''Método para crear un objeto libro digital a partir de un diccionario'''
        return DigitalBook(
            data["id"],
            data["title"],
            data["author"],
            data["publication_year"],
            data["genre"],
            data["quantity"],
            data["file_format"]
        )


class Member:
    '''Clase para definir los miembros de la biblioteca'''
    def __init__(self, member_id, name):
        '''Constructor de la clase Member'''
        self.id = member_id
        self.name = name
        self.issued_books = []
        memory_management.increment_heap_allocations(1)

    def __del__(self):
        memory_management.increment_heap_deallocations(1)

    def to_dict(self):
        '''Método para convertir los datos del miembro en un diccionario'''
        return {
            "id": self.id,
            "name": self.name,
            "issued_books": self.issued_books
        }

    @staticmethod
    def from_dict(data): # convertir desde diccionario
        '''Método para crear un objeto miembro a partir de un diccionario'''
        member = Member(data["id"], data["name"])
        member.issued_books = data["issued_books"]
        return member


class Library:
    '''Clase para definir la biblioteca'''
    def __init__(self):
        '''Constructor de la clase Library'''
        self.books = []
        self.members = []
        memory_management.increment_heap_allocations(1)

    def __del__(self):
        memory_management.increment_heap_deallocations(1)

    def add_book(self, book):
        '''Método para agregar un libro a la biblioteca'''
        self.books.append(book)
        print("\nEl libro fue agregado exitosamente!\n")
        memory_management.display_memory_usage()

    def find_book_by_id(self, book_id):
        '''Método para buscar un libro por su ID'''
        for book in self.books:
            if book.id == book_id:
                return book
        return None

    def display_books(self):
        '''Método para mostrar los libros disponibles en la biblioteca'''
        if not self.books:
            print("\nNo hay libros disponibles.\n")
            return
        print("\nLibros disponibles en biblioteca:\n")
        for book in self.books:
            print(f"ID libro: {book.id}\nTitulo: {book.title}\nAutor: {book.author}\nAno de publicacion: {book.publication_year}\nGenero: {book.genre}\nCantidad: {book.quantity}\n")
            if isinstance(book, DigitalBook):
                print(f"Formato de archivo: {book.file_format}\n") # solo lo tiene el libro digital
        memory_management.display_memory_usage()

    def add_member(self, member):
        '''Método para agregar un miembro a la biblioteca'''
        self.members.append(member)
        print("\nMiembro agregado exitosamente!\n")
        memory_management.display_memory_usage()

    def issue_book(self, book_id, member_id):
        '''Método para prestar un libro a un miembro'''
        book = self.find_book_by_id(book_id)
        member = self.find_member_by_id(member_id)
        if book and member and book.quantity > 0:
            book.quantity -= 1
            member.issued_books.append(book_id)
            print("\nLibro prestado satisfactoriamente!\n")
        else:
            print("\nLibro o miembro no encontrados.\n")
        memory_management.display_memory_usage()

    def return_book(self, book_id, member_id):
        '''Método para devolver un libro prestado por un miembro'''
        book = self.find_book_by_id(book_id)
        member = self.find_member_by_id(member_id)
        if book and member and book_id in member.issued_books:
            book.quantity += 1
            member.issued_books.remove(book_id)
            print("\nLibro devuelto satisfactoriamente!\n")
        else:
            print("\nLibro o miembro no encontrados.\n")
        memory_management.display_memory_usage()

    def find_member_by_id(self, member_id):
        '''Método para buscar un miembro por su ID'''
        for member in self.members:
            if member.id == member_id:
                return member
        return None

    def display_members(self):
        '''Método para mostrar los miembros disponibles en la biblioteca'''
        if not self.members:
            print("\nNo hay miembros disponibles.\n")
            return
        print("\nMiembros disponibles en biblioteca:\n")
        for member in self.members:
            print(f"ID miembro: {member.id}\nNombre: {member.name}\nCantidad de libros prestados: {len(member.issued_books)}\n")
            for book_id in member.issued_books:
                book = self.find_book_by_id(book_id)
                if book:
                    print(f"  Libro ID: {book.id}\n  Titulo: {book.title}\n  Autor: {book.author}\n")
        memory_management.display_memory_usage()

    def search_member(self, member_id):
        '''Método para buscar un miembro por su ID y mostrar los libros prestados'''
        member = self.find_member_by_id(member_id)
        if member:
            print(f"ID miembro: {member.id}\nNombre: {member.name}\nCantidad de libros prestados: {len(member.issued_books)}\n")
            for book_id in member.issued_books:
                book = self.find_book_by_id(book_id)
                if book:
                    print(f"  Libro ID: {book.id}\n  Titulo: {book.title}\n  Autor: {book.author}\n")
        else:
            print("\nMiembro no encontrado.\n")
        memory_management.display_memory_usage()

    def save_library_to_file(self, filename):
        '''Método para guardar la biblioteca en un archivo JSON'''
        with open(filename, 'w', encoding='UTF-8') as file:
            json.dump([book.to_dict() for book in self.books], file) # archivo
        print(f"Biblioteca guardada exitosamente en {filename}\n")
        memory_management.display_memory_usage()

    def load_library_from_file(self, filename):
        '''Método para cargar la biblioteca desde un archivo JSON'''
        try:
            with open(filename, 'r', encoding='UTF-8') as file:
                books_data = json.load(file)
                self.books = [DigitalBook.from_dict(data) if "file_format" in data else Book.from_dict(data) for data in books_data]
            print(f"Biblioteca cargada exitosamente desde {filename}\n")
        except FileNotFoundError:
            print("Error al abrir el archivo para cargar la biblioteca.\n")
        memory_management.display_memory_usage()

    def save_members_to_file(self, filename):
        '''Método para guardar los miembros en un archivo JSON'''
        with open(filename, 'w', encoding='UTF-8') as file:
            json.dump([member.to_dict() for member in self.members], file)
        print(f"Miembros guardados exitosamente en {filename}\n")
        memory_management.display_memory_usage()

    def load_members_from_file(self, filename):
        '''Método para cargar los miembros desde un archivo JSON'''
        try:
            with open(filename, 'r', encoding='UTF-8') as file:
                members_data = json.load(file)
                self.members = [Member.from_dict(data) for data in members_data]
            print(f"Miembros cargados exitosamente desde {filename}\n")
        except FileNotFoundError:
            print("Error al abrir el archivo para cargar los miembros.\n")
        memory_management.display_memory_usage()


def main():
    '''Función principal para ejecutar el programa'''
    library = Library()
    library.load_library_from_file("library.json")
    library.load_members_from_file("members.json")

    while True:
        print("\nMenu de sistema de manejo de biblioteca\n")
        print("\t1. Agregar un libro")
        print("\t2. Mostrar libros disponibles")
        print("\t3. Agregar un miembro")
        print("\t4. Prestar libro")
        print("\t5. Devolver libro")
        print("\t6. Mostrar miembros disponibles")
        print("\t7. Buscar miembro")
        print("\t8. Guardar y salir")
        choice = int(input("Indica tu opcion: "))

        if choice == 1:
            book_id = int(input("Ingresa ID del libro: "))
            title = input("Ingresa titulo del libro: ")
            author = input("Ingresa nombre del autor: ")
            publication_year = int(input("Ingresa el ano de publicacion: "))
            genre = input("Ingresa el genero del libro: ")
            quantity = int(input("Ingresa la cantidad de libros: "))
            is_digital = input("Es un libro digital? (s/n): ").lower() == 's'
            if is_digital:
                file_format = input("Ingresa el formato del archivo: ")
                book = DigitalBook(book_id, title, author, publication_year, genre, quantity, file_format)
            else:
                book = Book(book_id, title, author, publication_year, genre, quantity)
            library.add_book(book)
        elif choice == 2:
            library.display_books()
        elif choice == 3:
            member_id = int(input("Ingresa el ID del miembro: "))
            name = input("Ingresa el nombre del miembro: ")
            member = Member(member_id, name)
            library.add_member(member)
        elif choice == 4:
            member_id = int(input("Ingresa el ID del miembro: "))
            book_id = int(input("Ingresa el ID del libro: "))
            library.issue_book(book_id, member_id)
        elif choice == 5:
            member_id = int(input("Ingresa el ID del miembro: "))
            book_id = int(input("Ingresa el ID del libro: "))
            library.return_book(book_id, member_id)
        elif choice == 6:
            library.display_members()
        elif choice == 7:
            member_id = int(input("Ingresa el ID del miembro: "))
            library.search_member(member_id)
        elif choice == 8:
            library.save_library_to_file("library.json")
            library.save_members_to_file("members.json")
            print("Saliendo del programa\n")
            break
        else:
            print("Esta no es una opcion valida!!!\n")


if __name__ == "__main__":
    main()
```

## 𖦹 **Clases y Funcionalidades Principales**  

###  .✦ **Clase `Genre`**
Define géneros de libros como constantes de clase (simula un `enum`) y proporciona un método `all_genres()` para obtener todos.  

### v **Clase `Book`**
Representa libros físicos con atributos como ID, título, autor, año, género y cantidad. Tiene métodos para convertir a diccionario (`to_dict`) y reconstruir desde uno (`from_dict`).  

###  .✦ **Clase `DigitalBook`**
Hereda de `Book`, añadiendo el atributo `file_format`. Sobrescribe `to_dict` y `from_dict`, aplicando **polimorfismo** para adaptar el comportamiento.  

###  .✦ **Clase `Member`**
Representa a los usuarios registrados. Puede almacenar una lista de IDs de libros que tiene prestados.  

###  .✦ **Clase `Library`**
Administra los libros y miembros, incluyendo funcionalidades para:
- Agregar libros y miembros
- Mostrar registros
- Prestar y devolver libros
- Buscar miembros
- Guardar y cargar datos desde archivos JSON

Cada operación llama a métodos del módulo `memory_management` para mostrar el uso de memoria.  

---

##  .✦ **Manejo de Archivos**  

El sistema puede guardar y cargar el estado de la biblioteca en archivos `.json`, lo que facilita la persistencia de datos:  

```python
json.dump([book.to_dict() for book in self.books], file)
json.load(file)
```  

Esto permite almacenar libros y miembros como listas de diccionarios.  

---

##  .✦ **Gestión de Memoria**  

Cada clase relevante (Book, DigitalBook, Member, Library) usa un módulo externo `memory_management` para registrar:
- Asignaciones (`increment_heap_allocations`)
- Liberaciones (`increment_heap_deallocations`)

Esto refuerza el concepto de administración de recursos dentro de un sistema más grande.  

---

##  .✦ **Interfaz Principal (`main`)**  

La función principal proporciona un menú interactivo para:
1. Agregar libro
2. Mostrar libros
3. Agregar miembro
4. Prestar libro
5. Devolver libro
6. Mostrar miembros
7. Buscar miembro
8. Guardar y salir

Cada opción interactúa con el usuario a través de la terminal.  

```python
choice = int(input("Indica tu opcion: "))
```

Esto permite probar el sistema de forma manual y validarlo sin interfaz gráfica.  

---

##  .✦ **Conclusión**  

El desarrollo de este sistema permitió reforzar habilidades en **POO**, así como el uso de archivos JSON y buenas prácticas como modularidad y separación de responsabilidades. Además, el manejo de memoria ofreció una perspectiva útil sobre la administración de recursos.  

⋆˙⟡ Una solución práctica y educativa para el manejo de bibliotecas digitales y físicas.

---
