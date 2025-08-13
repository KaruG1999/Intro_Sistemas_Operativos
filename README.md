# Sistemas Operativos

## Objetivo General
Proporcionar los conceptos fundamentales de los Sistemas Operativos desde el enfoque clásico del SO como **administrador eficiente de recursos** y **facilitador del uso** para el usuario, desarrollando casos experimentales en sistemas de mayor penetración en el mercado.

## Contenido del Curso

### Tema 1: Introducción
**Objetivo:** Establecer las bases conceptuales de los sistemas operativos y su evolución.

**Temas importantes:**
- Definición del SO como administrador de recursos
- Servicios del SO y arquitecturas basadas en servicios
- Tipos de sistemas: Batch, Multiprogrammed, Time-Sharing, Paralelos, Distribuidos, Tiempo Real
- Estructuras del SO: kernel, capas, máquinas virtuales
- Conceptos básicos: eventos, interrupciones, excepciones, llamadas al sistema

### Tema 2: Procesos y Scheduling
**Objetivo:** Comprender la gestión de procesos y la planificación de la CPU.

**Temas importantes:**
- Definición, estructura y creación de procesos
- Planificación de procesos y administración de CPU
- Políticas de scheduling: Round-Robin, FIFO, SJF, colas multinivel
- Conceptos de quantum, tiempo de retorno, preemption
- Hilos (threads) y comunicación entre procesos

### Tema 3: Administración de Memoria
**Objetivo:** Analizar las políticas y técnicas de gestión de memoria.

**Temas importantes:**
- Políticas de administración: Monitor Residente, Particionado, Paginado, Segmentado
- Resolución de direcciones y carga dinámica
- Memoria Virtual: overlays, paginación bajo demanda
- Concepto de localidad, espacio de trabajo e hiperpaginado
- Análisis de rendimiento de sistemas de paginación

### Tema 4: Entrada/Salida
**Objetivo:** Estudiar la gestión de dispositivos de E/S y su optimización.

**Temas importantes:**
- Relación con hardware de E/S: controladores, buses, polling
- Interfaz aplicación-E/S y scheduling de I/O
- Técnicas de optimización: Buffering, Caching, Spooling
- Algoritmos de scheduling de disco: FCFS, SSTF, SCAN, LOOK

### Tema 5: Administración de Archivos
**Objetivo:** Comprender la organización y gestión del sistema de archivos.

**Temas importantes:**
- Concepto de filesystem y tipos de archivos
- Estructura física y operaciones sobre archivos
- Gestión de directorios y protección
- Métodos de asignación de espacio

### Tema 6: Buffer Cache (System V, Unix)
**Objetivo:** Analizar el sistema de cache de buffers en sistemas Unix.

**Temas importantes:**
- Estructura y estados del buffer
- Buffer pool, free list y hash queues
- Algoritmos de recuperación de buffers
- Ventajas y desventajas del buffer cache

## Enfoque Práctico
El curso incluye casos experimentales en sistemas operativos de mayor penetración en el mercado, aplicando los conceptos teóricos a implementaciones reales.