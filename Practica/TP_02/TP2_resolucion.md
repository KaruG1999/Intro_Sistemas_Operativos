# Resolución Trabajo Práctico N° 2 - Sistemas Operativos

## 1. Conceptos Fundamentales

### a. ¿Qué es un Sistema Operativo?
Un Sistema Operativo es un programa que actúa como intermediario entre el usuario y el hardware, gestionando los recursos del sistema (CPU, memoria, dispositivos) y proporcionando servicios a los programas de aplicación.

### b. Componentes del Hardware necesarios
- **CPU** con modos de ejecución (usuario/kernel)
- **Memoria principal** (RAM)
- **Timer/Reloj** para interrupciones
- **Controladores de interrupciones**
- **MMU** (Memory Management Unit)
- **Dispositivos de E/S**
- **Buses** de comunicación

### c. Componentes de un Sistema Operativo
- **Kernel** (núcleo del SO)
- **Administrador de procesos**
- **Administrador de memoria**
- **Sistema de archivos**
- **Administrador de dispositivos**
- **Shell/Interfaz de usuario**
- **Servicios del sistema**

### d. System Call (Llamada al Sistema)
Es el mecanismo que permite a los programas de usuario solicitar servicios del kernel. Se implementa mediante:
- **Interrupciones por software**
- **Cambio de modo** (usuario → kernel)
- **Tabla de vectores** de interrupciones

### e. Programa vs Proceso
- **Programa**: Código estático almacenado en disco
- **Proceso**: Programa en ejecución con estado dinámico (registros, memoria, etc.)

### f. Información mínima del Kernel sobre un proceso
Almacenada en el **PCB (Process Control Block)**:
- PID (Process ID)
- Estado del proceso
- Registros de CPU
- Información de memoria
- Lista de archivos abiertos
- Información de scheduling

### g. Objetivos de los algoritmos de planificación
- **Maximizar utilización de CPU**
- **Maximizar throughput**
- **Minimizar tiempo de retorno**
- **Minimizar tiempo de espera**
- **Minimizar tiempo de respuesta**
- **Equidad** entre procesos

### h. Preemptive vs Non-Preemptive
- **Preemptive**: El SO puede quitar la CPU a un proceso en ejecución
- **Non-Preemptive**: El proceso ejecuta hasta completarse o bloquearse

### i. Módulos de planificación
- **Short Term Scheduler**: Selecciona proceso de Ready Queue para CPU
- **Long Term Scheduler**: Controla grado de multiprogramación
- **Medium Term Scheduler**: Maneja swapping (memoria ↔ disco)

### j. Dispatcher vs Loader
- **Dispatcher**: Realiza el context switch entre procesos
- **Loader**: Carga programas del disco a memoria

### k. CPU Bound vs I/O Bound
- **CPU Bound**: Procesos que usan intensivamente la CPU
- **I/O Bound**: Procesos que realizan muchas operaciones de E/S

### l. Estados de un proceso
```
[NEW] → [READY] → [RUNNING] → [TERMINATED]
             ↑         ↓
           [BLOCKED] ←
```
- **NEW**: Proceso siendo creado
- **READY**: Esperando asignación de CPU
- **RUNNING**: Ejecutándose
- **BLOCKED**: Esperando evento (E/S)
- **TERMINATED**: Proceso finalizado

### m. Schedulers y transiciones
- **Long Term**: NEW → READY
- **Short Term**: READY → RUNNING
- **Medium Term**: READY ↔ SUSPENDED

### n. Tiempos de proceso
- **TR (Tiempo Retorno)**: Tiempo total desde llegada hasta finalización
- **TE (Tiempo Espera)**: Tiempo esperando en Ready Queue

### o. Tiempos promedio
- **TPR**: Promedio de tiempos de retorno
- **TPE**: Promedio de tiempos de espera

### p. Tiempo de respuesta
Tiempo desde llegada del proceso hasta primera respuesta del sistema.

## 2. Algoritmos de Scheduling

### FCFS (First Come First Served)
**Funcionamiento**: Atiende procesos por orden de llegada
- **Parámetros**: Ninguno
- **Ventajas**: Simple, sin starvation
- **Desventajas**: Efecto convoy, TR alto para procesos cortos
- **Más adecuado**: Sistemas batch

