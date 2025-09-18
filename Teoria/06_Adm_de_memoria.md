# Administración de Memoria en Sistemas Operativos

## Introducción

La **administración de memoria** es uno de los aspectos más críticos en el diseño de sistemas operativos. Su objetivo principal es gestionar eficientemente la memoria principal (RAM) para permitir que múltiples procesos coexistan y se ejecuten correctamente.

### ¿Por qué es importante?

- Los programas y datos **deben estar en memoria principal** para poder ejecutarse
- El SO necesita **maximizar el número de procesos** que pueden residir en memoria
- Debe garantizar **protección** entre procesos y **eficiencia** en el uso de recursos

## Responsabilidades del Sistema Operativo

El SO debe manejar cuatro aspectos fundamentales:

1. **Registro de uso**: Llevar control de qué partes de memoria están ocupadas y cuáles libres
2. **Asignación**: Otorgar espacio de memoria a procesos que lo necesiten
3. **Liberación**: Recuperar memoria de procesos terminados
4. **Abstracción**: Permitir que el programador no se preocupe por dónde se ubicará su programa

### Objetivos adicionales:
- **Seguridad**: Evitar que procesos accedan a memoria de otros
- **Compartición**: Permitir acceso controlado a secciones comunes (librerías, código compartido)
- **Performance**: Garantizar rendimiento óptimo del sistema

## Requisitos Fundamentales

### 1. Reubicación
- El programador **no debe conocer** dónde se colocará su programa en RAM
- Un proceso puede ser **movido** durante su ejecución (swap)
- Las referencias de memoria deben **traducirse dinámicamente**

### 2. Protección
- Los procesos **no pueden acceder** a memoria de otros procesos (salvo permisos explícitos)
- El chequeo debe realizarse **durante la ejecución** (no se pueden anticipar todos los accesos)

### 3. Compartición
- Varios procesos pueden acceder a la **misma porción de memoria**
- Ejemplos: rutinas comunes, librerías, espacios compartidos
- Evita copias innecesarias y optimiza el uso de RAM

## Conceptos Clave: Espacios de Direcciones

### Espacio de Direcciones
Es el **rango de direcciones posibles** que un proceso puede usar para referenciar sus instrucciones y datos.

**Tamaño según arquitectura:**
- 32 bits: 0 a 2³² - 1 (4 GB)
- 64 bits: 0 a 2⁶⁴ - 1 (16 ExaBytes)

### Tipos de Direcciones

#### Direcciones Lógicas (Virtuales)
- Referencia a una localidad **independiente** de la ubicación real en RAM
- Representa una dirección en el **espacio de direcciones del proceso**
- Lo que "ve" y usa el programa

#### Direcciones Físicas
- Referencia **real** a una localidad en la RAM
- Dirección absoluta en la memoria física
- Lo que realmente usa el hardware

## Traducción de Direcciones

### Método Básico: Registros Base y Límite

**Registro Base**: Dirección de inicio del proceso en RAM
**Registro Límite**: Tamaño o dirección final del proceso

```
Dirección Física = Dirección Lógica + Registro Base
```

**Ejemplo:**
- Proceso cargado en dirección 7000
- Dirección lógica: 642
- Dirección física: 7000 + 642 = 7642

```
CPU                    MMU                    RAM
┌─────────────┐       ┌──────────────────┐   ┌─────────────────┐
│ Proceso P1  │       │ Registro Base:   │   │ Dir 0: ....     │
│ Dir Lógica  │──────►│ 7000            │   │ ...             │
│ 642         │       │                 │   │ Dir 7000: P1    │
└─────────────┘       │       (+)       │   │ Dir 7642: Dato  │◄─┐
                      │                 │   │ ...             │  │
                      │ Dir Física:     │   │ ...             │  │
                      │ 7642           │───┘ Dir 10000: ... │  │
                      └──────────────────┘   └─────────────────┘  │
                                                                  │
                      642 + 7000 = 7642 ─────────────────────────┘
```

