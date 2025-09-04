# Resolución Completa - Práctica 1: Conceptos Básicos GNU/Linux

## 1. Características de GNU/Linux

### (a) Características más relevantes de GNU/Linux:

- **Multiusuario**: Múltiples usuarios pueden trabajar simultáneamente
- **Multitarea**: Ejecuta múltiples procesos al mismo tiempo
- **Multiprocesador**: Aprovecha sistemas con varios procesadores
- **Portable**: Funciona en diferentes arquitecturas de hardware
- **Código abierto**: El código fuente está disponible libremente
- **Gratuito**: No requiere licencias pagadas para su uso
- **Estable y robusto**: Raramente requiere reinicio del sistema
- **Case sensitive**: Distingue entre mayúsculas y minúsculas
- **"Todo es archivo"**: Dispositivos y directorios son tratados como archivos
- **Sistemas de archivos flexibles**: Soporte para múltiples tipos de FS

### (b) Comparación con otros sistemas operativos:

| Característica      | GNU/Linux      | Windows        | macOS                 |
| ------------------- | -------------- | -------------- | --------------------- |
| **Costo**           | Gratuito       | Licencia paga  | Incluido con hardware |
| **Código fuente**   | Abierto        | Cerrado        | Cerrado               |
| **Personalización** | Alta           | Media          | Baja                  |
| **Estabilidad**     | Muy alta       | Media-Alta     | Alta                  |
| **Seguridad**       | Muy alta       | Media          | Alta                  |
| **Hardware**        | Amplio soporte | Amplio soporte | Solo Apple            |

### (c) ¿Qué es GNU?

**GNU = GNU No es Unix**

Es un proyecto iniciado por Richard Stallman en 1983 para crear un sistema operativo completamente libre, compatible con Unix. GNU proporciona las herramientas del sistema (compiladores, editores, shell, etc.) que junto con el kernel Linux forman GNU/Linux.

### (d) Historia del proyecto GNU:

- **1983**: Richard Stallman anuncia el proyecto GNU
- **1985**: Se crea la Free Software Foundation (FSF)
- **1989**: Se publica la primera versión de GPL
- **1990**: GNU tenía la mayoría de componentes excepto el kernel
- **1991**: Linus Torvalds crea Linux
- **1992**: Se combina GNU con Linux → nace GNU/Linux

### (e) Multitarea en GNU/Linux:

**Multitarea** es la capacidad del SO de ejecutar múltiples procesos aparentemente simultáneamente. GNU/Linux implementa:

- **Multitarea preventiva**: El kernel controla el tiempo de CPU asignado
- **Multiplexación temporal**: Alterna rápidamente entre procesos
- **Planificador**: Determina qué proceso ejecutar y cuándo

**Sí, GNU/Linux hace uso completo de multitarea.**

### (f) ¿Qué es POSIX?

**POSIX** (Portable Operating System Interface) es un estándar IEEE que define la interfaz entre aplicaciones y el sistema operativo. Garantiza portabilidad del código entre diferentes sistemas Unix-like. GNU/Linux es compatible con POSIX.

## 2. Distribuciones de GNU/Linux

### (a) ¿Qué es una distribución?

Una **distribución** es una versión customizada de GNU/Linux que incluye:

- Una versión específica del kernel
- Selección de software y herramientas
- Sistema de gestión de paquetes
- Configuraciones por defecto

**4 distribuciones principales:**

1. **Debian**: Estable, orientada a servidores, gestión .deb
2. **Ubuntu**: Basada en Debian, fácil uso, desktop friendly
3. **Red Hat/Fedora**: Empresarial, .rpm, innovación tecnológica
4. **Arch Linux**: Rolling release, DIY, usuarios avanzados

### (b) Diferencias entre distribuciones:

- **Gestión de paquetes**: .deb, .rpm, tar.xz
- **Filosofía**: Estabilidad vs innovación vs simplicidad
- **Público objetivo**: Principiantes, avanzados, servidores
- **Frecuencia de actualizaciones**: Fixed release vs rolling release
- **Desktop environment por defecto**: GNOME, KDE, XFCE

### (c) ¿Qué es Debian?

**Debian** es una distribución GNU/Linux completamente libre, desarrollada por una comunidad de voluntarios.

**Objetivos del proyecto Debian:**

- Crear un sistema operativo libre
- Mantener la calidad y estabilidad
- Proveer actualizaciones de seguridad
- Servir como base universal para otros SOs

**Cronología breve:**

- **1993**: Ian Murdock funda Debian
- **1996**: Primera versión estable (1.1)
- **1998**: Debian Social Contract
- **2000s**: Se convierte en base de Ubuntu
- **Actualidad**: Una de las distribuciones más influyentes

## 3. Estructura de GNU/Linux

### (a) 3 componentes fundamentales:

1. **Kernel**: Núcleo del sistema, gestiona hardware
2. **Shell**: Intérprete de comandos, interfaz usuario-kernel
3. **Herramientas/Utilidades**: Programas del sistema (GNU tools)

### (b) Estructura básica:

```
┌───────────────────────────────────┐
│           APLICACIONES              │
├───────────────────────────────────┤
│      SHELL (CLI/GUI)                │
├───────────────────────────────────┤
│    HERRAMIENTAS DEL SISTEMA         │
├───────────────────────────────────┤
│         KERNEL                      │
├───────────────────────────────────┤
│         HARDWARE                    │
└───────────────────────────────────┘
```

## 4. Kernel

### (a) ¿Qué es? Historia:

El **Kernel** es el núcleo del SO que gestiona los recursos del sistema.

**Historia:**

- **1991**: Linus Torvalds (21 años) inicia Linux como hobby
- **1991**: Versión 0.01 (10,000 líneas de código)
- **1994**: Versión 1.0 primera versión estable
- **1996**: Tux como mascota
- **2003**: Versión 2.6 con grandes mejoras
- **2011**: Versión 3.0 (20 aniversario)
- **2023**: Versión 6.4+ actual

### (b) Funciones principales:

- **Gestión de procesos**: Creación, planificación, terminación
- **Gestión de memoria**: Asignación, memoria virtual, protección
- **Gestión de E/S**: Controladores de dispositivos
- **Sistema de archivos**: Acceso a almacenamiento
- **Red**: Protocolos de comunicación
- **Seguridad**: Control de acceso, permisos

### (c) Esquema de versionado:

**Versión actual**: 6.4.11 (agosto 2023)

**Esquema anterior a 2.4:**

- **X.Y.Z** donde Y impar = desarrollo, Y par = estable
- Ejemplo: 2.1.x (desarrollo), 2.2.x (estable)

**Cambio desde 2.6:**

- **Se eliminó** la distinción desarrollo/estable
- **Todas las versiones** son consideradas estables
- **2.6.X** duró hasta 2011
- **Desde 3.0**: X.Y.Z donde X cambia raramente

### (d) ¿Múltiples kernels?

**Sí**, es posible tener múltiples versiones del kernel instaladas. El gestor de arranque (GRUB) permite seleccionar cuál usar al inicio.

### (e) Ubicación en el File System:

- **Kernel compilado**: `/boot/vmlinuz-<version>`
- **Imagen inicial**: `/boot/initrd-<version>`
- **Código fuente**: `/usr/src/linux/` (si está instalado)
- **Módulos**: `/lib/modules/<version>/`

### (f) ¿Es monolítico?

El kernel Linux es **monolítico híbrido**:

- **Monolítico**: Todas las funciones en espacio de kernel
- **Híbrido**: Soporta módulos cargables dinámicamente
- **Ventaja**: Rendimiento (no cambio de contexto)
- **Flexibilidad**: Módulos se cargan/descargan en tiempo de ejecución

## 5. Intérprete de comandos (Shell)

### (a) ¿Qué es?