### SJF (Shortest Job First)
**Funcionamiento**: Selecciona proceso con ráfaga más corta
- **Parámetros**: Estimación de tiempo de CPU
- **Ventajas**: Óptimo para minimizar tiempo promedio
- **Desventajas**: Starvation de procesos largos, requiere predicción
- **Más adecuado**: Sistemas batch con trabajos conocidos

### Round Robin
**Funcionamiento**: Asigna quantum fijo rotativo
- **Parámetros**: Quantum (Q)
- **Ventajas**: Equitativo, buena respuesta
- **Desventajas**: Overhead de context switch
- **Más adecuado**: Sistemas interactivos

### Prioridades
**Funcionamiento**: Selecciona proceso con mayor prioridad
- **Parámetros**: Valor de prioridad
- **Ventajas**: Flexibilidad, control granular
- **Desventajas**: Posible starvation
- **Más adecuado**: Sistemas de tiempo real

## 3. Ejercicio: Lote con llegada en t=0

**Datos:**
```
Job | CPU
----|----
1   | 7
2   | 15
3   | 12
4   | 4
5   | 9
```

### a. Diagramas de Gantt

**FCFS:**
```
|1111111|222222222222222|333333333333|4444|555555555|
0       7              22           34   38        47
```

**SJF:**
```
|4444|1111111|555555555|333333333333|222222222222222|
0    4       11        20           32             47
```

**Round Robin (Q=4):**
```
|1111|2222|3333|4444|5555|1111|2222|3333|555|22222222|3333333|
0    4    8    12   16   20   24   28   32  35      43       50
```

### b. Cálculos de tiempos

**FCFS:**
- Job 1: TR=7, TE=0
- Job 2: TR=22, TE=7
- Job 3: TR=32, TE=20
- Job 4: TR=35, TE=31
- Job 5: TR=42, TE=33
- **TPR = 27.6, TPE = 18.2**

**SJF:**
- Job 1: TR=11, TE=4
- Job 2: TR=47, TE=32
- Job 3: TR=32, TE=20
- Job 4: TR=4, TE=0
- Job 5: TR=20, TE=11
- **TPR = 22.8, TPE = 13.4**

**Round Robin:**
- Job 1: TR=24, TE=17
- Job 2: TR=43, TE=28
- Job 3: TR=50, TE=38
- Job 4: TR=16, TE=12
- Job 5: TR=35, TE=26
- **TPR = 33.6, TPE = 24.2**

### c. Comparación
**SJF** es el más eficiente en términos de tiempo promedio, **RR** es mejor para tiempo de respuesta, **FCFS** es el más simple pero menos eficiente.

## 4. Ejercicio: Lote con diferentes llegadas

**Datos:**
```
Job | Llegada | CPU
----|---------|----
1   | 0       | 4
2   | 2       | 6
3   | 3       | 4
4   | 6       | 5
5   | 8       | 2
```

### Análisis Round Robin
Con **Q=1**: Mucho overhead de context switch
Con **Q=6**: Comportamiento similar a FCFS para algunos procesos

**Conclusión Q alto**: Reduce context switching pero puede perjudicar tiempo de respuesta
**Ventajas Q alto**: Menos overhead
**Desventajas Q alto**: Mayor tiempo de respuesta

## 5. SRTF (Shortest Remaining Time First)
Versión preemptiva de SJF que selecciona el proceso con menor tiempo restante.
**Ventaja**: Mejor para procesos I/O Bound, reduce tiempo promedio de espera.

## 6. Algoritmos con Prioridades
Con tabla de prioridades donde menor número = mayor prioridad.
**Variantes**:
- **No apropiativa**: Proceso ejecuta hasta terminar o bloquearse
- **Apropiativa**: Proceso puede ser expulsado por otro de mayor prioridad

## 7. Inanición (Starvation)

### a. Qué significa
Situación donde un proceso espera indefinidamente por recursos sin poder ejecutarse.

### b. Algoritmos que la provocan
- **SJF**: Procesos largos pueden nunca ejecutarse
- **Prioridades**: Procesos de baja prioridad
- **SRTF**: Similar a SJF

### c. Técnica para evitarla
**Aging**: Incrementar gradualmente la prioridad de procesos que esperan mucho tiempo.

## 8. Planificación con I/O
Los procesos realizan operaciones de E/S que los bloquean temporalmente:
- Proceso sale de CPU y va a cola del dispositivo
- Cuando termina E/S, regresa a Ready Queue
- Cada dispositivo tiene su propio scheduler (típicamente FCFS)

