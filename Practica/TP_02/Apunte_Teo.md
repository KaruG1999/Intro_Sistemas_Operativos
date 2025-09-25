# Administración de Procesos - Sistemas Operativos

## Tiempos de Proceso

**Métricas fundamentales:**
- **CPU (T_CPU)**: Tiempo efectivo que el proceso usa la CPU
- **Retorno (T_R)**: Tiempo total desde llegada hasta completar ejecución
- **Espera (T_E)**: Tiempo en el sistema sin ejecutarse (T_R - T_CPU)
- **Promedios (TPR y TPE)**: Tiempos promedio de retorno y espera del lote

## Algoritmos de Planificación

### FIFO (First Come, First Served)
- Selecciona el proceso más antiguo
- No favorece ningún tipo específico de proceso
- Los procesos CPU Bound terminan en su primera ráfaga
- Los procesos I/O Bound requieren múltiples ráfagas

### SJF (Shortest Job First)
- **No preemptivo** que selecciona el proceso con la ráfaga más corta
- Los procesos cortos se colocan delante de los largos
- **Problema**: Los procesos largos pueden sufrir *starvation* (inanición)

### Round Robin (RR)
- Política basada en un **quantum (Q)** de tiempo
- Quantum pequeño → overhead de context switch
- Quantum grande → comportamiento similar a FIFO
- Proceso expulsado va al final de la Ready Queue

**Variantes del Timer:**
- **Timer Variable**: Contador se reinicia a Q cada asignación (más utilizado)
- **Timer Fijo**: Contador se reinicia a Q solo cuando llega a 0

### Algoritmos con Prioridades
- Cada proceso tiene un valor de prioridad (menor valor = mayor prioridad)
- Una Ready Queue por nivel de prioridad
- **Problema**: Procesos de baja prioridad pueden sufrir starvation
- **Solución**: Aging o Penalty para cambiar prioridades dinámicamente
- Puede ser preemptivo o no preemptivo

### SRTF (Shortest Remaining Time First)
- Versión **preemptiva** de SJF
- Selecciona el proceso con menor tiempo restante
- **Favorece**: Procesos I/O Bound

## Planificación con I/O

### Características
- Ciclo de vida: uso de CPU + operaciones de I/O
- Cada dispositivo tiene su cola de procesos en espera
- I/O independiente de CPU (DMA, PCI) permite uso simultáneo

### Criterios de Desempate
1. Orden de llegada de los procesos
2. PID de los procesos
3. Mantener siempre la misma política

## Esquemas Avanzados

### Colas Multinivel
- Ready queue dividida en varias colas (similar a prioridades)
- Cada cola tiene su propio algoritmo (**planificador horizontal**)
- Algoritmo que planifica las colas (**planificador vertical**)
- **Retroalimentación**: Procesos pueden cambiar de cola

**Ejemplo típico:**
- Q0: RR con q=8
- Q1: RR con q=16  
- Q2: FCFS

**Comportamiento:**
- Procesos ingresan en Q0
- Si no terminan en el quantum, bajan a Q1
- Si no terminan en Q1, bajan a Q2
- **Beneficia**: Procesos CPU Bound
- **Problema**: Puede causar inanición en procesos I/O Bound

## Planificación con Múltiples Procesadores

### Criterios
- **Planificación temporal**: Qué proceso y durante cuánto tiempo
- **Planificación espacial**: En qué procesador ejecutar

### Conceptos Clave
- **Huella**: Estado que el proceso deja en la cache del procesador
- **Afinidad**: Preferencia de un proceso para ejecutar en un procesador específico

### Asignación de Procesos
- **Estática**: Afinidad fija proceso-CPU
- **Dinámica**: Balanceo de carga compartida

### Clasificaciones

**Por homogeneidad:**
- **Homogéneos**: Todas las CPUs son iguales
- **Heterogéneos**: Cada procesador con su cola, clock y algoritmo propios

**Por acoplamiento:**
- **Débilmente acoplados**: Cada CPU con memoria y canales propios
- **Fuertemente acoplados**: Comparten memoria y canales
- **Especializados**: CPUs generales + CPUs de uso específico

### Políticas
- **Tiempo compartido**: Cola global o cola local por procesador
- **Espacio compartido**: Organización en grupos (threads)

## Apunte clase 

- Cada proceso se coloca segun algún criterio -> First Fit, Worst Fit, Next Fit, Best Fit.
- Objetivo es crear la menor cantidad de fragmentacion de un proceso 
- Particiones fijas 

Técnica de páginación -> particiones fijas (páginas)
- Primero direccion -> 0
- Ult partición -> n   

Examen práctica
Traducir dim fisica a dim lógica 
Fallo página -> la página queda en disco swap 
Administrar espacio en disco
Fragmentación

Proceso -> páginas (128megas)
Memoria Ram -> marcos (128megas)
- El proceso se divide en páginas y esas páginas se almacenan en marcos 

Preg examen teorico
Cúal es la mayor fragmentacion posible con la páginacion? 
-> tamaño de pág - 1byte


1) Calcular lugar en marco(memoria) teniendo datos de página y usando tabla de páginación (y viceversa)
Con div saco el offset


