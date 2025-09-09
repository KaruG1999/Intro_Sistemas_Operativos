# Introducción a los Sistemas Operativos

## ¿Qué es un Sistema Operativo?

Un **Sistema Operativo (SO)** es un software que actúa como intermediario entre el usuario y el hardware de la computadora. Es importante recordar que como es software, necesita procesador y memoria para ejecutarse.

### Funciones principales:
- **Gestiona el hardware** de la computadora
- **Controla la ejecución** de los procesos
- **Interfaz** entre las aplicaciones y el hardware
- **Intermediario** entre el usuario y el hardware

## Dos Perspectivas del Sistema Operativo

### 1. Perspectiva desde el Usuario (de arriba hacia abajo)
El SO proporciona una **abstracción** del hardware, ocultando la complejidad de la arquitectura (conjunto de instrucciones, organización de memoria, E/S, estructura de bus).

**Ejemplo:** Cuando guardas un archivo, no necesitas saber cómo se organizan físicamente los datos en el disco duro. El SO te presenta una interfaz simple (carpetas y archivos) que oculta la complejidad del hardware.

- Los programas de aplicación son los "clientes" del SO
- Busca **comodidad** y **amigabilidad** para el usuario
- Comparación: usar interfaz gráfica vs. comandos de texto

### 2. Perspectiva desde la Administración de Recursos (de abajo hacia arriba)
El SO administra los recursos de hardware de uno o más procesos de manera eficiente.

- Maneja memoria secundaria y dispositivos de I/O
- Permite ejecución simultánea de procesos
- **Multiplexación**: reparte recursos en tiempo (CPU) y en espacio (memoria)

**Ejemplo:** Cuando tienes múltiples programas abiertos (navegador, editor de texto, reproductor de música), el SO distribuye el tiempo de CPU entre todos ellos tan rápido que parecen ejecutarse simultáneamente.

## Objetivos de los Sistemas Operativos

1. **Comodidad**: Hacer más fácil el uso del hardware
2. **Eficiencia**: Optimizar el uso de los recursos del sistema
3. **Evolución**: Permitir nuevas funciones sin interferir con las existentes

## Componentes de un Sistema Operativo

### Kernel (Núcleo)
Es la "porción de código" que:
- Se encuentra **siempre en memoria principal**
- Se encarga de la **administración de recursos**
- Implementa servicios esenciales:
  - Manejo de memoria
  - Manejo de la CPU
  - Administración de procesos
  - Comunicación y concurrencia
  - Gestión de entrada/salida

### Shell
Interfaz de usuario que puede ser:
- **GUI** (Interfaz Gráfica de Usuario)
- **CLI/CUI** (Interfaz de Línea de Comandos)

### Herramientas
- Editores
- Compiladores
- Librerías
- Otros utilitarios

## Servicios Principales de un Sistema Operativo

### 1. Administración y Planificación del Procesador
- **Multiplexación** de la carga de trabajo
- **Imparcialidad** en la ejecución (fairness)
- Prevención de **bloqueos**
- **Manejo de prioridades**

**Ejemplo:** Si estás ejecutando un antivirus (alta prioridad) y un juego al mismo tiempo, el SO dará más tiempo de CPU al antivirus para que termine más rápido.

### 2. Administración de Memoria
- Administración **eficiente** de la memoria
- Manejo de **memoria física vs. memoria virtual**
- **Jerarquías de memoria** (RAM, cache, disco)
- **Protección** entre programas que se ejecutan concurrentemente

### 3. Administración del Almacenamiento - Sistema de Archivos
- Acceso a **medios de almacenamiento externos**
- **Ocultamiento** de dependencias de hardware
- Administración de **accesos simultáneos**

### 4. Detección de Errores y Respuestas
**Errores de Hardware:**
- Internos y externos
- Errores de memoria/CPU
- Errores de dispositivos

**Errores de Software:**
- Errores aritméticos (división por cero)
- Acceso no permitido a direcciones de memoria
- Incapacidad del SO para conceder solicitudes de aplicaciones

### 5. Interacción del Usuario (Shell)
Proporciona la interfaz para que el usuario interactúe con el sistema.

### 6. Contabilidad
- Recolección de **estadísticas de uso**
- **Monitoreo** de parámetros de rendimiento
- **Anticipación** de necesidades futuras
- Elementos para **facturación** de tiempo de procesamiento

## Complejidad de los Sistemas Operativos

Los SO son software **extenso y complejo** que se desarrollan por partes. Cada componente debe ser analizado y desarrollado entendiendo:
- Su **función específica**
- Sus **entradas**
- Sus **salidas**
- Su **interacción** con otros componentes

---

**Palabras clave:** Sistemas Operativos, Hardware, Interrupciones, Registros

**Referencias:** William Stallings - Sistemas Operativos: Aspectos internos y principios de diseño | Silberschatz, Galvin, Gagne - Conceptos de sistemas operativos