## 9. Análisis de algoritmos con procesos CPU/I/O Bound
- **Round Robin**: Puede perjudicar procesos I/O Bound si el quantum es muy grande
- **SRTF**: Favorece procesos I/O Bound pero puede causar starvation en CPU Bound

## 10. VRR (Virtual Round Robin)

### Funcionamiento
- Procesos que regresan de I/O van a **cola auxiliar**
- Cola auxiliar tiene **prioridad** sobre ready queue
- Se asigna tiempo restante del quantum anterior

### Ventaja
Mejora el rendimiento de procesos I/O Bound al darles preferencia y tiempo justo.

## 11. Quantum y interrupciones de reloj
Sí, puede suceder que el quantum nunca llegue a 0 si el proceso se bloquea por I/O antes de consumir todo su tiempo asignado.

## 12. Estimación de ráfagas CPU

### Fórmula 1 (Media simple):
Si = (T1 + T2 + ... + Ti-1) / (i-1)

### Ejemplo con S1=10, ráfagas [6,4,6,4,13,13,13]:
- S2 = 10, S3 = 8, S4 = 6.67, S5 = 6, S6 = 8.2, S7 = 9.33, S8 = 10.71

### Fórmula 2 (Media exponencial):
Si+1 = α × Ti + (1-α) × Si

### Análisis de α:
- **α cercano a 1**: Mayor peso a casos recientes
- **α cercano a 0**: Mayor peso a casos históricos

## 13. Colas Multinivel

### a. Algoritmos por tipo de proceso:
- **Procesos Interactivos**: Round Robin (buena respuesta)
- **Procesos Batch**: FCFS o SJF (eficiencia)

### b. Algoritmo entre colas:
**Prioridades fijas** donde procesos interactivos tienen mayor prioridad que batch.

## 14. Ejercicio Colas Multinivel
Sistema con 3 colas por prioridad, cada una con RR (Q=3), planificadas por prioridades.
Se requiere diagrama de Gantt considerando apropiación y no apropiación.

## 15. Envejecimiento en Colas Multinivel
Técnica que mueve procesos a colas de mayor prioridad después de cierto tiempo de espera para evitar starvation.

## 16. Colas Multinivel con Realimentación
Sistema que penaliza procesos con mucho tiempo de CPU moviéndolos a colas de menor prioridad:
- **Cola 1**: RR con Q pequeño (procesos cortos)
- **Cola 2**: RR con Q medio (procesos medios)  
- **Cola 3**: FCFS (procesos largos)

## 17. Beneficios por tipo de trabajo

### a. SJF estático:
**Beneficia**: Trabajos cortos (CPU Bound y I/O Bound)
**Perjudica**: Trabajos largos

### b. Prioridad dinámica inversamente proporcional al tiempo desde última I/O:
**Beneficia**: Trabajos I/O Bound (cortos y largos)
**Perjudica**: Trabajos CPU Bound

## 18. Round Robin vs FIFO
Al incrementar el quantum sin límite en Round Robin, el algoritmo se aproxima a FIFO porque:
- Con quantum muy grande, los procesos terminan antes de ser expulsados
- Se elimina la rotación característica de RR
- El comportamiento se vuelve secuencial como FCFS

## 19. Análisis de programas con fork()

### Caso 1
```c
printf("hola")
x = fork()
if x < 1 {
    execv("ls")
    printf("mundo")  // No se ejecuta
    exit(0)
}
exit(0)
```
**Salida**: "hola" (1 vez) + resultado de "ls" (1 vez)
- El proceso hijo ejecuta "ls" y termina
- "mundo" nunca se imprime porque execv() reemplaza la imagen del proceso

### Caso 2
```c
printf("hola")
x = fork()
if x < 1 {
    execv("ps")
    printf("mundo")  // No se ejecuta
    exit(0)
}
execv("ls")
printf("fin")  // No se ejecuta
exit(0)
```
**Salida**: "hola" (1 vez) + resultado de "ps" (1 vez) + resultado de "ls" (1 vez)
- Proceso hijo ejecuta "ps"
- Proceso padre ejecuta "ls" 
- "mundo" y "fin" no se imprimen

