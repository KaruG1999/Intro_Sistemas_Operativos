# Práctica 1: Conceptos Generales de Sistemas Operativos

## ¿Qué es un Sistema Operativo?

Un **Sistema Operativo** es:
- **Parte esencial** de cualquier sistema de cómputo
- **Programa intermediario** entre el usuario y el hardware
- **Propósito**: crear un entorno cómodo y eficiente para la ejecución de programas
- **Obligación**: garantizar el correcto funcionamiento del sistema

### Sus funciones principales:
- **Administrar la memoria**
- **Administrar la CPU**
- **Administrar los dispositivos**

### Definiciones alternativas:
- **Wikipedia**: "Conjunto de programas de computación destinados a realizar muchas tareas"
- **Usuario estándar**: "Lo que aparece cuando prendo la PC"

## GNU/Linux - Introducción

**GNU/Linux** es un Sistema Operativo tipo Unix (Unix like), pero **libre**.

### Características principales:
- **Diseñado** por miles de programadores
- **Gratuito** y de libre distribución (Web, CD, etc.)
- **Diversas distribuciones** (customizaciones)
- **Código abierto**: podemos estudiarlo, personalizarlo, auditarlo
- **Ventaja clave**: ¡Podemos ver cómo está hecho!

## Historia de GNU

**GNU = GNU No es Unix**

### Cronología:
- **1983**: Richard Stallman inicia GNU para crear un Unix libre
- **1985**: Stallman crea la FSF (Free Software Foundation) para financiar GNU
- **1990**: GNU tenía editor (Emacs), compilador (GCC) y bibliotecas
- **Faltaba**: El componente principal → El Núcleo (Kernel)

### Desarrollo del Kernel:
- **1988**: Se abandona TRIX (muy complejo, hardware costoso)
- **Se adopta**: MACH para crear GNU Hurd (tampoco prosperó)
- **1991**: Linus Torvalds desarrolla Linux bajo licencia GPL
- **1992**: Torvalds y Stallman fusionan proyectos → nace **GNU/Linux**

## Las 4 Libertades del Software Libre

GNU se basa en **4 libertades principales**:

1. **Libertad de usar** el programa con cualquier propósito
2. **Libertad de estudiar** su funcionamiento
3. **Libertad para distribuir** sus copias
4. **Libertad para mejorar** los programas

> *"Los programas son una forma de expresión de ideas. Son propiedad de la humanidad y deben ser compartidos con todo el mundo"*

## Software Libre vs Software Propietario

### Software Libre:
- **Uso libre**: copiado, estudiado, modificado y redistribuido libremente
- **⚠️ No siempre gratuito**: error común asociar libre con gratuito
- **Código fuente incluido**: se distribuye con el código fuente
- **Corrección rápida**: la comunidad puede solucionar fallas
- **Libertades del usuario**: ejecutar, copiar, distribuir, estudiar, cambiar y mejorar

### Software Propietario:
- **Costo asociado**: generalmente tiene precio
- **Distribución restringida**: no se puede distribuir libremente
- **No modificable**: no permite modificaciones
- **Sin código fuente**: no se distribuye el código
- **Corrección centralizada**: solo el propietario corrige fallas
- **Menor especialización**: menos necesidad de técnicos especializados

## GPL: General Public License

**Licencia Pública General de GNU**
- **Creada**: 1989 por la FSF
- **Objetivo**: proteger la libre distribución, modificación y uso del software GNU
- **Propósito**: declarar que software bajo esta licencia es libre y está protegido
- **Versión actual**: GPL v3

## Características Generales de GNU/Linux

### Características del Sistema:
- **Multiusuario**: múltiples usuarios simultáneamente
- **Multitarea y multiprocesador**: múltiples procesos y procesadores
- **Altamente portable**: funciona en diferentes arquitecturas
- **Intérpretes programables**: diversos shells disponibles
- **Usuarios y permisos**: control de acceso granular
- **"Todo es archivo"**: dispositivos y directorios son archivos
- **Particiones independientes**: cada directorio en partición diferente
- **Case sensitive**: distingue mayúsculas y minúsculas
- **Código abierto**: acceso completo al código

### Diseño:
- **Portabilidad**: desarrollado para múltiples sistemas
- **Desarrollo en capas**: arquitectura modular
- **Separación de funciones**: cada capa como "caja negra"
- **Desarrollo distribuido**: colaboración global
- **Diversos File Systems**: soporte múltiples sistemas de archivos
- **Memoria virtual**: RAM + SWAP
- **Lenguajes**: principalmente C y assembler, también Java, Perl, Python

## Núcleo (Kernel)

### ¿Qué es el Kernel?
- **Ejecuta programas** y gestiona hardware
- **Coordinador**: hace que software y hardware trabajen juntos
- **Administra**: memoria, CPU y E/S
- **En sentido estricto**: ES el sistema operativo

