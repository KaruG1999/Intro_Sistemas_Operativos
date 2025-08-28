# Guía de GNU/Linux para Principiantes - Debian

## ¿Qué es un archivo y para qué lo necesito?

### Concepto Básico de Archivos
Un **archivo** es como un contenedor digital que guarda información. Es similar a una carpeta física donde guardas documentos, pero en la computadora.

### ¿Para qué crear archivos?
- **Guardar información**: Tu nombre, notas, configuraciones
- **Documentar lo que haces**: Como un cuaderno digital
- **Configurar el sistema**: Decirle al sistema cómo comportarse
- **Programar**: Escribir instrucciones para la computadora
- **Respaldo**: Guardar copias de información importante

### Ejemplos Prácticos de Cuándo Crear Archivos:

1. **Archivo de configuración personal** (`mi_config.txt`)
   - Para guardar tus preferencias del sistema
   - Comandos que usas frecuentemente

2. **Lista de tareas** (`tareas.txt`)
   - Para organizarte y no olvidar cosas importantes

3. **Script de comandos** (`mis_comandos.sh`)
   - Para automatizar tareas repetitivas

4. **Archivo de pruebas** (`prueba.txt`)
   - Para practicar comandos sin romper nada importante

---

## 1. Terminal y Comandos Básicos

### ¿Qué es el Terminal?
El terminal es una ventana donde escribes **comandos** (instrucciones) que la computadora ejecuta. Es como hablar directamente con el sistema operativo.

**Para abrir el terminal:** `Ctrl + Alt + T`

### Comandos de Navegación

#### `pwd` - ¿Dónde estoy?
```bash
pwd
# Resultado: /home/tu_usuario
```
**¿Para qué?** Saber tu ubicación actual en el sistema de archivos. Es como preguntar "¿en qué calle estoy?"

#### `ls` - ¿Qué hay aquí?
```bash
ls          # Lista archivos y carpetas
ls -l       # Lista con detalles (permisos, tamaño, fecha)
ls -la      # Lista TODO, incluyendo archivos ocultos
```
**¿Para qué?** Ver qué archivos y carpetas están en tu ubicación actual.

#### `cd` - Ir a otro lugar
```bash
cd Documents        # Ir a la carpeta Documents
cd /home/usuario   # Ir a una ruta específica
cd ~               # Ir a tu directorio personal
cd ..              # Subir un nivel (ir a la carpeta padre)
cd -               # Volver al directorio anterior
```
**¿Para qué?** Moverte entre carpetas, como caminar por las calles de una ciudad.

### Comandos para Crear y Manejar Archivos

#### `mkdir` - Crear carpetas
```bash
mkdir mi_carpeta              # Crear una carpeta
mkdir -p carpeta1/carpeta2   # Crear carpetas anidadas
```
**¿Para qué?** Organizar tus archivos en carpetas, como cajones en un escritorio.

#### `touch` - Crear archivos vacíos
```bash
touch archivo.txt           # Crear archivo vacío
touch archivo1.txt archivo2.txt  # Crear varios archivos
```
**¿Para qué?** Crear un archivo nuevo y vacío, listo para agregar contenido.

#### `cp` - Copiar
```bash
cp archivo.txt copia.txt           # Copiar archivo
cp archivo.txt /ruta/destino/      # Copiar a otra ubicación
cp -r carpeta/ /ruta/destino/      # Copiar carpeta completa
```
**¿Para qué?** Hacer copias de seguridad o duplicar archivos.

#### `mv` - Mover/Renombrar
```bash
mv archivo.txt nuevo_nombre.txt    # Renombrar archivo
mv archivo.txt /otra/carpeta/      # Mover archivo
```
**¿Para qué?** Cambiar el nombre de archivos o moverlos a otra ubicación.

#### `rm` - Eliminar
```bash
rm archivo.txt        # Eliminar archivo
rm -r carpeta/        # Eliminar carpeta y su contenido
rm -f archivo.txt     # Forzar eliminación
```
**⚠️ CUIDADO:** Una vez eliminado, no se puede recuperar fácilmente.

---

## 2. Editores de Texto

### Vim (El editor que debes dominar)

#### ¿Por qué vim y no nano?
- **Está en TODOS los sistemas Unix/Linux** - Siempre disponible
- **Mucho más potente** - Edición súper eficiente
- **Modo sin mouse** - Perfecto para servidores remotos
- **Estándar en la industria** - Lo usan profesionales
- **Una vez que lo aprendes, eres súper rápido**

#### Conceptos básicos de vim:

```bash
vim mi_archivo.txt
```

**🔑 CONCEPTO CLAVE: Vim tiene MODOS**

#### Los 3 modos principales:

1. **MODO NORMAL** (por defecto) - Para navegar y comandos
2. **MODO INSERCIÓN** - Para escribir texto
3. **MODO COMANDO** - Para guardar, salir, buscar