### Caso 3
```c
printf("Anda a rendir el Primer Parcial de Promo!")
newpid = fork()
if newpid == 0 {
    printf("Estoy comenzando el Examen")
    execv("ps")
    printf("Termine el Examen")  // No se ejecuta
}
printf("¿Como te fue?")
exit(0)
printf("Ahora anda a descansar")  // No se ejecuta
```
**Salida**: 
- "Anda a rendir el Primer Parcial de Promo!" (1 vez)
- "Estoy comenzando el Examen" (1 vez)
- "¿Como te fue?" (2 veces - padre e hijo antes de exec)
- Resultado de "ps" (1 vez)

## 20. Programa con bucle fork

```c
for (c = 0; c < 3; c++) {
    pid = fork();
}
printf("Proceso\n");
```

### a. Líneas "Proceso"
Se crean **2³ = 8 procesos** total, por lo tanto aparecen **8 líneas** con "Proceso"

**Explicación**: 
- Iteración 0: 1 → 2 procesos
- Iteración 1: 2 → 4 procesos  
- Iteración 2: 4 → 8 procesos

### b. Número de procesos
Sí, el número de líneas coincide exactamente con el número de procesos que ejecutaron el printf.

## 21. Variables en procesos fork

```c
int p = 0;
for (c = 0; c < 3; c++) {
    pid = fork();
}
p++;
printf("Proceso %d\n", p);
```

### a, b, c. Valores impresos
**Todas las líneas tendrán el valor 1**

**Explicación**: 
- Cada proceso tiene su propia copia de la variable `p`
- Todos los procesos incrementan su copia de 0 a 1
- No hay memoria compartida entre procesos

### d. Modificaciones sugeridas
- Cambiar posición de `p++` dentro del bucle
- Modificar valor inicial de `p`
- Cambiar número de iteraciones del bucle

## 22. MMU (Memory Management Unit)
**Funciones:**
- Traducción direcciones lógicas → físicas
- Protección de memoria
- Control de acceso
- Soporte para paginación/segmentación

## 23. Espacio de direcciones
Rango de direcciones lógicas que puede usar un proceso, independiente de la ubicación física en memoria.

## 24. Direcciones Lógicas vs Físicas
- **Lógica/Virtual**: Generada por CPU, relativa al programa
- **Física**: Dirección real en memoria RAM

## 25. Particiones Múltiples

### Particiones Fijas:
- Tamaño predeterminado al arranque
- **Ventajas**: Simple implementación, rápida asignación
- **Desventajas**: Fragmentación interna, inflexibilidad

### Particiones Dinámicas:
- Tamaño según necesidad del proceso
- **Ventajas**: Mejor uso de memoria, flexibilidad
- **Desventajas**: Fragmentación externa, complejidad

### Información del Kernel:
- **Fijas**: Tabla de particiones con estado (libre/ocupada)
- **Dinámicas**: Lista de espacios libres y ocupados

### Transformación de direcciones:
```
Dirección Física = Dirección Base + Dirección Lógica
(si Dirección Lógica < Tamaño Partición)
```

## 26. Particiones Fijas: Tamaños

### Igual tamaño:
- **Ventajas**: Gestión muy simple
- **Desventajas**: Mucha fragmentación interna

### Diferente tamaño:
- **Ventajas**: Menos fragmentación interna
- **Desventajas**: Complejidad en asignación

## 27. Fragmentación

### a. Tipos:
- **Interna**: Espacio desperdiciado dentro de una partición asignada
- **Externa**: Espacios libres muy pequeños entre particiones para ser útiles

### b. Solución para Externa:
**Compactación**: Reubicación de procesos para consolidar espacios libres en un bloque contiguo grande.

## 28. Técnicas de Asignación de Particiones

**First Fit**: Asigna primera partición suficientemente grande
- **Ventaja**: Rápido
- **Desventaja**: Fragmentación al inicio

**Best Fit**: Asigna partición más pequeña suficiente
- **Ventaja**: Minimiza desperdicio
- **Desventaja**: Lento, fragmentación externa

**Worst Fit**: Asigna partición más grande disponible
- **Ventaja**: Deja espacios grandes
- **Desventaja**: Desperdicia memoria

**Next Fit**: Como First Fit pero continúa desde última asignación
- **Ventaja**: Distribuye mejor las asignaciones
- **Desventaja**: Puede ser menos eficiente