### Tipo de Núcleo:
**Núcleo monolítico híbrido:**
- Drivers y código ejecutan en **modo privilegiado**
- **Híbrido**: carga/descarga funcionalidad via **módulos**
- **Licencia**: GPL v2

### Historia del Kernel Linux:
- **1991**: Linus Torvalds inicia programación basada en Minix
- **5 oct 1991**: Primera versión oficial Linux 0.02
- **1992**: Se combina con GNU → GNU/Linux
- **14 mar 1994**: Versión 1.0
- **Mayo 1996**: **Tux** como mascota oficial
- **Desarrollo**: miles de programadores mundiales

### Evolución de Versiones:
- **2.0** (jul 1996): definición nomenclatura versionado
- **2.2** (ene 1999): mejoras de portabilidad  
- **2.4** (2001): catapultó GNU/Linux como SO estable
- **2.6** (fin 2003): soporte hilos, mejoras planificación
- **3.0** (jul 2011): por 20 años del SO
- **Actual**: 6.4.11 (agosto 2023)

### Nomenclatura (2.6 y 3.x):
- **A**: Versión (cambia poco frecuente)
- **B**: Mayor revisión 
- **C**: Menor revisión (nuevos drivers/características)
- **D**: Corrección errores graves sin funcionalidad nueva

## Intérprete de Comandos (Shell)

**CLI (Command Line Interface)** = modo de comunicación usuario-SO

### Características:
- **Ejecuta programas** desde comandos
- **Personalizable**: cada usuario su interfaz
- **Programable**: permite scripts
- **Tipos**:
  - **Bourne Shell (sh)**
  - **Korn Shell (ksh)**  
  - **Bourne Again Shell (bash)**: autocompletado, historial, alias

## Sistemas de Archivos

Organizan el almacenamiento de archivos en dispositivos.

### Tipos:
- **FAT, NTFS** (Windows)
- **ext2, ext3, ext4** (Linux) → **Extended** adoptado por GNU/Linux
- **ReiserFS, Btrfs**

### Btrfs (B-tree FS) - Futuro:
- **Mayor tamaño** de archivos
- **Tolerante a fallas**: verificación sin desmontar
- **Avanzado**: indexación, snapshots, compresión, desfragmentación

### Directorios según FHS (Filesystem Hierarchy Standard):
- **/** : Raíz (como C:\)
- **/home** : Archivos usuarios (Mis documentos)
- **/var** : Información variable (logs, BD, spools)
- **/etc** : Archivos configuración
- **/bin** : Binarios y ejecutables
- **/dev** : Enlaces a dispositivos
- **/usr** : Aplicaciones usuarios

## Utilidades del Sistema

Diferencian distribuciones entre sí:

### Editores: **vi, emacs, joe**
### Herramientas Red: **wireshark, tcpdump**
### Oficina: **OpenOffice**
### Interfaces Gráficas: **GNOME/CINNAMON, KDE, LXDE**

## Distribuciones

**Distribución** = customización GNU/Linux con:
- Versión específica del kernel
- Programas con configuraciones
- Herramientas específicas

**Ejemplos**: Ubuntu, Debian, CentOS, Fedora, Arch Linux

## Organización Física de Discos

### MBR (Master Boot Record)
Sector reservado (cilindro 0, cabeza 0, sector 1)

**Características**:
- Existe en **todos los discos**
- Solo uno es **Primary Master Disk**
- **Tamaño**: 512 bytes

**Estructura (512 bytes)**:
- Primeros bytes: **Master Boot Code (MBC)**
- Byte 446+: **Tabla particiones (64 bytes)**
- Últimos 2 bytes: Libres/firma

**Funcionamiento**:
1. BIOS lee MBC (última acción)
2. MBC se carga en memoria y ejecuta
3. Bootloader según sistema instalado

## Particiones

**Partición** = división lógica del disco físico

### ¿Por qué particionar?
- **Limitaciones SO**: DOS/W95 no manejan >2GB
- **Separar SOs**: cada uno en partición separada
- **Diferentes filesystems**: FAT, NTFS, ext, etc.
- **Buenas prácticas**:
  - Separar datos usuario de aplicaciones/SO
  - Partición de restore
  - Kernel en partición solo lectura

### Limitaciones MBR:
- **Máximo 4 particiones primarias**
- **O 3 primarias + 1 extendida**

### Tipos:
1. **Primaria**: División directa disco (máx 4), info en MBR
2. **Extendida**: Contiene lógicas (solo 1 por disco)
3. **Lógica**: Dentro de extendida, se conectan como lista enlazada

### Requerimientos:
- **Mínimo**: 1 partición (/)
- **Recomendado**: 2 (/ y SWAP)

### Herramientas:
- **Destructivas**: fdisk (crear/eliminar)
- **No destructivas**: fips, gparted (crear/eliminar/modificar)

## Virtualización - Ambiente de Trabajo

### VirtualBox
Ambiente controlado para el curso.