El **Shell** es la interfaz entre el usuario y el kernel. Es un programa que interpreta comandos y los traduce en llamadas al sistema.

### (b) Funciones:

- **Interpretar comandos** del usuario
- **Ejecutar programas**
- **Gestión de archivos** básica
- **Control de procesos**
- **Redirección** de E/S
- **Programación** (scripts)
- **Autocompletado** y historial

### (c) 3 intérpretes principales:

1. **Bash (Bourne Again Shell)**:

   - Más popular y completo
   - Autocompletado, historial, alias
   - Compatible con sh
   - Programación avanzada

2. **Zsh (Z Shell)**:

   - Muy personalizable
   - Mejor autocompletado
   - Temas y plugins
   - Compatible con bash

3. **Fish (Friendly Interactive Shell)**:
   - Sintaxis moderna
   - Sugerencias inteligentes
   - Colores por defecto
   - Menos compatible con bash

### (d) Ubicación de comandos:

- **Comandos internos**: Integrados en el shell (cd, echo, exit)
- **Comandos externos**:
  - `/bin/`: Comandos básicos del sistema
  - `/usr/bin/`: Comandos de usuario
  - `/sbin/`: Comandos de administración
  - `/usr/sbin/`: Comandos administrativos de usuario

### (e) ¿Por qué Shell no es parte del Kernel?

- **Separación de responsabilidades**: Kernel maneja hardware, shell interactúa con usuario
- **Flexibilidad**: Se puede cambiar shell sin afectar kernel
- **Seguridad**: Shell ejecuta en espacio de usuario, no privilegiado
- **Modularidad**: Diferentes shells para diferentes necesidades

### (f) ¿Shell distinto por usuario?

**Sí**, cada usuario puede tener un shell diferente.

- **Se define en**: `/etc/passwd` (campo 7)
- **Cambio**: Comando `chsh` o editar `/etc/passwd`
- **Restricciones**: Solo shells listados en `/etc/shells`
- **Permisos**: Usuario normal puede cambiar su propio shell

## 6. Sistema de Archivos (File System)

### (a) ¿Qué es?

Un **sistema de archivos** es la forma en que el SO organiza y almacena archivos en dispositivos de almacenamiento. Define estructura, metadatos, y métodos de acceso.

### (b) Sistemas soportados por GNU/Linux:

- **Linux nativos**: ext2, ext3, ext4, Btrfs, XFS, ReiserFS
- **Windows**: FAT, FAT32, NTFS
- **Otros Unix**: UFS, JFS
- **Redes**: NFS, CIFS/SMB
- **Especiales**: tmpfs, procfs, sysfs

### (c) ¿Particiones FAT y NTFS visibles?

**Sí**, GNU/Linux puede leer y escribir particiones FAT y NTFS:

- **FAT/FAT32**: Soporte nativo completo
- **NTFS**: Mediante ntfs-3g (lectura/escritura)
- **Montaje**: Se montan como cualquier otro FS

### (d) Estructura básica FHS:

**FHS = Filesystem Hierarchy Standard**

```
/                  # Raíz del sistema
├── bin/           # Binarios esenciales
├── boot/          # Archivos de arranque (kernel, grub)
├── dev/           # Archivos de dispositivos
├── etc/           # Archivos de configuración
├── home/          # Directorios de usuarios
├── lib/           # Bibliotecas compartidas
├── media/         # Puntos de montaje medios removibles
├── mnt/           # Puntos de montaje temporales
├── opt/           # Software opcional
├── proc/          # Sistema de archivos virtual (procesos)
├── root/          # Directorio del usuario root
├── run/           # Datos de runtime
├── sbin/          # Binarios del sistema
├── sys/           # Sistema de archivos virtual (kernel)
├── tmp/           # Archivos temporales
├── usr/           # Aplicaciones de usuario
│   ├── bin/       # Binarios de usuario
│   ├── lib/       # Bibliotecas de usuario
│   └── local/     # Software local
└── var/           # Datos variables (logs, bases de datos)
```

## 7. Particiones

### (a) Definición y tipos:

**Partición**: División lógica de un disco físico que se comporta como unidad independiente.

**Tipos:**

- **Primaria**: Máximo 4 por disco, booteable
- **Extendida**: Contenedor para particiones lógicas (1 por disco)
- **Lógica**: Dentro de partición extendida

**Ventajas:**

- Organización de datos
- Diferentes sistemas de archivos
- Aislamiento de fallos
- Sistemas operativos múltiples

**Desventajas:**

- Complejidad de gestión
- Posible desperdicio de espacio
- Rigidez en tamaños

### (b) Identificación en GNU/Linux:

- **IDE**: `/dev/hda1`, `/dev/hdb2` (obsoleto)
- **SATA/SCSI**: `/dev/sda1`, `/dev/sdb2`
- **NVMe**: `/dev/nvme0n1p1`

**Formato**: `/dev/sdXY`

- **sd**: SCSI/SATA disk
- **X**: Letra del disco (a, b, c...)
- **Y**: Número de partición (1, 2, 3...)

### (c) Particiones mínimas para GNU/Linux:

**Mínimo absoluto: 1 partición**

- `/` (raíz) - Primaria - ext4 - punto montaje /

**Recomendado: 2 particiones**

1. `/` (raíz) - Primaria - ext4 - punto montaje /
2. `swap` - Primaria - swap - área de intercambio

### (d) Ejemplos de particionado:

**Desktop básico:**

- `/` (20GB) - ext4
- `swap` (2GB) - swap

**Servidor web:**

- `/boot` (500MB) - ext4
- `/` (10GB) - ext4
- `/var` (20GB) - ext4 (logs, bases de datos)
- `/home` (50GB) - ext4
- `swap` (4GB) - swap

**Desarrollo:**

- `/boot` (500MB) - ext4
- `/` (15GB) - ext4
- `/home` (100GB) - ext4
- `/var` (10GB) - ext4
- `swap` (8GB) - swap

### (e) Software para particionar:

**Línea de comandos:**

- **fdisk**: Tradicional, MBR y GPT
- **gdisk**: Especializado en GPT
- **parted**: Más moderno, redimensionado

**Gráficos:**

- **GParted**: Interfaz GTK, muy completo
- **KDE Partition Manager**: Para entorno KDE
- **GNOME Disks**: Simple, integrado en GNOME

**Comparación:**

- **fdisk**: Potente pero complejo, destructivo
- **GParted**: Fácil uso, no destructivo, redimensionado
- **parted**: Scripting, automatización

## 8. Arranque (Bootstrap)

### (a) ¿Qué es el BIOS?

**BIOS** (Basic Input/Output System) es el firmware almacenado en ROM que:

- **Inicializa** el hardware al encender
- **Ejecuta POST** (Power-On Self Test)
- **Carga** el bootloader desde el MBR
- **Transfiere** control al sistema operativo

### (b) ¿Qué es UEFI?

**UEFI** (Unified Extensible Firmware Interface) es el sucesor moderno del BIOS:

- **Interfaz gráfica** en lugar de texto
- **Soporte GPT** en lugar de solo MBR
- **Secure Boot** para verificar firma de bootloaders
- **Networking** y funciones avanzadas
- **Boot manager** integrado

### (c) MBR y MBC:

**MBR** (Master Boot Record):

- **Ubicación**: Sector 0 del disco (512 bytes)
- **Contiene**: Código de arranque + tabla de particiones
- **Limitaciones**: 4 particiones primarias, discos <2TB

**MBC** (Master Boot Code):

- **Primeros 446 bytes** del MBR
- **Función**: Código que ejecuta el BIOS
- **Tarea**: Encontrar partición activa y cargar su bootloader

### (d) GPT:

**GPT** (GUID Partition Table) sustituye a MBR:

**Características:**

- **128 particiones** por defecto
- **Soporte discos** >2TB (hasta 8ZB)
- **Redundancia**: Tabla al inicio y final del disco
- **CRC32**: Verificación de integridad