**Mejor para particiones dinámicas**: First Fit (balance velocidad/eficiencia)
**Mejor para particiones fijas**: Best Fit (minimiza desperdicio)

## 29. Segmentación

### a. Funcionamiento
Divide programa en segmentos lógicos (código, datos, stack). Cada segmento puede tener diferente tamaño. Direcciones: (segmento, desplazamiento)

### b. Estructuras necesarias
- **Tabla de segmentos** por proceso
- **Registro base y límite** por segmento

### c. Traducción de direcciones
```
Dirección lógica: (s, d)
Dirección física = Base[s] + d
(si d < Límite[s])
```

### d. Fragmentación
Solo externa (segmentos de tamaño variable)

### e. Técnica de asignación
First Fit generalmente es la mejor opción

## 30. Ejercicio Segmentación

**Tabla de segmentos:**
```
Seg | Base  | Tamaño
----|-------|--------
0   | 102   | 12500
1   | 28699 | 24300
2   | 68010 | 15855
3   | 80001 | 400
```

**Traducciones**:
- **0000:9001** → Base[0] + 9001 = 102 + 9001 = **9103**
- **0001:24301** → **ERROR** (24301 > 24300)
- **0002:5678** → Base[2] + 5678 = 68010 + 5678 = **73688**
- **0001:18976** → Base[1] + 18976 = 28699 + 18976 = **47675**
- **0003:0** → Base[3] + 0 = 80001 + 0 = **80001**

## 31. Paginación

### a. Funcionamiento
Memoria dividida en marcos (frames) de tamaño fijo. Procesos divididos en páginas del mismo tamaño. Tabla de páginas para traducción.

### b. Estructuras necesarias
- Tabla de páginas por proceso
- Tabla de marcos libres
- MMU para traducción

### c. Traducción de direcciones
```
Dirección lógica = Página × Tamaño_Página + Desplazamiento
Dirección física = Marco × Tamaño_Página + Desplazamiento
```

### d. Fragmentación
Solo interna (en la última página del proceso)

## 32. Ejercicio Paginación Completo

**Sistema**: Página = 512 bytes, Memoria = 10240 bytes, Proceso = 2000 bytes

**Tabla de páginas P1:**
```
Página | Marco
-------|------
0      | 3
1      | 5
2      | 2
3      | 6
```

### b. Traducciones de direcciones lógicas:
- **35**: Página=0, Offset=35 → Marco 3: 3×512+35 = **1571**
- **512**: Página=1, Offset=0 → Marco 5: 5×512+0 = **2560**
- **2051**: **ERROR** (fuera del espacio lógico de 2000 bytes)
- **0**: Página=0, Offset=0 → Marco 3: 3×512+0 = **1536**
- **1325**: Página=2, Offset=301 → Marco 2: 2×512+301 = **1325**
- **602**: Página=1, Offset=90 → Marco 5: 5×512+90 = **2650**

### c. Direcciones físicas a lógicas:
- **509**: Marco=0, no usado por P1 → **No válida**
- **1500**: Marco=2 (1024-1535), Offset=476 → **1500** (página 2)
- **0**: Marco=0, no usado por P1 → **No válida**
- **3215**: Marco=6 (3072-3583), Offset=143 → **1679** (página 3)
- **1024**: Marco=2 (1024-1535), Offset=0 → **1024** (página 2)
- **2000**: **No válida** (fuera del espacio del proceso)

## 33. Ejercicio Paginación con 2KiB

**Páginas de 2048 bytes, tabla:**
```
Página | Marco
-------|------
0      | 16
1      | 13
2      | 9
3      | 2
4      | 0
```

**Traducciones**:
- **5120**: Página=2, Offset=1024 → Marco 9: 9×2048+1024 = **19456**
- **3242**: Página=1, Offset=1194 → Marco 13: 13×2048+1194 = **27818**
- **1578**: Página=0, Offset=1578 → Marco 16: 16×2048+1578 = **34354**
- **2048**: Página=1, Offset=0 → Marco 13: 13×2048+0 = **26624**
- **8191**: Página=3, Offset=2047 → Marco 2: 2×2048+2047 = **6143**

## 34. Tamaño de Página

### a. Página pequeña:
- **Ventajas**: Menos fragmentación interna, mejor utilización memoria
- **Desventajas**: Tablas de páginas grandes, más overhead

