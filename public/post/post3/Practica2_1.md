---
title: "Práctica 2 Programa 1"
date: 2025-05-16
draft: false
---

#  **Práctica #2.1: Análisis Programa Control de Memoria**  

## **Montes Solis Kimberly 376169**  

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