### Memory Management Unit (MMU)
- **Dispositivo de hardware** que realiza la traducción
- Es parte del procesador
- Su reprogramación es una **operación privilegiada** (solo kernel)
- Los procesos **nunca usan direcciones físicas directamente**

## Mecanismos de Asignación de Memoria

### 1. Particiones Fijas
- Memoria dividida en **particiones de tamaño fijo**
- Cada partición aloja **un proceso**
- Criterios de asignación: First Fit, Best Fit, Worst Fit, Next Fit

**Problema:** Fragmentación Interna

### 2. Particiones Dinámicas
- Particiones **varían en tamaño y número**
- Se crean dinámicamente del **tamaño exacto** necesario

**Problema:** Fragmentación Externa

## Fragmentación

### Fragmentación Interna
- Ocurre en **particiones fijas**
- Es la **porción no utilizada** dentro de una partición
- Ejemplo: Proceso de 3KB en partición de 4KB → 1KB desperdiciado

```
Particiones Fijas - Fragmentación Interna
┌─────────────────────────────────────┐
│ Partición 1 (4KB)                   │
│ ┌─────────────────┐ ┌─────────────┐ │
│ │ Proceso A (3KB) │ │ Libre (1KB) │ │ ← Fragmentación Interna
│ └─────────────────┘ └─────────────┘ │
├─────────────────────────────────────┤
│ Partición 2 (4KB)                   │
│ ┌─────────────────────────────────┐ │
│ │ Proceso B (4KB)                 │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ Partición 3 (4KB)                   │
│ ┌───────────┐ ┌─────────────────────┐ │
│ │Proceso C  │ │ Libre (2KB)         │ │ ← Fragmentación Interna
│ │(2KB)      │ │                     │ │
│ └───────────┘ └─────────────────────┘ │
└─────────────────────────────────────┘
```

### Fragmentación Externa
- Ocurre en **particiones dinámicas**
- Son **huecos no contiguos** que quedan tras terminar procesos
- Puede haber memoria suficiente pero **no utilizable** por no ser contigua
- **Solución**: Compactación (costosa)

```
Particiones Dinámicas - Fragmentación Externa

Situación Inicial:
┌─────────────────────────────────────┐
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Proceso A│ │Proceso B│ │Proceso C│ │
│ │  (3KB)  │ │  (2KB)  │ │  (4KB)  │ │
│ └─────────┘ └─────────┘ └─────────┘ │
└─────────────────────────────────────┘

Después de terminar Proceso B:
┌─────────────────────────────────────┐
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │Proceso A│ │ LIBRE   │ │Proceso C│ │
│ │  (3KB)  │ │  (2KB)  │ │  (4KB)  │ │
│ └─────────┘ └─────────┘ └─────────┘ │
└─────────────────────────────────────┘

Después de terminar Proceso A:
┌─────────────────────────────────────┐
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │ LIBRE   │ │ LIBRE   │ │Proceso C│ │
│ │  (3KB)  │ │  (2KB)  │ │  (4KB)  │ │
│ └─────────┘ └─────────┘ └─────────┘ │
└─────────────────────────────────────┘
          ↑         ↑
    Total: 5KB libres, pero no contiguos
    No puede alojar un proceso de 4KB
```

## Soluciones Avanzadas

Debido a los problemas de fragmentación, se desarrollaron técnicas más sofisticadas:

### 1. Segmentación
- Divide el programa según la **visión del usuario**
- Un proceso es una **colección de segmentos**
- Segmentos típicos: programa principal, funciones, variables globales, stack
- **Puede causar fragmentación externa**

#### Características:
- Segmentos pueden tener **diferente tamaño**
- Direcciones lógicas: `<Número de Segmento, Desplazamiento>`
- Cada proceso tiene una **Tabla de Segmentos**