**Formato:**

```
LBA 0: Protective MBR
LBA 1: GPT Header
LBA 2-33: Partition Entry Array
...
LBA -33: Backup Partition Entry Array
LBA -1: Backup GPT Header
```

### (e) Gestor de Arranque:

**Funcionalidad:**

- **Cargar** el kernel del SO
- **Pasar parámetros** al kernel
- **Seleccionar** entre múltiples SOs
- **Proveer interfaz** de selección

**Tipos:**

- **En MBR**: GRUB Legacy
- **En partición**: GRUB2, LILO
- **UEFI**: GRUB2-EFI, systemd-boot

**Gestores conocidos:**

- **GRUB/GRUB2**: Más usado en Linux
- **LILO**: Obsoleto
- **Windows Boot Manager**: Windows
- **rEFInd**: UEFI multiplataforma

**Instalación:**

- **MBR**: Se instala en sector 0 del disco
- **Partición**: Se instala en sector boot de partición
- **UEFI**: Como aplicación en ESP (EFI System Partition)

### (f) Proceso de bootstrap:

1. **Power ON** → Alimentación eléctrica
2. **POST** → BIOS/UEFI verifica hardware
3. **Boot device** → BIOS busca dispositivo booteable
4. **MBR/UEFI** → Carga primer código de arranque
5. **Bootloader** → GRUB carga menú y kernel
6. **Kernel** → Se carga en memoria y toma control
7. **Init** → Kernel ejecuta primer proceso
8. **Sistema** → Se cargan servicios y login

### (g) Arranque en GNU/Linux:

1. **BIOS/UEFI** carga GRUB
2. **GRUB** muestra menú (opcional)
3. **GRUB** carga kernel (`vmlinuz`) e `initrd`
4. **Kernel** se descomprime y ejecuta
5. **initrd** proporciona drivers temporales
6. **Kernel** monta sistema de archivos raíz
7. **Init/systemd** se ejecuta como PID 1
8. **Servicios** se inician según runlevel/targets
9. **Login** se presenta al usuario

### (h) Proceso de shutdown:

1. **Comando shutdown** (`shutdown`, `halt`, `poweroff`)
2. **Señal SIGTERM** a todos los procesos
3. **Espera** terminación grácil de procesos
4. **Señal SIGKILL** a procesos restantes
5. **Unmount** sistemas de archivos
6. **Sync** datos pendientes al disco
7. **Apagar** o reiniciar sistema

### (i) ¿GNU/Linux con otro SO?

**Sí**, es perfectamente posible (dual boot):

**Métodos:**

- **Particiones separadas** para cada SO
- **GRUB** como gestor de arranque
- **Chainloading** para cargar otros bootloaders

**Consideraciones:**

- **Orden instalación**: Windows primero, Linux después
- **GRUB** detecta automáticamente Windows
- **Partición compartida** (FAT32) para intercambio de datos

## 9. Archivos y editores

### (a) Identificación en GNU/Linux:

- **Case sensitive**: `archivo.txt` ≠ `Archivo.TXT`
- **Sin extensiones obligatorias**: El tipo se determina por contenido
- **Nombres largos**: Hasta 255 caracteres
- **Caracteres especiales**: Permitidos (excepto `/` y `\0`)
- **Archivos ocultos**: Comienzan con `.`

### (b) Editores y comandos:

**vim:**

- Editor modal (comando/inserción)
- Comandos básicos:
  - `i`: Entrar modo inserción
  - `Esc`: Salir modo inserción
  - `:w`: Guardar
  - `:q`: Salir
  - `:wq`: Guardar y salir

**nano:**

- Editor simple y amigable
- Ctrl+O: Guardar
- Ctrl+X: Salir
- Ctrl+K: Cortar línea
- Ctrl+U: Pegar

**mcedit:**

- Editor simple, menús desplegables
- F2: Guardar, F10: Salir
- Más amigable para principiantes

**cat:**

- Muestra contenido completo del archivo
- `cat archivo.txt`
- Útil para archivos pequeños

**more:**

- Muestra contenido página por página
- `more archivo.txt`
- Espacio: siguiente página, q: salir

**less:**

- Similar a more pero más funcional
- `less archivo.txt`
- Navegación con flechas, buscar con /

### (c) **Ejercicio Práctico:** Crear archivo "prueba.exe":

```bash
vim prueba.exe
# Presionar 'i' para entrar en modo inserción
# Escribir: Número de alumno: 12345
#          Nombre: Juan Pérez
# Presionar 'Esc' luego ':wq' para guardar y salir
```

### (d) Comando file:

**file** determina el tipo de archivo analizando su contenido:

```bash
file prueba.exe          # ASCII text
file /bin/ls            # ELF 64-bit LSB executable
file imagen.jpg         # JPEG image data
file documento.pdf      # PDF document
```

**Diferencia**: No se basa en extensión sino en el contenido real del archivo.

### (e) Comandos relacionados con archivos:

**cd** - Cambiar directorio

```bash
cd /home/usuario        # Path absoluto
cd ../                  # Directorio padre
cd ~                   # Home del usuario
cd -                   # Directorio anterior
```

**mkdir** - Crear directorio

```bash
mkdir directorio       # Crear directorio
mkdir -p dir1/dir2     # Crear estructura completa
mkdir -m 755 directorio # Crear con permisos específicos
```

**rmdir** - Eliminar directorio vacío

```bash
rmdir directorio       # Solo si está vacío
rmdir -p dir1/dir2     # Eliminar estructura si está vacía
```

**ln** - Crear enlaces

```bash
ln archivo enlace_duro     # Enlace duro
ln -s archivo enlace_soft  # Enlace simbólico
ln -s /ruta/completa enlace # Enlace simbólico absoluto
```

**tail** - Últimas líneas

```bash
tail archivo.txt       # Últimas 10 líneas
tail -n 20 archivo     # Últimas 20 líneas
tail -f archivo.log    # Seguimiento en tiempo real
```

**locate** - Buscar archivos por nombre

```bash
locate filename        # Buscar archivo
locate "*.conf"       # Buscar con patrón
updatedb              # Actualizar base de datos
```

**ls** - Listar contenido

```bash
ls                    # Lista básica
ls -l                 # Lista detallada
ls -la                # Incluye archivos ocultos
ls -lh                # Tamaños legibles
```

**pwd** - Directorio actual

```bash
pwd                   # Mostrar directorio actual
```

**cp** - Copiar archivos

```bash
cp archivo destino    # Copiar archivo
cp -r directorio dest # Copiar directorio recursivamente
cp -p archivo dest    # Conservar permisos y fechas
```

**mv** - Mover/renombrar

```bash
mv archivo nuevo_nombre   # Renombrar
mv archivo /otro/lugar    # Mover archivo
mv directorio /destino    # Mover directorio
```

**find** - Buscar archivos

```bash
find /ruta -name "*.txt"     # Por nombre
find . -type f -size +10M    # Por tamaño
find /home -user juan        # Por propietario
```

## 10. **Ejercicios Prácticos** - Comandos básicos

### (a) Crear carpeta ISOCSO:

```bash
mkdir ISOCSO
```

### (b) Acceder a carpeta:

```bash
cd ISOCSO
```

### (c) Crear archivos:

```bash
touch isocso.txt isocso.csv
```

### (d) Listar contenido:

```bash
ls
ls -l    # Lista detallada
ls -la   # Incluye archivos ocultos
```

### (e) Ver directorio actual:

```bash
pwd
```

### (f) Buscar archivos:

```bash
find . -name "iso*"
find / -name "*iso*" 2>/dev/null
```

### (g) Espacio libre en disco:

```bash
df -h    # En formato legible (KB, MB, GB)
```

### (h) Usuarios conectados:

