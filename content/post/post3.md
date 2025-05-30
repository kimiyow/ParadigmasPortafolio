---
title: "Práctica 2"
date: 2025-05-16
draft: false
---

#  **Práctica #2, Programa 1: Análisis Programa Control de Memoria**  

## **Montes Solis Kimberly**  

## **── .✦ Introducción**  

En este reporte se analiza el funcionamiento de un módulo en Python diseñado para gestionar el uso de memoria. Se explica el propósito de cada componente del código y su ejecución.

---

## ᯓ★ **Descripción del Código**  

El siguiente programa en Python define una clase para llevar el control de la memoria asignada y liberada en el montón (*heap*).  

```python
'''Module to manage memory usage'''

class MemoryManagement:
    '''Class to manage memory usage'''
    def __init__(self):
        self.heap_allocations = 0
        self.heap_deallocations = 0

    def increment_heap_allocations(self, size):
        '''Increment heap allocations'''
        self.heap_allocations += size

    def increment_heap_deallocations(self, size):
        '''Increment heap deallocations	'''
        self.heap_deallocations += size

    def display_memory_usage(self):
        '''Display memory usage'''
        print(f"Heap allocations: {self.heap_allocations} bytes")
        print(f"Heap deallocations: {self.heap_deallocations} bytes")


memory_management = MemoryManagement()
```

---

###  .✦ **1. Definición del Módulo**

```python
'''Module to manage memory usage'''
```
Este comentario define el propósito general del módulo, que es gestionar el uso de memoria.

---

###  .✦ **2. Clase `MemoryManagement`**

```python
class MemoryManagement:
    '''Class to manage memory usage'''
```
Se define una clase que centraliza toda la lógica de gestión de memoria.

---

###  .✦ **3. Método Constructor**

```python
def __init__(self):
    self.heap_allocations = 0
    self.heap_deallocations = 0
```
Inicializa dos atributos para llevar el registro de las asignaciones y liberaciones de memoria.

---

###  .✦ **4. Método para Asignaciones**

```python
def increment_heap_allocations(self, size):
    self.heap_allocations += size
```
Este método incrementa el total de memoria asignada, sumando la cantidad indicada por el parámetro `size`.

---

### ➖ **5. Método para Liberaciones**

```python
def increment_heap_deallocations(self, size):
    self.heap_deallocations += size
```
Este método incrementa el total de memoria liberada, útil para simular la liberación de recursos.

---

###  .✦ **6. Mostrar Uso de Memoria**

```python
def display_memory_usage(self):
    print(f"Heap allocations: {self.heap_allocations} bytes")
    print(f"Heap deallocations: {self.heap_deallocations} bytes")
```
Muestra el estado actual de la memoria en consola, indicando cuánto se ha asignado y liberado.

---

###  .✦ **7. Crear una Instancia**

```python
memory_management = MemoryManagement()
```
Se crea una instancia del objeto para comenzar a usar las funciones de seguimiento de memoria.

---

## ⋆. 𐙚 ̊ **Conclusión**  

Este módulo simula la gestión de memoria mediante una clase personalizada. Aunque no controla directamente el uso de memoria del sistema, permite entender cómo llevar un registro estructurado de los recursos utilizados y liberados durante la ejecución de un programa. Es útil para fines didácticos y refuerza el uso de estructuras de clase en Python.

---

#  **Práctica #2, Programa 2: Sistema de Manejo de Biblioteca en Python**  

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

#  **Práctica #2, Programa 3: Aplicación Web de Biblioteca con Flask**  

## **Montes Solis Kimberly 376169**  

## **── .✦ Introducción**  
## Descripción General