### b. Página grande:
- **Ventajas**: Tablas de páginas pequeñas, menos overhead, mejor localidad
- **Desventajas**: Mayor fragmentación interna

## 35. Enfoques para tablas de páginas

### Tablas de 1 nivel:
- **Funcionamiento**: Una tabla directa página→marco
- **Ventajas**: Acceso directo, simple
- **Desventajas**: Tabla muy grande para espacios grandes

### Tablas de 2 niveles o más:
- **Funcionamiento**: Jerarquía de tablas, divide espacio de páginas
- **Ventajas**: Menos memoria para tablas, crecimiento dinámico
- **Desventajas**: Múltiples accesos a memoria

### Tablas invertidas:
- **Funcionamiento**: Una entrada por marco físico
- **Ventajas**: Tamaño fijo independiente de procesos
- **Desventajas**: Búsqueda más lenta, complejidad

## 36. Ejercicio Cálculos de Paginación

**Sistema**: 32 bits, página = 2048 bytes, Código = 51358 bytes, Datos = 68131 bytes

### a. Bits para página y desplazamiento:
- Desplazamiento: log₂(2048) = **11 bits**
- Página: 32 - 11 = **21 bits**

### b. Tamaño lógico máximo: 
2³² = **4 GiB**

### c. Páginas máximo P1: 
2²¹ = **2,097,152 páginas**

### d. Marcos disponibles con 4GiB RAM: 
4GiB ÷ 2048 = **2,097,152 marcos**

### e. Páginas para código: 
⌈51358 ÷ 2048⌉ = **26 páginas**

### f. Páginas para datos: 
⌈68131 ÷ 2048⌉ = **34 páginas**

### g. Fragmentación interna:
- Código: (26 × 2048) - 51358 = 1890 bytes
- Datos: (34 × 2048) - 68131 = 501 bytes
- **Total**: 2391 bytes

### h. Tabla 1 nivel: 
60 páginas × 1KiB = **60 KiB**

### i. Tabla 2 niveles: 
1 tabla primer nivel + 1 tabla segundo nivel = **2 KiB**

### j. Conclusión: 
Tablas de 2 niveles son más eficientes para procesos pequeños.

## 37. Comparación de Técnicas

| Aspecto | Part. Fijas | Part. Dinámicas | Segmentación | Paginación |
|---------|-------------|-----------------|--------------|------------|
| Fragmentación | Interna | Externa | Externa | Interna |
| Flexibilidad | Baja | Media | Alta | Media |
| Complejidad | Baja | Media | Alta | Alta |
| Protección | Básica | Básica | Buena | Buena |
| Reubicación | No | Difícil | Fácil | Fácil |
| Compartición | No | No | Sí | Sí |

## 38. Segmentación Paginada

### a. Funcionamiento
Combina segmentación (organización lógica) con paginación (gestión física):
- Cada segmento se divide en páginas
- Dirección: (segmento, página, desplazamiento)
- Doble traducción: segmento→tabla de páginas→marco físico

### b. Ventajas:
- Organización lógica de segmentos
- Gestión eficiente de memoria física
- Sin fragmentación externa
- Protección por segmento

### Desventajas:
- Mayor complejidad
- Doble indirección para traducción
- Overhead adicional

### c. Ejercicio traducciones:

**Tablas dadas:**
```
Tabla Segmentos:        Tabla Páginas:
Seg | Dir.Base          Seg-Pag | Dir.Base
----|--------          ---------|--------
1   | 500              1-1     | 40
2   | 1500             1-2     | 80
3   | 5000             1-3     | 60
                       2-1     | 20
                       2-2     | 25
                       2-3     | 0
                       3-1     | 120
                       3-2     | 150
```

**Traducciones**:
- **(2,1,1)**: Seg 2, Página 1 → Base 20, Dirección = 20 + 1 = **21**
- **(1,3,15)**: Seg 1, Página 3 → Base 60, Dirección = 60 + 15 = **75**
- **(3,1,10)**: Seg 3, Página 1 → Base 120, Dirección = 120 + 10 = **130**
- **(2,3,5)**: Seg 2, Página 3 → Base 0, Dirección = 0 + 5 = **5**

## Conclusiones Generales

Los algoritmos de scheduling deben balancear:
- **Eficiencia** (utilización CPU)
- **Equidad** (todos los procesos progresan)  
- **Tiempo de respuesta** (sistemas interactivos)