#### Tabla de Segmentos (por proceso):
- **Base**: Dirección física de inicio del segmento
- **Límite**: Tamaño del segmento

```
Ejemplo de Segmentación:

Programa del Usuario (Vista Lógica):
┌─────────────────────────┐
│ Segmento 0: main()      │  ← 1000 bytes
├─────────────────────────┤
│ Segmento 1: function()  │  ← 400 bytes  
├─────────────────────────┤
│ Segmento 2: data[]      │  ← 1100 bytes
├─────────────────────────┤
│ Segmento 3: stack       │  ← 1000 bytes
└─────────────────────────┘

Memoria Física:
┌─────────────────────────────────────┐ Dir 0
│ ┌─────────────────────────────────┐ │
│ │ Sistema Operativo               │ │
│ └─────────────────────────────────┘ │ Dir 1400
│ ┌─────────────────────────────────┐ │
│ │ Segmento 2 (data)              │ │ ← 1100 bytes
│ └─────────────────────────────────┘ │ Dir 2500
│ ┌─────────────────────────────────┐ │
│ │ Segmento 0 (main)              │ │ ← 1000 bytes
│ └─────────────────────────────────┘ │ Dir 3500
│ ┌─────────────┐                   │ │
│ │ Segmento 1  │                   │ │ ← 400 bytes
│ │ (function)  │                   │ │
│ └─────────────┘                   │ │ Dir 3900
│ ┌─────────────────────────────────┐ │
│ │ Segmento 3 (stack)             │ │ ← 1000 bytes
│ └─────────────────────────────────┘ │ Dir 4900
└─────────────────────────────────────┘

Tabla de Segmentos del Proceso:
┌───┬──────┬─────────┐
│Seg│ Base │ Límite  │
├───┼──────┼─────────┤
│ 0 │ 2500 │  1000   │
│ 1 │ 3500 │   400   │
│ 2 │ 1400 │  1100   │
│ 3 │ 3900 │  1000   │
└───┴──────┴─────────┘

Dirección Lógica <1, 53> = Segmento 1, Desplazamiento 53
→ Dir. Física = Base[1] + 53 = 3500 + 53 = 3553
```

### 2. Paginación
- Divide la memoria física en **marcos** de tamaño fijo
- Divide la memoria lógica en **páginas** del mismo tamaño
- **No produce fragmentación externa** (solo interna menor)

#### Conceptos Clave:
- **Marco**: Trozo fijo de memoria física
- **Página**: Trozo fijo de memoria lógica (mismo tamaño que marco)
- Direcciones lógicas: `<Número de Página, Desplazamiento>`

#### Tabla de Páginas (por proceso):
- Mapea cada **página lógica** a un **marco físico**
- Debe residir **siempre en RAM**

### Ejemplo de Paginación:
```
Dirección lógica: 2348 (página 2, desplazamiento 348)
Tabla indica: página 2 → marco 6
Dirección física: marco 6 + desplazamiento 348
```

```
Ejemplo Completo de Paginación (páginas de 1KB):

Memoria Lógica (Proceso):      Memoria Física (RAM):
┌─────────────────┐            ┌─────────────────┐ Marco 0
│ Página 0        │            │ Página 2        │
│ (0-1023)        │            ├─────────────────┤ Marco 1
├─────────────────┤            │ Página 1        │
│ Página 1        │            ├─────────────────┤ Marco 2
│ (1024-2047)     │            │ Sistema Op.     │
├─────────────────┤            ├─────────────────┤ Marco 3
│ Página 2        │            │ Página 3        │
│ (2048-3071)     │            ├─────────────────┤ Marco 4
├─────────────────┤            │ Página 0        │
│ Página 3        │            ├─────────────────┤ Marco 5
│ (3072-4095)     │            │ Libre           │
└─────────────────┘            ├─────────────────┤ Marco 6
                               │ Libre           │
Tabla de Páginas:              └─────────────────┘ Marco 7
┌─────┬───────┐
│Pág. │ Marco │               Traducción de Dirección:
├─────┼───────┤               Dir. Lógica: 2348
│  0  │   4   │               → Página: 2348 ÷ 1024 = 2
│  1  │   1   │               → Desplazamiento: 2348 % 1024 = 300
│  2  │   0   │               → Tabla[2] = Marco 0
│  3  │   3   │               → Dir. Física: 0×1024 + 300 = 300
└─────┴───────┘
```