```bash
who
w        # Más información
users    # Solo nombres
```

### (i) Editar archivo:

```bash
vim isocso.txt
# Presionar 'i', escribir "Nombre: Juan Apellido: Pérez"
# Presionar Esc, escribir :wq
```

### (j) Últimas líneas:

```bash
tail isocso.txt
tail -n 5 isocso.txt      # Últimas 5 líneas
```

## 11. Comandos del sistema - Funcionalidad y ubicación

### **man** - Manual del sistema

- **Ubicación**: `/usr/bin/man`
- **Función**: Mostrar páginas del manual

```bash
man comando           # Manual de un comando
man -k palabra        # Buscar en manuales
man 5 passwd         # Sección específica
```

### **shutdown** - Apagar sistema

- **Ubicación**: `/sbin/shutdown`
- **Función**: Apagar el sistema de forma segura

```bash
shutdown -h now       # Apagar inmediatamente
shutdown -h +10       # Apagar en 10 minutos
shutdown -r now       # Reiniciar ahora
```

### **reboot** - Reiniciar sistema

- **Ubicación**: `/sbin/reboot`
- **Función**: Reinicia el sistema

```bash
reboot               # Reinicio inmediato
reboot -f            # Reinicio forzado
```

### **halt** - Detener sistema

- **Ubicación**: `/sbin/halt`
- **Función**: Detiene el sistema (sin apagar la energía)

```bash
halt                 # Detener sistema
halt -p              # Detener y apagar
```

### **uname** - Información del sistema

- **Ubicación**: `/bin/uname`
- **Función**: Información del sistema

```bash
uname -a             # Toda la información
uname -r             # Versión del kernel
uname -m             # Arquitectura
```

### **dmesg** - Mensajes del kernel

- **Ubicación**: `/bin/dmesg`
- **Función**: Mensajes del kernel

```bash
dmesg                # Todos los mensajes
dmesg | tail         # Últimos mensajes
dmesg | grep error   # Filtrar errores
```

### **lspci** - Dispositivos PCI

- **Ubicación**: `/usr/bin/lspci`
- **Función**: Lista dispositivos PCI

```bash
lspci                # Dispositivos PCI
lspci -v             # Información detallada
```

### **at** - Programar tareas

- **Ubicación**: `/usr/bin/at`
- **Función**: Programa tareas para ejecutar más tarde

```bash
at 15:30             # Ejecutar a las 15:30
at now + 1 hour      # Ejecutar en 1 hora
atq                  # Ver tareas programadas
```

### **netstat** - Estadísticas de red

- **Ubicación**: `/bin/netstat`
- **Función**: Estadísticas de red y conexiones

```bash
netstat -tuln        # Puertos abiertos
netstat -i           # Interfaces de red
netstat -r           # Tabla de rutas
```

### **head** - Primeras líneas

- **Ubicación**: `/usr/bin/head`
- **Función**: Muestra primeras líneas de archivo

```bash
head archivo.txt     # Primeras 10 líneas
head -n 20 archivo   # Primeras 20 líneas
```

### **tail** - Últimas líneas

- **Ubicación**: `/usr/bin/tail`
- **Función**: Muestra últimas líneas de archivo

```bash
tail archivo.txt     # Últimas 10 líneas
tail -n 20 archivo   # Últimas 20 líneas
tail -f archivo.log  # Seguimiento en tiempo real
```

## 12. Procesos

### (a) ¿Qué es un proceso?

Un **proceso** es una instancia de un programa en ejecución. Es la unidad básica de ejecución en el sistema operativo.

**PID** (Process ID): Identificador único del proceso
**PPID** (Parent Process ID): PID del proceso padre

**Sí, todos los procesos en GNU/Linux tienen PID y PPID** (excepto el proceso init que tiene PPID 0).

**Otros atributos de un proceso:**

- **UID/GID**: Usuario y grupo propietario
- **Estado**: Running, Sleeping, Zombie, Stopped
- **Prioridad**: Importancia para el scheduler
- **Uso de CPU y memoria**
- **Directorio de trabajo actual**
- **Variables de entorno**

### (b) Comandos relacionados con procesos:

**top** - Monitor de procesos en tiempo real

- **Ubicación**: `/usr/bin/top`
- **Función**: Muestra procesos activos y recursos del sistema

```bash
top                  # Vista en tiempo real
top -u usuario       # Solo procesos de un usuario
top -p PID           # Solo un proceso específico
```

**htop** - Versión mejorada de top

- **Ubicación**: `/usr/bin/htop` (requiere instalación)
- **Función**: Interfaz más amigable que top

```bash
htop                 # Vista interactiva mejorada
# F9: Kill process, F6: Sort by, F4: Filter
```

**ps** - Lista de procesos

- **Ubicación**: `/bin/ps`
- **Función**: Muestra procesos en ejecución

```bash
ps                   # Procesos de la sesión actual
ps aux               # Todos los procesos detallados
ps -ef               # Formato estándar Unix
ps -u usuario        # Procesos de un usuario
```

**pstree** - Árbol de procesos

- **Ubicación**: `/usr/bin/pstree`
- **Función**: Muestra procesos en forma de árbol

```bash
pstree               # Árbol completo
pstree usuario       # Árbol de un usuario
pstree -p            # Mostrar PIDs
```

**kill** - Terminar procesos

- **Ubicación**: `/bin/kill`
- **Función**: Envía señales a procesos

```bash
kill PID             # SIGTERM (terminación grácil)
kill -9 PID          # SIGKILL (terminación forzada)
kill -HUP PID        # SIGHUP (reiniciar)
```

**pgrep** - Buscar procesos por nombre

- **Ubicación**: `/usr/bin/pgrep`
- **Función**: Encuentra PIDs por nombre de proceso

```bash
pgrep firefox        # PIDs de procesos firefox
pgrep -u usuario     # PIDs de procesos de usuario
```

**pkill** - Matar procesos por nombre

- **Ubicación**: `/usr/bin/pkill`
- **Función**: Mata procesos por nombre

```bash
pkill firefox        # Mata todos los firefox
pkill -u usuario     # Mata procesos de usuario
```

**killall** - Matar procesos por nombre

- **Ubicación**: `/usr/bin/killall`
- **Función**: Similar a pkill

```bash
killall firefox      # Mata todos los firefox
killall -i firefox   # Pregunta antes de matar
```

**nice** - Ejecutar con prioridad

- **Ubicación**: `/usr/bin/nice`
- **Función**: Ejecuta comando con prioridad específica

```bash
nice -n 10 comando   # Ejecutar con baja prioridad
nice -n -5 comando   # Ejecutar con alta prioridad
```

**renice** - Cambiar prioridad

- **Ubicación**: `/usr/bin/renice`
- **Función**: Cambia prioridad de proceso existente

```bash
renice 10 PID        # Cambiar prioridad a 10
renice -5 -u usuario # Cambiar para todos los procesos de usuario
```

**xkill** - Matar procesos gráficos

- **Ubicación**: `/usr/bin/xkill`
- **Función**: Mata procesos gráficos clickeando en ventana

```bash
xkill                # Click en ventana para matarla
```

**atop** - Monitor avanzado

- **Ubicación**: `/usr/bin/atop` (requiere instalación)
- **Función**: Monitor de recursos muy detallado

```bash
atop                 # Vista detallada del sistema
atop -1              # Actualización cada segundo
```

## 13. Proceso de Arranque SystemV

### (a) Pasos del proceso de inicio:

1. **Encendido** → BIOS/UEFI ejecuta POST
2. **Bootloader** → GRUB carga kernel
3. **Kernel** → Se carga en memoria y toma control
4. **Init** → Kernel ejecuta /sbin/init (PID 1)
5. **Runlevel** → Init lee /etc/inittab para determinar runlevel
6. **Scripts RC** → Se ejecutan scripts según runlevel
7. **Servicios** → Se inician demonios y servicios
8. **Getty** → Se inician terminales de login
9. **Login** → Sistema listo para usuarios