La administración de memoria evoluciona desde particiones simples hacia esquemas más sofisticados (paginación, segmentación) para optimizar el uso del recurso y proporcionar abstracción a los procesos.

**Tendencias actuales:**
- Sistemas combinan múltiples técnicas
- Paginación es dominante en sistemas modernos
- Segmentación se usa para organización lógica
- Memoria virtual permite abstracciones avanzadas

## Ejercicios Adicionales y Consideraciones

### Estimación de Ráfagas CPU (Ejercicio 12 completo)

**Ejemplo con ráfagas reales [6,4,6,4,13,13,13] y S₁=10:**

**Fórmula 1 (Media aritmética):**
- S₂ = 10 (valor inicial)
- S₃ = (6)/1 = 6
- S₄ = (6+4)/2 = 5
- S₅ = (6+4+6)/3 = 5.33
- S₆ = (6+4+6+4)/4 = 5
- S₇ = (6+4+6+4+13)/5 = 6.6
- S₈ = (6+4+6+4+13+13)/6 = 7.67

**Fórmula 2 con α=0.2:**
- S₂ = 10
- S₃ = 0.2×6 + 0.8×10 = 9.2
- S₄ = 0.2×4 + 0.8×9.2 = 8.16
- S₅ = 0.2×6 + 0.8×8.16 = 7.73
- S₆ = 0.2×4 + 0.8×7.73 = 6.98
- S₇ = 0.2×13 + 0.8×6.98 = 8.18
- S₈ = 0.2×13 + 0.8×8.18 = 9.14

**Fórmula 2 con α=0.5:**
- S₂ = 10
- S₃ = 0.5×6 + 0.5×10 = 8
- S₄ = 0.5×4 + 0.5×8 = 6
- S₅ = 0.5×6 + 0.5×6 = 6
- S₆ = 0.5×4 + 0.5×6 = 5
- S₇ = 0.5×13 + 0.5×5 = 9
- S₈ = 0.5×13 + 0.5×9 = 11

**Fórmula 2 con α=0.8:**
- S₂ = 10
- S₃ = 0.8×6 + 0.2×10 = 6.8
- S₄ = 0.8×4 + 0.2×6.8 = 4.56
- S₅ = 0.8×6 + 0.2×4.56 = 5.71
- S₆ = 0.8×4 + 0.2×5.71 = 4.34
- S₇ = 0.8×13 + 0.2×4.34 = 11.27
- S₈ = 0.8×13 + 0.2×11.27 = 12.65

**Análisis de precisión:**
- **α=0.8** se adapta más rápido a cambios (mejor para ráfagas [13,13,13])
- **α=0.2** es más estable pero lento para adaptarse
- **Media aritmética** mantiene valores más conservadores

### Colas Multinivel con Realimentación (Ejercicio 16)

**Diseño sugerido para penalizar procesos CPU-intensivos:**

**Cola 0** (Prioridad Alta):
- Algoritmo: RR con Q=2
- Procesos nuevos e I/O bound

**Cola 1** (Prioridad Media):  
- Algoritmo: RR con Q=4
- Procesos que consumieron Q completo en Cola 0

**Cola 2** (Prioridad Baja):
- Algoritmo: FCFS
- Procesos que consumieron Q completo en Cola 1

**Reglas de movimiento:**
- Nuevo proceso → Cola 0
- Si consume Q completo → baja una cola
- Si se bloquea por I/O → regresa a Cola 0
- Aging: después de 10 unidades en cola inferior → sube una cola

**Ventajas:**
- Procesos interactivos (I/O bound) mantienen alta prioridad
- Procesos CPU-intensivos bajan gradualmente
- Aging previene starvation

### Ejercicios Prácticos Adicionales

**Ejercicio de Context Switch:**
El costo de cambio de contexto incluye:
1. Guardar registros del proceso actual
2. Actualizar PCB
3. Cambiar tabla de páginas/segmentos
4. Invalidar cache/TLB
5. Cargar registros del nuevo proceso

**Tiempo típico:** 1-100 microsegundos dependiendo del hardware

**Ejercicio de Fragmentación:**
```
Memoria de 1024KB con particiones dinámicas:
- Proceso A: 200KB (posición 0-199)
- Proceso B: 300KB (posición 200-499)  
- Proceso C: 150KB (posición 500-649)
- Espacio libre: 374KB (posición 650-1023)

Si llega proceso D de 400KB:
- No puede asignarse (fragmentación externa)
- Solución: Compactación o particionamiento
```