Este código implementa una **API REST** utilizando **Flask** para manejar una **biblioteca digital**. Permite realizar operaciones CRUD (Crear, Leer, Actualizar, Eliminar) sobre libros y miembros. Está conectado con clases previamente definidas en el módulo `biblioteca` y usa una clase auxiliar `memory_management` para rastrear el uso de memoria.
```python
'''Este archivo contiene la implementación de la aplicación web de la biblioteca.
Se utiliza Flask para crear una API REST que permite realizar operaciones CRUD
sobre los libros y miembros de la biblioteca.'''

from flask import Flask, request, jsonify, render_template
from memory_management import memory_management
from biblioteca import Book, DigitalBook, Member, Library, Genre

app = Flask(__name__)
library = Library()

# Cargar datos al iniciar la aplicación
library.load_library_from_file("library.json")
library.load_members_from_file("members.json")


@app.route('/')
def index():
    '''Ruta principal de la aplicación web.'''
    return render_template('index.html')


@app.route('/books', methods=['GET'])
def get_books():
    '''Obtener todos los libros de la biblioteca.'''
    books = [book.to_dict() for book in library.books]
    return jsonify(books)


@app.route('/books', methods=['POST'])
def add_book():
    '''Agregar un libro a la biblioteca'''
    data = request.json
    is_digital = data.get('is_digital', False)
    if is_digital:
        book = DigitalBook(
            data['id'], data['title'], data['author'], data['publication_year'],
            data['genre'], data['quantity'], data['file_format']
        )
    else:
        book = Book(
            data['id'], data['title'], data['author'], data['publication_year'],
            data['genre'], data['quantity']
        )
    library.add_book(book)
    # Guardar datos después de agregar un libro
    library.save_library_to_file("library.json")
    memory_management.display_memory_usage()
    return jsonify(book.to_dict()), 201


@app.route('/members', methods=['GET'])
def get_members():
    '''Obtener todos los miembros de la biblioteca.'''
    members = [member.to_dict() for member in library.members]
    return jsonify(members)


@app.route('/members', methods=['POST'])
def add_member():
    '''Agregar un miembro a la biblioteca.'''
    data = request.json
    member = Member(data['id'], data['name'])
    library.add_member(member)
    # Guardar datos después de agregar un miembro
    library.save_members_to_file("members.json")
    memory_management.display_memory_usage()
    return jsonify(member.to_dict()), 201


@app.route('/issue_book', methods=['POST'])
def issue_book():
    '''Prestar un libro a un miembro de la biblioteca.'''
    data = request.json
    library.issue_book(data['book_id'], data['member_id'])
    # Guardar datos después de prestar un libro
    library.save_library_to_file("library.json")
    library.save_members_to_file("members.json")
    memory_management.display_memory_usage()
    return jsonify({"message": "Libro prestado satisfactoriamente!"})


@app.route('/return_book', methods=['POST'])
def return_book():
    '''Devolver un libro a la biblioteca.'''
    data = request.json
    library.return_book(data['book_id'], data['member_id'])
    # Guardar datos después de devolver un libro
    library.save_library_to_file("library.json")
    library.save_members_to_file("members.json")
    memory_management.display_memory_usage()
    return jsonify({"message": "Libro devuelto satisfactoriamente!"})


@app.route('/save', methods=['POST'])
def save():
    '''Guardar los datos de la biblioteca en un archivo.'''
    library.save_library_to_file("library.json")
    library.save_members_to_file("members.json")
    memory_management.display_memory_usage()
    return jsonify({"message": "Datos guardados exitosamente!"})


@app.route('/load', methods=['POST'])
def load():
    '''Cargar los datos de la biblioteca desde un archivo.'''
    library.load_library_from_file("library.json")
    library.load_members_from_file("members.json")
    memory_management.display_memory_usage()
    return jsonify({"message": "Datos cargados exitosamente!"})


@app.route('/genres', methods=['GET'])
def get_genres():
    '''Obtener todos los géneros de la biblioteca.'''
    genres = Genre.all_genres()
    return jsonify(genres)


if __name__ == '__main__':
    app.run(debug=True)
```
##  .✦Estructura General

- **Flask**: Se usa para crear endpoints HTTP.
- **JSON**: Los datos de entrada/salida están en formato JSON.
- **HTML**: Renderiza una página principal usando `render_template`.
- **Manejo de Archivos**: Guarda/carga datos en archivos `.json`.

##  .✦Rutas y Funcionalidades

### `/`
- Renderiza una página web principal (`index.html`).

### `/books` (GET)
- Retorna la lista de libros en formato JSON.

### `/books` (POST)
- Recibe un nuevo libro (físico o digital) en JSON y lo agrega a la biblioteca.
- Guarda automáticamente los cambios.

### `/members` (GET)
- Retorna la lista de miembros.

### `/members` (POST)
- Agrega un nuevo miembro a la biblioteca.

### `/issue_book` (POST)
- Presta un libro a un miembro.
- Requiere `book_id` y `member_id` en el JSON.

### `/return_book` (POST)
- Devuelve un libro previamente prestado.

### `/save` (POST)
- Guarda el estado actual de la biblioteca en archivos `.json`.

### `/load` (POST)
- Carga la biblioteca desde archivos `.json` existentes.

### `/genres` (GET)
- Retorna todos los géneros posibles definidos en la clase `Genre`.

##  .✦Clases Importadas

- `Book` y `DigitalBook`: Representan libros físicos y digitales.
- `Member`: Representa a un usuario de la biblioteca.
- `Library`: Administra los libros y miembros.
- `Genre`: Enumera los géneros literarios.
- `memory_management`: Rastrear asignaciones/liberaciones de memoria.

##  .✦Observaciones

- La API REST es adecuada para integrarse con frontend o clientes móviles.
- El diseño sigue el principio de **separación de responsabilidades**.
- Incluye control básico de errores mediante condiciones, aunque podría mejorarse con `try-except` y validación de datos.
- Es importante mantener sincronizados los archivos `library.json` y `members.json` para preservar datos entre ejecuciones.

##  .✦Ejecución

El servidor se ejecuta con:

```bash
python nombre_del_archivo.py
```

La aplicación correrá en `http://127.0.0.1:5000/` con el modo `debug` activado.
