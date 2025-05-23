+++
date = '2025-05-16T13:30:07-07:00'
draft = true
title = 'Practica3'
+++

#  **Práctica #2.3: Aplicación Web de Biblioteca con Flask**  

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