#### Comandos básicos de supervivencia:

**Para ESCRIBIR texto:**
```
i    → Entrar al modo inserción (insert)
ESC  → Volver al modo normal
```

**Para GUARDAR y SALIR:**
```
:w   → Guardar (write)
:q   → Salir (quit)
:wq  → Guardar y salir
:q!  → Salir sin guardar (forzado)
```

#### Tutorial paso a paso para tu primer archivo:

1. **Abrir vim:**
```bash
vim prueba.txt
```

2. **Entrar en modo inserción:**
```
i
```
(Verás "-- INSERT --" abajo)

3. **Escribir tu texto:**
```
Nombre: Tu Nombre
Legajo: 12345
Materia: Sistemas Operativos
```

4. **Salir del modo inserción:**
```
ESC
```

5. **Guardar y salir:**
```
:wq
```

#### Comandos útiles en modo normal:

**Navegación:**
```
h  → Izquierda
j  → Abajo  
k  → Arriba
l  → Derecha

gg → Ir al inicio del archivo
G  → Ir al final del archivo
```

**Edición básica:**
```
x   → Borrar carácter
dd  → Borrar línea completa
yy  → Copiar línea
p   → Pegar
u   → Deshacer (undo)
```

**Modo inserción avanzado:**
```
i  → Insertar antes del cursor
a  → Insertar después del cursor
o  → Nueva línea abajo
O  → Nueva línea arriba
```

#### ¿Cómo practicar vim?

**1. Ejecuta el tutorial interactivo:**
```bash
vimtutor
```
(¡Esto es ORO! 30 minutos que cambiarán tu vida)

**2. Práctica diaria:**
- Usa vim para TODOS tus archivos del TP
- No vuelvas a nano (aunque al principio sea más lento)
- En 1 semana ya serás más rápido que con nano

---

## 3. Sistema de Permisos (MUY IMPORTANTE)

### ¿Qué son los permisos?
Los permisos determinan **quién puede hacer qué** con un archivo o carpeta.

### Los tres tipos de usuarios:
1. **Usuario (u)** - El dueño del archivo
2. **Grupo (g)** - Usuarios que pertenecen al mismo grupo
3. **Otros (o)** - Todos los demás usuarios

### Los tres tipos de permisos:
1. **Lectura (r)** - Puede ver el contenido
2. **Escritura (w)** - Puede modificar el archivo
3. **Ejecución (x)** - Puede ejecutar el archivo como programa

### Ver permisos:
```bash
ls -l archivo.txt
# Resultado: -rw-r--r-- 1 usuario grupo 1234 fecha archivo.txt
#            |||||||||
#            ||||||||└─ Otros: lectura
#            |||||||└── Otros: escritura (-)
#            ||||||└─── Otros: ejecución (-)
#            |||||└──── Grupo: lectura
#            ||||└───── Grupo: escritura (-)
#            |||└────── Grupo: ejecución (-)
#            ||└─────── Usuario: lectura
#            |└──────── Usuario: escritura
#            └───────── Usuario: ejecución (-)
```

### Cambiar permisos:
```bash
chmod 755 archivo.txt    # rwx para usuario, rx para grupo y otros
chmod 644 archivo.txt    # rw para usuario, r para grupo y otros
chmod +x script.sh       # Agregar permiso de ejecución
```

### Números de permisos:
- **4** = Lectura (r)
- **2** = Escritura (w)  
- **1** = Ejecución (x)

**Ejemplos:**
- **7** = 4+2+1 = rwx (todos los permisos)
- **6** = 4+2 = rw- (lectura y escritura)
- **5** = 4+1 = r-x (lectura y ejecución)
- **4** = r-- (solo lectura)

---

## 4. Usuarios y Grupos

### Comandos básicos:
```bash
whoami          # ¿Quién soy?
id              # Mi información completa
su - root       # Cambiar a usuario root
sudo comando    # Ejecutar comando como administrador
```

### ¿Para qué necesito otros usuarios?
- **Seguridad**: Separar tareas administrativas de uso normal
- **Organización**: Diferentes usuarios para diferentes propósitos
- **Colaboración**: Múltiples personas usando el mismo sistema

---

## 5. Estructura del Sistema de Archivos

### Directorios importantes:

#### `/home/tu_usuario` - Tu casa digital
- Aquí guardas TUS archivos personales
- Documentos, configuraciones personales, descargas
- **Es tu espacio seguro para experimentar**

#### `/etc` - Configuraciones del sistema
- Archivos que controlan cómo funciona el sistema
- **Solo modificar con cuidado y conocimiento**

#### `/var` - Archivos variables
- Logs del sistema (registros de lo que pasa)
- Archivos temporales que cambian frecuentemente

#### `/tmp` - Archivos temporales
- Se borra automáticamente al reiniciar
- **Perfecto para pruebas**

#### `/usr` - Programas de usuario
- Donde están instalados la mayoría de programas