**Proceso**:
1. Crear máquina virtual + asignar recursos
2. Bootear desde medio instalación
3. Seguir instrucciones instalación
4. Verificar arranque sistemas

**Ejemplo particionado**:
- **/dev/hda1**: DOS con Windows (2 GB)
- **/dev/hda2**: /boot (≈60 MB)
- **/dev/hda3**: / (≈6 GB)
- **/dev/hda4**: SWAP

## Emulación vs Virtualización

### 1. Emulación:
- **Emula hardware** completo
- **Todas instrucciones CPU**
- **Muy costosa**, poco eficiente
- **Arquitecturas diferentes** posibles

### 2. Virtualización Completa:
- **SOs huéspedes** en host
- **Hypervisor** intermediario
- **Arquitectura compatible** requerida
- **Más eficiente** (Intel-VT, AMD-V)

### 3. Paravirtualización:
- **SOs modificados** para virtualización
- **Mayor eficiencia**

**Diferencia clave**: Virtualizadores aprovechan CPU nativo = más veloces

## Gestores de Arranque

### Función:
Cargar **imagen Kernel** desde partición para ejecución

### Ejecución:
Después del **código BIOS**

### Modos instalación:
1. **En MBR** (usa MBR gap)
2. **Sector arranque** partición raíz

**Ejemplos**: GRUB, LILO, NTLDR, GAG, YaST

## GRUB Legacy

**GRand Unified Bootloader** = más utilizado

### Fases:
- **Fase 1**: En MBR, carga Fase 1.5
- **Fase 1.5**: 30 KB siguientes (MBR gap), carga Fase 2  
- **Fase 2**: Interfaz usuario + carga Kernel

### Configuración `/boot/grub/menu.lst`:
```
default: SO por defecto
timeout: tiempo espera
title Debian GNU/Linux
root (hd0,1) #(Disco,Partición)
kernel /vmlinuz-2.6.26 ro quiet root=/dev/hda3
initrd /initrd-2.6.26.img
```

## GRUB 2

**Versión actual** mayoría distribuciones

### Mejoras:
- Soporte nuevas arquitecturas
- Caracteres no ASCII + idiomas
- Customización menús
- **No Fase 1.5**

### Configuración:
- **Archivo**: `/boot/grub/grub.cfg`
- **⚠️ No editar manual** → usar `update-grub`

## Proceso de Arranque (Bootstrap)

**Bootstrap** = inicio máquina + carga SO

### En x86:
**BIOS** inicia carga SO via MBC:
- Grabado en ROM/NVRAM
- Carga booteo desde MBR
- Bootloader carga Kernel
- Kernel prueba dispositivos → control a `init`

### Otras arquitecturas:
- **Mainframe**: Power on Reset + IPL
- **SPARC**: OBP (OpenBoot PROM)

**Característica**: Serie programas pequeños ejecución encadenada

## Problemas BIOS (años 80)

### Limitaciones:
- **Firmware BIOS** no lee filesystems fácilmente
- **MBC máximo 446 bytes**
- **GRUB** necesita sectores adyacentes (MBR gap) que deberían estar libres

## EFI (Extensible Firmware Interface)

### Definición:
**Nexo** entre SO y firmware

### Características:
- **Usa GPT** (GUID partition table) vs limitaciones MBR
- **GPT** especifica ubicación/formato tabla particiones
- **Parte de EFI**, sustituye MBR
- **Propiedad Intel**

## GPT (GUID Partition Table)

### Características:
- **Mantiene MBR** (compatibilidad BIOS)
- **LBA** (logical block addressing) vs cylinder-header-sector
- **MBR heredado**: LBA 0
- **Cabecera GPT**: LBA 1
- **Tabla particiones**: bloques sucesivos
- **Redundancia**: cabecera/tabla inicio y final disco

## UEFI (Unified Extensible Firmware Interface)

### UEFI Forum:
Alianza modernizar arranque: AMD, Apple, HP, Dell, IBM, Intel, Lenovo, Microsoft, etc.

### Diferencias:
- **EFI**: propiedad Intel
- **UEFI**: propiedad UEFI Forum
- **UEFI añade**: criptografía, autenticación red, interfaz gráfica

### Funciones:
- **Define ubicación** gestor arranque
- **Define interfaz** bootloader-firmware  
- **Expone información** hardware/configuración
- **BootManager** para aplicaciones UEFI
- **Bootloader** = aplicación UEFI en filesystem UEFI (FAT32)
- **GRUB** ya no necesita múltiples etapas

## Secure Boot

### Propósito:
**Arranque sin código malicioso**

### Funcionamiento:
- **Validación**: aplicaciones/drivers UEFI verificadas
- **Claves asimétricas**: pares de claves
- **Almacenamiento**: claves públicas en firmware
- **Verificación**: imágenes firmadas por proveedor autorizado
- **Falla**: si clave privada vencida/revocada

---

**Referencias:** 
- Conceptos Generales - Práctica 1 (2023)
- Facultad de Informática - UNLP