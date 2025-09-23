# Memoria Virtual - Síntesis y Conceptos Clave

## Introducción y Motivación

La **memoria virtual** permite que un proceso no necesite tener toda su imagen cargada en memoria física simultáneamente. Solo se cargan las "piezas" necesarias en cada momento.

### ¿Por qué es útil?
- Rutinas que se ejecutan una sola vez o nunca
- Partes del programa que no vuelven a ejecutarse
- Regiones de memoria dinámicamente alocadas y luego liberadas

## Conceptos Fundamentales

### Conjunto Residente (Working Set)
Porción del espacio de direcciones del proceso que **actualmente** se encuentra en memoria física.

### Ventajas de la Memoria Virtual
1. **Más procesos en memoria**: Solo se cargan secciones necesarias
2. **Mayor probabilidad de procesos Ready**: Más procesos disponibles para ejecución
3. **Procesos más grandes que la memoria física**: El usuario no se preocupa por limitaciones de tamaño
4. **Eficiencia**: No todo debe subirse a RAM, solo lo requerido

## Requisitos Técnicos

Para implementar memoria virtual se necesita:

1. **Hardware con soporte de paginación por demanda**
2. **Memoria secundaria** (disco) para área de intercambio (swap)
3. **SO capaz** de manejar movimiento de páginas entre memoria principal y secundaria

## Memoria Virtual con Paginación

### Bits de Control en Tabla de Páginas
- **Bit V (Valid)**: Indica si la página está en memoria
  - Activado/desactivado por el SO
  - Consultado por el HW
- **Bit M (Modified/Dirty)**: Indica si la página fue modificada
  - Activado/desactivado por el HW
  - Consultado por el SO para decisiones de escritura a disco

### Formato de Entrada x86 (32 bits)
```
Bits: 31-12  11  10  9   8   7   6   5   4   3   2   1   0
     [PFN  ] V   U   P  Cw  Gi  L   D   A  Cd  Wt  O   W
```

**Explicación de cada campo**:
- **PFN (31-12)**: Page Frame Number - Número del marco físico donde está la página
- **V (11)**: Valid - Bit de validez, indica si la página está presente en memoria
- **U (10)**: User - Bit de acceso de usuario (0=kernel, 1=usuario)
- **P (9)**: Present - Bit de presencia (similar a Valid)
- **Cw (8)**: Copy on Write - Para optimización de memoria compartida
- **Gi (7)**: Global - Página global (no se invalida en cambios de contexto)
- **L (6)**: Large page - Indica si es página de 4MB en lugar de 4KB
- **D (5)**: Dirty (Modified) - Bit M, indica si la página fue modificada
- **A (4)**: Accessed - Indica si la página fue accedida recientemente
- **Cd (3)**: Cache Disabled - Deshabilita cache para esta página
- **Wt (2)**: Write Through - Política de escritura de cache
- **O (1)**: Owner - Propietario (Kernel o User Mode)
- **W (0)**: Writable - Bit de escritura, indica si la página es escribible

## Fallo de Páginas (Page Fault)

### ¿Cuándo ocurre?
Cuando el proceso intenta usar una dirección de una página **no cargada en memoria** (Bit V=0).

### Proceso de Manejo:
1. **HW detecta** la situación y genera trap al SO
2. **SO coloca** el proceso en estado "Blocked" (opcional)
3. **SO busca** un frame libre en memoria
4. **Operación E/S** al disco para cargar la página necesaria
5. **SO puede asignar CPU** a otro proceso durante la E/S
6. **Cuando E/S finaliza**: SO actualiza tabla de páginas y proceso vuelve a Ready
7. **Reintentar** la instrucción que causó el fallo

### Performance del Sistema

**Tasa de Page Faults**: 0 ≤ p ≤ 1
- p = 0: No hay page faults
- p = 1: Cada acceso genera page fault

**Effective Access Time (EAT)**:
```
EAT = (1-p) × memory_access + p × (page_fault_overhead + [swap_page_out] + swap_page_in + restart_overhead)
```

> **Objetivo**: Trabajar para que p tienda a 0

## Organización de Tablas de Páginas

### 1. Tabla de 1 Nivel
**Problema con direcciones de 64 bits**:
- PTEs máximas: 2⁵²
- Tamaño tabla: Más de 16.000GB por proceso

### 2. Tabla de 2 Niveles (Multinivel)
**Ventajas**:
- Divide tabla lineal en múltiples tablas más pequeñas
- Solo se carga parcialidad necesaria
- Esquema de direccionamiento indirecto