## Organización de Tablas de Páginas

### Problema del Tamaño
En sistemas de 64 bits con páginas de 4KB:
- Tabla puede requerir **más de 16.000 GB por proceso**
- Solución: Organización multinivel

### Tipos de Organización:

#### 1. Tabla de 1 Nivel (Lineal)
- Una tabla única
- **Problema**: Tamaño excesivo en arquitecturas grandes

#### 2. Tabla de 2 Niveles (Multinivel)
- Divide la tabla grande en **múltiples tablas pequeñas**
- Solo se cargan las tablas **realmente necesarias**
- **Ventaja**: Mejor uso de RAM

```
Tabla de Páginas de 2 Niveles (Ejemplo con páginas de 4KB):

Dirección Lógica (32 bits):
┌───────────┬───────────┬─────────────┐
│10 bits    │10 bits    │12 bits      │
│Dir. Tabla │Pág. Tabla │Desplazamien.│
└───────────┴───────────┴─────────────┘
    ↓           ↓           ↓
    │           │           └─ Offset en página (4KB = 2¹²)
    │           └─ Página dentro de tabla (1024 entradas)
    └─ Directorio de tabla (1024 entradas)

Estructura en Memoria:
                        ┌─────────────────┐
                        │ Registro CR3    │ ← Apunta al Directorio
                        └─────────────────┘
                               ↓
┌─────────────────────────────────────┐
│ DIRECTORIO DE PÁGINAS              │ ← 1024 entradas × 4 bytes = 4KB
│ ┌─────┬─────────────────────────┐ │
│ │  0  │ → Tabla Páginas 0       │ │ ← Solo se carga si es necesaria
│ ├─────┼─────────────────────────┤ │
│ │  1  │ → Tabla Páginas 1       │ │
│ ├─────┼─────────────────────────┤ │
│ │ ... │ ...                     │ │
│ ├─────┼─────────────────────────┤ │
│ │1023 │ → Tabla Páginas 1023    │ │
│ └─────┴─────────────────────────┘ │
└─────────────────────────────────────┘
         ↓
┌─────────────────────────────────────┐
│ TABLA DE PÁGINAS 0                  │ ← 1024 entradas × 4 bytes = 4KB  
│ ┌─────┬─────────────────────────┐ │
│ │  0  │ → Marco Físico 245      │ │
│ ├─────┼─────────────────────────┤ │
│ │  1  │ → Marco Físico 891      │ │
│ ├─────┼─────────────────────────┤ │
│ │ ... │ ...                     │ │
│ └─────┴─────────────────────────┘ │
└─────────────────────────────────────┘

Ventaja: Si un proceso solo usa 12MB:
- Directorio: 1 página (4KB)
- 3 tablas de páginas: 3 × 4KB = 12KB
- Total: 16KB vs 4MB de tabla lineal
```

#### 3. Tabla Invertida
- **Una sola tabla para todo el sistema**
- Una entrada por **marco físico** (no por página virtual)
- Usa **hashing** para búsqueda rápida
- **Ventaja**: Tamaño fijo independiente del número de procesos

