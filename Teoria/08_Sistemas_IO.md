# Subsistema de Entrada/Salida en Sistemas Operativos

## 📋 Índice
1. [Introducción](#introducción)
2. [Tipos de Dispositivos de I/O](#tipos-de-dispositivos-de-io)
3. [Arquitectura de Hardware](#arquitectura-de-hardware)
4. [Técnicas de I/O](#técnicas-de-io)
5. [Responsabilidades del Sistema Operativo](#responsabilidades-del-sistema-operativo)
6. [Diseño del Subsistema de I/O](#diseño-del-subsistema-de-io)
7. [Performance y Optimización](#performance-y-optimización)

---

## Introducción

El subsistema de Entrada/Salida es uno de los componentes más complejos de un Sistema Operativo debido a la **heterogeneidad** de dispositivos y sus diferentes características. Los dispositivos de I/O suelen ser **mucho más lentos** que la CPU y la RAM, lo que representa un desafío importante para el diseño del sistema.

---

## Tipos de Dispositivos de I/O

### Según su función

**1. Legibles para el usuario**
- Usados para comunicación humano-computadora
- Ejemplos: impresoras, pantallas, teclados, mouse

**2. Legibles para la máquina**
- Comunicación entre componentes electrónicos
- Ejemplos: discos, cintas, sensores

**3. Comunicación**
- Conexión con dispositivos remotos
- Ejemplos: módems, interfaces de red, líneas digitales

### Según unidad de transferencia

**Dispositivos por bloques**
- Transfieren información en bloques de datos
- Operaciones: `read`, `write`, `seek`
- Ejemplo: discos duros

```
Disco:  [Bloque 1][Bloque 2][Bloque 3][Bloque 4]...
         └─ 512B ─┘└─ 512B ─┘└─ 512B ─┘
```

**Dispositivos por carácter**
- Transfieren datos carácter por carácter
- Operaciones: `get`, `put`
- Ejemplos: teclados, mouse, puertos seriales

```
Teclado: H → e → l → l → o
         (un carácter a la vez)
```

### Según tipo de acceso

**Formas de acceso:**
- **Secuencial**: Los datos se leen en orden (ej: cinta magnética)
- **Aleatorio**: Se puede acceder a cualquier posición (ej: disco duro)

**Compartición:**
- **Compartido**: Varios procesos pueden usarlo simultáneamente (ej: disco)
- **Exclusivo**: Solo un proceso a la vez (ej: impresora)

**Operaciones:**
- **Read only**: Solo lectura (ej: CD-ROM)
- **Write only**: Solo escritura (ej: pantalla)
- **Read/Write**: Lectura y escritura (ej: disco)

---

## Arquitectura de Hardware

### Componentes principales

```
┌─────────┐
│   CPU   │
└────┬────┘
     │
┌────┴──────────────────────────┐
│      Bus del Sistema          │
└────┬────────────┬─────────────┘
     │            │
┌────┴────┐  ┌───┴──────────┐
│ Memoria │  │ Controladora │
│   RAM   │  │ Dispositivo  │
└─────────┘  └───┬──────────┘
                 │
            ┌────┴─────┐
            │Dispositivo│
            └──────────┘
```

### Controladores (Controllers)

Cada dispositivo tiene una **controladora** que:
- Contiene registros para señales de control y datos
- Traduce comandos del SO a señales del hardware
- Maneja los detalles específicos del dispositivo

### Comunicación CPU-Controladora

La CPU se comunica con la controladora mediante **registros**:

**Tipos de comandos:**
1. **Control**: Qué hacer (ej: girar el disco)
2. **Test**: Controlar el estado (ej: ¿hay error?)
3. **Read/Write**: Transferir información

### Mapeo de E/S

**Memory Mapped I/O** (Correspondencia en memoria)
```
Espacio de Direcciones Unificado:
┌──────────────────┐
│  0x0000 - 0x7FFF │ ← Memoria RAM
├──────────────────┤
│  0x8000 - 0x8FFF │ ← Dispositivo A
├──────────────────┤
│  0x9000 - 0x9FFF │ ← Dispositivo B
└──────────────────┘
```
- Dispositivos y memoria comparten espacio de direcciones
- No requiere instrucciones especiales
- Se usa las mismas instrucciones que para memoria

**Isolated I/O** (E/S Aislada con Puertos)
```
Espacio de Memoria:     Espacio de I/O:
┌──────────────┐       ┌──────────────┐
│   Memoria    │       │ Puerto 0x60  │ ← Teclado
│     RAM      │       ├──────────────┤
└──────────────┘       │ Puerto 0x3F8 │ ← Serial
                       └──────────────┘
```
- Espacios de direcciones separados
- Requiere instrucciones especiales (IN/OUT)
- Conjunto limitado de instrucciones

---

## Técnicas de I/O

### 1. I/O Programada (Polling)

**Características:**
- La CPU controla directamente la operación
- La CPU **espera activamente** hasta que el dispositivo complete
- Se desperdician ciclos de CPU

**Flujo de operación:**
```
CPU                    Dispositivo
 │                          │
 ├─ Envía comando ─────────>│
 │                          │
 ├─ ¿Listo? ──────────────>│
 │<─ No ────────────────────┤
 │                          │
 ├─ ¿Listo? ──────────────>│
 │<─ No ────────────────────┤
 │                          │
 ├─ ¿Listo? ──────────────>│ (Busy-wait)
 │<─ Sí ────────────────────┤
 │                          │
 ├─ Lee datos ─────────────>│
 │<─ Datos ─────────────────┤
```

**Desventaja:** La CPU queda bloqueada esperando, no puede hacer otras tareas.

### 2. I/O Manejada por Interrupciones

**Características:**
- La CPU no espera activamente
- El dispositivo interrumpe a la CPU cuando termina
- La CPU puede ejecutar otras instrucciones mientras espera

**Flujo de operación:**
```
CPU                    Dispositivo
 │                          │
 ├─ Envía comando ─────────>│
 │                          │
 ├─ Continúa ejecutando     │ (procesando...)
 │    otras tareas          │
 │                          │
 │<─ INTERRUPCIÓN! ─────────┤ (¡Terminé!)
 │                          │
 ├─ Lee datos ─────────────>│
 │<─ Datos ─────────────────┤
 │                          │
 └─ Reanuda ejecución       │
```

**Ventaja:** La CPU aprovecha mejor el tiempo ejecutando otros procesos.

### 3. DMA (Direct Memory Access)

**Características:**
- Un componente especial (DMA Controller) controla la transferencia
- Transfiere **bloques completos** entre memoria y dispositivo
- La CPU solo es interrumpida al finalizar el bloque completo

**Flujo de operación:**
```
CPU          DMA Controller       Memoria      Dispositivo
 │                │                  │              │
 ├─ Configura ───>│                  │              │
 │   transferencia│                  │              │
 │                │                  │              │
 ├─ Continúa      │                  │              │
 │   ejecutando   ├─ Lee dato ──────>│              │
 │   otras tareas │<─ Dato ──────────┤              │
 │                ├─ Escribe ───────────────────────>│
 │                │                  │              │
 │                │  (repite varias veces)          │
 │                │                  │              │
 │<─ INTERRUPCIÓN!│                  │              │
 │  (Bloque completo transferido)    │              │
```

**Ventaja:** Minimiza la intervención de la CPU, ideal para grandes volúmenes de datos.

---

## Responsabilidades del Sistema Operativo

### Objetivos principales

**1. Controlar dispositivos de E/S**
- Generar comandos
- Manejar interrupciones
- Gestionar errores

**2. Proporcionar interfaz uniforme**
- Ocultar complejidad del hardware
- Operaciones estándar: `open`, `close`, `read`, `write`, `lock`, `unlock`

**3. Eficiencia**
- Usar multiprogramación para que un proceso espere I/O mientras otro se ejecuta
- Minimizar el impacto de la lentitud de los dispositivos

### Servicios del Subsistema de I/O

**Buffering**
- Almacenar datos en memoria durante la transferencia
- Soluciona diferencias de velocidad entre dispositivos

```
Ejemplo: Lectura de disco

Disco (lento)  →  [Buffer en RAM]  →  Proceso (rápido)
                   ↑
                   Almacenamiento temporal
```

**Caching**
- Mantener copia de datos de acceso reciente en memoria
- Mejora la performance evitando accesos repetidos al dispositivo

```
1º acceso: Disco → Cache → Proceso (lento)
2º acceso: Cache → Proceso (rápido!)
```

**Spooling**
- Administrar cola de requerimientos para dispositivos de acceso exclusivo
- Ejemplo: Cola de impresión

```
Proceso A ─┐
Proceso B ─┼─→ [Cola de Spooling] ─→ Impresora
Proceso C ─┘
```

**Planificación**
- Organizar requerimientos para optimizar el acceso
- Ejemplo: Ordenar lecturas de disco para minimizar movimiento del cabezal

**Manejo de Errores**
- Administrar fallos de dispositivos
- Retornar códigos de error
- Mantener logs de errores

**Reserva de Dispositivos**
- Garantizar acceso exclusivo cuando es necesario

### Tipos de I/O

**Bloqueante**
- El proceso se suspende hasta que se completa la operación
- Fácil de usar y entender
- Ejemplo: `read()` tradicional

```
Proceso ejecuta read()
    ↓
[BLOQUEADO]
    ↓
Datos listos
    ↓
[LISTO/RUNNING]
```

**No Bloqueante**
- El requerimiento retorna inmediatamente
- Permite al proceso hacer otras cosas mientras espera
- Ejemplo: Interfaces de usuario que leen teclado mientras actualizan pantalla

```
Proceso ejecuta read()
    ↓
Retorna inmediatamente
    ↓
Proceso sigue ejecutando
    ↓
Verifica si datos están listos
```

---

## Diseño del Subsistema de I/O

### Arquitectura en capas

```
┌─────────────────────────────────────┐
│     APLICACIONES DE USUARIO         │
├─────────────────────────────────────┤ MODO USUARIO
│  Librerías (acceso a syscalls)      │
╞═════════════════════════════════════╡
│  Software Independiente del SO      │
│  (interfaz uniforme, buffering,     │
│   manejo de errores, planificación) │
├─────────────────────────────────────┤ MODO KERNEL
│  Drivers (código del dispositivo)   │
├─────────────────────────────────────┤
│  Gestor de Interrupciones           │
├─────────────────────────────────────┤
│  HARDWARE (controladoras)           │
└─────────────────────────────────────┘
```

### Componentes principales

**1. Capa de Usuario**
- **Librerías**: Permiten acceso a syscalls del kernel
- **Procesos de apoyo**: Demonios (ej: demonio de impresión)

**2. Software Independiente del SO**
- Implementa los principales servicios de I/O
- Mantiene información de estado de cada dispositivo:
  - Archivos abiertos
  - Conexiones de red
  - Buffers, memoria utilizada, etc.

**3. Drivers (Controladores)**

Características:
- Contienen código **dependiente del dispositivo**
- Manejan un tipo específico de dispositivo
- Traducen requerimientos abstractos en comandos concretos
- Forman parte del espacio de memoria del Kernel
- Se cargan como **módulos dinámicos**

```
Aplicación: "leer archivo X"
    ↓
SO: "leer bloque 1234 del disco"
    ↓
Driver: "mover cabezal a cilindro 50,
         leer sector 12, DMA a dirección 0xXXXX"
    ↓
Hardware: ejecuta comandos físicos
```

**Ejemplo en Linux:**

Los drivers distinguen 3 tipos:
- **Carácter**: I/O programada o por interrupciones
- **Bloque**: DMA
- **Red**: Puertos de comunicaciones

Operaciones mínimas requeridas:
- `init_module`: Instalar el driver
- `cleanup_module`: Desinstalar el driver
- `open`: Abrir el dispositivo
- `release`: Cerrar el dispositivo
- `read`: Leer bytes
- `write`: Escribir bytes
- `ioctl`: Órdenes de control

Operaciones adicionales:
- `llseek`, `flush`, `poll`, `mmap`, `fsync`, `fasync`, `lock`, etc.

**4. Gestor de Interrupciones**
- Atiende todas las interrupciones del hardware
- Deriva al driver correspondiente
- Resguarda información de contexto
- Independiente del driver específico

---

## Ciclo de Vida de un Requerimiento de I/O

### Ejemplo: Lectura de archivo en disco

```
1. Proceso invoca: read(file, buffer, size)
          ↓
2. Syscall al Kernel
          ↓
3. Determinar dispositivo que contiene el archivo
   (traducir nombre → representación del dispositivo)
          ↓
4. Traducir requerimiento en bloques de disco
   (trabajo del sistema de archivos)
          ↓
5. Driver realiza lectura física
   - Configura controladora
   - Inicia transferencia DMA
          ↓
6. Proceso queda BLOQUEADO
   (otro proceso puede ejecutarse)
          ↓
7. Dispositivo completa transferencia
   - Genera interrupción
          ↓
8. Gestor de interrupciones
   - Llama al driver
   - Driver verifica estado
          ↓
9. Marcar datos como disponibles
   - Desbloquear proceso
          ↓
10. Retornar control al proceso
```

### Diagrama visual del ciclo

```
PROCESO                 KERNEL              HARDWARE
   │                       │                    │
   ├─ read() ─────────────>│                    │
   │                       ├─ Solicita I/O ────>│
   │                       │                    │
   │ [BLOQUEADO]           │                    │ (procesando)
   │                       │                    │
   │                       │<─ INTERRUPCIÓN ────┤
   │                       │                    │
   │<─ Datos listos ───────┤                    │
   │                       │                    │
   │ [RUNNING]             │                    │
```

---

## Performance y Optimización

### Factores que afectan la performance

1. **Uso intensivo de CPU**
   - Ejecución de drivers
   - Código del subsistema de I/O

2. **Context switches**
   - Por interrupciones
   - Por bloqueo de procesos

3. **Uso del bus de memoria**
   - Copias de datos: Aplicación ↔ Kernel
   - Copias de datos: Kernel ↔ Controladora

### Estrategias de optimización

**1. Reducir context switches**
- Minimizar interrupciones innecesarias
- Agrupar operaciones

**2. Reducir copias de datos**
- Usar técnicas de zero-copy cuando sea posible
- Mapeo directo de memoria

**3. Reducir frecuencia de interrupciones**
- Transferir grandes cantidades de datos de una vez
- Usar controladoras más inteligentes (con buffers propios)
- Polling inteligente (cuando la espera es mínima)

**4. Usar DMA**
- Elimina intervención de CPU en transferencias
- Ideal para grandes volúmenes de datos

### Comparación de eficiencia

```
I/O Programada:
CPU: ████████████████████ (100% ocupada)
Throughput: Bajo

I/O con Interrupciones:
CPU: ████░░░░████░░░░████ (mejor aprovechamiento)
Throughput: Medio

DMA:
CPU: █░░░░░░░█░░░░░░░█░░░ (mínima intervención)
Throughput: Alto
```

---

## Resumen de Conceptos Clave

### Problemas principales
- **Heterogeneidad**: Gran variedad de dispositivos
- **Velocidad**: Dispositivos mucho más lentos que CPU/RAM
- **Formatos**: Diferentes formas de transferir datos

### Soluciones del SO
- **Abstracción**: Interfaz uniforme para todos los dispositivos
- **Eficiencia**: Multiprogramación, DMA, interrupciones
- **Servicios**: Buffering, caching, spooling, planificación

### Evolución de técnicas

```
I/O Programada
  ↓ (menos desperdicio de CPU)
I/O por Interrupciones
  ↓ (menos intervención de CPU)
DMA
  ↓ (máxima eficiencia)
```

### Principio de diseño

> "El SO debe ocultar la complejidad del hardware de I/O, proporcionando una interfaz simple y uniforme, mientras optimiza la performance mediante técnicas como buffering, caching y DMA."

---

## Glosario

- **Buffer**: Área de memoria temporal para almacenar datos durante transferencias
- **Cache**: Memoria rápida que guarda copias de datos frecuentemente accedidos
- **DMA**: Direct Memory Access, transferencia directa entre memoria y dispositivo sin CPU
- **Driver**: Software que controla un dispositivo específico
- **Polling**: Técnica donde la CPU pregunta repetidamente el estado del dispositivo
- **Spooling**: Sistema de cola para gestionar acceso a dispositivos exclusivos
- **Syscall**: Llamada al sistema, interfaz entre aplicaciones y kernel

---

*Documento generado para estudio de Introducción a Sistemas Operativos*