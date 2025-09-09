# Resumen: Anexo Evolución de los Sistemas Operativos

## Propósito de la Evolución de los S.O.

Los sistemas operativos evolucionan con tres objetivos principales:
- **Soportar nuevos tipos de hardware**
- **Brindar nuevos servicios**
- **Ofrecer mejoras y alternativas** a problemas existentes (planificación, manejo de memoria, etc.)

## Evolución Histórica

### 1. Procesamiento en Serie
**Características:**
- **No existía sistema operativo**
- Las máquinas se operaban desde una consola con luces, interruptores, dispositivos de entrada e impresoras
- **Problemas principales:**
  - **Planificación**: Alto nivel de especialización y costos elevados
  - **Configuración compleja**: Carga manual del compilador, código fuente, guardado del programa compilado, carga y linkeo

### 2. Sistemas por Lotes Sencillos (Batch)
**Características:**
- **Monitor Residente**: Software que controla la secuencia de eventos
- Los trabajos se agrupan y procesan en lotes
- Los programas retornan el control al monitor al finalizar
- **No hay interacción con el usuario** durante la ejecución
- Se utilizaban **tarjetas perforadas** para entrada de datos

**Ejemplo histórico**: Sistema IBM 1401 con unidad de procesamiento, lectora-perforadora de tarjetas 1402 y impresora 1403.

### 3. Limitaciones del Sistema Batch
**Problemas identificados:**
- **Baja utilización de CPU**: Los dispositivos de E/S eran mucho más lentos que el procesador
- **Tiempo de espera**: Ante instrucciones de E/S, el procesador permanecía ocioso
- **Ineficiencia**: Solo se podía continuar la ejecución después de completar la E/S

### 4. Multiprogramación
**Mejoras introducidas:**
- **Spooling de tareas**: Solapamiento entre E/S de una tarea y ejecución de otra
- **Flexibilidad en ejecución**: Las tareas ya no necesitaban ejecutarse en el orden de carga
- **Job scheduling**: Planificación de trabajos basada en prioridades
- **Múltiples tareas en memoria**: El S.O. mantiene varios programas cargados simultáneamente

**Funcionamiento:**
- Secuencia de programas según prioridad u orden de llegada
- Cuando un proceso requiere E/S, la CPU se asigna a otro proceso
- Después de completar una interrupción, el control puede o no retornar al programa original

**Organización:** Particiones de memoria para diferentes trabajos (Job A, Job B, Job C, S.O.)

### 5. Tiempo Compartido (Time Sharing)
**Características:**
- **Utiliza multiprogramación** para manejar múltiples trabajos interactivos
- **Tiempo del procesador compartido** entre múltiples trabajos
- **Acceso simultáneo**: Múltiples usuarios pueden acceder usando terminales
- **Quantum de tiempo**: Cada proceso usa la CPU por un período máximo, luego se asigna a otro proceso

## Línea de Tiempo de Sistemas Operativos (1955-2003)

El anexo incluye una línea temporal que muestra la evolución de diferentes familias de S.O.:

**Sistemas IBM:**
- IOCS, DOS/360, OS/360, MVS/370, MVS/XA, VS/ESA

**Sistemas Unix/Linux:**
- MULTICS, UNIX, UNIX V.7, System III/V, AIX, SUN OS, Solaris, BSD, Linux

**Sistemas Microsoft:**
- MS-DOS, Windows (3.0, 3.1, NT, 95/98, 2000, XP, Server 2003)

**Otros sistemas:**
- CP/M, OS/2, VMS, POSIX

## Contexto Académico

**Este anexo es claramente material de repaso y contextualización histórica.** Su propósito es:

1. **Proporcionar perspectiva histórica** sobre cómo surgieron los conceptos que se estudiarán en la materia
2. **Mostrar la evolución natural** de los problemas y sus soluciones en sistemas operativos
3. **Contextualizar** por qué existen ciertas características en los S.O. modernos
4. **Preparar conceptualmente** para entender la motivación detrás de técnicas como multiprogramación, scheduling, etc.

## Relevancia para el Curso

Este material sirve como **fundamento conceptual** para entender:
- **Por qué** se desarrollaron técnicas como la multiprogramación
- **Cómo** los problemas de eficiencia llevaron a innovaciones
- **Qué** motivó el desarrollo de sistemas de tiempo compartido
- **De dónde** vienen los conceptos modernos de scheduling y gestión de procesos

Es un anexo **complementario** que proporciona el trasfondo histórico necesario para apreciar la complejidad y evolución de los sistemas operativos modernos.