# Introducción a los Sistemas Operativos
## Procesos - Parte I

**Palabras clave:** Procesos, PCB, Stack, Contexto, Espacio de Direcciones

---

## 1. Definición de Proceso

### 1.1 Concepto Básico
> **Proceso:** Es un **programa en ejecución**

**Términos sinónimos:** Tarea, job y proceso

### 1.2 Diferencias: Programa vs. Proceso

| Programa | Proceso |
|----------|---------|
| **Estático** | **Dinámico** |
| No tiene program counter | Tiene program counter |
| Existe desde que se edita hasta que se borra | Su ciclo de vida va desde que se solicita ejecutar hasta que termina |

---

## 2. El Modelo de Proceso

### 2.1 Multiprogramación
- **Modelo conceptual:** 4 procesos secuenciales e independientes
- **Restricción:** Solo un proceso se encuentra activo en cualquier instante (si tenemos una sola CPU)
- **Abstracción:** Los procesos parecen ejecutarse simultáneamente, pero en realidad se alternan el uso de la CPU

---

## 3. Componentes de un Proceso

Un proceso (para poder ejecutarse) incluye **como mínimo:**

### 3.1 Sección de Código (Texto)
- Contiene las instrucciones del programa
- Es de solo lectura durante la ejecución

### 3.2 Sección de Datos
- Variables globales del programa
- Datos inicializados y no inicializados

### 3.3 Stack(s)
- **Datos temporarios:** parámetros, variables temporales y direcciones de retorno
- **Cantidad:** Un proceso cuenta con 1 o más stacks
- **Tipos:** En general - modo Usuario y modo Kernel

---

## 4. Stacks - Detalle

### 4.1 Características
- Se crean **automáticamente**
- Su tamaño se **ajusta en tiempo de ejecución (run-time)**
- Formado por **stack frames**

### 4.2 Stack Frames
- **Push:** Se añaden al llamar a una rutina
- **Pop:** Se eliminan cuando se retorna de la rutina
- **Contenido:**
  - Parámetros de la rutina (variables locales)
  - Datos necesarios para recuperar el stack frame anterior
  - Contador de programa
  - Valor del stack pointer en el momento del llamado

---

## 5. Atributos de un Proceso

### 5.1 Información de Identificación
- **PID:** Identificación del proceso
- **PPID:** Identificación del proceso padre
- **Usuario:** Identificación del usuario que lo "disparó"
- **Grupo:** En estructura de grupos, grupo que lo disparó

### 5.2 Información del Entorno
- **Terminal:** En ambientes multiusuario, desde qué terminal se ejecutó
- **Ejecutor:** Quién lo ejecutó

---

## 6. Process Control Block (PCB)

### 6.1 Definición
> **PCB:** Estructura de datos asociada al proceso (abstracción)

### 6.2 Características
- **Unicidad:** Existe una por proceso
- **Ciclo de vida:**
  - Es lo **primero** que se crea cuando se crea un proceso
  - Es lo **último** que se borra cuando termina

### 6.3 Información Contenida
- **Identificación:** PID, PPID, etc.
- **Registros CPU:** Valores de registros (PC, AC, etc.)
- **Planificación:** Estado, prioridad, tiempo consumido, etc.
- **Memoria:** Ubicación (representación) en memoria
- **Accounting:** Información de contabilidad
- **E/S:** Estado, operaciones pendientes, etc.

### 6.4 Campos Comunes del PCB
```
- Process ID (PID)
- Parent Process ID (PPID)
- User ID
- Estado del proceso
- Contador de programa (PC)
- Registros de la CPU
- Información de planificación
- Información de gestión de memoria
- Información de E/S
- Información de contabilidad
```

---

## 7. Espacio de Direcciones

### 7.1 Definición
> **Espacio de direcciones de un proceso:** Es el conjunto de direcciones de memoria que ocupa el proceso

### 7.2 Incluye
- **Stack**
- **Código (text)**
- **Datos**

### 7.3 NO Incluye
- PCB del proceso
- Tablas asociadas al proceso

### 7.4 Restricciones de Acceso
- **Modo usuario:** El proceso puede acceder **solo a su espacio de direcciones**
- **Modo kernel:** Se puede acceder a:
  - Estructuras internas (PCB del proceso)
  - Espacios de direcciones de otros procesos

---

## 8. Contexto de un Proceso

### 8.1 Definición
> **Contexto:** Incluye toda la información que el SO necesita para administrar el proceso, y la CPU para ejecutarlo correctamente.