```
Tabla Invertida vs Tabla Normal:

TABLA NORMAL (por proceso):                TABLA INVERTIDA (sistema):
┌─────────────────────────┐              ┌─────────────────────────┐
│ Proceso A               │              │ ÚNICA TABLA DEL SISTEMA │
│ ┌─────┬─────────────┐   │              │ ┌───────┬─────────────┐ │
│ │Pág.0│→ Marco 15   │   │              │ │Marco 0│PID:B,Pág:2  │ │
│ │Pág.1│→ Marco 23   │   │              │ │Marco 1│PID:C,Pág:0  │ │
│ └─────┴─────────────┘   │              │ │Marco 2│Libre        │ │
└─────────────────────────┘              │ │...    │...          │ │
┌─────────────────────────┐              │ │Marco15│PID:A,Pág:0  │ │
│ Proceso B               │              │ │...    │...          │ │
│ ┌─────┬─────────────┐   │              │ │Marco23│PID:A,Pág:1  │ │
│ │Pág.0│→ Marco 8    │   │              │ └───────┴─────────────┘ │
│ │Pág.1│→ Marco 12   │   │              └─────────────────────────┘
│ │Pág.2│→ Marco 0    │   │
│ └─────┴─────────────┘   │              Búsqueda con Hash:
└─────────────────────────┘              Dirección Virtual (PID:A, Pág:1)
┌─────────────────────────┐                        ↓
│ Proceso C               │              Hash(A,1) = índice tabla
│ ┌─────┬─────────────┐   │                        ↓
│ │Pág.0│→ Marco 1    │   │              Si hash[índice] = (A,1)
│ └─────┴─────────────┘   │              → Marco = 23
└─────────────────────────┘              Sino → colisión, buscar cadena

Problema: Una página virtual (PID:A, Pág:0)
         ¿En qué marco está?
         → Hay que buscar en TODA la tabla
Solución: Usar función HASH para acceso directo
```

## Translation Lookaside Buffer (TLB)

### Problema:
Cada acceso a memoria puede requerir **2 accesos físicos**:
1. Uno para obtener la entrada de tabla de páginas
2. Otro para obtener los datos

### Solución: TLB
- **Cache de alta velocidad** para entradas de tabla de páginas
- Almacena las entradas **más recientemente usadas**
- Proceso:
  1. MMU examina TLB
  2. **Hit**: Obtiene marco directamente
  3. **Miss**: Consulta tabla de páginas y actualiza TLB

```
Funcionamiento del TLB:

┌─────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────┐
│ CPU         │    │ TLB (Cache)     │    │ Tabla Páginas   │    │ RAM     │
│             │    │ ┌─────┬───────┐ │    │ ┌─────┬───────┐ │    │         │
│Dir. Virtual │───►│ │Pág.2│Marco 6│ │    │ │Pág.0│Marco 4│ │    │ Datos   │
│ (Pág:2,Off:│    │ │Pág.5│Marco 1│ │    │ │Pág.1│Marco 1│ │◄──►│         │
│  348)       │    │ │Pág.7│Marco 3│ │    │ │Pág.2│Marco 6│ │    │         │
└─────────────┘    │ └─────┴───────┘ │    │ │...  │...    │ │    └─────────┘
                   └─────────────────┘    │ └─────┴───────┘ │
                                          └─────────────────┘

CASO 1 - TLB HIT:
Página 2 → Encontrada en TLB → Marco 6
Accesos a memoria: 1 (solo para obtener datos)
Tiempo: ~1 ciclo

CASO 2 - TLB MISS:
Página 8 → No está en TLB
1. Buscar en Tabla de Páginas → Marco X
2. Actualizar TLB con nueva entrada
3. Obtener datos del Marco X
Accesos a memoria: 2 (tabla + datos)
Tiempo: ~2 ciclos

TLB típico:
- 64-1024 entradas
- Tiempo acceso: 1 ciclo
- Hit ratio: 95-99%
```

### Consideraciones:
- Cambio de contexto **invalida TLB**
- Quantum pequeño en Round Robin → muchos misses
- Quantum grande → mejor aprovechamiento de TLB

## Tamaño de Página: Trade-offs

### Páginas Pequeñas:
**Ventajas:**
- Menor fragmentación interna
- Más páginas pueden residir en memoria

