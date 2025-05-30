---
title: "Práctica 1"
date: 2025-03-14
draft: false
---

#  **Práctica #1: Elementos Básicos de los Lenguajes de Programación**

## **Montes Solis Kimberly**

## ✦ Administración de Memoria en C

---

### 𖦹 **Código #1: Definición del Módulo de Administración de Memoria**

```c
#ifndef MEMORY_MANAGEMENT_H
#define MEMORY_MANAGEMENT_H
#include <stdio.h>

#ifndef MEMORY_MANAGEMENT_DISPLAY
#define MEMORY_MANAGEMENT_DISPLAY 0
#endif

extern int heap_allocations;
extern int heap_deallocations;
extern int stack_allocations;
extern int stack_deallocations;

#if MEMORY_MANAGEMENT_DISPLAY
void displayMemoryUsage();
void incrementHeapAllocations(void *pointer, size_t size);
void incrementHeapDeallocations(void *pointer);
void incrementStackAllocations();
void incrementStackDeallocations();
#else
#define displayMemoryUsage() ((void)0)
#define incrementHeapAllocations(pointer, size) ((void)0)
#define incrementHeapDeallocations(pointer) ((void)0)
#define incrementStackAllocations() ((void)0)
#define incrementStackDeallocations() ((void)0)
#endif

#endif // MEMORY_MANAGEMENT_H
```

---

### ✦ Elementos Clave del Código

- **Variables Globales**: `heap_allocations`, `stack_allocations` para rastrear el uso de memoria.
- **Macros Condicionales**: Controlan la visibilidad y ejecución del código.
- **Subprogramas**: Separan funciones según propósito, favoreciendo la modularidad.
- **Tipos de Datos**: Uso de punteros (`void *`) y `size_t`.

---

### 𖦹 **Código #2: Implementación de la Lógica de Rastreo**

```c
#include "memory_management.h"
#include <stdlib.h>

int heap_allocations = 0;
int heap_deallocations = 0;
int stack_allocations = 0;
int stack_deallocations = 0;

typedef struct MemoryRecord {
    void *pointer;
    size_t size;
    struct MemoryRecord *next;
} MemoryRecord;

MemoryRecord *heap_memory_records = NULL;

void addMemoryRecord(void *pointer, size_t size) {
    MemoryRecord *record = (MemoryRecord *)malloc(sizeof(MemoryRecord));
    record->pointer = pointer;
    record->size = size;
    record->next = heap_memory_records;
    heap_memory_records = record;
}

void removeMemoryRecord(void *pointer) {
    MemoryRecord **current = &heap_memory_records;
    while (*current) {
        if ((*current)->pointer == pointer) {
            MemoryRecord *to_free = *current;
            *current = (*current)->next;
            free(to_free);
            return;
        }
        current = &(*current)->next;
    }
}
```

---

### ✦ Funciones Adicionales

```c
#if MEMORY_MANAGEMENT_DISPLAY
void displayMemoryUsage() {
    // Muestra resumen y detalles de memoria
}

void incrementHeapAllocations(void *pointer, size_t size) {
    heap_allocations++;
    addMemoryRecord(pointer, size);
}

void incrementHeapDeallocations(void *pointer) {
    heap_deallocations++;
    removeMemoryRecord(pointer);
}

void incrementStackAllocations() {
    stack_allocations++;
}

void incrementStackDeallocations() {
    stack_deallocations++;
}
#endif
```

---

### ✦ Puntos Clave

- **Gestión del Heap**: Registra asignaciones y liberaciones con listas enlazadas.
- **Visualización de Uso**: Condicional al macro `MEMORY_MANAGEMENT_DISPLAY`.
- **Estructura**: Modular y flexible para integrar en proyectos grandes.
- **Control de Secuencia**: Mediante `#if` y estructuras iterativas como `while`.

---

### ⋆⟡ **Conclusión**

Este módulo en C muestra un manejo detallado del uso de memoria, destacando el uso de estructuras, punteros, macros y funciones condicionales. Es una herramienta eficaz para monitorear recursos en sistemas complejos.

---