### (b) Proceso INIT:

**¿Quién lo ejecuta?** El kernel automáticamente al finalizar su inicialización.

**Objetivo:** Ser el proceso padre de todos los procesos del sistema y gestionar el sistema de inicialización según runlevels.

### (c) RunLevels:

**¿Qué son?** Estados o modos de operación del sistema que determinan qué servicios están activos.

**Objetivo:** Permitir diferentes configuraciones del sistema según el uso previsto.

### (d) Niveles de ejecución estándar:

- **0**: Halt (apagar sistema)
- **1**: Single-user mode (modo mantenimiento)
- **2**: Multi-user sin red (Debian/Ubuntu: completo)
- **3**: Multi-user con red (modo texto)
- **4**: Sin usar (definible por usuario)
- **5**: Multi-user con red y entorno gráfico
- **6**: Reboot (reiniciar sistema)

**¿Dónde se define?** En `/etc/inittab` línea `id:X:initdefault:`

**¿Todas las distribuciones respetan?** No exactamente. Debian/Ubuntu usan 2-5 igual, Red Hat diferencia 3 y 5.

### (e) Archivo /etc/inittab:

**Finalidad:** Controlar el proceso de inicialización y definir qué hacer en cada runlevel.

**Información almacenada:**

- Runlevel por defecto
- Acciones para cada runlevel
- Terminales getty
- Acciones especiales (Ctrl+Alt+Del)

**Estructura:** `id:runlevels:action:process`

- **id**: Identificador único (4 caracteres max)
- **runlevels**: En qué runlevels actuar
- **action**: Qué tipo de acción
- **process**: Comando a ejecutar

**Ejemplo:**

```
id:3:initdefault:
si::sysinit:/etc/rc.d/rc.sysinit
l0:0:wait:/etc/rc.d/rc 0
l1:1:wait:/etc/rc.d/rc 1
1:2345:respawn:/sbin/mingetty tty1
```

### (f) Cambio de runlevel:

**Desde runlevel X a runlevel Y:**

```bash
init Y              # Cambio inmediato
telinit Y           # Alternativa a init
```

**¿Es permanente?** No, solo para la sesión actual. Para cambio permanente se debe editar `/etc/inittab`.

### (g) Scripts RC:

**Finalidad:** Iniciar y detener servicios según el runlevel.

**Ubicación:**

- **Scripts principales**: `/etc/rc.d/` o `/etc/init.d/`
- **Enlaces por runlevel**: `/etc/rc.d/rcX.d/` o `/etc/rcX.d/`

**Determinación de scripts:**

- **Enlaces simbólicos** en `/etc/rcX.d/` apuntan a scripts en `/etc/init.d/`
- **Nomenclatura**: `SXXnombre` (Start) o `KXXnombre` (Kill)
- **XX**: Número de orden (00-99)

**Orden de ejecución:**

1. Primero scripts K (Kill) en orden numérico
2. Después scripts S (Start) en orden numérico
3. Los números determinan la secuencia de ejecución

## 14. SystemD

### (a) ¿Qué es systemd?

**systemd** es un sistema de inicialización y gestor de servicios moderno que reemplaza a SystemV init. Proporciona paralelización de servicios, activación bajo demanda y gestión avanzada de dependencias.

### (b) Concepto de Unit:

Una **Unit** es la unidad básica de trabajo en systemd. Tipos principales:

- **service**: Servicios del sistema
- **target**: Grupos de units (equivalente a runlevels)
- **mount**: Puntos de montaje
- **socket**: Sockets de red o IPC
- **timer**: Tareas programadas
- **device**: Dispositivos hardware

### (c) Comando systemctl:

**systemctl** es la herramienta principal para controlar systemd:

```bash
systemctl status servicio    # Estado del servicio
systemctl start servicio     # Iniciar servicio
systemctl stop servicio      # Detener servicio
systemctl restart servicio   # Reiniciar servicio
systemctl enable servicio    # Habilitar al arranque
systemctl disable servicio   # Deshabilitar al arranque
systemctl list-units         # Listar units activas
systemctl daemon-reload      # Recargar configuración
```

### (d) Concepto de target:

Los **targets** son el equivalente moderno a los runlevels. Agrupan units que deben estar activas para un estado del sistema:

- **poweroff.target**: Equivalente a runlevel 0
- **rescue.target**: Equivalente a runlevel 1
- **multi-user.target**: Equivalente a runlevel 3
- **graphical.target**: Equivalente a runlevel 5
- **reboot.target**: Equivalente a runlevel 6

### (e) **Ejercicio Práctico:** Comando pstree:

```bash
pstree
```

**Observación:** Se puede ver el árbol de procesos con systemd como proceso raíz (PID 1), y todos los demás procesos como sus descendientes, mostrando la jerarquía parent-child de todos los procesos del sistema.

## 15. Usuarios

### (a) Archivos de información de usuarios:

- **`/etc/passwd`**: Información básica de usuarios
- **`/etc/shadow`**: Contraseñas encriptadas y políticas
- **`/etc/group`**: Información de grupos
- **`/etc/gshadow`**: Contraseñas de grupos (si existen)

### (b) UID y GID:

**UID** (User ID): Identificador numérico único del usuario
**GID** (Group ID): Identificador numérico único del grupo

**¿Pueden coexistir UIDs iguales?** Técnicamente sí, pero causa conflictos. El sistema los tratará como el mismo usuario, lo cual no es recomendable.

### (c) Usuario root:

**root** es el superusuario con privilegios administrativos completos.

**¿Más de un usuario con perfil root?** Sí, cualquier usuario con UID 0 tiene privilegios de root.

**UID de root:** 0 (siempre)

### (d) **Ejercicio Práctico:** Gestión de usuarios:

```bash
# Crear grupo informatica
sudo groupadd informatica

# Crear usuario isocso
sudo useradd -d /home/isocso -m -g informatica isocso

# Establecer contraseña
sudo passwd isocso

# Crear archivo en su home
sudo touch /home/isocso/archivo_test
sudo chown isocso:informatica /home/isocso/archivo_test

# Eliminar usuario
sudo userdel -r isocso

# Eliminar grupo
sudo groupdel informatica

# Verificar eliminación
grep isocso /etc/passwd /etc/shadow /etc/group
```

### (e) Comandos de gestión de usuarios:

**useradd/adduser** - Agregar usuario

- **Ubicación**: `/usr/sbin/useradd`, `/usr/sbin/adduser`

```bash
useradd -d /home/user -m -s /bin/bash usuario
adduser usuario                    # Más interactivo
```

**usermod** - Modificar usuario

- **Ubicación**: `/usr/sbin/usermod`

```bash
usermod -d /new/home -m usuario    # Cambiar home
usermod -g grupo usuario           # Cambiar grupo primario
usermod -a -G grupos usuario       # Agregar a grupos
```

**userdel** - Eliminar usuario

- **Ubicación**: `/usr/sbin/userdel`

```bash
userdel usuario                    # Solo eliminar usuario
userdel -r usuario                 # Eliminar usuario y home
```

**su** - Cambiar usuario

- **Ubicación**: `/bin/su`

```bash
su                                 # Cambiar a root
su usuario                         # Cambiar a usuario
su -                              # Login shell completo
```

**groupadd** - Crear grupo

- **Ubicación**: `/usr/sbin/groupadd`

```bash
groupadd grupo                     # Crear grupo
groupadd -g 1001 grupo            # Con GID específico
```

**groupdel** - Eliminar grupo

- **Ubicación**: `/usr/sbin/groupdel`

```bash
groupdel grupo                     # Eliminar grupo
```

**who** - Usuarios conectados

- **Ubicación**: `/usr/bin/who`