### 8.2 Componentes del Contexto
- **Registros de CPU** (incluyendo el contador de programa)
- **Prioridad del proceso**
- **Estado de E/S** (si tiene E/S pendientes)
- **Información de planificación**
- **Estado de memoria**

---

## 9. Cambio de Contexto (Context Switch)

### 9.1 Definición
> **Context Switch:** Se produce cuando la CPU cambia de un proceso a otro.

### 9.2 Proceso
1. **Resguardar** el contexto del proceso saliente (pasa a espera)
2. **Cargar** el contexto del nuevo proceso
3. **Comenzar** desde la instrucción siguiente a la última ejecutada en dicho contexto

### 9.3 Características
- Es **tiempo no productivo** de CPU
- El tiempo que consume **depende del soporte de hardware**
- Es una operación crítica para el rendimiento del sistema

### 9.4 Ejemplo de Cambio de Contexto
```
Proceso A ejecutándose → Interrupción → 
Guardar contexto de A → Cargar contexto de B → 
Proceso B ejecutándose → Interrupción → 
Guardar contexto de B → Cargar contexto de A → 
Proceso A continúa ejecutándose
```

---

## 10. Kernel del Sistema Operativo

### 10.1 Definición
> **Kernel:** Es un conjunto de módulos de software que se ejecuta en el procesador como cualquier otro proceso.

### 10.2 Pregunta Fundamental
**¿El kernel es un proceso? Y de ser así, ¿quién lo controla?**

### 10.3 Diferentes Enfoques de Diseño

---

## 11. Enfoque 1 - Kernel como Entidad Independiente

### 11.1 Características Principales
- El Kernel se ejecuta **fuera de todo proceso**
- Arquitectura utilizada por los **primeros SO**
- El Kernel **NO es un proceso**
- Se ejecuta como **entidad independiente en modo privilegiado**

### 11.2 Funcionamiento
1. Cuando un proceso es "interrumpido" o realiza una **System Call**
2. El contexto del proceso se **guarda**
3. El control se **pasa al Kernel** del sistema operativo
4. El Kernel procesa la solicitud
5. **Devuelve el control** al proceso (o a otro diferente)

### 11.3 Recursos del Kernel
- **Memoria propia:** El Kernel tiene su propia región de memoria
- **Stack propio:** El Kernel tiene su propio Stack
- **Concepto de proceso:** Solo se asocia a programas de usuario

---

## 12. Enfoque 2 - Kernel "Dentro" del Proceso

### 12.1 Características Principales
- El **código del Kernel** se encuentra dentro del espacio de direcciones de **cada proceso**
- El Kernel se ejecuta en el **mismo contexto** que algún proceso de usuario
- El Kernel se puede ver como una **colección de rutinas** que el proceso utiliza

### 12.2 Estructura del Proceso
**Dentro de un proceso se encuentra:**
- **Código del programa** (user)
- **Código de los módulos de SW del SO** (kernel)

### 12.3 Stacks
- Cada proceso tiene **su propio stack**:
  - **Uno en modo usuario**
  - **Otro en modo kernel**

### 12.4 Modos de Ejecución
- **Proceso:** Se ejecuta en **Modo Usuario**
- **Kernel del SO:** Se ejecuta en **Modo Kernel** (cambio de modo)

### 12.5 Compartición y Eficiencia
- **Código compartido:** El código del Kernel es compartido por todos los procesos
- **Atención de interrupciones:** Cada interrupción (incluyendo System Calls) es atendida en el contexto del proceso que se encontraba en ejecución
  - **Pero en modo Kernel!**
  - Se pasa a este modo **sin necesidad de cambio de contexto completo**

### 12.6 Ventajas de Rendimiento
- Si el SO determina que el proceso debe seguir ejecutándose después de atender la interrupción:
  - Cambia a **modo usuario**
  - **Devuelve el control**
  - Es **más económico y performante**

---

## Resumen Comparativo de Enfoques

| Aspecto | Enfoque 1: Independiente | Enfoque 2: Integrado |
|---------|-------------------------|---------------------|
| **Ubicación del Kernel** | Fuera de procesos | Dentro de cada proceso |
| **Memoria** | Región propia | Compartida con proceso |
| **Stack** | Propio del kernel | Uno por modo (user/kernel) |
| **Cambio de contexto** | Completo | Parcial (solo cambio de modo) |
| **Rendimiento** | Menor | Mayor |
| **Complejidad** | Menor | Mayor |

---

## Referencias
- Andrew S. Tanenbaum - Sistemas Operativos Modernos
- William Stallings - Sistemas Operativos: Aspectos internos y principios de diseño