# Introducción a los Sistemas Operativos
## Procesos - Parte II

**Palabras clave:** Procesos, Estados, Scheduler, Long Term, Medium Term, Short Term

---

## 1. Estados de un Proceso

### 1.1 Modelo Mínimo de Estados

Los procesos en un sistema operativo pasan por diferentes estados durante su ciclo de vida:

- **New (Nuevo):** Proceso en creación
- **Ready (Listo):** Proceso cargado en memoria, esperando CPU
- **Running (Ejecutándose):** Proceso usando actualmente la CPU
- **Waiting/Blocked (En espera/Bloqueado):** Proceso esperando un evento
- **Terminated (Terminado):** Proceso finalizado

---

## 2. Colas en la Planificación de Procesos

### 2.1 Uso de PCB para Planificación
- El SO utiliza la **PCB de cada proceso** como abstracción del mismo
- Las **PCB se enlazan en colas** siguiendo un orden determinado

### 2.2 Tipos de Colas

#### Cola de Trabajos o Procesos
- Contiene **todas las PCB** de procesos en el sistema

#### Cola de Procesos Listos (Ready Queue)
- PCB de procesos **residentes en memoria principal**
- Esperando para ejecutarse

#### Cola de Dispositivos
- PCB de procesos **esperando por un dispositivo** de I/O

### 2.3 Flujo de Colas
```
Ready Queue → Dispatch → CPU
     ↑           ↓
Time out    I/O Request
     ↑           ↓
   CPU ←   I/O Queue → I/O Device
           (I/O wait)      ↓
              ↑      I/O occurs
              ←──────────────
```

---

## 3. Módulos de la Planificación

### 3.1 Definición
> **Módulos de planificación:** Son módulos (SW) del Kernel que realizan distintas tareas asociadas a la planificación.

### 3.2 Eventos que los Activan
- **Creación/Terminación** de procesos
- **Eventos de sincronización** o de E/S
- **Finalización de lapso** de tiempo
- Otros eventos del sistema

### 3.3 Tipos de Schedulers
**Su nombre proviene de la frecuencia de ejecución:**

#### 3.3.1 Long Term Scheduler (Largo Plazo)
- **Función:** Controla el **grado de multiprogramación**
- **Decisión:** Cantidad de procesos en memoria
- **Puede no existir:** Absorber esta tarea el de short term

#### 3.3.2 Short Term Scheduler (Corto Plazo)
- **Función:** Determina **cuál proceso listo** se ejecutará a continuación
- **Ambiente:** Multiprogramado
- **Implementa:** Algoritmo de planificación

#### 3.3.3 Medium Term Scheduler (Mediano Plazo)
- **Función:** Reduce el grado de multiprogramación si es necesario
- **Acción:** Saca temporalmente de memoria los procesos necesarios
- **Términos:** Swap out (sacar), swap in (volver a memoria)

### 3.4 Otros Módulos

#### Dispatcher
- **Función:** Hace cambio de contexto y cambio de modo
- **Acción:** "Despacha" el proceso elegido por Short Term
- **Resultado:** Salta a la instrucción a ejecutar

#### Loader
- **Función:** Carga en memoria el proceso elegido por Long Term

---

## 4. Comportamiento de los Procesos

### 4.1 Patrón de Ejecución
> **Los procesos alternan ráfagas de CPU y de I/O**

### 4.2 Tipos de Procesos

#### CPU-bound
- **Característica:** Mayor parte del tiempo utilizando la CPU
- **Comportamiento:** Pocas operaciones de I/O, cálculos intensivos

#### I/O-bound
- **Característica:** Mayor parte del tiempo esperando por I/O
- **Comportamiento:** Frecuentes operaciones de entrada/salida

### 4.3 Consideración Importante
- **Velocidad:** La CPU es **mucho más rápida** que los dispositivos de E/S
- **Estrategia:** Atender rápidamente procesos I/O-bound para:
  - Mantener el dispositivo ocupado
  - Aprovechar la CPU para procesos CPU-bound

---

## 5. Algoritmos de Planificación

### 5.1 Clasificación por Control

#### Algoritmos Apropiativos (Preemptive)
- **Característica:** Existen situaciones que **expulsan al proceso** de la CPU
- **Control:** El SO puede quitar la CPU al proceso

#### Algoritmos No Apropiativos (Non-preemptive)
- **Característica:** Los procesos se ejecutan hasta que **abandonan voluntariamente** la CPU
- **Situaciones de abandono:**
  - Termina (syscall exit)
  - Se bloquea voluntariamente (syscall wait, sleep)
  - Solicita operación de E/S bloqueante (syscall read, write)
- **Restricción:** No hay decisiones durante interrupciones de reloj

### 5.2 Metas Generales
- **Equidad:** Otorgar parte justa de CPU a cada proceso
- **Balance:** Mantener ocupadas todas las partes del sistema

### 5.3 Categorías por Ambiente

#### 5.3.1 Procesos Batch (Por Lotes)

**Características:**
- **No hay usuarios** esperando respuesta en terminal
- Se pueden usar algoritmos **no apropiativos**

**Metas:**
- **Rendimiento:** Maximizar trabajos por hora
- **Tiempo de Retorno:** Minimizar tiempo entre comienzo y finalización
- **Uso de CPU:** Mantener CPU ocupada el mayor tiempo posible