---

## 6. Comandos de Información del Sistema

### Estado del sistema:
```bash
uname -a        # Información completa del sistema
df -h           # Espacio en disco (human readable)
free -h         # Memoria RAM disponible
top             # Procesos en tiempo real (presiona 'q' para salir)
ps aux          # Lista de todos los procesos
```

### ¿Para qué usar estos comandos?
- **Diagnosticar problemas**: Ver qué consume recursos
- **Monitorear rendimiento**: Saber si el sistema está lento
- **Administración**: Gestionar procesos y recursos

---

## 7. Ejercicios Prácticos para Empezar

### Ejercicio 1: Crear tu espacio de trabajo
```bash
# 1. Ve a tu directorio personal
cd ~

# 2. Crea una carpeta para practicar
mkdir practica_linux

# 3. Entra a la carpeta
cd practica_linux

# 4. Verifica dónde estás
pwd
```

### Ejercicio 2: Crear y editar tu primer archivo con vim
```bash
# 1. Crea un archivo con tu información
vim mi_info.txt

# 2. Una vez que se abra vim:
#    - Presiona 'i' para entrar en modo inserción
#    - Escribe:
#      Nombre: Tu Nombre
#      Universidad: UNLP
#      Materia: Sistemas Operativos  
#      Fecha: [fecha actual]
#      Legajo: [tu número]
#
#    - Presiona ESC para salir del modo inserción
#    - Escribe :wq para guardar y salir

# 3. Ver el contenido del archivo creado
cat mi_info.txt
```

### Ejercicio 3: Practicar con permisos
```bash
# 1. Ver permisos actuales
ls -l mi_info.txt

# 2. Cambiar permisos (solo lectura y escritura para ti)
chmod 600 mi_info.txt

# 3. Verificar el cambio
ls -l mi_info.txt
```

### Ejercicio 4: Crear un directorio organizado
```bash
# 1. Crear estructura de carpetas
mkdir documentos tareas scripts

# 2. Crear archivos en cada carpeta
touch documentos/notas.txt
touch tareas/tp1.txt
touch scripts/mi_script.sh

# 3. Ver la estructura
ls -la
```

---

## 8. Comandos de Ayuda

### `man` - Manual de comandos
```bash
man ls          # Manual del comando ls
man chmod       # Manual del comando chmod
```
**Navegación en man:**
- `q` para salir
- `Espacio` para avanzar página
- `Enter` para avanzar línea

### `--help` - Ayuda rápida
```bash
ls --help       # Ayuda rápida del comando ls
```

---

## 9. Consejos para Principiantes

### DO (Hacer):
✅ **Practica en tu directorio personal** (`/home/tu_usuario`)
✅ **Usa TAB para autocompletar** (te ahorra tiempo y evita errores)
✅ **Lee los mensajes de error** (te dicen qué está mal)
✅ **Haz copias de archivos importantes** antes de modificarlos
✅ **Experimenta con archivos de prueba** primero

### DON'T (No hacer):
❌ **No modifiques archivos del sistema** sin saber qué haces
❌ **No uses `sudo rm -rf /`** (¡JAMÁS!)
❌ **No ignores las advertencias** del sistema
❌ **No te desesperes** - es normal cometer errores al principio

---

## 10. Orden de Aprendizaje Sugerido

### Semana 1: Fundamentos
- [ ] Navegación básica (cd, ls, pwd)
- [ ] Crear y eliminar archivos/carpetas
- [ ] Usar nano para editar texto

### Semana 2: Organización
- [ ] Entender permisos básicos
- [ ] Copiar y mover archivos
- [ ] Organizar carpetas

### Semana 3: Administración
- [ ] Usuarios y grupos
- [ ] Comandos de sistema
- [ ] Monitoreo básico

### Semana 4: Automatización
- [ ] Scripts básicos
- [ ] Comprimir archivos
- [ ] Tareas programadas

---

## 11. Resolución de Problemas Comunes

### "Permission denied"
**Problema:** No tienes permisos para hacer algo
**Solución:** Usa `sudo` o cambia los permisos con `chmod`

### "No such file or directory"
**Problema:** El archivo/carpeta no existe o escribiste mal la ruta
**Solución:** Verifica con `ls` y usa TAB para autocompletar

### "Command not found"
**Problema:** El comando no existe o no está instalado
**Solución:** Verifica la escritura o instala el programa necesario

---

## ¿Por qué es importante aprender esto?

1. **Control total** sobre tu sistema
2. **Eficiencia** - Los comandos son más rápidos que la interfaz gráfica
3. **Automatización** - Puedes automatizar tareas repetitivas
4. **Administración remota** - Manejar servidores sin interfaz gráfica
5. **Comprensión profunda** - Entender cómo funciona realmente el sistema

---

¡Recuerda: La práctica hace al maestro! No tengas miedo de experimentar y cometer errores. Es la mejor forma de aprender.