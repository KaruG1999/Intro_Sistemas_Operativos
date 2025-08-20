# Resumen: Anexo Arquitectura de Computadoras - Sistemas Operativos

## Elementos Básicos de una Computadora

### Componentes Principales
- **Procesador**: Unidad central de procesamiento
- **Memoria Principal**: 
  - Volátil (se pierde al apagar)
  - También llamada memoria real o primaria
- **Componentes de E/S**:
  - Dispositivos de memoria secundaria
  - Equipamiento de comunicación (monitor, teclado, mouse)
- **Bus del Sistema**: Comunicación entre procesador, memoria y dispositivos

## Registros del Procesador

### Registros Visibles por el Usuario
- Pueden ser utilizados por aplicaciones
- Accesibles desde lenguaje de máquina
- **Tipos**:
  - Registros de datos
  - Registros de direcciones (índice, puntero de segmento, puntero de pila)

### Registros de Control y Estado
- **Program Counter (PC)**: Contiene la dirección de la próxima instrucción
- **Instruction Register (IR)**: Contiene la instrucción actual a ejecutar
- **Program Status Word (PSW)**: 
  - Códigos de resultado de operaciones
  - Habilita/deshabilita interrupciones
  - Indica modo de ejecución (supervisor/usuario)

## Ciclo de Ejecución de Instrucciones

### Proceso Básico
1. **Fetch**: El procesador busca la instrucción en memoria
2. **Execute**: El procesador ejecuta la instrucción

### Categorías de Instrucciones
- **Procesador-Memoria**: Transferencia de datos entre procesador y memoria
- **Procesador-E/S**: Transferencia con dispositivos periféricos
- **Procesamiento de Datos**: Operaciones aritméticas y lógicas
- **Control**: Alteran la secuencia de ejecución

## Interrupciones

### Concepto Fundamental
Las interrupciones interrumpen el secuenciamiento normal del procesador durante la ejecución, principalmente porque los dispositivos de E/S son más lentos que el procesador.

### Necesidades del Sistema Operativo
- Postergar el manejo de interrupciones en momentos críticos
- Atender eficientemente según el dispositivo
- Manejar varios niveles de prioridad

### Clases de Interrupciones
El documento distingue entre flujo de control con y sin interrupciones, mostrando cómo las interrupciones alteran la secuencia normal de ejecución.

### Interrupt Handler
Programa o rutina que:
- Determina la naturaleza de una interrupción
- Realiza las acciones necesarias para atenderla
- Generalmente forma parte del sistema operativo

## Ciclo de Interrupción

### Proceso
1. El procesador verifica la existencia de interrupciones
2. Si no hay interrupciones: ejecuta la próxima instrucción del programa
3. Si hay interrupciones pendientes: suspende el programa actual y ejecuta la rutina de manejo

## Tipos de Interrupciones

### No Enmascarables
- Reservadas para eventos críticos (errores de memoria no recuperables)
- No pueden ser deshabilitadas por la CPU

### Enmascarables
- Pueden ser "apagadas" durante secuencias críticas
- Usadas por controladores de dispositivos
- En procesadores Intel:
  - Números 1-31: No enmascarables (errores de condición)
  - Números 32-255: Enmascarables (interrupciones de dispositivos)

## Manejo de Múltiples Interrupciones

### Estrategias
1. **Deshabilitar interrupciones**: Mientras se procesa una interrupción
2. **Sistema de prioridades**: Definir prioridades para diferentes interrupciones

## Importancia para Sistemas Operativos

Este anexo establece los fundamentos de hardware necesarios para comprender cómo los sistemas operativos:
- Gestionan los recursos del hardware
- Manejan las interrupciones de dispositivos
- Controlan la ejecución de programas
- Mantienen el control del sistema mediante registros de estado

El conocimiento de estos conceptos es esencial para entender el funcionamiento interno de los sistemas operativos y su interacción con el hardware subyacente.