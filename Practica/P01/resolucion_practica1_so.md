# Resolución - Práctica 1: Conceptos Básicos GNU/Linux

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

| Característica | GNU/Linux | Windows | macOS |
|---|---|---|---|
| **Costo** | Gratuito | Licencia paga | Incluido con hardware |
| **Código fuente** | Abierto | Cerrado | Cerrado |
| **Personalización** | Alta | Media | Baja |
| **Estabilidad** | Muy alta | Media-Alta | Alta |
| **Seguridad** | Muy alta | Media | Alta |
| **Hardware** | Amplio soporte | Amplio soporte | Solo Apple |

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
┌─────────────────────────────────────┐
│           APLICACIONES              │
├─────────────────────────────────────┤
│      SHELL (CLI/GUI)                │
├─────────────────────────────────────┤
│    HERRAMIENTAS DEL SISTEMA         │
├─────────────────────────────────────┤
│         KERNEL                      │
├─────────────────────────────────────┤
│         HARDWARE                    │
└─────────────────────────────────────┘
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

## 9. Archivos

### (a) Identificación en GNU/Linux:

- **Case sensitive**: `archivo.txt` ≠ `Archivo.TXT`
- **Sin extensiones obligatorias**: El tipo se determina por contenido
- **Nombres largos**: Hasta 255 caracteres
- **Caracteres especiales**: Permitidos (excepto `/` y `\0`)
- **Archivos ocultos**: Comienzan con `.`

### (b) Editores y comandos:

**vi (vim):**
- Editor modal (comando/inserción)
- Comandos básicos:
  - `i`: Entrar modo inserción
  - `Esc`: Salir modo inserción
  - `:w`: Guardar
  - `:q`: Salir
  - `:wq`: Guardar y salir

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

### (c) Crear archivo "prueba.exe":

```bash
vi prueba.exe
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

## 10. Comandos básicos

### (a) Crear carpeta:
```bash
mkdir ISO2017
```

### (b) Acceder a carpeta:
```bash
cd ISO2017
```

### (c) Crear archivos:
```bash
touch iso2017-1 iso2017-2
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
vi iso2017-1
# o
nano iso2017-1
```

### (j) Últimas líneas:
```bash
tail archivo.txt
tail -n 20 archivo.txt    # Últimas 20 líneas
tail -f archivo.log       # Seguimiento en tiempo real
```

## 11. Comandos del sistema

### (a) **shutdown**
Apaga el sistema de forma segura
```bash
shutdown -h now     # Apagar inmediatamente
shutdown -h +10     # Apagar en 10 minutos
shutdown -r now     # Reiniciar ahora
```

### (b) **reboot**
Reinicia el sistema
```bash
reboot              # Reinicio inmediato
reboot -f           # Reinicio forzado
```

### (c) **halt**
Detiene el sistema (sin apagar la energía)
```bash
halt                # Detener sistema
halt -p             # Detener y apagar
```

### (d) **locate**
Busca archivos por nombre usando base de datos
```bash
locate filename     # Buscar archivo
updatedb           # Actualizar base de datos
```

### (e) **uname**
Información del sistema
```bash
uname -a           # Toda la información
uname -r           # Versión del kernel
uname -m           # Arquitectura
```

### (f) **dmesg**
Mensajes del kernel
```bash
dmesg              # Todos los mensajes
dmesg | tail       # Últimos mensajes
dmesg | grep error # Filtrar errores
```

### (g) **lspci**
Lista dispositivos PCI
```bash
lspci              # Dispositivos PCI
lspci -v           # Información detallada
```

### (h) **at**
Programa tareas para ejecutar más tarde
```bash
at 15:30           # Ejecutar a las 15:30
at now + 1 hour    # Ejecutar en 1 hora
atq                # Ver tareas programadas
```

### (i) **netstat**
Estadísticas de red y conexiones
```bash
netstat -tuln      # Puertos abiertos
netstat -i         # Interfaces de red
netstat -r         # Tabla de rutas
```

### (j) **mount**
Monta sistemas de archivos
```bash
mount /dev/sda1 /mnt/disco    # Montar partición
mount -t ntfs /dev/sdb1 /mnt  # Especificar tipo
mount                         # Ver montajes actuales
```

### (k) **umount**
Desmonta sistemas de archivos
```bash
umount /mnt/disco     # Desmontar por punto
umount /dev/sda1      # Desmontar por dispositivo
```

### (l) **head**
Muestra primeras líneas de archivo
```bash
head archivo.txt      # Primeras 10 líneas
head -n 20 archivo    # Primeras 20 líneas
```

### (m) **losetup**
Gestiona dispositivos loop
```bash
losetup /dev/loop0 imagen.iso    # Asociar imagen
losetup -d /dev/loop0            # Desasociar
```

### (n) **write**
Envía mensajes a usuarios conectados
```bash
write usuario        # Escribir a usuario
write usuario pts/1  # Especificar terminal
```

### (ñ) **mkfs**
Crea sistemas de archivos
```bash
mkfs.ext4 /dev/sda1     # Formatear como ext4
mkfs.fat /dev/sdb1      # Formatear como FAT
```

### (o) **fdisk**
⚠️ **¡CUIDADO! Puede destruir datos**
Gestor de particiones
```bash
fdisk -l              # Listar particiones
fdisk /dev/sda        # Editar particiones del disco
# Comandos dentro de fdisk:
# p: mostrar particiones
# n: nueva partición  
# d: eliminar partición
# w: escribir cambios
# q: salir sin guardar
```

## 12. Ubicación de comandos

### (a) Directorios donde se almacenan:

```bash
which comando     # Ubicación de comando específico
whereis comando   # Ubicación y manuales
```

**Ubicaciones típicas:**

- **`/bin/`**: Comandos básicos del sistema
  - `ls`, `cat`, `cp`, `mv`, `rm`, `mkdir`, `rmdir`

- **`/usr/bin/`**: Comandos de usuario
  - `find`, `locate`, `head`, `tail`, `file`

- **`/sbin/`**: Comandos de administración del sistema
  - `fdisk`, `mkfs`, `mount`, `umount`, `shutdown`, `halt`, `reboot`

- **`/usr/sbin/`**: Comandos administrativos de usuario
  - `lspci`, `updatedb`

- **Comandos internos del shell**: 
  - `cd`, `pwd`, `echo`, `exit` (no tienen archivo físico)

**Verificación:**
```bash
ls -la /bin/ | grep -E "(ls|cat|cp|mv)"
ls -la /sbin/ | grep -E "(mount|fdisk|shutdown)"
ls -la /usr/bin/ | grep -E "(find|locate|head)"
```

---

**Referencias:**
- Documentación GNU/Linux
- Man pages del sistema
- Práctica 1 - ISO 2023 - UNLP