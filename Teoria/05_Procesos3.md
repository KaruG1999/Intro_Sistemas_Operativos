# Procesos III - Creación y Terminación de Procesos

## Introducción

En esta clase vamos a ver cómo nacen y mueren los procesos en los sistemas operativos. Es importante entender que **un proceso nunca surge de la nada**, siempre es creado por otro proceso que ya existe. Esto forma una estructura jerárquica muy interesante que veremos a continuación.

## Creación de Procesos

### Conceptos Fundamentales

**¿Quién crea los procesos?**
- Un proceso siempre es creado por otro proceso
- El proceso que crea se llama **proceso padre**
- El proceso creado se llama **proceso hijo**
- Un proceso padre puede tener múltiples hijos
- Esto forma lo que llamamos un **árbol de procesos**

Imaginate esto como un árbol genealógico: tenés un proceso "abuelo" que crea un proceso "padre", y ese padre puede crear varios procesos "hijos".

### ¿Qué pasa cuando se crea un proceso?

El sistema operativo tiene que hacer varias tareas importantes:

1. **Crear el PCB (Process Control Block)**: Es como el "documento de identidad" del proceso
2. **Asignar un PID único**: Cada proceso necesita un número que lo identifique (Process IDentification)
3. **Asignar memoria**: El proceso necesita espacio para:
   - **Stack**: Para las variables locales y llamadas a funciones
   - **Text**: Para el código del programa
   - **Datos**: Para las variables globales
4. **Crear estructuras de datos asociadas**: Toda la información que el SO necesita para manejar el proceso

## Relación entre Procesos Padre e Hijo

### Respecto a la Ejecución

Cuando un padre crea un hijo, pueden pasar dos cosas:

1. **Ejecución concurrente**: El padre y el hijo ejecutan al mismo tiempo, cada uno hace lo suyo
2. **El padre espera**: El padre se queda esperando a que el hijo termine antes de continuar

### Respecto al Espacio de Direcciones

Acá es donde se nota la diferencia entre sistemas operativos:

**En UNIX/Linux:**
- El hijo es una **copia exacta** del padre
- Se duplica todo: código, datos, stack
- Es como hacer un "Ctrl+C, Ctrl+V" del proceso completo

**En Windows:**
- Se crea un espacio de direcciones **completamente nuevo y vacío**
- Después se carga el programa que querés ejecutar

## System Calls para Creación de Procesos

### En UNIX/Linux: El Combo fork() + execve()

**fork():**
- Crea un proceso hijo que es copia exacta del padre
- Retorna valores diferentes según quién lo vea:
  - En el proceso **hijo**: retorna **0**
  - En el proceso **padre**: retorna el **PID del hijo** (número positivo)
  - Si hay **error**: retorna un número **negativo**

**¿Cómo funciona fork() en la práctica?**

```c
int pid = fork();

if (pid == 0) {
    // Estoy en el proceso hijo
    printf("Soy el hijo\n");
} 
else if (pid > 0) {
    // Estoy en el proceso padre
    printf("Soy el padre, mi hijo tiene PID: %d\n", pid);
}
else {
    // Hubo un error
    printf("Error al crear el proceso\n");
}
```

**execve():**
- Se usa típicamente después de fork()
- Reemplaza el programa actual con otro programa
- Es como decirle al proceso: "dejá de hacer lo que estás haciendo y empezá a ejecutar este otro programa"

### En Windows: CreateProcess()

- Con una sola system call hace todo el trabajo
- Crea el proceso Y carga el programa de una vez
- Más directo pero menos flexible que el sistema Unix

## Terminación de Procesos

### ¿Cómo termina un proceso?

**Terminación normal:**
- El proceso llama a `exit()` con un código de retorno
- Es como decir "terminé mi trabajo, acá está el resultado"
- El proceso padre puede usar `wait()` para recibir ese código

**Terminación forzada:**
- El proceso padre puede "matar" a sus hijos usando `kill()`
- Útil cuando algo sale mal o ya no necesitás que el hijo siga ejecutando

### ¿Cuándo un padre mata a sus hijos?

1. **La tarea del hijo ya terminó**: No tiene más sentido que siga ejecutando
2. **El padre está por terminar**: Generalmente no se permite que los hijos queden "huérfanos"

**Terminación en cascada:**
Cuando el padre muere, todos sus hijos también mueren. Es como un efecto dominó hacia abajo en el árbol de procesos.

## Ejemplo Práctico: Shell (Terminal)

El shell es un ejemplo perfecto de todo esto:

1. Cuando escribís un comando (ej: `ls`), el shell hace `fork()`
2. En el proceso hijo, hace `execve()` para ejecutar el comando `ls`
3. El shell padre espera con `wait()` a que termine `ls`
4. Cuando `ls` termina, el shell vuelve a mostrar el prompt y espera el siguiente comando

## System Calls Importantes

**En Unix/Linux:**
- `fork()`: Crear proceso hijo
- `execve()`: Cargar nuevo programa
- `exit()`: Terminar proceso
- `wait()`: Esperar a que termine un hijo

**En Windows:**
- `CreateProcess()`: Crear proceso y cargar programa
- `ExitProcess()`: Terminar proceso
- `WaitForSingleObject()`: Esperar a que termine un proceso

## Puntos Clave para Recordar

1. **Los procesos forman un árbol**: Siempre hay una relación padre-hijo
2. **fork() crea una copia exacta**: El hijo es idéntico al padre al momento de la creación
3. **Los valores de retorno de fork() son diferentes**: 0 para el hijo, PID para el padre
4. **execve() reemplaza el programa**: No crea un nuevo proceso, cambia lo que ejecuta el proceso actual
5. **La terminación puede ser en cascada**: Si el padre muere, generalmente los hijos también
6. **El shell es el ejemplo clásico**: Cada comando que ejecutás es un nuevo proceso

Esta arquitectura de procesos es fundamental para entender cómo funciona cualquier sistema Unix/Linux, y muchos de estos conceptos se aplican también a otros sistemas operativos.