```bash
who                               # Usuarios conectados
who -u                            # Con información detallada
```

**passwd** - Cambiar contraseña

- **Ubicación**: `/usr/bin/passwd`

```bash
passwd                            # Cambiar propia contraseña
passwd usuario                    # Cambiar contraseña de usuario (root)
```

## 16. FileSystem y permisos

### (a) Definición de permisos:

Los permisos en GNU/Linux se definen mediante 3 grupos de 3 bits cada uno:

- **Usuario (u)**: Propietario del archivo
- **Grupo (g)**: Grupo al que pertenece el archivo
- **Otros (o)**: Todos los demás usuarios

**Tipos de permisos:**

- **r (4)**: Lectura (read)
- **w (2)**: Escritura (write)
- **x (1)**: Ejecución (execute)

### (b) Comandos de permisos:

**chmod** - Cambiar permisos

- **Ubicación**: `/bin/chmod`

```bash
chmod 755 archivo                 # Notación octal
chmod u+x archivo                 # Notación simbólica
chmod g-w,o+r archivo            # Múltiples cambios
chmod -R 644 directorio          # Recursivo
```

**chown** - Cambiar propietario

- **Ubicación**: `/bin/chown`

```bash
chown usuario archivo            # Cambiar usuario
chown usuario:grupo archivo      # Cambiar usuario y grupo
chown :grupo archivo            # Solo cambiar grupo
chown -R usuario directorio     # Recursivo
```

**chgrp** - Cambiar grupo

- **Ubicación**: `/bin/chgrp`

```bash
chgrp grupo archivo             # Cambiar grupo
chgrp -R grupo directorio       # Recursivo
```

### (c) Notación octal:

La **notación octal** representa permisos con 3 dígitos:

- **Primer dígito**: Permisos del usuario
- **Segundo dígito**: Permisos del grupo
- **Tercer dígito**: Permisos de otros

**Valores:**

- **4**: Lectura (r)
- **2**: Escritura (w)
- **1**: Ejecución (x)
- **0**: Sin permisos

**Ejemplos:**

- **755**: rwxr-xr-x (usuario: rwx, grupo: r-x, otros: r-x)
- **644**: rw-r--r-- (usuario: rw-, grupo: r--, otros: r--)
- **777**: rwxrwxrwx (todos los permisos para todos)

### (d) Acceso sin permisos:

**Sí**, el usuario **root** puede acceder a cualquier archivo independientemente de los permisos. Es la única excepción al sistema de permisos.

**Prueba:**

```bash
# Como usuario normal
echo "contenido" > archivo_test
chmod 000 archivo_test
cat archivo_test                 # Error: Permission denied

# Como root
sudo cat archivo_test            # Funciona sin problemas
```

### (e) Path absoluto vs relativo:

**Full path name (absoluto):** Ruta completa desde la raíz (/)

- Ejemplo: `/home/usuario/documentos/archivo.txt`
- Siempre comienza con `/`

**Relative path name (relativo):** Ruta desde el directorio actual

- Ejemplo: `documentos/archivo.txt` o `../usuario/archivo.txt`
- No comienza con `/`

### (f) Directorio actual y navegación:

**Comando para directorio actual:**

```bash
pwd                              # Print Working Directory
```

**Acceso al directorio personal:**

```bash
cd ~                            # Usando tilde
cd $HOME                        # Usando variable
cd                              # Sin argumentos
```

**Acceso a otros directorios:**

```bash
cd ~usuario                     # Home de otro usuario
cd ~root                        # Home de root
cd /usr/share/~usuario          # Combinaciones
```

### (g) Comandos del FileSystem:

**mount** - Montar sistemas de archivos

- **Ubicación**: `/bin/mount`

```bash
mount /dev/sda1 /mnt            # Montar partición
mount -t ext4 /dev/sdb1 /media  # Especificar tipo
mount -o ro /dev/sdc1 /tmp      # Solo lectura
mount                           # Ver montajes actuales
```

**umount** - Desmontar sistemas de archivos

- **Ubicación**: `/bin/umount`

```bash
umount /mnt                     # Desmontar por punto
umount /dev/sda1               # Desmontar por dispositivo
umount -f /mnt                 # Forzar desmontaje
```

**df** - Espacio en disco

- **Ubicación**: `/bin/df`

```bash
df                             # Espacio de todos los FS
df -h                          # En formato legible
df /home                       # De un directorio específico
```

**du** - Uso de disco

- **Ubicación**: `/usr/bin/du`

```bash
du directorio                  # Uso de directorio
du -h directorio               # En formato legible
du -s directorio               # Solo resumen
du -sh *                       # Resumen de todos los items
```

**mkfs** - Crear sistema de archivos

- **Ubicación**: `/sbin/mkfs*`

```bash
mkfs.ext4 /dev/sda1           # Crear ext4
mkfs.fat /dev/sdb1            # Crear FAT
mkfs -t ext3 /dev/sdc1        # Especificar tipo
```

**fdisk** ⚠️ - Gestionar particiones

- **Ubicación**: `/sbin/fdisk`

```bash
fdisk -l                      # Listar particiones
fdisk /dev/sda               # Editar particiones
# p: mostrar, n: nueva, d: eliminar, w: guardar, q: salir
```

**write** - Enviar mensajes

- **Ubicación**: `/usr/bin/write`

```bash
write usuario                 # Escribir a usuario
write usuario pts/1           # Especificar terminal
```

**losetup** - Dispositivos loop

- **Ubicación**: `/sbin/losetup`

```bash
losetup /dev/loop0 imagen.iso # Asociar imagen
losetup -d /dev/loop0        # Desasociar
losetup -a                   # Ver asociaciones
```

**stat** - Estadísticas de archivo

- **Ubicación**: `/usr/bin/stat`

```bash
stat archivo                  # Información completa
stat -f archivo              # Información del filesystem
```

## 17. Procesos - Background y Foreground

### (a) Background vs Foreground:

**Foreground:** Proceso que controla la terminal y recibe entrada del usuario.
**Background:** Proceso que ejecuta sin controlar la terminal, permitiendo usar la shell.

### (b) Ejecución en Background:

**Ejecutar en background:**

```bash
comando &                     # Ejecutar en background
nohup comando &              # Ejecutar independiente de terminal
```

**Pasar de foreground a background:**

```bash
Ctrl+Z                       # Suspender proceso
bg                          # Continuar en background
```

**Pasar de background a foreground:**

```bash
fg                          # Traer último proceso
fg %1                       # Traer proceso específico
jobs                        # Ver procesos en background
```

### (c) Pipe (|):

**Finalidad:** Conectar la salida de un comando con la entrada de otro.

**Ejemplos:**

```bash
ls -l | grep archivo         # Buscar archivo en listado
ps aux | grep usuario        # Procesos de usuario
cat archivo | wc -l          # Contar líneas
history | tail -10           # Últimos 10 comandos
dmesg | less                # Ver mensajes del kernel paginado
```

### (d) Redirección:

**Tipos de redirección:**

**Salida estándar (stdout):**

```bash
comando > archivo           # Redirigir y sobrescribir
comando >> archivo          # Redirigir y append
```

**Error estándar (stderr):**

```bash
comando 2> archivo          # Solo errores
comando 2>> archivo         # Errores en append
```

**Combinadas:**

```bash
comando > archivo 2>&1      # Salida y errores al mismo archivo
comando &> archivo          # Equivalente más corto
comando 2>/dev/null         # Descartar errores
```

**Entrada estándar (stdin):**

```bash
comando < archivo           # Tomar entrada de archivo
comando << EOF              # Here document
texto
EOF
```

## 18. Otros comandos de Linux

### (a) Concepto de empaquetar:

**Empaquetar** es reunir múltiples archivos y directorios en un solo archivo contenedor, sin compresión necesariamente. Facilita la transferencia y gestión de conjuntos de archivos.