**Ejemplos de Algoritmos:**
- **FCFS** - First Come First Served
- **SJF** - Shortest Job First

#### 5.3.2 Procesos Interactivos

**Características:**
- **Interacción con usuarios** y servidores
- **Necesarios algoritmos apropiativos** para evitar acaparamiento de CPU

**Metas:**
- **Tiempo de Respuesta:** Responder a peticiones con rapidez
- **Proporcionalidad:** Cumplir expectativas de usuarios
  - *Ejemplo: Si el usuario pone STOP al reproductor, que la música pare rápidamente*

**Ejemplos de Algoritmos:**
- **Round Robin**
- **Prioridades**
- **Colas Multinivel**
- **SRTF** - Shortest Remaining Time First

#### 5.3.3 Procesos en Tiempo Real
- Requieren **garantías temporales estrictas**
- **Deadlines críticos** que deben cumplirse

---

## 6. Política vs. Mecanismo

### 6.1 Concepto
- **Necesidad:** Situaciones donde la planificación debe comportarse diferente
- **Solución:** Algoritmo de planificación **parametrizado**

### 6.2 Separación de Responsabilidades
- **Kernel:** Implementa el **mecanismo**
- **Usuario/Proceso/Administrador:** Determina la **política** usando parámetros

### 6.3 Ejemplo Práctico
- **Mecanismo:** Algoritmo de planificación por prioridades
- **Política:** System call que permite modificar prioridad (comando `nice`)
- **Aplicación:** Un proceso puede determinar prioridades de sus procesos hijos según importancia

---

## 7. Estados Detallados de Procesos

### 7.1 Estado New (Nuevo)
- **Origen:** Usuario "dispara" el proceso o un proceso crea otro (proceso padre)
- **Acción:** Se crean estructuras asociadas
- **Ubicación:** Cola de procesos, esperando ser cargado en memoria

### 7.2 Estado Ready (Listo)
- **Llegada:** Scheduler de largo plazo eligió el proceso para cargarlo
- **Característica:** Solo necesita que se le **asigne CPU**
- **Ubicación:** Cola de procesos listos (ready queue)

### 7.3 Estado Running (En Ejecución)
- **Llegada:** Scheduler de corto plazo lo eligió
- **Duración:** Hasta que:
  - Termine el período asignado (quantum/time slice)
  - Termine el proceso
  - Necesite realizar operación de E/S

### 7.4 Estado Waiting (En Espera)
- **Causa:** Necesita que se cumpla un **evento esperado**
- **Eventos típicos:**
  - Terminación de E/S solicitada
  - Llegada de señal de otro proceso
- **Estado:** Sigue en memoria, pero **sin CPU**
- **Transición:** Al cumplirse el evento, pasa a Ready

---

## 8. Transiciones entre Estados

### 8.1 Transiciones Básicas

#### New → Ready
- **Acción:** Scheduler de largo plazo elige el proceso
- **Mecanismo:** Loader carga el programa en memoria

#### Ready → Running
- **Acción:** Scheduler de corto plazo elige el proceso
- **Mecanismo:** Dispatcher asigna la CPU

#### Running → Waiting
- **Causa:** El proceso "se pone a dormir"
- **Motivo:** Esperando por un evento

#### Waiting → Ready
- **Causa:** Terminó la espera
- **Resultado:** Compite nuevamente por CPU

### 8.2 Caso Especial: Running → Ready

**Situación:**
- Proceso termina su **quantum** sin necesitar interrupción por evento
- Pasa a Ready para **competir nuevamente** por CPU
- **No está esperando** por ningún evento

**Características:**
- Proceso **expulsado contra su voluntad**
- Se da en **algoritmos apropiativos**

---

## 9. Diagrama Completo con Swapping

### 9.1 Estados Extendidos

**1. Ejecución en modo usuario**
**2. Ejecución en modo kernel**
**3. Listo para ejecutar** (cuando sea elegido)
**4. En espera** en memoria principal
**5. Listo, pero swappeado** (debe volver a memoria antes de ejecutar)
**6. En espera** en memoria secundaria
**7. Retornando de kernel a user** (pero kernel se apropia para context switch)
**8. Recién creado** (existe, pero no listo ni dormido)
**9. Estado zombie** (ejecutó exit, registra datos de uso, estado final)

### 9.2 Procesos Swappeados
- **Propósito:** Medium term scheduler reduce multiprogramación
- **Acción:** Saca temporalmente procesos de memoria
- **Beneficio:** Mantiene equilibrio del sistema

---

## Resumen de Schedulers

| Scheduler | Frecuencia | Función Principal | Decisión |
|-----------|------------|------------------|----------|
| **Long Term** | Segundos/Minutos | Controlar multiprogramación | ¿Qué procesos cargar en memoria? |
| **Short Term** | Milisegundos | Asignar CPU | ¿Qué proceso ejecutar ahora? |
| **Medium Term** | Segundos | Balance de memoria | ¿Qué procesos swappear? |

---

## Referencias
- William Stallings - Sistemas Operativos: Aspectos internos y principios de diseño
- Silberschatz - Operating Systems Concepts