### Consideraciones de Rendimiento

**Thrashing en Paginación:**
- Ocurre cuando el sistema pasa más tiempo intercambiando páginas que ejecutando procesos
- Solución: Reducir grado de multiprogramación

**Localidad de Referencia:**
- **Temporal**: Referencias recientes tienden a repetirse
- **Espacial**: Referencias cercanas tienden a ocurrir juntas
- Importante para diseño de cache y algoritmos de reemplazo

**Algoritmos de Reemplazo de Páginas:**
- FIFO: Simple pero puede sufrir anomalía de Belady
- LRU: Óptimo en práctica pero costoso de implementar
- Clock: Aproximación eficiente de LRU

### Problemas Comunes y Soluciones

**Starvation:**
- Problema: Procesos nunca obtienen recursos
- Solución: Aging, límites de tiempo, colas múltiples

**Deadlock:**
- Condiciones: Exclusión mutua, retención, no apropiación, espera circular
- Prevención: Eliminar una condición
- Detección: Grafos de espera
- Recovery: Rollback de procesos

**Priority Inversion:**
- Proceso de baja prioridad bloquea recurso necesario por alta prioridad
- Solución: Priority inheritance protocol

### Métricas de Evaluación

**Para Scheduling:**
- Utilización de CPU (70-90% típico)
- Throughput (trabajos/unidad tiempo)
- Tiempo de retorno promedio
- Tiempo de respuesta (< 1 segundo para interactivos)
- Equidad (coeficiente de variación)

**Para Memoria:**
- Fragmentación interna/externa
- Overhead de tablas
- Tiempo de traducción de direcciones
- Hit ratio de TLB

## Casos de Estudio Reales

### Windows Scheduler
- **Algoritmo base**: Colas multinivel con realimentación
- **32 niveles de prioridad** (0-31)
- **Quantum variable** según nivel
- **Boost temporal** para procesos interactivos

### Linux CFS (Completely Fair Scheduler)
- **Algoritmo**: Red-black tree ordenado por tiempo virtual
- **Equidad perfecta** en el límite
- **Latencia objetivo**: 6ms para procesos interactivos
- **Sin niveles de prioridad** tradicionales

### Memoria Virtual en x86-64
- **Paginación de 4 niveles**
- **Páginas de 4KB** (típico) o 2MB/1GB (huge pages)
- **48 bits de dirección virtual** efectivos
- **TLB jerárquico** (L1, L2)

## Ejercicios de Repaso

### 1. Diseño de Scheduler Híbrido
Diseñe un scheduler que combine:
- Round Robin para procesos interactivos (Q=2)
- SJF para procesos batch
- Prioridades dinámicas basadas en comportamiento I/O

### 2. Cálculo de Overhead
En un sistema con:
- Context switch: 2ms
- Quantum RR: 20ms
- 10 procesos en ready queue

Calcule el overhead porcentual del scheduling.

**Solución:**
- Tiempo total por ciclo: 10×20ms + 10×2ms = 220ms
- Tiempo de overhead: 20ms
- Overhead %: (20/220)×100 = 9.09%

### 3. Optimización de Tamaño de Página
Para un sistema con:
- Direcciones de 32 bits
- Procesos promedio: 4MB
- Overhead deseado de tablas: <5%

Determine el tamaño óptimo de página.

**Análisis:**
- Página 1KB: 4MB/1KB = 4096 entradas × 4B = 16KB tabla (0.39%)
- Página 4KB: 4MB/4KB = 1024 entradas × 4B = 4KB tabla (0.098%) ✓
- Página 16KB: 4MB/16KB = 256 entradas × 4B = 1KB tabla (0.024%)

**Recomendación:** 4KB balancee overhead y fragmentación interna.

## Tendencias Futuras

**Scheduling:**
- Machine Learning para predicción de comportamiento
- Heterogeneous computing (CPU + GPU + NPU)
- Real-time constraints en sistemas generales

**Memoria:**
- Persistent memory (Intel Optane)
- Memory disaggregation en data centers
- Hardware-assisted virtualization

**Seguridad:**
- Speculative execution mitigations
- Memory encryption
- Capability-based addressing