### (b) **Ejercicio Práctico:** Empaquetar archivos:

```bash
# Crear archivos de prueba
echo "archivo1" > test1.txt
echo "archivo2" > test2.txt
echo "archivo3" > test3.txt
echo "archivo4" > test4.txt

# Ver tamaño individual
ls -lh test*.txt
du -sh test*.txt

# Crear archivo empaquetado
tar -cf archivos.tar test*.txt

# Comparar tamaños
ls -lh archivos.tar
du -sh archivos.tar test*.txt
```

**Característica notada:** El archivo .tar puede ser ligeramente mayor que la suma debido a metadatos (headers), pero facilita el manejo de múltiples archivos como uno solo.

### (c) **Ejercicio Práctico:** Comprimir archivos:

```bash
# Secuencia de comandos para comprimir 4 archivos:
tar -cf archivos.tar test1.txt test2.txt test3.txt test4.txt
gzip archivos.tar
# Resultado: archivos.tar.gz
```

### (d) Compresión en un comando:

**Sí, se pueden comprimir múltiples archivos con un solo comando:**

```bash
tar -czf archivos.tar.gz test1.txt test2.txt test3.txt test4.txt
tar -cjf archivos.tar.bz2 test*.txt        # Con bzip2
tar -cJf archivos.tar.xz test*.txt         # Con xz
```

### (e) Funcionalidad de comandos:

**tar** - Archivador

- **Ubicación**: `/bin/tar`

```bash
tar -cf archivo.tar files     # Crear archivo
tar -xf archivo.tar           # Extraer archivo
tar -czf archivo.tar.gz files # Crear y comprimir con gzip
tar -xzf archivo.tar.gz       # Extraer comprimido
tar -tf archivo.tar           # Listar contenido
```

**grep** - Buscar patrones

- **Ubicación**: `/bin/grep`

```bash
grep "patrón" archivo         # Buscar en archivo
grep -r "patrón" directorio   # Búsqueda recursiva
grep -i "patrón" archivo      # Case insensitive
grep -v "patrón" archivo      # Invertir búsqueda
grep -n "patrón" archivo      # Mostrar números de línea
```

**gzip** - Comprimir archivos

- **Ubicación**: `/bin/gzip`

```bash
gzip archivo                  # Comprimir archivo
gzip -d archivo.gz           # Descomprimir
gunzip archivo.gz            # Descomprimir alternativo
gzip -9 archivo              # Máxima compresión
```

**zgrep** - Grep en archivos comprimidos

- **Ubicación**: `/bin/zgrep`

```bash
zgrep "patrón" archivo.gz    # Buscar en archivo comprimido
zgrep -i "patrón" *.gz       # Case insensitive en comprimidos
```

**wc** - Contar palabras/líneas

- **Ubicación**: `/usr/bin/wc`

```bash
wc archivo                   # Líneas, palabras, caracteres
wc -l archivo               # Solo líneas
wc -w archivo               # Solo palabras
wc -c archivo               # Solo caracteres
```

## 19. **Ejercicio Práctico:** Análisis de comandos

Analizando la secuencia desde usuario no-root:

```bash
ls -l > prueba              # ✓ Redirige listado a archivo 'prueba'
ps > PRUEBA                 # ✓ Redirige procesos a archivo 'PRUEBA'
chmod 710 prueba            # ✓ Cambia permisos: rwx--x---
chown root:root PRUEBA      # ✗ ERROR: Solo root puede cambiar a root
chmod 777 PRUEBA            # ✗ ERROR: No es propietario después del chown fallido
chmod 700 /etc/passwd       # ✗ ERROR: No es propietario de /etc/passwd
passwd root                 # ✗ ERROR: Solo root puede cambiar password de root
rm PRUEBA                   # ✓ Puede eliminar su propio archivo
man /etc/shadow             # ✓ Muestra manual de shadow
find / -name *.conf         # ✓ Busca archivos .conf (muchos errores por permisos)
usermod root -d /home/newroot -L  # ✗ ERROR: Solo root puede modificar usuarios
cd /root                    # ✗ ERROR: Sin permisos para acceder a /root
rm *                        # ✗ No ejecuta porque cd falló
cd /etc                     # ✓ Cambia a /etc
cp * /home -R               # ✗ ERROR: Muchos archivos requieren permisos de root
shutdown                    # ✗ ERROR: Solo root puede apagar sistema
```

## 20. **Ejercicio Práctico:** Comandos necesarios

### (a) Terminar proceso PID 23:

```bash
kill 23
```

### (b) Terminar init/systemd:

```bash
kill 1
```

**Resultado:** Error - el sistema no permite matar el proceso init/systemd pues causaría kernel panic.

### (c) Buscar archivos .conf:

```bash
find /home -name "*.conf" 2>/dev/null
```

### (d) Guardar lista de procesos:

```bash
ps aux > /home/<usuario>/procesos
```

### (e) Cambiar permisos de xxxx (rwxr-x--x):

```bash
chmod 751 /home/<usuario>/xxxx
```

### (f) Cambiar permisos de yyyy (rw-r-----):

```bash
chmod 640 /home/<usuario>/yyyy
```

### (g) Borrar archivos de /tmp:

```bash
rm /tmp/*
```

### (h) Cambiar propietario:

```bash
chown isocso /opt/isodata
```

### (i) Guardar directorio actual (append):

```bash
pwd >> /home/<usuario>/donde
```

## 21. **Ejercicio Práctico:** Gestión completa de usuarios

### (a) Ingresar como root:

```bash
su -
# o
sudo su -
```

### (b) Crear usuario (ej: jperez):

```bash
useradd -m -d /home/jperez -s /bin/bash jperez
passwd jperez
```

### (c) Archivos modificados:

- `/etc/passwd` - Información del usuario
- `/etc/shadow` - Contraseña encriptada
- `/etc/group` - Si se creó grupo nuevo
- Directorio creado: `/home/jperez`

### (d) Crear directorio:

```bash
mkdir /tmp/miCursada
```

### (e) Copiar archivos:

```bash
cp -r /var/log/* /tmp/miCursada/
```

### (f) Cambiar propietario y grupo:

```bash
chown -R jperez:users /tmp/miCursada
```

### (g) Cambiar permisos recursivamente:

```bash
chmod -R 753 /tmp/miCursada
```

### (h) Nueva terminal:

```bash
# En nueva terminal
su - jperez
```

### (i) Nombre de terminal:

```bash
tty
```

### (j) Procesos activos:

```bash
ps aux | wc -l
```

### (k) Usuarios conectados:

```bash
who | wc -l
```

### (l) Enviar mensaje:

```bash
# Desde terminal de root
write jperez
# Escribir mensaje sobre apagado
```

### (m) Apagar sistema:

```bash
shutdown -h now
```

## 22. **Ejercicio Práctico:** Trabajo con archivos

### (n) Crear directorio con número de legajo:

```bash
mkdir 12345
cd 12345
```

### (o) Crear archivo con vi:

```bash
vi LEAME
# Escribir información personal
```

### (p) Cambiar permisos LEAME:

```bash
chmod 017 LEAME
# Dueño: --- (ningún permiso)
# Grupo: --x (ejecución)
# Otros: rwx (todos los permisos)
```

### (q) Ir a /etc y crear listado:

```bash
cd /etc
ls -la > ~/12345/leame
```

**Razón:** Linux es case-sensitive, "LEAME" ≠ "leame"

### (r) Localizar archivos en filesystem:

**Un archivo específico:**

```bash
find / -name "archivo" 2>/dev/null
locate archivo
```

**Múltiples archivos similares:**

```bash
find / -name "*.conf" 2>/dev/null
find / -type f -name "*log*" 2>/dev/null
```

**Concepto teórico:**

