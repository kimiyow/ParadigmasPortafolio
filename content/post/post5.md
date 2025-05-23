---
title: "Práctica 4"
date: 2025-05-29
draft: false
---

# **Práctica #4: Introducción al Paradigma Lógico con Prolog**

## **Montes Solis Kimberly**

## ✦ Instalación del Entorno de Desarrollo e Introducción a Prolog

Para comenzar con Prolog se recomienda usar **SWI-Prolog**, un entorno gratuito y de código abierto.

🌐 Puedes descargarlo desde: [https://www.swi-prolog.org/download/stable](https://www.swi-prolog.org/download/stable)

Una vez instalado, puedes iniciar la consola de Prolog y comenzar a escribir hechos, reglas y consultas.

---

## ✦ Programación en Prolog

### ⤷ Hechos

Un hecho representa una afirmación básica:

```prolog
cat(tom).
loves_to_eat(jorge, pasta).
lazy(juan).
```

### ⤷ Reglas

Las reglas se definen con `:-` y permiten inferir información:

```prolog
happy(lili) :- dances(lili).
goToPlay(ryan) :- isClosed(school), free(ryan).
```

### ⤷ Consultas

Se realizan para verificar hechos o inferir conocimiento:

```prolog
?- cat(tom).
?- happy(lili).
```

---

## ✦ Aplicaciones con Prolog

### ⋆.˚ Base de Conocimientos

Prolog permite modelar relaciones complejas entre objetos:

```prolog
parent(pam, bob).
male(bob).
female(pam).
mother(X, Y) :- parent(X, Y), female(X).
```

También se pueden definir relaciones más complejas como:

```prolog
brother(X,Y) :- parent(Z,X), parent(Z,Y), male(X), male(Y), X \== Y.
```

### ⋆.˚ Recursión y Árbol Genealógico

```prolog
predecessor(X, Z) :- parent(X, Z).
predecessor(X, Z) :- parent(X, Y), predecessor(Y, Z).
```

### ⋆.˚ Operaciones Matemáticas

```prolog
calc :- X is 100 + 200, write(X).
```

### ⋆.˚ Bucles y Decisiones

```prolog
count_to_10(10) :- write(10).
count_to_10(X) :- write(X), Y is X + 1, count_to_10(Y).
```

```prolog
gte(X,Y) :- X > Y, write('X is greater').
gte(X,Y) :- X =:= Y, write('X and Y are same').
```

### ✦ Listas y Manipulación

Prolog ofrece potentes mecanismos para manipular listas:

```prolog
list_member(X, [X|_]).
list_member(X, [_|TAIL]) :- list_member(X, TAIL).
```

### ✦ Entrada/Salida y Archivos

Puedes manipular entradas/salidas y trabajar con archivos usando predicados como `read/1`, `write/1`, etc.

---

## ⋆⟡ **Conclusión**

Prolog es un lenguaje poderoso para la lógica simbólica, relaciones y estructuras de conocimiento. Su capacidad para realizar inferencias lo hace ideal en áreas como **IA, sistemas expertos y árboles genealógicos**. La sintaxis declarativa y la potencia del motor lógico permiten resolver problemas complejos de manera elegante.

---
