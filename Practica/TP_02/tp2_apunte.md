# Apunte TP2 - Sistemas Operativos
## Scheduling y Administración de Memoria

### 1. Conceptos Fundamentales

#### Sistema Operativo
Programa que actúa como intermediario entre el usuario y el hardware, gestionando recursos (CPU, memoria, dispositivos) y proporcionando servicios.

#### Estados de un Proceso
```
[NEW] → [READY] → [RUNNING] → [TERMINATED]
             ↗         ↓
           [BLOCKED] ←
```

#### Tiempos Importantes
- **TR (Tiempo Retorno)**: Tiempo total desde llegada hasta finalización
- **TE (Tiempo Espera)**: Tiempo esperando en Ready Queue
- **TPR**: Promedio de tiempos de retorno
- **TPE**: Promedio de tiempos de espera

### 2. Algoritmos de Scheduling

#### FCFS (First Come First Served)
- **Funcionamiento**: Atiende procesos por orden de llegada
- **Ventajas**: Simple, sin starvation
- **Desventajas**: Efecto convoy, TR alto para procesos cortos
- **Uso**: Sistemas batch

#### SJF (Shortest Job First)
- **Funcionamiento**: Selecciona proceso con ráfaga más corta
- **Ventajas**: Óptimo para minimizar tiempo promedio
- **Desventajas**: Starvation de procesos largos
- **Uso**: Sistemas batch con trabajos conocidos

#### Round Robin
- **Funcionamiento**: Asigna quantum fijo rotativo
- **Parámetro**: Quantum (Q)
- **Ventajas**: Equitativo, buena respuesta
- **Desventajas**: Overhead de context switch
- **Uso**: Sistemas interactivos

#### Prioridades
- **Funcionamiento**: Selecciona proceso con mayor prioridad
- **Variantes**: Apropiativo/No apropiativo
- **Problema**: Posible starvation
- **Solución**: Aging (envejecimiento)

### 3. Estimación de Ráfagas CPU

#### Fórmula 1 (Media simple)
```
Si = (T1 + T2 + ... + Ti-1) / (i-1)
```

#### Fórmula 2 (Media exponencial)
```
Si+1 = α × Ti + (1-α) × Si
```
- **α cercano a 1**: Mayor peso a casos recientes
- **α cercano a 0**: Mayor peso a casos históricos

### 4. Colas Multinivel

#### Organización
- **Cola Alta**: Procesos interactivos (RR)
- **Cola Media**: Procesos mixtos (RR)
- **Cola Baja**: Procesos batch (FCFS)

#### Con Realimentación
- Procesos pueden moverse entre colas
- Penaliza procesos CPU-intensivos
- Favorece procesos I/O-bound

### 5. Direcciones de Memoria

#### Transformación Básica
```
Dirección Física = Dirección Base + Dirección Lógica
```
*(Válida si Dirección Lógica < Tamaño Partición)*

### 6. Técnicas de Administración de Memoria

#### Particiones Fijas
- **Ventajas**: Simple implementación
- **Desventajas**: Fragmentación interna
- **Técnica**: Best Fit/First Fit/Worst Fit

#### Particiones Dinámicas
- **Ventajas**: Mejor uso de memoria
- **Desventajas**: Fragmentación externa
- **Solución**: Compactación

#### Segmentación
- **Dirección**: (segmento, desplazamiento)
- **Traducción**: Base[s] + desplazamiento
- **Validación**: desplazamiento < Límite[s]
- **Fragmentación**: Solo externa

#### Paginación
- **Dirección Lógica**: Página × Tamaño_Página + Offset
- **Dirección Física**: Marco × Tamaño_Página + Offset
- **Fragmentación**: Solo interna (última página)

### 7. Fórmulas Importantes

#### Cálculos de Paginación
```
Bits desplazamiento = log₂(Tamaño_Página)
Bits página = Bits_totales - Bits_desplazamiento
Páginas necesarias = ⌈Tamaño_Proceso ÷ Tamaño_Página⌉
Fragmentación interna = (Páginas × Tamaño_Página) - Tamaño_Proceso
```

#### Ejemplo de Traducción Paginación
- Sistema: Página = 2048 bytes, Dirección = 5120
- Página = 5120 ÷ 2048 = 2 (parte entera)
- Offset = 5120 mod 2048 = 1024
- Si Marco[2] = 9: Dirección Física = 9 × 2048 + 1024 = 19456