- **find**: Busca en tiempo real recorriendo directorios
- **locate**: Busca en base de datos preindexada (más rápido)

### (s) Buscar archivos .so:

```bash
find / -name "*.so" 2>/dev/null > ~/12345/ejercicioF
```

## 23. **Ejercicio Práctico:** Secuencia de comandos detallada

### Análisis paso a paso:

```bash
01. mkdir iso                    # ✓ Crea directorio 'iso'
02. cd ./iso; ps > f0           # ✓ Entra a iso y guarda procesos en f0
03. ls > f1                     # ✓ Lista contenido en f1
04. cd /                        # ✓ Va a raíz del sistema
05. echo $HOME                  # ✓ Muestra directorio home del usuario
06. ls -l $> $HOME/iso/ls       # ✗ SINTAXIS ERROR: $> no válido
07. cd $HOME; mkdir f2          # ✓ Va a home y crea directorio f2
08. ls -ld f2                   # ✓ Lista información de f2
09. chmod 341 f2                # ✓ Cambia permisos: -wx-r---x
10. touch dir                   # ✓ Crea archivo 'dir'
11. cd f2                       # ✗ ERROR: Sin permisos de ejecución para user
12. cd ~/iso                    # ✓ Va a directorio iso
13. pwd > f3                    # ✓ Guarda directorio actual en f3
14. ps | grep 'ps' | wc -l >> ../f2/f3  # ✗ ERROR: No puede escribir en f2
15. chmod 700 ../f2 ; cd ..     # ✓ Cambia permisos de f2 y sube directorio
16. find . -name etc/passwd     # ✓ Busca etc/passwd (probablemente sin resultado)
17. find / -name etc/passwd     # ✗ ERROR: Busca archivo llamado "etc/passwd"
18. mkdir ejercicio5            # ✓ Crea directorio ejercicio5
```

### (a) Explicación detallada:

**Ejercicio práctico:** Ejecutar en una terminal y documentar en otra:

```bash
# En terminal 1: ejecutar comandos paso a paso
# En terminal 2: crear archivo de explicación
vi ~/12345/explicacion_de_ejercicio
```

### (b) Comandos 19 y 20 completados:

```bash
19. cp -r iso ejercicio5/       # Copiar directorio iso al ejercicio5
20. cp f2 dir ejercicio5/       # Copiar f2 y dir al ejercicio5
```

### (c) Ejecutar y comentar:

```bash
# Ejecutar comandos 19 y 20 y documentar resultados
cp -r iso ejercicio5/
cp f2 dir ejercicio5/
# Comentar en el archivo de explicación los resultados
```

## 24. **Ejercicio Práctico:** Estructura de directorios

### Esquema de estructura a crear:

```
/home/usuario/
├── dir1/
│   ├── f1
│   ├── f2
│   └── dir11/
│       ├── f3
│       └── f4
├── dir2/
│   └── f5
└── dir3/
    └── f6
```

### Comandos para crear la estructura:

```bash
cd /home/usuario
mkdir -p dir1/dir11 dir2 dir3
touch dir1/f1 dir1/f2 dir1/dir11/f3 dir1/dir11/f4 dir2/f5 dir3/f6
```

### Acciones solicitadas:

### (a) Mover f3 a /home/usuario:

```bash
mv dir1/dir11/f3 /home/usuario/
```

### (b) Copiar f4 a dir11:

```bash
cp dir1/dir11/f4 dir1/dir11/
# (Ya está en dir11, sería redundante)
```

### (c) Copiar f4 a dir11 con nombre f7:

```bash
cp dir1/dir11/f4 dir1/dir11/f7
```

### (d) Crear directorio copia y copiar contenido de dir1:

```bash
mkdir copia
cp -r dir1/* copia/
```

### (e) Renombrar f1 a archivo y ver permisos:

```bash
mv dir1/f1 dir1/archivo
ls -l dir1/archivo
```

### (f) Cambiar permisos de archivo (rw---xrwx):

```bash
chmod 607 dir1/archivo
```

### (g) Renombrar f3 y f4:

```bash
mv f3 f3.exe
mv dir1/dir11/f4 dir1/dir11/f4.exe
```

### (h) Cambiar permisos de archivos .exe (---w-wx):

```bash
chmod 023 f3.exe dir1/dir11/f4.exe
```

## 25. **Ejercicio Práctico:** Empaquetado y compresión

### (a) Crear directorio logs:

```bash
mkdir /tmp/logs
```

### (b) Copiar contenido de /var/log:

```bash
cp -r /var/log/* /tmp/logs/
```

### (c) Empaquetar directorio:

```bash
cd /tmp
tar -cf misLogs.tar logs/
```

### (d) Empaquetar y comprimir:

```bash
tar -czf misLogs.tar.gz logs/
```

### (e) Copiar archivos al home:

```bash
cp misLogs.tar misLogs.tar.gz ~/
```

### (f) Eliminar directorio logs:

```bash
rm -rf /tmp/logs
```

### (g) Desempaquetar en directorios diferentes:

```bash
cd ~
mkdir desempaquetado1 desempaquetado2
cd desempaquetado1
tar -xf ../misLogs.tar
cd ../desempaquetado2
tar -xzf ../misLogs.tar.gz
```

## **Comandos adicionales importantes no cubiertos:**

### Comandos de red y sistema:

```bash
# Información de red
ip addr show                    # Interfaces de red (reemplaza ifconfig)
ip route show                   # Tabla de rutas
ping -c 4 google.com           # Prueba de conectividad
wget https://ejemplo.com       # Descargar archivos
curl -O https://ejemplo.com    # Alternativa a wget

# Información de hardware
lscpu                          # Información de CPU
lsblk                          # Dispositivos de bloque
lsusb                          # Dispositivos USB
free -h                        # Memoria RAM
uptime                         # Tiempo de funcionamiento

# Monitoreo de sistema
iostat                         # Estadísticas de E/S
vmstat                         # Estadísticas de memoria virtual
sar                           # Actividad del sistema
```

### Comandos de texto avanzados:

```bash
# Procesamiento de texto
sed 's/old/new/g' archivo     # Reemplazar texto
awk '{print $1}' archivo      # Procesar columnas
sort archivo                  # Ordenar líneas
uniq archivo                  # Eliminar duplicados
cut -d: -f1 /etc/passwd       # Extraer columnas

# Comparación de archivos
diff archivo1 archivo2        # Diferencias entre archivos
comm archivo1 archivo2        # Comparar archivos ordenados
cmp archivo1 archivo2         # Comparación binaria
```

### Comandos de compresión adicionales:

```bash
# Otros formatos de compresión
bzip2 archivo                 # Comprimir con bzip2
bunzip2 archivo.bz2           # Descomprimir bzip2
xz archivo                    # Comprimir con xz
unxz archivo.xz               # Descomprimir xz
zip archivo.zip files         # Crear archivo ZIP
unzip archivo.zip             # Extraer archivo ZIP
```

### Variables de entorno importantes:

```bash
# Variables del sistema
echo $PATH                    # Ruta de búsqueda de comandos
echo $HOME                    # Directorio home
echo $USER                    # Usuario actual
echo $PWD                     # Directorio actual
echo $SHELL                   # Shell actual

# Configurar variables
export VARIABLE=valor         # Exportar variable
env                          # Ver todas las variables
printenv                     # Alternativa a env
```

---

**Esta guía completa cubre todos los aspectos de la Práctica 1, incluyendo:**

- ✅ Conceptos teóricos fundamentales
- ✅ Ejercicios prácticos paso a paso
- ✅ Comandos con ubicaciones y parámetros
- ✅ Ejemplos de uso real
- ✅ Análisis detallado de secuencias de comandos
- ✅ Comandos adicionales importantes

**Total de comandos cubiertos:** Más de 80 comandos con explicaciones detalladas y ejemplos prácticos.
