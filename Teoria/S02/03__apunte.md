# Sistemas Operativos - Apuntes Teóricos

## 📚 Introducción

**Fuentes:** William Stallings (Sistemas Operativos: Aspectos internos y principios de diseño) y Silberschatz, Galvin, Gagne (Conceptos de sistemas operativos)

---

## 🎯 Funciones Principales de un Sistema Operativo

Un Sistema Operativo cumple tres funciones esenciales:

### 1. Abstracciones de Alto Nivel
- Proporciona interfaces simplificadas para que los procesos de usuario interactúen con el hardware
- Oculta la complejidad del hardware subyacente

### 2. Administración Eficiente de Recursos
- **CPU:** Planificación y asignación de tiempo de procesador
- **Memoria:** Gestión del espacio de direcciones y asignación
- **Dispositivos:** Control de periféricos y hardware

### 3. Asistencia para Entrada/Salida (E/S)
- Gestiona las operaciones de I/O de forma segura y eficiente
- Proporciona servicios de E/S a los procesos de usuario

---

## ⚠️ Problemas que el SO Debe Prevenir

### Monopolización de Recursos
- **CPU:** Evitar que un proceso acapare indefinidamente el procesador
- **Memoria:** Impedir accesos fuera del espacio asignado a cada proceso

### Ejecución de Instrucciones Peligrosas
- Prevenir que procesos de usuario ejecuten instrucciones privilegiadas
- Controlar acceso a operaciones de E/S y flags del procesador

### Protección de Estructuras Críticas
- Proteger el vector de interrupciones
- Salvaguardar las RAI (Rutinas de Atención de Interrupciones)

---

## 🏗️ Apoyo del Hardware

Para que el SO funcione correctamente, necesita soporte específico del hardware:

### Modos de Ejecución
- Define limitaciones en el conjunto de instrucciones disponibles
- Controla qué operaciones puede realizar cada proceso

### Interrupción de Clock
- Mecanismo temporal para evitar monopolización de CPU
- Permite al SO retomar control periódicamente

### Protección de Memoria
- Registros base y límite para definir espacios de direcciones
- Hardware que detecta accesos ilegales a memoria

---

## 🔐 Modos de Ejecución

### Modo Kernel (Supervisor)
**Características:**
- Acceso completo a todas las instrucciones del procesador
- Puede ejecutar instrucciones privilegiadas
- Gestiona recursos del sistema
- **El kernel del SO siempre se ejecuta en este modo**

**Responsabilidades:**
- **Gestión de procesos:** creación, terminación, planificación, sincronización
- **Gestión de memoria:** reserva de espacios, swapping, paginación
- **Gestión de E/S:** buffers, canales, dispositivos
- **Funciones de soporte:** interrupciones, auditoría, monitoreo

### Modo Usuario
**Características:**
- Subconjunto limitado de instrucciones
- Solo puede acceder a su propio espacio de direcciones
- No puede interactuar directamente con hardware
- **Todos los procesos de usuario se ejecutan en este modo**

**Responsabilidades:**
- Debug de procesos
- Definición de protocolos de comunicación
- Gestión de aplicaciones (compiladores, editores, aplicaciones)
- Tareas que no requieren accesos privilegiados

### Transiciones entre Modos

```
Arranque del sistema → Modo Supervisor
↓
Inicio de proceso usuario → Modo Usuario (instrucción especial)
↓
Trap/Interrupción → Modo Kernel (automático)
```

**⚠️ Importante:** Solo las traps e interrupciones pueden cambiar de modo usuario a kernel. El proceso no puede hacer este cambio por sí mismo.

---

## 🛡️ Mecanismos de Protección

### Protección de Memoria

**Objetivo:** Delimitar el espacio de direcciones de cada proceso

**Implementación:**
- **Registro Base:** Dirección inicial del espacio del proceso
- **Registro Límite:** Tamaño máximo del espacio asignado
- **Control por Hardware:** Verifica cada acceso a memoria

```
Dirección Lógica + Registro Base = Dirección Física
Si (Dirección Lógica < Registro Límite) → Acceso Permitido
Sino → Trap de Error de Direccionamiento
```

### Protección de E/S

**Principio:** Todas las instrucciones de E/S son privilegiadas

**Mecanismo:**
- Solo se ejecutan en Modo Kernel
- Los procesos de usuario acceden a E/S mediante System Calls
- El kernel gestiona todas las operaciones de E/S

### Protección de CPU

**Problema:** Evitar monopolización del procesador

**Solución:** Interrupción por Clock
- Timer decremental que genera interrupciones periódicas
- Al llegar a cero, el kernel puede cambiar de proceso
- Garantiza multitarea cooperativa

---

## 📞 System Calls (Llamadas al Sistema)

### Definición
**Interfaz** entre los programas de usuario y los servicios del Sistema Operativo

### Características Clave
- Se ejecutan en **modo kernel**
- Único mecanismo seguro para acceder a servicios del SO
- Cambian automáticamente de modo usuario a kernel

### Mecanismo de Funcionamiento

```
1. Proceso usuario prepara parámetros
2. Ejecuta instrucción de system call (trap)
3. Hardware cambia a modo kernel
4. SO identifica y ejecuta la system call
5. Retorna resultado y vuelve a modo usuario
```

### Ejemplo Práctico
```c
count = read(file, buffer, nbytes);
```

**Pasos internos:**
1. Preparar número de syscall y parámetros
2. Generar trap/excepción
3. SO evalúa y ejecuta la llamada
4. Retorna resultado al proceso

### Categorías de System Calls

1. **Control de Procesos**
   - `fork()`, `exit()`, `wait()`

2. **Manejo de Archivos**
   - `open()`, `read()`, `write()`, `close()`

3. **Manejo de Dispositivos**
   - Operaciones con periféricos

4. **Mantenimiento de Información**
   - `getpid()`, `time()`

5. **Comunicaciones**
   - IPC, sockets, pipes

---

## 🖥️ Casos Específicos

### MS-DOS (Ejemplo Histórico)
- **Procesador Intel 8088:** Sin modo dual ni protección hardware
- **Problema:** Aplicaciones podían acceder directamente a funciones básicas de E/S
- **Consecuencia:** Menos seguridad y estabilidad

### Windows 2000+
- **Modo Kernel:** Servicios ejecutivos del sistema
- **Modo Usuario:** Procesos de usuario
- **BSOD:** Pantalla azul cuando falla el modo supervisor

---

## 🔑 Conceptos Clave para Recordar

- **Modo Dual:** Fundamento de la protección en sistemas modernos
- **System Calls:** Única vía segura de acceso a servicios del SO
- **Protección Hardware:** Esencial para funcionamiento seguro
- **Kernel:** Núcleo que se ejecuta siempre en modo privilegiado
- **Traps/Interrupciones:** Mecanismo para cambio de contexto seguro

---

## ✅ Puntos de Estudio Importantes

1. Diferencias entre modo kernel y usuario
2. Mecanismos de protección (memoria, E/S, CPU)
3. Funcionamiento de system calls
4. Transiciones entre modos de ejecución
5. Apoyo del hardware para SO modernos

---

*Versión: Agosto 2024 - I.S.O.*