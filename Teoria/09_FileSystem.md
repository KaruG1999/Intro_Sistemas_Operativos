# Sistema de Archivos (File System)

## 📋 Índice
1. [Conceptos Fundamentales](#conceptos-fundamentales)
2. [Estructura y Organización](#estructura-y-organización)
3. [Métodos de Asignación](#métodos-de-asignación)
4. [Gestión de Espacio Libre](#gestión-de-espacio-libre)
5. [Implementaciones: UNIX y Windows](#implementaciones-unix-y-windows)
6. [Protección y Seguridad](#protección-y-seguridad)

---

## Conceptos Fundamentales

### ¿Por qué necesitamos archivos?

1. **Almacenar grandes cantidades de datos**
2. **Persistencia**: Almacenamiento a largo plazo
3. **Compartir información**: Permitir que distintos procesos accedan a los mismos datos

### ¿Qué es un archivo?

> **Archivo**: Entidad abstracta con nombre que representa un espacio lógico continuo y direccionable

**Características principales:**
- Provee datos de entrada a los programas
- Permite guardar datos de salida
- El programa mismo es información almacenada como archivo

```
┌─────────────────────────────┐
│      ARCHIVO (Abstracción)  │
│  "documento.txt" - 1.5 MB   │
└──────────────┬──────────────┘
               │
    ┌──────────┴──────────┐
    │  Sistema de Archivos │
    └──────────┬──────────┘
               │
    ┌──────────┴──────────┐
    │   Bloques en Disco   │
    │  [B1][B2][B3][B4]... │
    └─────────────────────┘
```

### Terminología básica

**Sector**
- Unidad mínima de almacenamiento en discos rígidos
- Típicamente 512 bytes o 4096 bytes

**Bloque/Cluster**
- Conjunto de sectores consecutivos
- Unidad de asignación del sistema de archivos
- Ejemplo: 1 bloque = 8 sectores = 4 KB

```
Disco Físico:
┌────┬────┬────┬────┬────┬────┬────┬────┐
│ S1 │ S2 │ S3 │ S4 │ S5 │ S6 │ S7 │ S8 │ Sectores
└────┴────┴────┴────┴────┴────┴────┴────┘
└───────────────────┘ └───────────────────┘
      Bloque 1              Bloque 2
```

**File System**
- Define la forma en que los datos son almacenados y organizados
- Determina cómo se asignan bloques a archivos
- Ejemplos: FAT, NTFS, ext4, etc.

**FAT (File Allocation Table)**
- Estructura que contiene información sobre dónde están ubicados los archivos
- Mapeo entre archivos y bloques de disco

---

## Estructura y Organización

### Tipos de Archivos

**1. Archivos Regulares**

*Texto Plano:*
- Archivos de código fuente (.c, .java, .py)
- Documentos de texto (.txt, .md)

*Binarios:*
- Archivos objeto (.o, .obj)
- Ejecutables (.exe, .out)
- Imágenes, videos, etc.

**2. Directorios**
- Archivos especiales que mantienen la estructura del sistema
- Contienen referencias a otros archivos y directorios

**3. Archivos Especiales (en UNIX)**
- Dispositivos: /dev/sda (disco), /dev/tty (terminal)
- Named pipes: Comunicación entre procesos
- Links: Referencias a otros archivos
- Links simbólicos: Enlaces entre diferentes file systems

### Atributos de un Archivo

```
┌─────────────────────────────────┐
│   Metadatos del Archivo         │
├─────────────────────────────────┤
│ Nombre: documento.txt           │
│ Identificador: i-nodo #12345    │
│ Tipo: Archivo regular           │
│ Tamaño: 2,048 bytes             │
│ Localización: Bloques 100-103   │
│ Owner: usuario123               │
│ Permisos: rw-r--r--             │
│ Creado: 2025-01-15 10:30        │
│ Modificado: 2025-11-09 14:22    │
│ Último acceso: 2025-11-09 16:45 │
└─────────────────────────────────┘
```

**Ejemplo en Linux:**
```
-rw-r--r--  1  juan  staff  2048  Nov 09 14:22  documento.txt
│  │  │  │   │   │     │      │       │              │
│  │  │  │   │   │     │      │       │              └─ Nombre
│  │  │  │   │   │     │      │       └─ Fecha modificación
│  │  │  │   │   │     │      └─ Tamaño
│  │  │  │   │   │     └─ Grupo dueño
│  │  │  │   │   └─ Usuario dueño
│  │  │  │   └─ Referencias (hard links)
│  │  │  └─ Permisos otros (read)
│  │  └─ Permisos grupo (read)
│  └─ Permisos usuario (read+write)
└─ Tipo (- = archivo regular)
```

### Directorios

**Funciones principales:**
- Contener información sobre archivos y subdirectorios
- Resolver la correspondencia entre nombres y archivos
- Organizar archivos jerárquicamente

**Operaciones en directorios:**
- Buscar un archivo
- Crear/eliminar archivos (entradas)
- Listar contenido
- Renombrar archivos
- Cambiar permisos

### Estructura Jerárquica (Árbol)

```
                    / (raíz)
                    │
        ┌───────────┼───────────┬───────────┐
        │           │           │           │
      home        usr         var         etc
        │           │           │           │
    ┌───┴───┐   ┌───┴───┐   ┌───┴───┐   config
    │       │   │       │   │       │
  juan   maria bin    lib  www    log
    │                       │
documents.txt          index.html
```

### Rutas (Paths)

**PATH Absoluto**
- Comienza desde el directorio raíz
- Ruta completa e inequívoca
- Linux: `/home/juan/documents.txt`
- Windows: `C:\Users\Juan\Documents\archivo.txt`

**PATH Relativo**
- Relativo al directorio actual (working directory)
- Usa `.` (directorio actual) y `..` (directorio padre)

```
Si estoy en: /var/spool/mail/
Para llegar a: /var/www/index.html

PATH Absoluto:  /var/www/index.html
PATH Relativo:  ../../www/index.html
                │  │   └─ directorio destino
                │  └─ sube a /var
                └─ sube a /var/spool
```

---

## Métodos de Asignación

Los sistemas de archivos deben decidir cómo asignar bloques de disco a los archivos. Existen dos estrategias principales de asignación:

### Pre-asignación vs. Asignación Dinámica

**Pre-asignación:**
- Se define el tamaño del archivo al crearlo
- Se asigna todo el espacio de una vez
- Ventaja: Puede usar bloques contiguos (acceso rápido)
- Desventaja: Desperdicio de espacio, rigidez

**Asignación Dinámica:**
- El espacio se solicita a medida que se necesita
- Los bloques pueden quedar dispersos en el disco
- Ventaja: Flexibilidad, mejor uso del espacio
- Desventaja: Posible fragmentación

---

### 1. Asignación Continua

**Concepto:** Conjunto continuo de bloques asignados a un archivo

```
Disco con archivos:
┌────┬────┬────┬────┬────┬────┬────┬────┬────┬────┐
│ A  │ A  │ A  │ B  │ B  │ -- │ -- │ C  │ C  │ C  │
└────┴────┴────┴────┴────┴────┴────┴────┴────┴────┘
  0    1    2    3    4    5    6    7    8    9

Archivo A: Inicio=0, Longitud=3
Archivo B: Inicio=3, Longitud=2
Archivo C: Inicio=7, Longitud=3
```

**Tabla de Asignación (FAT):**
```
Archivo  | Inicio | Longitud
---------|--------|----------
A        |   0    |    3
B        |   3    |    2
C        |   7    |    3
```

**Ventajas:**
- ✅ Lectura muy rápida (una sola operación de I/O)
- ✅ FAT simple (solo inicio y longitud)
- ✅ Ideal para acceso secuencial y aleatorio

**Desventajas:**
- ❌ Fragmentación externa (espacios libres dispersos)
- ❌ Difícil encontrar espacio contiguo para archivos grandes
- ❌ Difícil hacer crecer archivos existentes

**Problema de fragmentación:**
```
Estado inicial:
┌────┬────┬────┬────┬────┬────┬────┬────┐
│ A  │ A  │ A  │ B  │ B  │ C  │ C  │ C  │
└────┴────┴────┴────┴────┴────┴────┴────┘

Después de borrar B:
┌────┬────┬────┬────┬────┬────┬────┬────┐
│ A  │ A  │ A  │ -- │ -- │ C  │ C  │ C  │
└────┴────┴────┴────┴────┴────┴────┴────┘
              └──┬──┘
           2 bloques libres (no sirve para archivo de 3 bloques)

¿Cómo agregar un archivo D de 3 bloques?
NO HAY ESPACIO CONTIGUO suficiente!
```

**Solución: Compactación**
```
Antes de compactar:
┌────┬────┬────┬────┬────┬────┬────┬────┐
│ A  │ A  │ A  │ -- │ -- │ C  │ C  │ C  │
└────┴────┴────┴────┴────┴────┴────┴────┘

Después de compactar:
┌────┬────┬────┬────┬────┬────┬────┬────┐
│ A  │ A  │ A  │ C  │ C  │ C  │ -- │ -- │
└────┴────┴────┴────┴────┴────┴────┴────┘
                              └────┬────┘
                        Espacio contiguo disponible
```
⚠️ La compactación es **muy costosa** (requiere mover muchos datos)

---

### 2. Asignación Encadenada

**Concepto:** Cada bloque contiene un puntero al siguiente bloque del archivo

```
Archivo A distribuido en el disco:
┌─────┬─────┬─────┬─────┬─────┬─────┐
│  0  │  1  │  2  │  3  │  4  │  5  │ Bloques
└─────┴─────┴─────┴─────┴─────┴─────┘
  │           │           │     
  A→4         -- A→2      A→fin

Archivo A: Bloques 0 → 4 → 2

Representación detallada:
Bloque 0:  [Datos de A | Puntero→4]
Bloque 4:  [Datos de A | Puntero→2]
Bloque 2:  [Datos de A | Puntero→FIN]
```

**Tabla de Asignación (FAT):**
```
Archivo  | Inicio | Tamaño
---------|--------|--------
A        |   0    |   3
```

**Ventajas:**
- ✅ No hay fragmentación externa
- ✅ Los archivos pueden crecer fácilmente
- ✅ No requiere bloques contiguos
- ✅ Eficiente para acceso secuencial

**Desventajas:**
- ❌ Acceso aleatorio lento (debe seguir la cadena)
- ❌ Parte del bloque se usa para el puntero
- ❌ Si se pierde un enlace, se pierde el resto del archivo

**Variante: FAT de Windows**

En lugar de guardar el puntero en cada bloque, se guarda en una tabla centralizada (FAT):

```
FAT (File Allocation Table):
┌───────┬──────────┐
│Bloque │ Siguiente│
├───────┼──────────┤
│   0   │    4     │ ← Archivo A
│   1   │   FIN    │ ← Archivo B
│   2   │   FIN    │ ← Archivo A (último)
│   3   │  LIBRE   │
│   4   │    2     │ ← Archivo A
│   5   │  LIBRE   │
└───────┴──────────┘

Códigos especiales:
- FIN: Último bloque del archivo
- LIBRE: Bloque disponible
- DAÑADO: Bloque con errores
```

**Consolidación de bloques:**

Para mejorar performance, se pueden reorganizar los bloques:

```
Antes (fragmentado):
Archivo A: 0 → 7 → 15 → 23

Después (consolidado):
Archivo A: 0 → 1 → 2 → 3

Ventaja: Lectura más rápida por cercanía física
```

---

### 3. Asignación Indexada

**Concepto:** Un bloque especial (índice) contiene punteros a todos los bloques del archivo

```
┌──────────────┐
│ Bloque Índice│  ← FAT apunta aquí
├──────────────┤
│  Ptr → 4     │ ─┐
│  Ptr → 7     │  │
│  Ptr → 12    │  │  Punteros a bloques de datos
│  Ptr → 15    │  │
│     ...      │ ─┘
└──────────────┘
       │
       └─────┬──────┬──────┬─────►
             ▼      ▼      ▼      ▼
           ┌───┐  ┌───┐  ┌───┐  ┌───┐
           │ 4 │  │ 7 │  │12 │  │15 │ Bloques de datos
           └───┘  └───┘  └───┘  └───┘
```

**Tabla de Asignación:**
```
Archivo  | Bloque Índice
---------|---------------
A        |      3
B        |      8
```

**Ventajas:**
- ✅ No hay fragmentación externa
- ✅ Acceso aleatorio eficiente (índice directo)
- ✅ Fácil conocer todos los bloques del archivo

**Desventajas:**
- ❌ Espacio extra para el bloque índice
- ❌ Para archivos pequeños, se desperdicia el índice

---

#### Variante 1: Asignación por Secciones

Cada entrada del índice apunta a un grupo contiguo de bloques:

```
┌──────────────────┐
│  Bloque Índice   │
├──────────────────┤
│ Inicio=4, Len=3  │ → [4][5][6]
│ Inicio=10, Len=2 │ → [10][11]
│ Inicio=15, Len=4 │ → [15][16][17][18]
└──────────────────┘

Ventaja: Reduce el tamaño del índice
```

---

#### Variante 2: Niveles de Indirección (UNIX)

Combina acceso directo e indirecto para optimizar archivos de diferentes tamaños:

```
┌─────────────────────────┐
│       I-NODO            │
├─────────────────────────┤
│ Directo 1    → [Bloque] │ ← Acceso directo (rápido)
│ Directo 2    → [Bloque] │
│ Directo 3    → [Bloque] │
│ Directo 4    → [Bloque] │
│ Directo 5    → [Bloque] │
│ Directo 6    → [Bloque] │
│ Directo 7    → [Bloque] │
├─────────────────────────┤
│ Indirecto Simple → [Índice] → [Bloques...]
│                     └─ 256 punteros
├─────────────────────────┤
│ Indirecto Doble → [Índice] → [Índices] → [Bloques...]
│                    └─ 256 índices de 256 punteros c/u
└─────────────────────────┘
```

**Ejemplo de cálculo de tamaño máximo:**

Configuración:
- Bloques de 1 KB
- Direcciones de 32 bits (4 bytes)
- 7 bloques directos
- 1 bloque indirecto simple
- 1 bloque indirecto doble

```
Referencias por bloque índice:
1 KB / 4 bytes = 256 direcciones

Tamaño máximo de archivo:
= Directos + Indirecto Simple + Indirecto Doble
= (7 × 1KB) + (256 × 1KB) + (256 × 256 × 1KB)
= 7 KB + 256 KB + 65,536 KB
= 65,799 KB ≈ 64.25 MB
```

**¿Por qué esta estructura?**
- Archivos pequeños: Acceso directo rápido (mayoría de los casos)
- Archivos medianos: Indirección simple
- Archivos grandes: Indirección doble (o triple)

---

## Gestión de Espacio Libre

El sistema de archivos debe mantener registro de qué bloques están disponibles.

### 1. Tabla de Bits (Bitmap)

Un bit por cada bloque del disco:
- 0 = bloque libre
- 1 = bloque ocupado

```
Estado del disco:
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ A │ A │ - │ B │ B │ B │ - │ - │
└───┴───┴───┴───┴───┴───┴───┴───┘
  0   1   2   3   4   5   6   7

Bitmap:
┌───┬───┬───┬───┬───┬───┬───┬───┐
│ 1 │ 1 │ 0 │ 1 │ 1 │ 1 │ 0 │ 0 │
└───┴───┴───┴───┴───┴───┴───┴───┘
```

**Ejemplo visual más grande:**
```
Bitmap:
00111  (bloques 0,1 libres; 2,3,4 ocupados)
00001  (bloques 0,1,2,3 libres; 4 ocupado)
11110  (bloques 0,1,2,3 ocupados; 4 libre)
00011  (bloques 0,1,2 libres; 3,4 ocupados)
11111  (todos ocupados)
11110  (4 libre)
11000  (0,1 ocupados; resto libre)
```

**Ventajas:**
- ✅ Fácil encontrar bloques libres consecutivos
- ✅ Operaciones rápidas con operaciones bit a bit

**Desventajas:**
- ❌ Tamaño en memoria puede ser grande

```
Cálculo de tamaño del bitmap:
Tamaño = (Tamaño disco / Tamaño bloque) bits

Ejemplo: Disco de 16 GB con bloques de 512 bytes
= 16 GB / 512 bytes = 33,554,432 bloques
= 33,554,432 bits = 4 MB de bitmap
```

---

### 2. Bloques Libres Encadenados

Puntero al primer bloque libre, cada bloque libre apunta al siguiente:

```
Puntero inicial → Bloque 3
                     ↓
    ┌────────────────────┐
    │ Bloque 3 (libre)   │
    │ Siguiente → 7      │
    └────────────────────┘
                 ↓
    ┌────────────────────┐
    │ Bloque 7 (libre)   │
    │ Siguiente → 15     │
    └────────────────────┘
                 ↓
    ┌────────────────────┐
    │ Bloque 15 (libre)  │
    │ Siguiente → 20     │
    └────────────────────┘
                 ↓
               ...
```

**Ventajas:**
- ✅ No requiere espacio extra en memoria
- ✅ Simple de implementar

**Desventajas:**
- ❌ Ineficiente para buscar múltiples bloques libres
- ❌ Requiere múltiples operaciones de I/O
- ❌ Pérdida de un enlace puede causar problemas
- ❌ Difícil encontrar bloques consecutivos

---

### 3. Indexación (Agrupamiento)

Variante mejorada de bloques encadenados:

Cada bloque índice contiene N direcciones de bloques libres:
- Las primeras N-1 son bloques libres utilizables
- La N-ésima es puntero al siguiente bloque índice

```
┌─────────────────┐
│ Bloque Índice 8 │
├─────────────────┤
│ Libre: 15       │ ◄─ Bloque libre
│ Libre: 20       │ ◄─ Bloque libre
│ Libre: 27       │ ◄─ Bloque libre
│ Libre: 33       │ ◄─ Bloque libre
│ Next:  40       │ ◄─ Siguiente índice
└─────────────────┘
         │
         ▼
┌─────────────────┐
│ Bloque Índice 40│
├─────────────────┤
│ Libre: 42       │
│ Libre: 56       │
│ Libre: 68       │
│ Libre: 70       │
│ Next:  ...      │
└─────────────────┘
```

**Ventajas:**
- ✅ Más eficiente que encadenamiento simple
- ✅ Menos operaciones de I/O

**Desventajas:**
- ❌ Aún puede requerir varios accesos a disco

---

### 4. Recuento (Counting)

Variante de indexación optimizada para bloques contiguos:

En lugar de listar cada bloque libre, se guarda:
- Dirección del primer bloque libre
- Cantidad de bloques contiguos libres

```
┌──────────────────────┐
│   Índice de Libres   │
├──────────────────────┤
│ Inicio: 8,  Count: 3 │ → [8][9][10]
│ Inicio: 15, Count: 1 │ → [15]
│ Inicio: 20, Count: 5 │ → [20][21][22][23][24]
│ Inicio: 27, Count: 2 │ → [27][28]
│ Next: 40             │
└──────────────────────┘
```

**Ventajas:**
- ✅ Estructura más compacta
- ✅ Eficiente cuando hay bloques contiguos libres
- ✅ Ideal para asignación continua

**Desventajas:**
- ❌ Más compleja de mantener
- ❌ Menos eficiente si hay mucha fragmentación

---

## Implementaciones: UNIX y Windows

### Sistema UNIX (i-nodos)

#### Estructura del Volumen

```
┌─────────────────────────────────────┐
│         Boot Block                  │ ← Código para arranque
├─────────────────────────────────────┤
│         Superblock                  │ ← Metadatos del FS
│  - Bloques totales                  │
│  - Bloques libres                   │
│  - Lista de bloques libres          │
│  - Tamaño de i-nodos                │
└─────────────────────────────────────┘
┌─────────────────────────────────────┐
│       I-NODE Table                  │ ← Tabla de i-nodos
│  [i-nodo 1][i-nodo 2]...[i-nodo N]  │
├─────────────────────────────────────┤
│       Data Blocks                   │ ← Bloques de datos
│  [Bloque 1][Bloque 2]...[Bloque N]  │
└─────────────────────────────────────┘
```

#### I-NODO (Index Node)

**Estructura de control que contiene información clave del archivo:**

```
┌─────────────────────────────────┐
│         I-NODO #12345           │
├─────────────────────────────────┤
│ Tipo: Archivo regular           │
│ Permisos: rw-r--r--             │
│ Owner UID: 1000                 │
│ Group GID: 1000                 │
│ Tamaño: 10,240 bytes            │
│ Timestamps:                     │
│   - Creación                    │
│   - Modificación                │
│   - Último acceso               │
│ Links count: 2                  │
├─────────────────────────────────┤
│ Bloques Directos:               │
│   [0] → Bloque 1024             │
│   [1] → Bloque 1025             │
│   [2] → Bloque 1026             │
│   [3] → Bloque 1027             │
│   [4] → Bloque 1028             │
│   [5] → Bloque 1029             │
│   [6] → Bloque 1030             │
├─────────────────────────────────┤
│ Indirecto Simple → Bloque 2000  │
├─────────────────────────────────┤
│ Indirecto Doble → Bloque 3000   │
└─────────────────────────────────┘
```

⚠️ **Importante:** El nombre del archivo NO está en el i-nodo

#### Directorios en UNIX

Los directorios son archivos especiales que contienen una tabla de:
- Número de i-nodo
- Nombre del archivo

```
Directorio /home/juan/:
┌─────────┬──────────────┐
│ I-nodo  │   Nombre     │
├─────────┼──────────────┤
│  12345  │ documento.txt│
│  12346  │ foto.jpg     │
│  12347  │ script.sh    │
│  54321  │ carpeta/     │
└─────────┴──────────────┘
```

#### Ejemplo: Buscar /usr/ast/mbox

```
Paso 1: Leer directorio raíz (/)
┌────────┬────────┐
│I-nodo  │ Nombre │
├────────┼────────┤
│   2    │ usr    │ ◄── Buscar "usr"
│   5    │ home   │
│   8    │ var    │
└────────┴────────┘

Paso 2: Leer i-nodo #2 → obtener bloques del directorio "usr"
        Leer contenido del directorio /usr/
┌────────┬────────┐
│I-nodo  │ Nombre │
├────────┼────────┤
│  150   │ bin    │
│  298   │ ast    │ ◄── Buscar "ast"
│  340   │ lib    │
└────────┴────────┘

Paso 3: Leer i-nodo #298 → obtener bloques del directorio "ast"
        Leer contenido del directorio /usr/ast/
┌────────┬────────┐
│I-nodo  │ Nombre │
├────────┼────────┤
│ 5420   │ mbox   │ ◄── Encontrado!
│ 5421   │ file.c │
└────────┴────────┘

Resultado: El archivo /usr/ast/mbox tiene i-nodo #5420
```

#### Tipos de Archivos en UNIX

```
Tipo         Símbolo   Descripción
──────────────────────────────────────────
Regular         -      Archivo común
Directorio      d      Carpeta
Link            l      Enlace simbólico
Bloque          b      Dispositivo de bloques
Carácter        c      Dispositivo de caracteres
Socket          s      Socket de red
Pipe            p      Named pipe (FIFO)
```

#### Links en UNIX

**Hard Link:**
- Comparten el mismo i-nodo
- Solo dentro del mismo filesystem
- Contador de referencias en i-nodo

```
archivo1.txt (i-nodo 12345) ◄─┐
                               ├─ Mismo i-nodo
archivo2.txt (i-nodo 12345) ◄─┘

Links count = 2
Si se borra archivo1.txt, archivo2.txt sigue funcionando
```

**Symbolic Link (Soft Link):**
- Tiene su propio i-nodo
- Contiene la ruta al archivo destino
- Puede cruzar entre filesystems
- Si se borra el original, el link queda roto

```
enlace.txt (i-nodo 99999)
     │
     └─ Contiene texto: "/home/juan/archivo.txt"
            │
            └─► archivo.txt (i-nodo 12345)

Si se borra archivo.txt, enlace.txt apunta a un archivo inexistente
```

---

### Sistema Windows (FAT y NTFS)

#### File Systems Soportados

**Por medio físico:**
- **CDFS** (CD-ROM File System) → CDs
- **UDF** (Universal Disk Format) → DVDs, Blu-Ray

**Para discos duros:**
- **FAT12, FAT16, FAT32** → Compatibilidad y dispositivos antiguos
- **NTFS** (New Technology File System) → Sistema moderno

---

### FAT (File Allocation Table)

#### ¿Por qué Windows aún soporta FAT?

1. **Compatibilidad** con otros sistemas operativos (multiboot)
2. **Upgrades** desde versiones anteriores de Windows
3. **Dispositivos removibles** (USB, tarjetas SD)
4. **Sistemas embebidos** que lo requieren

#### Estructura de FAT

```
┌─────────────────────────────────────────────────┐
│           Boot Sector                           │ ← Información de arranque
├─────────────────────────────────────────────────┤
│    File Allocation Table 1 (FAT)                │ ← Tabla principal
├─────────────────────────────────────────────────┤
│    File Allocation Table 2 (copia)              │ ← Respaldo
├─────────────────────────────────────────────────┤
│         Root Directory                          │ ← Directorio raíz
├─────────────────────────────────────────────────┤
│    Other directories and all files              │ ← Datos
│    [Datos de archivos y subdirectorios]         │
└─────────────────────────────────────────────────┘
```

#### Funcionamiento de FAT

**Asignación encadenada mejorada:**
- Los punteros NO están en los clusters
- Los punteros están en la **tabla FAT centralizada**
- La FAT tiene tantas entradas como clusters existen

**Ejemplo visual:**

```
Archivo "documento.txt" ocupa clusters 4, 7, 12:

FAT (File Allocation Table):
┌─────────┬──────────┐
│ Cluster │  Valor   │
├─────────┼──────────┤
│    0    │ Reservado│
│    1    │ Reservado│
│    2    │   EOF    │ ← Otro archivo
│    3    │  LIBRE   │
│    4    │    7     │ ◄─ documento.txt (inicio)
│    5    │  LIBRE   │
│    6    │   EOF    │ ← Otro archivo
│    7    │   12     │ ◄─ documento.txt (continúa)
│    8    │  LIBRE   │
│    9    │  DAÑADO  │
│   10    │  LIBRE   │
│   11    │  LIBRE   │
│   12    │   EOF    │ ◄─ documento.txt (fin)
└─────────┴──────────┘

Códigos especiales:
- EOF (End Of File): Último cluster
- LIBRE: Cluster disponible
- DAÑADO: Cluster con errores físicos
- Número: Siguiente cluster del archivo
```

**Directorio almacena:**
```
┌──────────────┬─────────┬─────────┬────────┐
│   Nombre     │ Tamaño  │ Atributos│Cluster │
│              │         │          │ Inicio │
├──────────────┼─────────┼─────────┼────────┤
│documento.txt │ 10,240  │ rw-      │   4    │ ◄─ Apunta a FAT[4]
│ foto.jpg     │ 2,048   │ rw-      │   2    │
└──────────────┴─────────┴─────────┴────────┘
```

---

#### FAT12

**Características:**
- Usa **12 bits** para identificar clusters
- Máximo: 2^12 = **4,096 clusters**
- Tamaño de cluster: 512 bytes a 8 KB

```
Cálculo de tamaño máximo:
= 4,096 clusters × 8 KB
= 32 MB de volumen máximo
```

**Uso histórico:**
- Diskettes de 3.5" (1.44 MB)
- Sistemas MS-DOS 3.3 a 4.0 (años 1980)

```
Diskette 3.5":
┌─────────────────────┐
│ 1.44 MB total       │
│ FAT12               │
│ 512 bytes/cluster   │
└─────────────────────┘
```

---

#### FAT16

**Características:**
- Usa **16 bits** para identificar clusters
- Máximo: 2^16 = **65,536 clusters**
- Tamaño de cluster: 512 bytes a 64 KB

```
Cálculo de tamaño máximo:
= 65,536 clusters × 64 KB
= 4 GB de volumen máximo
```

**Limitaciones:**
- Nombres cortos de archivo (8.3: 8 caracteres + 3 de extensión)
- No soporta particiones mayores a 4 GB
- Ineficiente en discos grandes (clusters muy grandes)

**Problema del tamaño de cluster:**
```
Disco de 2 GB con FAT16:
- Necesita clusters de 32 KB
- Archivo de 1 KB desperdicia 31 KB
- ¡Gran fragmentación interna!
```

**Uso:**
- MS-DOS 6.22
- Windows 95 (versiones tempranas)

---

#### FAT32

**Características:**
- Usa **28 bits efectivos** (32 bits, pero 4 reservados)
- Máximo: 2^28 = **268,435,456 clusters**
- Tamaño de cluster: 512 bytes a 32 KB

```
Cálculo de capacidad teórica:
= 268,435,456 clusters × 32 KB
= 8 TB (terabytes)

Capacidad práctica con clusters de 512 bytes:
= 268,435,456 clusters × 512 bytes
= 128 GB
```

**Ventajas sobre FAT16:**
- ✅ Soporta volúmenes mucho mayores
- ✅ Más eficiente (clusters más pequeños)
- ✅ Nombres largos de archivo (255 caracteres)
- ✅ Mejor aprovechamiento del espacio

**Limitaciones:**
- ❌ Tamaño máximo de archivo: 4 GB
- ❌ Sin journaling (no transaccional)
- ❌ Sin permisos de seguridad
- ❌ Sin compresión ni encriptación

**Uso actual:**
- USB flash drives
- Tarjetas SD/microSD
- Compatibilidad multiplataforma (Windows, Mac, Linux)

---

#### Comparación FAT

```
┌──────────┬─────────┬──────────┬──────────────┬────────────┐
│ Sistema  │  Bits   │ Clusters │ Cluster Max  │  Vol. Max  │
├──────────┼─────────┼──────────┼──────────────┼────────────┤
│ FAT12    │   12    │  4,096   │    8 KB      │   32 MB    │
│ FAT16    │   16    │ 65,536   │   64 KB      │    4 GB    │
│ FAT32    │   28    │ 268M     │   32 KB      │    8 TB    │
└──────────┴─────────┴──────────┴──────────────┴────────────┘

Evolución temporal:
1980s: FAT12 (diskettes)
1990s: FAT16 (discos pequeños)
1996+: FAT32 (discos grandes)
2001+: NTFS (sistema moderno)
```

---

### NTFS (New Technology File System)

**Sistema de archivos nativo de Windows desde Windows NT**

#### Características Principales

**1. Capacidad:**
- Usa **64 bits** para referenciar clusters
- Capacidad teórica: **16 Exabytes** (16 billones de GB)
- Tamaño máximo de archivo: 16 Exabytes

```
Comparación de capacidad:
FAT32:    4 GB por archivo
NTFS:  16 EB por archivo (16,000,000,000 GB)
```

**2. Ventajas sobre FAT:**

```
┌──────────────────────────────────────────────────┐
│         NTFS vs FAT                              │
├──────────────────────────────────────────────────┤
│ ✅ Tamaños de archivo y disco mucho mayores      │
│ ✅ Mejor performance en discos grandes           │
│ ✅ Nombres de archivo hasta 255 caracteres       │
│ ✅ Atributos de seguridad (ACLs)                 │
│ ✅ Sistema transaccional (journaling)            │
│ ✅ Compresión de archivos                        │
│ ✅ Encriptación (EFS)                            │
│ ✅ Cuotas de disco por usuario                   │
│ ✅ Links simbólicos y hard links                 │
│ ✅ Recuperación ante fallos                      │
└──────────────────────────────────────────────────┘
```

**3. Journaling (Transaccional):**

NTFS mantiene un registro (log) de todas las operaciones:

```
Operación: Mover archivo de A a B

Sin Journaling (FAT):
1. Borrar entrada en A
2. [¡Fallo de energía!] ◄─ Archivo perdido
3. Crear entrada en B (nunca se ejecuta)

Con Journaling (NTFS):
1. Escribir en log: "Voy a mover archivo"
2. Borrar entrada en A
3. Crear entrada en B
4. Escribir en log: "Operación completada"

Si hay fallo:
- Al reiniciar, lee el log
- Completa o revierte la operación
- ¡Archivo no se pierde!
```

**4. Seguridad:**

```
Permisos detallados por usuario y grupo:
┌────────────────────────────────────┐
│  Archivo: confidencial.docx        │
├────────────────────────────────────┤
│ Usuario Juan:                      │
│   ✅ Leer                          │
│   ✅ Escribir                      │
│   ✅ Modificar permisos            │
│                                    │
│ Grupo Ventas:                      │
│   ✅ Leer                          │
│   ❌ Escribir                      │
│                                    │
│ Otros usuarios:                    │
│   ❌ Sin acceso                    │
└────────────────────────────────────┘
```

**5. Características avanzadas:**

- **Compresión transparente**: Archivos comprimidos automáticamente
- **Encriptación (EFS)**: Protección a nivel de archivo
- **Sparse files**: Archivos con "huecos" que no ocupan espacio
- **Alternate Data Streams**: Metadatos adicionales
- **Volume Shadow Copy**: Copias de seguridad automáticas

---

## Protección y Seguridad

### Objetivos de la Protección

**El propietario/administrador debe controlar:**

1. **QUÉ se puede hacer** (Derechos de acceso)
2. **QUIÉN lo puede hacer** (Usuarios/grupos)

### Compartir Archivos

En ambientes multiusuario es necesario:
- ✅ Permitir que varios usuarios accedan a los mismos archivos
- ✅ Bajo un esquema de protección adecuado
- ✅ Manejar accesos simultáneos correctamente

```
Escenario de colaboración:
┌─────────────────────────────────┐
│  Proyecto compartido            │
├─────────────────────────────────┤
│ Juan (Owner): Leer/Escribir     │
│ María (Grupo): Leer/Escribir    │
│ Pedro (Grupo): Solo Leer        │
│ Otros: Sin acceso               │
└─────────────────────────────────┘
```

---

### Derechos de Acceso

#### 1. Reading (Lectura)
- Ver el contenido del archivo
- Listar archivos en un directorio

#### 2. Writing (Escritura)
- Modificar el contenido
- Sobrescribir datos existentes

#### 3. Appending (Añadir)
- Agregar datos al final
- **No puede** modificar o borrar contenido existente
- Útil para logs y archivos de auditoría

```
Ejemplo - Log de sistema:
Usuario solo puede AÑADIR nuevas entradas
No puede modificar entradas anteriores
```

#### 4. Updating (Actualización)
- Modificar datos
- Borrar contenido
- Agregar datos
- Crear archivos
- Renombrar archivos

#### 5. Execution (Ejecución)
- Ejecutar un archivo como programa
- Para directorios: Atravesar (entrar al directorio)

#### 6. Changing Protection (Cambiar Protección)
- Modificar los permisos del archivo
- Solo el propietario o administrador

#### 7. Deletion (Eliminación)
- Borrar el archivo del sistema

---

### Permisos en Directorios

Los directorios tienen permisos especiales:

```
┌─────────────────────────────────────────┐
│ Permiso en Directorio │ Significado     │
├───────────────────────┼─────────────────┤
│ Read (r)              │ Listar archivos │
│ Write (w)             │ Crear/Borrar    │
│ Execute (x)           │ Entrar (cd)     │
└─────────────────────────────────────────┘

Ejemplo:
drwxr-x---  juan  staff  proyecto/
│││││││││
│││└┴┴┴┴┴─ Otros: Sin acceso
││└──────── Grupo: Leer y entrar (no crear)
│└───────── Usuario: Todos los permisos
└────────── Es un directorio
```

**Importante:** Necesitas permiso de ejecución (x) en un directorio para acceder a archivos dentro, incluso si tienes permisos sobre esos archivos.

---

### Clases de Usuarios

**1. Owner (Propietario)**
- Tiene todos los derechos por defecto
- Puede otorgar derechos a otros

**2. Grupos**
- Conjunto de usuarios con permisos compartidos
- Ejemplo: equipo de desarrollo, departamento de ventas

**3. Others (Otros)**
- Todos los demás usuarios del sistema
- Archivos públicos tienen permisos para "others"

```
Representación:
┌────────┬────────┬────────┐
│ Owner  │ Group  │ Others │
├────────┼────────┼────────┤
│  rwx   │  r-x   │  r--   │
└────────┴────────┴────────┘
```

---

### Protección en UNIX

#### Sistema de Permisos

**3 tipos de acceso × 3 clases de usuarios = 9 bits de permisos**

```
Estructura de permisos:
   Owner   Group   Others
  ┌─┴─┐  ┌─┴─┐  ┌─┴─┐
  rwx    r-x    r--
  |||    |||    |||
  ││└─ Execute (1)
  │└── Write (2)
  └─── Read (4)
```

#### Valores Numéricos (Octal)

Cada permiso tiene un valor:
- **r (read)**: 4
- **w (write)**: 2
- **x (execute)**: 1

```
Ejemplos:
rwx = 4+2+1 = 7
rw- = 4+2+0 = 6
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
--- = 0+0+0 = 0

Permiso completo: rwxrwxrwx = 777
Permiso típico:   rw-r--r-- = 644
Solo owner:       rwx------ = 700
```

#### Comandos en UNIX

**chmod** (cambiar permisos):
```bash
# Formato numérico
chmod 755 archivo.txt    # rwxr-xr-x
chmod 644 documento.txt  # rw-r--r--
chmod 600 privado.txt    # rw-------

# Formato simbólico
chmod u+x script.sh      # Agregar ejecución al usuario
chmod g-w archivo.txt    # Quitar escritura al grupo
chmod o+r public.txt     # Agregar lectura a otros
chmod a+r todos.txt      # Agregar lectura a todos (all)
```

**chown** (cambiar propietario):
```bash
chown juan archivo.txt           # Cambiar owner
chown juan:staff archivo.txt     # Cambiar owner y grupo
chown :developers proyecto/      # Solo cambiar grupo
```

**chgrp** (cambiar grupo):
```bash
chgrp staff archivo.txt
```

#### Ejemplos Visuales

```
Archivo de script ejecutable:
-rwxr-xr-x  1  juan  staff  2048  Nov 09  script.sh
 │││││││││
 │││││││││─ Tipo: archivo regular
 │││└┴┴───── Others: Leer y ejecutar
 ││└────────── Group: Leer y ejecutar
 │└─────────── Owner: Leer, escribir y ejecutar
 └──────────── 1 hard link

Archivo privado:
-rw-------  1  juan  staff  1024  Nov 09  privado.txt
 │││││││││
 │││└┴┴───── Others: Sin permisos
 ││└────────── Group: Sin permisos
 │└─────────── Owner: Leer y escribir
 └──────────── archivo regular

Directorio compartido:
drwxrwxr-x  2  juan  devs  4096  Nov 09  proyecto/
││││││││││
│││└┴┴───── Others: Leer y entrar
││└────────── Group: Todos los permisos
│└─────────── Owner: Todos los permisos
└──────────── Es un directorio
```

---

### Protección en Windows

#### ACLs (Access Control Lists)

Windows usa un sistema más granular que UNIX:

```
┌─────────────────────────────────────────┐
│   Access Control List (ACL)             │
│   Archivo: Informe.docx                 │
├─────────────────────────────────────────┤
│ Juan (Owner)                            │
│   ✅ Control total                      │
│                                         │
│ Grupo: Ventas                           │
│   ✅ Leer                               │
│   ✅ Escribir                           │
│   ❌ Eliminar                           │
│   ❌ Cambiar permisos                   │
│                                         │
│ María                                   │
│   ✅ Leer                               │
│   ❌ Escribir                           │
│                                         │
│ Usuarios Autenticados                   │
│   ✅ Leer                               │
│   ❌ Todo lo demás                      │
└─────────────────────────────────────────┘
```

#### Permisos Básicos en Windows

```
┌──────────────────┬────────────────────────────┐
│    Permiso       │      Permite               │
├──────────────────┼────────────────────────────┤
│ Full Control     │ Todo (incluso permisos)    │
│ Modify           │ Leer, escribir, eliminar   │
│ Read & Execute   │ Leer y ejecutar            │
│ Read             │ Solo leer                  │
│ Write            │ Escribir y crear           │
└──────────────────┴────────────────────────────┘
```

#### Permisos Especiales

Windows permite permisos muy detallados:
- Crear archivos / Escribir datos
- Crear carpetas / Agregar datos
- Atributos de lectura
- Atributos de escritura
- Eliminar subcarpetas y archivos
- Leer permisos
- Cambiar permisos
- Tomar posesión
- Y más...

#### Herencia de Permisos

```
Carpeta Padre: C:\Proyectos\
  Permisos: Juan = Control Total

  ↓ Herencia automática

Carpeta Hija: C:\Proyectos\Ventas\
  Permisos heredados: Juan = Control Total

  ↓ Herencia automática

Archivo: C:\Proyectos\Ventas\informe.docx
  Permisos heredados: Juan = Control Total

Se pueden bloquear o modificar permisos heredados
```

---

## Resumen Comparativo

### Métodos de Asignación

```
┌─────────────┬────────────┬────────────┬─────────────┐
│   Método    │    FAT     │ Fragm.Ext. │  Acc.Aleatorio│
├─────────────┼────────────┼────────────┼─────────────┤
│ Continua    │  Simple    │     Sí     │   Excelente │
│ Encadenada  │  Simple    │     No     │     Pobre   │
│ Indexada    │  Compleja  │     No     │   Excelente │
└─────────────┴────────────┴────────────┴─────────────┘
```

### Gestión de Espacio Libre

```
┌─────────────┬───────────┬────────────┬──────────────┐
│   Método    │ Mem.Usada │ Velocidad  │ Complejidad  │
├─────────────┼───────────┼────────────┼──────────────┤
│ Bitmap      │   Alta    │   Rápida   │    Simple    │
│ Encadenada  │   Baja    │   Lenta    │    Simple    │
│ Indexación  │   Media   │   Media    │    Media     │
│ Recuento    │   Baja    │   Media    │   Compleja   │
└─────────────┴───────────┴────────────┴──────────────┘
```

### Sistemas de Archivos

```
┌──────────┬────────────┬────────────┬─────────────────┐
│  Sistema │   Tipo     │  Seguridad │   Transaccional │
├──────────┼────────────┼────────────┼─────────────────┤
│  FAT12   │ Encadenada │     No     │       No        │
│  FAT16   │ Encadenada │     No     │       No        │
│  FAT32   │ Encadenada │     No     │       No        │
│  NTFS    │  Indexada  │    ACLs    │       Sí        │
│  UNIX    │  Indexada  │   rwx ugo  │   Depende FS    │
└──────────┴────────────┴────────────┴─────────────────┘
```

---

## Conceptos Clave para Recordar

### 🎯 Archivo
- Abstracción de almacenamiento persistente
- Espacio lógico continuo
- Identificado por nombre único (path)

### 🎯 Sistema de Archivos
Define cómo se organizan y almacenan los datos:
- Métodos de asignación de bloques
- Gestión de espacio libre
- Estructura de directorios
- Metadatos y atributos

### 🎯 Asignación
- **Continua**: Rápida pero fragmenta
- **Encadenada**: Flexible pero lenta en acceso aleatorio
- **Indexada**: Equilibrada, usada en sistemas modernos

### 🎯 i-nodo (UNIX)
- Estructura de control del archivo
- Contiene metadatos y referencias a bloques
- El nombre está en el directorio, no en el i-nodo

### 🎯 FAT (Windows)
- Tabla centralizada de asignación
- Asignación encadenada mejorada
- Versiones: FAT12, FAT16, FAT32

### 🎯 NTFS
- Sistema moderno de Windows
- Journaling, seguridad avanzada, gran capacidad
- Reemplaza a FAT en aplicaciones modernas

### 🎯 Protección
- Control de acceso por usuario/grupo
- UNIX: rwx para ugo (user, group, others)
- Windows: ACLs detalladas

---

## Glosario

- **Bloque/Cluster**: Unidad de asignación, grupo de sectores
- **FAT**: File Allocation Table, tabla de asignación de archivos
- **i-nodo**: Estructura de control de archivos en UNIX
- **Fragmentación externa**: Espacios libres dispersos
- **Fragmentación interna**: Espacio desperdiciado dentro de bloques
- **Hard link**: Enlaces que comparten i-nodo
- **Symbolic link**: Enlace que contiene ruta a otro archivo
- **ACL**: Access Control List, lista de control de acceso
- **Journaling**: Sistema transaccional que registra operaciones
- **Superblock**: Metadatos del sistema de archivos
- **Path absoluto**: Ruta completa desde la raíz
- **Path relativo**: Ruta desde el directorio actual

---

*Documento generado para estudio de Introducción a Sistemas Operativos*