### 8. Fragmentación

#### Interna
Espacio desperdiciado dentro de una partición/página asignada

#### Externa  
Espacios libres muy pequeños entre particiones para ser útiles

### 9. Técnicas de Asignación

#### First Fit
Asigna primera partición suficientemente grande
- **Ventaja**: Rápido
- **Desventaja**: Fragmentación al inicio

#### Best Fit
Asigna partición más pequeña suficiente
- **Ventaja**: Minimiza desperdicio
- **Desventaja**: Lento, fragmentación externa

#### Worst Fit
Asigna partición más grande disponible
- **Ventaja**: Deja espacios grandes
- **Desventaja**: Desperdicia memoria

### 10. Starvation (Inanición)

#### Definición
Situación donde un proceso espera indefinidamente sin poder ejecutarse

#### Algoritmos que la provocan
- SJF: Procesos largos
- Prioridades: Procesos de baja prioridad
- SRTF: Similar a SJF

#### Solución
**Aging**: Incrementar gradualmente la prioridad de procesos que esperan mucho tiempo

### 11. Observaciones y Correcciones

#### Quantum en Round Robin
- **Q muy pequeño**: Mucho overhead de context switch
- **Q muy grande**: Comportamiento similar a FCFS
- **Q óptimo**: Balance entre respuesta y overhead (típicamente 10-100ms)

#### VRR (Virtual Round Robin)
Mejora para procesos I/O-bound:
- Cola auxiliar con prioridad
- Tiempo restante del quantum anterior
- Evita penalizar procesos que se bloquean por I/O

#### Tablas de Páginas Multinivel
Para espacios de direcciones grandes:
- **1 nivel**: Tabla muy grande
- **2+ niveles**: Menos memoria, múltiples accesos
- **Invertidas**: Tamaño fijo, búsqueda más lenta

### 12. Segmentación Paginada

#### Funcionamiento
Combina segmentación (organización lógica) con paginación (gestión física)

#### Dirección
```
(segmento, página, desplazamiento)
```

#### Traducción
1. Segmento → Tabla de páginas
2. Página → Marco físico
3. Marco + desplazamiento → Dirección física

### 13. Consideraciones de Diseño

#### Para Sistemas Interactivos
- Round Robin con quantum pequeño
- Colas multinivel con prioridad a I/O-bound
- Tiempo de respuesta < 1 segundo

#### Para Sistemas Batch
- SJF/FCFS para eficiencia
- Particiones fijas para simplicidad
- Optimizar throughput sobre respuesta

#### Para Sistemas de Tiempo Real
- Prioridades estrictas
- Scheduling apropiativo
- Garantías de tiempo acotado

### 14. Errores Comunes

#### En Scheduling
- Olvidar considerar I/O en diagramas de Gantt
- No aplicar correctamente la apropiación
- Confundir tiempo de retorno con tiempo de espera

#### En Memoria
- Errores en cálculos de traducción de direcciones
- No validar límites en segmentación
- Confundir fragmentación interna con externa

#### En Estimación
- No aplicar correctamente las fórmulas exponenciales
- Usar valores incorrectos de α
- No considerar el valor inicial S₁

### 15. Tips para Exámenes

#### Diagramas de Gantt
1. Marcar tiempos de llegada
2. Aplicar algoritmo paso a paso
3. Considerar I/O si está presente
4. Verificar apropiación cuando corresponde

#### Cálculos de Tiempo
```
TR = Tiempo_finalización - Tiempo_llegada
TE = TR - Tiempo_CPU_usado
TPR = Σ(TR) / Número_procesos
TPE = Σ(TE) / Número_procesos
```

#### Traducción de Direcciones
1. Identificar tipo (paginación/segmentación)
2. Extraer componentes de la dirección
3. Validar límites
4. Aplicar fórmula de traducción
5. Verificar resultado

### 16. Comparación de Técnicas

| Técnica | Fragmentación | Complejidad | Flexibilidad |
|---------|---------------|-------------|--------------|
| Part. Fijas | Interna | Baja | Baja |
| Part. Dinámicas | Externa | Media | Media |
| Segmentación | Externa | Alta | Alta |
| Paginación | Interna | Alta | Media |

La elección depende de los requisitos específicos del sistema: simplicidad vs eficiencia, tiempo real vs interactivo, recursos limitados vs abundantes.