**Desventajas:**
- Tablas de páginas más grandes
- Mayor overhead de gestión

### Páginas Grandes:
**Ventajas:**
- Transferencias más eficientes desde almacenamiento secundario
- Tablas de páginas más pequeñas

**Desventajas:**
- Mayor fragmentación interna

## Segmentación vs Paginación

| Aspecto | Segmentación | Paginación |
|---------|-------------|------------|
| **Visibilidad** | Visible al programador | Transparente |
| **Tamaño** | Variable | Fijo |
| **Fragmentación** | Externa | Solo interna |
| **Compartición** | Fácil (por segmento) | Compleja |
| **Protección** | Por segmento | Por página |

## Segmentación Paginada

**Combina ambas técnicas:**
- Cada **segmento se divide en páginas**
- Aprovecha ventajas de ambos enfoques:
  - **Segmentación**: Modularidad, compartición, protección
  - **Paginación**: Eliminación de fragmentación externa

### Proceso de traducción:
1. Dirección lógica → `<Segmento, Página, Desplazamiento>`
2. Tabla de segmentos → Tabla de páginas del segmento
3. Tabla de páginas → Marco físico
4. Marco + desplazamiento → Dirección física final

```
Segmentación Paginada - Traducción de Direcciones:

Dirección Lógica: <Segmento, Página, Desplazamiento>
                     ↓
             ┌─────────────────┐
             │ Tabla Segmentos │
             │ ┌─────┬───────┐ │
             │ │Seg 0│→TP0   │ │ ← Apunta a Tabla de Páginas 0
             │ │Seg 1│→TP1   │ │
             │ │Seg 2│→TP2   │ │
             │ └─────┴───────┘ │
             └─────────────────┘
                     ↓
     ┌─────────────────────────────────┐
     │ Tabla de Páginas del Segmento   │
     │ ┌─────┬─────────────────────┐   │
     │ │Pág 0│→ Marco 15           │   │
     │ │Pág 1│→ Marco 23           │   │
     │ │Pág 2│→ Marco 8            │   │
     │ └─────┴─────────────────────┘   │
     └─────────────────────────────────┘
                     ↓
             ┌─────────────────┐
             │ Memoria Física  │
             │ ┌─────────────┐ │
             │ │ Marco 8     │ │ ← Página del segmento
             │ │ + Desplaz.  │ │
             │ │ = Dir.Final │ │
             │ └─────────────┘ │
             └─────────────────┘

Ejemplo:
Dir. Lógica: <1, 2, 348>
1. Segmento 1 → Tabla de Páginas del Seg.1
2. Página 2 en TP1 → Marco 8  
3. Marco 8 + Desplazamiento 348 = Dirección Física Final

Ventajas:
✓ Segmentos de diferente tamaño (flexibilidad)
✓ No fragmentación externa (páginas fijas)
✓ Protección por segmento
✓ Compartición fácil de segmentos completos
```

## Ejemplo Práctico: Intel x386

El procesador Intel x386 implementa segmentación paginada:
- **Segmentación** para organización lógica
- **Paginación de 2 niveles** para gestión física
- Páginas de 4KB o 4MB
- Directorio de páginas + Tablas de páginas

Esta arquitectura híbrida permite aprovechar los beneficios de ambas técnicas mientras minimiza sus desventajas.

## Resumen

La administración de memoria ha evolucionado desde simples particiones hasta complejos sistemas de paginación multinivel. Los conceptos clave incluyen:

- **Abstracción** de direcciones (lógicas vs físicas)
- **Protección** entre procesos
- **Traducción eficiente** con MMU y TLB
- **Eliminación de fragmentación** con paginación
- **Organización flexible** con segmentación
- **Optimización** mediante técnicas híbridas

Estos mecanismos permiten que los sistemas operativos modernos gestionen eficientemente la memoria, proporcionando un entorno seguro y eficiente para la ejecución de múltiples procesos.