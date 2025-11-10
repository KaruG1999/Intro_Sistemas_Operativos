# Práctica: Memoria Virtual y Administración de Discos

## 📋 Índice
1. [Memoria Virtual con Paginación](#memoria-virtual-con-paginación)
2. [Algoritmos de Reemplazo de Páginas](#algoritmos-de-reemplazo-de-páginas)
3. [Administración de Discos Rígidos](#administración-de-discos-rígidos)
4. [Algoritmos de Planificación de Disco](#algoritmos-de-planificación-de-disco)
5. [Ejercicios Resueltos](#ejercicios-resueltos)

---

## Memoria Virtual con Paginación

### Conceptos Fundamentales

**Objetivo:** Alocar la mayor cantidad de páginas necesarias posibles en los marcos disponibles.

**Page Fault (Fallo de Página):**
- Ocurre cuando se necesita una página que no está en memoria
- **Hard page fault**: Se debe cargar la página desde disco

```
Proceso solicita página 5
         ↓
¿Está en memoria? ─── NO ──► PAGE FAULT
         │                        ↓
        SÍ                   Cargar desde disco
         │                        ↓
    Continuar              Actualizar tabla
```

### El Problema del Reemplazo

**¿Qué pasa cuando no hay marcos libres?**

```
Memoria Física (4 marcos):
┌────┬────┬────┬────┐
│ P1 │ P3 │ P7 │ P2 │  ← TODOS OCUPADOS
└────┴────┴────┴────┘
        ↓
  Llega página P5 → PAGE FAULT
        ↓
¿Qué página sacar? ← PÁGINA VÍCTIMA
```

**Algoritmo ideal:**
- Seleccionar como víctima la página que NO será referenciada en un futuro próximo
- Problema: ¡No podemos predecir el futuro!
- Solución: Predecir el futuro basándose en el comportamiento pasado

---

## Algoritmos de Reemplazo de Páginas

### 1. Algoritmo Óptimo (OPT)

**Estrategia:** Reemplazar la página cuya próxima referencia está más lejana.

**Ventaja:** Garantiza el menor número de page faults posible  
**Desventaja:** IMPOSIBLE de implementar (requiere conocer el futuro)

**Ejemplo:**

```
Secuencia de referencias: 1, 2, 1, 3, 4, 1, 4, 3, 5
Marcos disponibles: 3

┌──────┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ Ref  │ 1 │ 2 │ 1 │ 3 │ 4 │ 1 │ 4 │ 3 │ 5 │
├──────┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ F1   │ 1 │ 1 │ 1 │ 1 │ 1 │ 1 │ 1 │ 1 │ 5 │
│ F2   │   │ 2 │ 2 │ 2 │ 4 │ 4 │ 4 │ 4 │ 4 │
│ F3   │   │   │   │ 3 │ 3 │ 3 │ 3 │ 3 │ 3 │
├──────┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ PF?  │ X │ X │   │ X │ X │   │   │   │ X │
└──────┴───┴───┴───┴───┴───┴───┴───┴───┴───┘

Total de Page Faults: 5

Explicación del PF en columna 4:
- Víctima elegida: página 2
- ¿Por qué? Mirando adelante:
  - Página 1: se referencia en posición 6
  - Página 2: NO aparece más en la secuencia
  - Página 3: ya está en la secuencia actual
```

**Uso:** Solo como referencia teórica para comparar otros algoritmos.

---

### 2. Algoritmo LRU (Least Recently Used)

**Estrategia:** Reemplazar la página que no fue referenciada por más tiempo.

**Principio:** "La página que no se usó recientemente, probablemente no se use pronto"

**Implementación:** Cada página tiene un timestamp de su última referencia.

**Ejemplo:**

```
Secuencia: 1, 2, 1, 3, 4, 1, 4, 3, 5

┌──────┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ Ref  │ 1 │ 2 │ 1 │ 3 │ 4 │ 1 │ 4 │ 3 │ 5 │
├──────┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ F1   │ 1 │ 1 │ 1 │ 1 │ 1 │ 1 │ 1 │ 1 │ 1 │
│ F2   │   │ 2 │ 2 │ 2 │ 4 │ 4 │ 4 │ 4 │ 5 │
│ F3   │   │   │   │ 3 │ 3 │ 3 │ 3 │ 3 │ 3 │
├──────┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ PF?  │ X │ X │   │ X │ X │   │   │   │ X │
└──────┴───┴───┴───┴───┴───┴───┴───┴───┴───┘

Total de Page Faults: 5

Detalle del PF en columna 4:
Última referencia de cada página:
- Página 1: columna 3 (más reciente)
- Página 2: columna 2 (menos reciente) ← VÍCTIMA
- Víctima: página 2

Detalle del PF en columna 5:
- Página 1: columna 3
- Página 2: columna 2 (menos reciente) ← VÍCTIMA
- Página 3: columna 4 (más reciente)
```

**Ventajas:**
- ✅ Buen rendimiento en la práctica
- ✅ Aproximación razonable al óptimo

**Desventajas:**
- ❌ Overhead: necesita actualizar timestamp en cada referencia
- ❌ Complejidad de implementación

---

### 3. Algoritmo FIFO (First In, First Out)

**Estrategia:** Reemplazar la página más antigua en memoria (cola circular).

**Principio:** "La primera que entró, es la primera que sale"

**Ejemplo:**

```
Secuencia: 1, 2, 1, 3, 4, 1, 4, 3, 5

┌──────┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ Ref  │ 1 │ 2 │ 1 │ 3 │ 4 │ 1 │ 4 │ 3 │ 5 │
├──────┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ F1   │ 1 │ 1 │ 1 │ 1 │ 4 │ 4 │ 4 │ 4 │ 5 │
│ F2   │   │ 2 │ 2 │ 2 │ 2 │ 1 │ 1 │ 1 │ 1 │
│ F3   │   │   │   │ 3 │ 3 │ 3 │ 3 │ 3 │ 3 │
├──────┼───┼───┼───┼───┼───┼───┼───┼───┼───┤
│ PF?  │ X │ X │   │ X │ X │ X │   │   │ X │
└──────┴───┴───┴───┴───┴───┴───┴───┴───┴───┘

Total de Page Faults: 6

Cola circular (orden de llegada):
Columna 5: [1, 2, 3] → Víctima: 1 (más antigua)
Columna 6: [4, 2, 3] → Víctima: 2 (más antigua)
Columna 9: [4, 1, 3] → Víctima: 4 (más antigua)
```

**Ventajas:**
- ✅ Muy simple de implementar
- ✅ Bajo overhead

**Desventajas:**
- ❌ Puede reemplazar páginas muy usadas
- ❌ Sufre la Anomalía de Belady (más marcos = más page faults)
- ❌ Peor rendimiento que LRU

---

### 4. Algoritmo FIFO con Segunda Oportunidad (Second Chance)

**Mejora de FIFO usando un bit de referencia (R)**

**Funcionamiento:**
1. Cada página tiene un bit R (inicialmente 0)
2. Cuando se referencia una página: R = 1
3. Para elegir víctima:
   - Buscar en orden FIFO
   - Si R = 1: cambiar a R = 0 y dar segunda oportunidad
   - Si R = 0: es la víctima

```
┌────────────┐
│ Bit R = 1  │ ──► Segunda oportunidad (R = 0)
│ Bit R = 0  │ ──► VÍCTIMA
└────────────┘
```

**Ejemplo:**

```
Secuencia: 1, 2, 1, 3, 4, 1, 4, 3, 5

┌──────┬────┬────┬────┬────┬────┬────┬────┬────┬────┐
│ Ref  │ 1  │ 2  │ 1  │ 3  │ 4  │ 1  │ 4  │ 3  │ 5  │
├──────┼────┼────┼────┼────┼────┼────┼────┼────┼────┤
│ F1   │ 1  │ 1  │ 1* │ 1* │ 1  │ 1* │ 1* │ 1* │ 5  │
│ F2   │    │ 2  │ 2  │ 2  │ 4  │ 4  │ 4* │ 4* │ 4  │
│ F3   │    │    │    │ 3  │ 3  │ 3  │ 3  │ 3* │ 3  │
├──────┼────┼────┼────┼────┼────┼────┼────┼────┼────┤
│ PF?  │ X  │ X  │    │ X  │ X  │    │    │    │ X  │
└──────┴────┴────┴────┴────┴────┴────┴────┴────┴────┘

* = bit R = 1

Total de Page Faults: 5

Explicación detallada:

Columna 3 (referencia a 1):
- Página 1 está en memoria → R = 1

Columna 5 (page fault, necesita 4):
- Cola FIFO: [1*, 2, 3]
- Página 1 tiene R=1 → R=0, segunda oportunidad
- Cola ahora: [2, 3, 1]
- Página 2 tiene R=0 → VÍCTIMA
- Carga página 4

Columna 9 (page fault, necesita 5):
- Cola: [1*, 4*, 3*] (todas referenciadas)
- Página 1 tiene R=1 → R=0
- Página 4 tiene R=1 → R=0
- Página 3 tiene R=1 → R=0
- Vuelve a página 1 con R=0 → VÍCTIMA
```

**Ventajas:**
- ✅ Mejor que FIFO simple
- ✅ Bajo overhead (solo un bit)
- ✅ Da oportunidad a páginas usadas recientemente

---

### Comparación de Algoritmos

```
┌──────────┬────────┬─────────┬──────────────┬─────────────┐
│Algoritmo │  PFs   │Overhead │ Complejidad  │  Calidad    │
├──────────┼────────┼─────────┼──────────────┼─────────────┤
│ Óptimo   │   5    │   N/A   │ Imposible    │   Ideal     │
│ LRU      │   5    │  Alto   │ Alta         │  Muy buena  │
│ FIFO     │   6    │  Bajo   │ Baja         │   Regular   │
│ 2nd Ch.  │   5    │  Bajo   │ Media        │   Buena     │
└──────────┴────────┴─────────┴──────────────┴─────────────┘

Secuencia usada: 1, 2, 1, 3, 4, 1, 4, 3, 5
```

---

## Asignación de Marcos

### ¿Cómo distribuir los marcos disponibles entre procesos?

**Dos estrategias principales:**

### 1. Asignación Fija

Se asigna una cantidad fija de marcos a cada proceso.

**a) Reparto Equitativo:**
```
Marcos disponibles (m) = 100
Procesos (p) = 4

Marcos por proceso = m ÷ p = 100 ÷ 4 = 25 marcos

┌──────────┬──────────┐
│ Proceso  │  Marcos  │
├──────────┼──────────┤
│   P1     │    25    │
│   P2     │    25    │
│   P3     │    25    │
│   P4     │    25    │
└──────────┴──────────┘
```

**b) Reparto Proporcional:**
```
Basado en el tamaño de cada proceso

Fórmula: Marcos_Pi = (Tamaño_Pi / Tamaño_Total) × m

Ejemplo:
m = 100 marcos
P1: 50 páginas
P2: 30 páginas
P3: 20 páginas
Total: 100 páginas

Marcos_P1 = (50/100) × 100 = 50 marcos
Marcos_P2 = (30/100) × 100 = 30 marcos
Marcos_P3 = (20/100) × 100 = 20 marcos
```

### 2. Asignación Dinámica

Los procesos reciben marcos según su necesidad actual.

```
Tiempo 1:
P1 (activo): 40 marcos
P2 (activo): 35 marcos
P3 (bloqueado): 5 marcos

Tiempo 2:
P1 (activo): 50 marcos  ← Aumentó
P2 (bloqueado): 10 marcos  ← Disminuyó
P3 (activo): 40 marcos  ← Aumentó
```

---

## Alcance del Reemplazo

### Reemplazo Global

El page fault de un proceso puede reemplazar páginas de CUALQUIER proceso.

```
Proceso A necesita página:
  ↓
Puede sacar página de:
- Proceso A
- Proceso B  ✓
- Proceso C  ✓
- Proceso D  ✓
```

**Ventaja:** Mejor uso global de la memoria  
**Desventaja:** Un proceso puede afectar el rendimiento de otros

### Reemplazo Local

El page fault de un proceso solo puede reemplazar SUS PROPIAS páginas.

```
Proceso A necesita página:
  ↓
Solo puede sacar página de:
- Proceso A  ✓
```

**Ventaja:** Aislamiento entre procesos  
**Desventaja:** Puede desperdiciar memoria global

---

## Descarga Asincrónica de Páginas

### Problema

Cuando una página víctima está modificada, debe escribirse a disco antes de reemplazarla (operación lenta).

### Solución

**Reservar marcos especiales para descarga asincrónica:**

```
Estado inicial:
┌─────────────────────────────────┐
│ Marcos normales │ Marco descarga│
├─────────────────┼───────────────┤
│  [P1][P3][P7]   │    [libre]    │
└─────────────────┴───────────────┘

Page fault → necesita P5, víctima P3 (modificada):

Paso 1: P5 va al marco de descarga
┌─────────────────────────────────┐
│  [P1][P3*][P7]  │     [P5]      │
└─────────────────┴───────────────┘
                  ↓
         P3* se descarga a disco
         (proceso asíncrono)

Paso 2: Cuando P3 termina de descargarse
┌─────────────────────────────────┐
│  [P1][P5][P7]   │    [libre]    │
└─────────────────┴───────────────┘
      ↑
   P5 pasa al marco que era de P3
   Marco de descarga queda libre
```

**Ventajas:**
- ✅ El proceso continúa ejecutando mientras se descarga
- ✅ Reduce la latencia de page faults
- ✅ Mejora el rendimiento general

---

## Performance

### Effective Access Time (Tiempo de Acceso Efectivo)

**Fórmula:**
```
TAE = At + (1 - p) × Am + p × (Tf + Am)

Donde:
- At = Tiempo de acceso a la tabla de páginas
- Am = Tiempo de acceso a memoria
- Tf = Tiempo de atención de un page fault
- p = Tasa de page faults (0 ≤ p ≤ 1)
```

**Ejemplo numérico:**

```
Datos:
- At = 100 ns (con TLB miss)
- Am = 100 ns
- Tf = 8 ms = 8,000,000 ns
- p = 0.01 (1% de page faults)

TAE = 100 + (1 - 0.01) × 100 + 0.01 × (8,000,000 + 100)
    = 100 + 99 + 80,001
    = 80,200 ns

Sin page faults (p = 0):
TAE = 100 + 100 = 200 ns

¡Los page faults aumentan el tiempo 400 veces!
```

---

## Thrashing (Hiperpaginación)

### ¿Qué es?

**Sistema en thrashing:** Pasa más tiempo paginando que ejecutando procesos.

```
Estado Normal:
┌─────────────────────────────┐
│ Ejecución: 90%              │
│ Paginación: 10%             │
└─────────────────────────────┘

Estado de Thrashing:
┌─────────────────────────────┐
│ Ejecución: 10%              │
│ Paginación: 90% ← ¡PROBLEMA!│
└─────────────────────────────┘
```

### Causas

1. **Muy pocos marcos por proceso**
2. **Demasiados procesos activos**
3. **Working set no cabe en memoria**

```
Ciclo vicioso del thrashing:

Proceso necesita página
        ↓
    Page fault
        ↓
Sacar página víctima
        ↓
Esa página se necesita pronto
        ↓
    Page fault
        ↓
  (se repite...)
```

---

## Working Set (Conjunto de Trabajo)

### Modelo de Localidad

**Principio:** Las referencias a memoria tienden a agruparse.

```
Programa típico:
┌─────────────────────────────────┐
│ Fase 1: Inicialización          │
│ Páginas: {1, 2, 3}              │
│                                 │
│ Fase 2: Procesamiento principal │
│ Páginas: {5, 6, 7, 8}           │
│                                 │
│ Fase 3: Salida de resultados    │
│ Páginas: {10, 11}               │
└─────────────────────────────────┘

En cada fase, solo se necesitan pocas páginas
```

### Definición de Working Set

**Working Set (WS):** Conjunto de páginas referenciadas en las últimas Δ referencias.

```
Δ = ventana de trabajo (tamaño)

Secuencia de referencias:
... 3, 5, 7, 5, 3, 8, 7, 5, 3, 8 ...
                    └─────┬─────┘
                      Δ = 5

Working Set = {7, 5, 3, 8}
(páginas únicas en las últimas 5 referencias)
```

### Selección de Δ

**Δ muy chico:**
```
Δ = 2
WS = {3, 8}
Problema: No cubre toda la localidad
Resultado: Muchos page faults
```

**Δ muy grande:**
```
Δ = 20
WS = {1, 2, 3, 5, 6, 7, 8, 9, 10, 11, 12}
Problema: Incluye varias localidades
Resultado: Desperdicio de memoria
```

**Δ óptimo:**
```
Δ = 8
WS = {3, 5, 7, 8}
Cubre exactamente la localidad actual
```

### Prevención de Thrashing

**Fórmula de control:**

```
m = marcos disponibles totales
D = ΣWSSi = demanda total de marcos
WSSi = working set size del proceso i

Condición de thrashing:
D > m  → ¡THRASHING!

Solución:
D ≤ m  → Sistema estable
```

**Ejemplo:**

```
Sistema con m = 100 marcos

Proceso 1: WSS1 = 30 marcos
Proceso 2: WSS2 = 25 marcos
Proceso 3: WSS3 = 20 marcos
Proceso 4: WSS4 = 35 marcos

D = 30 + 25 + 20 + 35 = 110 > 100
         ↓
    ¡THRASHING!

Soluciones:
1. Suspender un proceso (ej: P4)
   D = 30 + 25 + 20 = 75 < 100 ✓

2. Agregar más memoria física

3. Reducir multiprogramación
```

---

## Administración de Discos Rígidos

### Organización Física de un HDD

```
Vista externa:        Vista interna:
┌──────────┐         ┌────────────┐
│          │         │  Plato 1   │
│  Disco   │         │  Plato 2   │
│  Rígido  │         │  Plato 3   │
│   HDD    │         │    ...     │
└──────────┘         └────────────┘
```

### Componentes

**1. Platos (Platters)**
- Discos magnéticos que giran
- Cada plato tiene 2 caras (superior e inferior)

**2. Caras/Superficies**
- Cada cara puede almacenar datos
- Algunas caras pueden no usarse

**3. Pistas (Tracks)**
- Círculos concéntricos en cada cara
- Numeradas desde el exterior hacia el centro

```
Vista superior de una cara:
        ┌─────────────────┐
        │    Pista 0      │ ← Exterior
        │   ┌─────────┐   │
        │   │ Pista 1 │   │
        │   │ ┌─────┐ │   │
        │   │ │Pista│ │   │
        │   │ │  2  │ │   │
        └───┴─┴─────┴─┴───┘
```

**4. Sectores**
- División de cada pista
- Unidad mínima de lectura/escritura
- Típicamente: 512 bytes o 4096 bytes

```
Pista dividida en sectores:
       Sector 0
          ↓
    ┌─────────┐
 S7 │    0    │ S1
    │7   │   1│
    │  ┌─┐  │
    │6 │ │ 2│
    │  └─┘  │
 S6 │5   3   4│ S3
    └─────────┘
       S4   S5
```

**5. Cilindro**
- Todas las pistas en la misma posición de todos los platos
- Las pistas n-ésimas de todas las caras

```
Vista lateral:
         Brazo
          ↓
Cara 0  ──●── Pista 5
Cara 1  ──●── Pista 5
Cara 2  ──●── Pista 5  ← Cilindro 5
Cara 3  ──●── Pista 5
Cara 4  ──●── Pista 5
```

---

### Capacidad de un HDD

**Fórmula:**
```
Capacidad = W × X × Y × Z

Donde:
W = Cantidad de caras
X = Cantidad de pistas por cara
Y = Cantidad de sectores por pista
Z = Tamaño de sector (bytes)
```

**Ejemplo:**

```
Disco con:
- 6 platos
- 2 caras útiles por plato → W = 6 × 2 = 12 caras
- 1500 pistas por cara → X = 1500
- 700 sectores por pista → Y = 700
- 256 bytes por sector → Z = 256

Capacidad = 12 × 1500 × 700 × 256 bytes
          = 3,225,600,000 bytes
          = 3,225,600,000 / 1,073,741,824
          = 3.004 GiB (Gibibytes)
```

---

### Prefijos Binarios

```
┌─────────┬────────┬─────────────┬─────────────────┐
│ Prefijo │ Símbolo│   Valor     │   Equivalencia  │
├─────────┼────────┼─────────────┼─────────────────┤
│  Kibi   │  KiB   │    2^10     │   1,024 bytes   │
│  Mebi   │  MiB   │    2^20     │   1,024 KiB     │
│  Gibi   │  GiB   │    2^30     │   1,024 MiB     │
│  Tebi   │  TiB   │    2^40     │   1,024 GiB     │
└─────────┴────────┴─────────────┴─────────────────┘

Comparación con prefijos decimales (SI):
1 KB (kilobyte) = 1,000 bytes
1 KiB (kibibyte) = 1,024 bytes

¡Importante! En la práctica se adoptan los prefijos binarios
```

---

### Tiempo de Acceso a un HDD

**Componentes del tiempo de acceso:**

```
Tiempo Total = Seek Time + Latency Time + Transfer Time
```

**1. Seek Time (Tiempo de Posicionamiento)**
- Tiempo para mover el brazo al cilindro correcto
- Variable según la distancia

```
Posición actual: Cilindro 10
Destino: Cilindro 50
        ↓
Seek time = Tiempo para mover 40 cilindros
```

**2. Latency Time (Tiempo de Latencia)**
- Tiempo desde que el brazo llega hasta que el sector pasa bajo la cabeza
- Depende de la velocidad de rotación (RPM)

```
Si no se conoce: Latency ≈ Tiempo de media vuelta

Ejemplo: Disco de 5400 RPM
5400 vueltas → 60 segundos = 60,000 ms
1 vuelta → 60,000/5400 = 11.11 ms
1/2 vuelta → 11.11/2 = 5.56 ms
```

**Fórmula general:**
```
Latency (ms) = (60,000 ms / RPM) / 2
```

**3. Transfer Time (Tiempo de Transferencia)**
- Tiempo para leer/escribir los datos
- Depende de la velocidad de transferencia

```
Ejemplo:
Velocidad = 15 Mibits/s
Tamaño del sector = 256 bytes = 2048 bits

15,728,640 bits → 1000 ms
2,048 bits → x ms

x = (2,048 × 1000) / 15,728,640
x = 0.1302 ms por sector
```

---

### Tipos de Almacenamiento

**Almacenamiento Secuencial:**
```
Fórmula:
Seek + Latency + (Transfer_Time × #Bloques)

Ejemplo: Leer 4500 sectores contiguos
= 2 + 2.38 + (0.13 × 4500)
= 2 + 2.38 + 585
= 589.38 ms

Ventaja: Una sola operación de posicionamiento
```

**Almacenamiento Aleatorio:**
```
Fórmula:
(Seek + Latency + Transfer_Time) × #Bloques

Ejemplo: Leer 4500 sectores dispersos
= (2 + 2.38 + 0.13) × 4500
= 4.51 × 4500
= 20,295 ms

Desventaja: Muchas operaciones de posicionamiento
```

**Comparación:**
```
Secuencial:  589.38 ms  ← 34 veces más rápido
Aleatorio:  20,295 ms
```

---

### Ejercicio Completo: Cálculos en HDD

**Datos del disco:**
- 6 platos, 2 caras útiles por plato
- 1500 pistas por cara
- 700 sectores por pista
- 256 bytes por sector
- Velocidad: 12600 RPM
- Seek time: 2 ms
- Velocidad de transferencia: 15 Mibits/s

#### a) Capacidad total

```
Capacidad = 12 caras × 1500 × 700 × 256 bytes
          = 3,225,600,000 bytes
          = 3.004 GiB
```

#### b) Caras que ocupa un archivo de 513 MiB

```
Paso 1: Capacidad de una cara
= 1500 pistas × 700 sectores × 256 bytes
= 268,800,000 bytes

Paso 2: Convertir tamaño del archivo
513 MiB = 513 × 1,048,576 = 537,919,488 bytes

Paso 3: Dividir
= 537,919,488 / 268,800,000
= 2.0011

Resultado: 3 caras (se redondea hacia arriba)
```

#### c) Tiempo para transferir 4500 sectores

```
Paso 1: Calcular latencia
12600 RPM → 1/2 vuelta = 60,000 / 12600 / 2 = 2.38 ms

Paso 2: Calcular tiempo de transferencia por sector
15 Mibits/s = 15,728,640 bits/s
256 bytes = 2,048 bits

Tiempo = 2,048 bits × 1000 ms / 15,728,640 bits
       = 0.1302 ms por sector

Paso 3: Calcular tiempos totales

Almacenamiento Secuencial:
= 2 + 2.38 + (0.1302 × 4500)
= 590.28 ms

Almacenamiento Aleatorio:
= (2 + 2.38 + 0.1302) × 4500
= 20,299.95 ms
```

---

## Algoritmos de Planificación de Disco

### Objetivo

**Minimizar el movimiento del brazo** (seek time es el parámetro más costoso)

**Método:** Ordenar lógicamente los requerimientos en la cola según el número de cilindro.

### Tratamiento de Pistas Duplicadas

```
Cola de requerimientos: {10, 40, 70, 10}

FCFS: Se atienden por separado
→ Atiende: 10, luego 40, luego 70, luego 10 nuevamente

SSTF/SCAN/LOOK/C-SCAN/C-LOOK: Se atienden consecutivamente
→ Atiende: 10 una sola vez (ambas solicitudes juntas)
```

---

### Configuración de Ejemplos

**Disco con:**
- 200 pistas (0-199)
- Cola: {98, 183, 37, 122, 14, 124, 65, 67}
- Viene de: pista 61
- Posición actual: pista 53
- Dirección: derecha (→)

---

### 1. FCFS (First Come First Served)

**Estrategia:** Atiende en orden de llegada.

```
Secuencia de atención:
53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67

Diagrama:
199├─────────────────────────────────────
   │           183
   │          /│\
   │         / │ \
   │        /  │  \
150├───────/───┼───\──────────────────
   │      /    │    \
   │     /     │     \   122 124
   │    /      │      \  /│\ /│\
100├──/────────┼───────\/─┴─\─┴─────
   │98         │       /\    /\
   │/│\        │      /  \  /  \
   │ │ \       │     /    \/    \
 50├─┼──●──────┼────/─────/\─────\──
   │ │  53*    │   /  65 67 \     \
   │ │    \    │  /    /│\ /│\     \
   │ │     \   │ /     ──┴───┴──    \
   │ │      \  │/    37              14
  0└─┴───────\─┴──────/│\────────────/│\

Movimientos:
|53-98| + |98-183| + |183-37| + |37-122| + 
|122-14| + |14-124| + |124-65| + |65-67|
= 45 + 85 + 146 + 85 + 108 + 110 + 59 + 2
= 640 movimientos
```

**Ventajas:**
- ✅ Simple y justo
- ✅ Sin inanición

**Desventajas:**
- ❌ Ineficiente (muchos movimientos)
- ❌ No aprovecha la localidad

---

### 2. SSTF (Shortest Seek Time First)

**Estrategia:** Atiende el requerimiento más cercano.

```
Posición: 53 → Más cercano: 65

Secuencia:
53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183

Diagrama:
199├─────────────────────────────────────
   │                               183
   │                              /│\
   │                             / │
   │                            /  │
150├───────────────────────────/───┼───
   │                         /     │
   │                    124 /      │
   │                122/│\/        │
100├────────────────/\─┴──\────────┼───
   │            98 /      \        │
   │           /│\/        \       │
   │          /  /          \      │
 50├────●────/──/────65 67───\─────┼───
   │   53  /  /     /│\/│\    \    │
   │      /  /   37/ ──┴──     \   │
   │     /  /    /│\            \  │
   │    /  /    /                \ │
  0└───/──/────/──────────────────\┴───
      /  /  14                      
     /  / /│\
    /__/ ─┘

Movimientos:
|53-65| + |65-67| + |67-37| + |37-14| + 
|14-98| + |98-122| + |122-124| + |124-183|
= 12 + 2 + 30 + 23 + 84 + 24 + 2 + 59
= 236 movimientos
```

**Ventajas:**
- ✅ Muy eficiente
- ✅ Reduce movimientos significativamente

**Desventajas:**
- ❌ Puede causar inanición (requests lejanos nunca se atienden)
- ❌ No es justo

---

### 3. SCAN (Algoritmo del Ascensor)

**Estrategia:** Barre en una dirección hasta el final del disco, luego cambia de dirección.

```
Dirección inicial: → (derecha)

Secuencia:
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 (fin)
199 → 37 → 14

Diagrama:
199├──────────────────────────────┐────
   │                          183 │
   │                         /│\  ↓
   │                        / │   │
150├───────────────────────/──┼───│───
   │                      /   │   │
   │                 124 /    │   │
   │             122/│\/      │   │
100├──────────────/\─┴─\──────┼───│───
   │          98 /      \     │   │
   │         /│\/        \    │   │
   │        /  /          \   │   │
 50├───●───/──/────65 67──\───┼───┘───
   │  53  /  /    /│\/│\   \  │
   │ ↓   /  /    / ──┴──    \ │
   │ │  /  /   37             \│
   │ ↓ /  /   /│\              ↓
  0└─┴─/──/───└─┘──────────────┴───
      /  /  14                 
     /  / /│\
    /__/ ─┘

Movimientos:
Ida: |53-199| = 146
Vuelta: |199-14| = 185
Total = 236 movimientos
```

**Ventajas:**
- ✅ Evita inanición
- ✅ Más justo que SSTF
- ✅ Predecible

**Desventajas:**
- ❌ Llega hasta el final aunque no haya requerimientos

---

### 4. LOOK

**Estrategia:** Como SCAN, pero solo llega hasta el último requerimiento.

```
Dirección inicial: → (derecha)

Secuencia:
53 → 65 → 67 → 98 → 122 → 124 → 183 (último)
183 → 37 → 14

Diagrama:
199├─────────────────────────────────
   │                          183
   │                         /│\┐
   │                        / │ │
150├───────────────────────/──┼─│───
   │                      /   │ │
   │                 124 /    │ │
   │             122/│\/      │ ↓
100├──────────────/\─┴─\──────┼─┘───
   │          98 /      \     │
   │         /│\/        \    │
   │        /  /          \   │
 50├───●───/──/────65 67──\───┼─────
   │  53  /  /    /│\/│\   \  │
   │ ↓   /  /    / ──┴──    \ │
   │ │  /  /   37             \│
   │ ↓ /  /   /│\              ↓
  0└─┴─/──/───└─┘──────────────┴───
      /  /  14                 
     /  / /│\
    /__/ ─┘

Movimientos:
Ida: |53-183| = 130
Vuelta: |183-14| = 169
Total = 208 movimientos
```

**Ventajas:**
- ✅ Más eficiente que SCAN
- ✅ No desperdicia movimientos

---

### 5. C-SCAN (Circular SCAN)

**Estrategia:** Barre solo en una dirección. Al llegar al final, salta al inicio sin atender.

```
Dirección: → (siempre derecha)

Secuencia:
53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 (fin)
→ SALTO a 0 (no cuenta)
0 → 14 → 37

Diagrama:
199├──────────────────────────────┐───
   │                          183 │
   │                         /│\  ↓
   │                        / │   │
150├───────────────────────/──┼───┘───
   │                      /   │   ↑salto
   │                 124 /    │   ↓
   │             122/│\/      │   0
100├──────────────/\─┴─\──────┼───┼───
   │          98 /      \     │   │
   │         /│\/        \    │   ↓
   │        /  /          \   │   │14 37
 50├───●───/──/────65 67──\───┼───│/│\/│\
   │  53  /  /    /│\/│\   \  │   │──┴──
   │     /  /    / ──┴──    \ │   │
   │    /  /                 \│   │
  0└───/──/────────────────────┴───┴───

Movimientos:
|53-199| + |0-37| = 146 + 37 = 183
(El salto de 199 a 0 NO se cuenta)
```

**Ventajas:**
- ✅ Tiempo de espera más uniforme
- ✅ Mejor para cargas pesadas

**Desventajas:**
- ❌ Puede desperdiciar movimientos

---

### 6. C-LOOK (Circular LOOK)

**Estrategia:** Como C-SCAN, pero solo llega hasta el último requerimiento.

```
Dirección: → (siempre derecha)

Secuencia:
53 → 65 → 67 → 98 → 122 → 124 → 183 (último)
→ SALTO a 14 (no cuenta)
14 → 37

Diagrama:
199├─────────────────────────────────
   │                          183
   │                         /│\┐
   │                        / │ │
150├───────────────────────/──┼─│───
   │                      /   │ │
   │                 124 /    │ │
   │             122/│\/      │ ↓
100├──────────────/\─┴─\──────┼─┘───
   │          98 /      \     │  ↑salto
   │         /│\/        \    │  ↓
   │        /  /          \   │  │14 37
 50├───●───/──/────65 67──\───┼──│/│\/│\
   │  53  /  /    /│\/│\   \  │  │──┴──
   │     /  /    / ──┴──    \ │  │
   │    /  /                 \│  │
  0└───/──/────────────────────┴──┴───

Movimientos:
|53-183| + |14-37| = 130 + 23 = 153
(El salto de 183 a 14 NO se cuenta)
```

**Ventajas:**
- ✅ Más eficiente que C-SCAN
- ✅ Mejor que todos los anteriores en este ejemplo

---

### Comparación de Algoritmos

```
┌──────────┬─────────────┬────────────┬─────────────┐
│Algoritmo │ Movimientos │ Inanición  │ Complejidad │
├──────────┼─────────────┼────────────┼─────────────┤
│  FCFS    │    640      │     No     │    Baja     │
│  SSTF    │    236      │     Sí     │    Media    │
│  SCAN    │    236      │     No     │    Media    │
│  LOOK    │    208      │     No     │    Media    │
│ C-SCAN   │    183      │     No     │    Media    │
│ C-LOOK   │    153      │     No     │    Media    │
└──────────┴─────────────┴────────────┴─────────────┘

Mejor rendimiento: C-LOOK (153 movimientos)
Peor rendimiento: FCFS (640 movimientos)
```

---

## Algoritmos con Page Faults

### Reglas Especiales

**Page Faults (PF):** Requerimientos de mayor prioridad que deben atenderse con urgencia.

**Reglas generales:**
1. Los PF se atienden **inmediatamente después** del requerimiento actual
2. Los movimientos para atender PF **sí se cuentan**
3. Una vez atendidos todos los PF, se continúa según el algoritmo

**Lógica por algoritmo:**

```
┌───────────┬─────────────────────────────────────┐
│ Algoritmo │  Comportamiento con PF              │
├───────────┼─────────────────────────────────────┤
│  FCFS     │  Orden FCFS de los PF               │
│  SSTF     │  PF más cercano primero             │
│  SCAN     │  Puede cambiar de sentido           │
│  C-SCAN   │  Mantiene el sentido original       │
│  LOOK     │  Como SCAN                          │
│  C-LOOK   │  Como C-SCAN                        │
└───────────┴─────────────────────────────────────┘
```

---

### Ejemplo con Page Faults

**Configuración:**
- Pistas: 100 (0-99)
- Cola inicial: {55, 75, 52^PF, 45, 10}
- Después de 30 movimientos: {25^PF, 60}
- Después de 40 movimientos totales: {90, 10}
- Viene de: pista 15
- Atendiendo: pista 20
- Dirección: derecha →

---

### FCFS con Page Faults

```
Secuencia:
20 → 55 → 75 → 52(PF) → 45 → 10 
[30 mov] → 25(PF) → 60 
[+10 mov = 40 total] → 90 → 10

Detalle de movimientos:
1. |20-55| = 35
2. |55-75| = 20  ← Total hasta aquí: 55
3. |75-52| = 23  (PF, atención inmediata)
4. |52-45| = 7
5. |45-10| = 35  ← Total hasta aquí: 120
6. En mov 30: entra PF en 25 y req 60
7. |10-25| = 15  (PF, atención inmediata)
8. |25-60| = 35  ← Total hasta aquí: 170
9. En mov 40: entran 90 y 10
10. |60-90| = 30
11. |90-10| = 80

Total: 334 movimientos

Diagrama:
99├──────────────────────────────┐───
  │                          90  │
  │                         /│\  │
  │                        / │   │
75├──────────●────────────/──┼───│───
  │         75\          /   │   │
  │  60        \        /    │   │
  │ /│\     55  \   52 /     │   │
50├─┴──────/│\──●──/│\/──────┼───│───
  │       /  \    \/         │   │
  │   45 /    \   /\         │   │
  │  /│\/      \ /  \        │   │
25├──┴──●───────●────\────────┼───│───
  │    25       │     \       │   │
20├─────●───────┼──────\──────┼───│───
  │    20*      │       \     │   │
  │             │        \    │   │
10├─────────────┼─────────●───┼───●───
  │            10        10   │  10
 0└─────────────┴──────────────┴───┘
```

---

### SSTF con Page Faults

```
Secuencia:
20 → 52(PF más cercano) → 55 → 60 → 75 → 45 → 25(PF) → 10 → 90 → 10

Razonamiento:
1. Desde 20, PF en 52 es más cercano que 55
2. Después de PF, sigue con reqs normales en orden SSTF
3. Cuando entra 25(PF) después de 30 mov, se atiende inmediato
4. Continúa con SSTF

Movimientos:
|20-52| + |52-55| + |55-60| + |60-75| + |75-45| + 
|45-25| + |25-10| + |10-90| + |90-10|
= 32 + 3 + 5 + 15 + 30 + 20 + 15 + 80 + 80
= 154 movimientos
```

---

### SCAN con Page Faults

```
Secuencia:
20 → 52(PF) → 55 → 60 → 75 → 90 → 99 (fin)
99 → 45 → 25(PF) → 10 → 10

Explicación:
- Dirección inicial: → derecha
- Atiende 52(PF) en el camino
- Llega hasta 99 (último del disco en esa dirección)
- Cambia de dirección: ← izquierda
- Atiende 25(PF) cuando aparece
- Cuando entran 90 y 10, ya pasó por 90, solo atiende el segundo 10

Movimientos:
Ida: |20-99| = 79
Vuelta: |99-10| = 89
Total: 174 movimientos
```

---

### LOOK con Page Faults

```
Secuencia:
20 → 52(PF) → 55 → 60 → 75 → 90 (último req)
90 → 45 → 25(PF) → 10 → 10

Diferencia con SCAN:
- No llega hasta 99, solo hasta 90 (último requerimiento)

Movimientos:
|20-90| + |90-10| = 70 + 80 = 154 movimientos
```

---

### C-SCAN con Page Faults

```
Secuencia:
20 → 52(PF) → 55 → 60 → 75 → 90 → 99 (fin)
→ SALTO a 0
0 → 10 → 10 → 25(PF) → 45

Explicación:
- Dirección: → siempre derecha
- Atiende todo hasta 99
- Salta a 0 (salto no cuenta)
- Continúa hacia la derecha

Movimientos:
|20-99| + |0-45| = 79 + 45 = 124
(Salto no cuenta)

Nota: El segundo 10 ya fue atendido, se atiende una sola vez
```

---

### C-LOOK con Page Faults

```
Secuencia:
20 → 52(PF) → 55 → 60 → 75 → 90 (último)
→ SALTO a 10 (primer req del otro extremo)
10 → 10 → 25(PF) → 45

Movimientos:
|20-90| + |10-45| = 70 + 35 = 105 movimientos
(Salto no cuenta)
```

---

### Comparación con Page Faults

```
┌──────────┬─────────────┬──────────────────────┐
│Algoritmo │ Movimientos │     Observaciones    │
├──────────┼─────────────┼──────────────────────┤
│  FCFS    │    334      │ Muy ineficiente      │
│  SSTF    │    154      │ Atiende PF cercanos  │
│  SCAN    │    174      │ Puede cambiar sentido│
│  LOOK    │    154      │ Más eficiente        │
│ C-SCAN   │    124      │ Mantiene dirección   │
│ C-LOOK   │    105      │ Mejor rendimiento    │
└──────────┴─────────────┴──────────────────────┘

Mejor: C-LOOK con 105 movimientos
Peor: FCFS con 334 movimientos
```

---

## Ejercicios Resueltos

### Ejercicio 1: Cálculo de Capacidad

**Enunciado:**
Disco con 4 platos, 2 caras útiles, 2000 pistas por cara, 500 sectores por pista de 512 bytes.

**Solución:**
```
W = 4 × 2 = 8 caras
X = 2000 pistas
Y = 500 sectores
Z = 512 bytes

Capacidad = 8 × 2000 × 500 × 512
          = 4,096,000,000 bytes
          = 3.815 GiB
```

---

### Ejercicio 2: Tiempo de Latencia

**Enunciado:**
Disco de 7200 RPM. Calcular latencia promedio.

**Solución:**
```
7200 vueltas → 60,000 ms
1 vuelta → 60,000 / 7200 = 8.33 ms
1/2 vuelta (latencia) → 8.33 / 2 = 4.17 ms
```

---

### Ejercicio 3: Comparar Algoritmos

**Enunciado:**
Disco con 150 pistas (0-149).
Cola: {85, 45, 120, 30, 90}
Posición actual: 50
Viene de: 40
Dirección: →

**Solución SSTF:**
```
50 → 45 → 30 → 85 → 90 → 120

Movimientos:
|50-45| + |45-30| + |30-85| + |85-90| + |90-120|
= 5 + 15 + 55 + 5 + 30
= 110 movimientos
```

**Solución C-LOOK:**
```
50 → 85 → 90 → 120 → SALTO → 30 → 45

Movimientos:
|50-120| + |30-45|
= 70 + 15
= 85 movimientos
```

---

## Resumen de Conceptos Clave

### Memoria Virtual

**🎯 Page Fault:** Ocurre cuando una página no está en memoria
**🎯 Algoritmo Óptimo:** Teórico, imposible de implementar
**🎯 LRU:** Práctico y eficiente, pero con overhead
**🎯 FIFO:** Simple pero puede ser ineficiente
**🎯 Segunda Oportunidad:** Mejora FIFO con un bit de referencia

### Thrashing

**🎯 Working Set:** Páginas necesarias en un momento dado
**🎯 Δ (Delta):** Ventana de trabajo
**🎯 Condición:** D > m causa thrashing
**🎯 Solución:** Reducir multiprogramación o aumentar memoria

### Discos Rígidos

**🎯 Componentes:** Platos, caras, pistas, sectores, cilindros
**🎯 Tiempo de acceso:** Seek + Latency + Transfer
**🎯 Secuencial vs Aleatorio:** Gran diferencia de rendimiento

### Algoritmos de Planificación

**🎯 FCFS:** Simple pero ineficiente
**🎯 SSTF:** Eficiente pero puede causar inanición
**🎯 SCAN/LOOK:** Justos y eficientes
**🎯 C-SCAN/C-LOOK:** Mejor para cargas pesadas
**🎯 Page Faults:** Prioridad máxima, se atienden inmediatamente

---

## Fórmulas Importantes

### Memoria Virtual
```
TAE = At + (1-p)×Am + p×(Tf + Am)
D = ΣWSSi (demanda total de frames)
Thrashing: D > m
```

### Discos
```
Capacidad = W × X × Y × Z
Latencia = (60,000 / RPM) / 2
Secuencial = Seek + Latency + (Transfer × #Bloques)
Aleatorio = (Seek + Latency + Transfer) × #Bloques
```

### Conversiones
```
1 KiB = 1,024 bytes = 2^10 bytes
1 MiB = 1,048,576 bytes = 2^20 bytes
1 GiB = 1,073,741,824 bytes = 2^30 bytes
```

---

*Documento generado para práctica de Introducción a Sistemas Operativos*