**Ejemplo**: 3 páginas referencian 12MB de espacio (4MB datos + 4MB texto + 4MB stack)

### 3. Tabla Invertida
**Características**:
- Una entrada por cada marco físico
- Una sola tabla para todo el sistema
- Usa técnica de HASH para búsqueda
- Usada en PowerPC, UltraSPARC, IA-64

## Translation Lookaside Buffer (TLB)

### Problema
Cada referencia virtual puede causar 2+ accesos a memoria física:
1. Obtener entrada en tabla de páginas
2. Obtener los datos

### Solución: TLB
**Cache de alta velocidad** que almacena entradas de páginas usadas recientemente.

### Funcionamiento:
1. **TLB Hit**: Se obtiene frame directamente
2. **TLB Miss**: Se busca en tabla de páginas del proceso
3. **Page Fault**: Si página no está en memoria
4. **Actualización**: TLB incluye nueva entrada

> **Importante**: Cambio de contexto invalida entradas TLB
> 
> **Nota**: TLB es parte de la MMU

## Asignación de Marcos

### Asignación Fija
- **Equitativa**: Misma cantidad para cada proceso
- **Proporcional**: Acorde al tamaño del proceso

**Fórmula Proporcional**:
```
aᵢ = (sᵢ/S) × m
```
Donde: sᵢ = tamaño proceso, S = suma total tamaños, m = marcos totales

### Asignación Dinámica
El número de marcos para cada proceso **varía** según necesidades.

> **En práctica real**: El kernel utiliza ambas maneras de asignación

## Algoritmos de Reemplazo de Páginas

### Objetivo
Seleccionar página víctima que **no será referenciada en futuro próximo**.

### Algoritmos Principales:
1. **ÓPTIMO**: Teórico, conoce referencias futuras
2. **FIFO**: Más sencillo, selecciona víctima/frames como cola circular
3. **LRU**: Requiere soporte HW, favorece páginas recientes
4. **Segunda Chance**: Avance del FIFO
5. **NRU**: Utiliza bits R y M

## Alcance del Reemplazo

### Reemplazo Global
- Fallo puede reemplazar página de **cualquier proceso**
- Proceso alta prioridad puede tomar frames de baja prioridad
- SO no controla tasa individual de page-faults

### Reemplazo Local
- Solo puede reemplazar **sus propias páginas**
- Mantiene conjunto residente fijo
- SO determina tasa de page-faults por proceso

## Políticas de Manejo de Memoria Virtual

1. **Cuándo cargar**: Por demanda vs. prepaginación
2. **Dónde ubicar**: Best-fit, first-fit, etc.
3. **Elección de víctima**: Algoritmos de reemplazo
4. **Cuántas páginas traer**: Una vs. múltiples
5. **Cuándo escribir a disco**: Páginas modificadas
6. **Número de procesos**: Control de multiprogramación

## Tamaño de Página: Trade-offs

### Páginas Pequeñas
✅ Menor fragmentación interna  
❌ Tablas de páginas más grandes  
✅ Más páginas en memoria  

### Páginas Grandes
❌ Mayor fragmentación interna  
✅ Transferencia más eficiente desde disco  
✅ Menor overhead de tabla de páginas

### Relación con E/S - Ejemplo Práctico
**Configuración**: Vel. transferencia 2 Mb/s, Latencia 8ms, Búsqueda 20ms

- **Página 512 bytes**: 28.2ms total (solo 1% transferencia real)
- **Página 1024 bytes**: 28.4ms total (mayor eficiencia)

---

## Términos Importantes - Glosario

- **Conjunto Residente**: Páginas del proceso actualmente en memoria física
- **Page Fault**: Interrupción cuando se accede a página no cargada
- **TLB**: Cache de traducción de direcciones, parte de la MMU
- **Bit V**: Bit de validez, indica si página está en memoria
- **Bit M**: Bit de modificación, indica si página fue alterada
- **Frame/Marco**: Bloque físico de memoria donde se cargan páginas
- **Swap**: Área de intercambio en disco para páginas no residentes
- **MMU**: Memory Management Unit, hardware de gestión de memoria

---

> **Recordatorio**: La MMU es parte del hardware y trabaja junto con el kernel para hacer eficiente el manejo de memoria virtual. El kernel se focaliza en lo que debe mantener en RAM y administra eficientemente los recursos.