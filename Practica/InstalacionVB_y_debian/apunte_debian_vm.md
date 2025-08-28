# Recuperar red e instalar entorno gráfico en Debian 13 (VM VirtualBox)

## 1. Problema inicial
- Debian instalado en modo mínimo (sin sudo, sin dhclient, sin entorno gráfico).
- La VM no tenía salida a Internet.
- `apt install` no encontraba paquetes porque solo estaban configurados repositorios `deb-src`.

## 2. Solución paso a paso

### A. Acceder como root
```bash
su -
```

### B. Configurar la red en VirtualBox
- Apagar la VM.
- En VirtualBox → Configuración → Red → Adaptador 1 → **NAT**.

### C. Asignar IP temporal si es necesario
```bash
ip link set enp0s3 up
ip addr add 10.0.2.15/24 dev enp0s3
ip route add default via 10.0.2.2
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

### D. Corregir los repositorios de APT
1. Confirmar versión de Debian:
```bash
cat /etc/debian_version
```
(Salió `13`, que corresponde a *trixie*).

2. Editar `/etc/apt/sources.list` y dejar:
```bash
deb http://deb.debian.org/debian trixie main contrib non-free non-free-firmware
deb http://security.debian.org/debian-security trixie-security main contrib non-free non-free-firmware
deb http://deb.debian.org/debian trixie-updates main contrib non-free non-free-firmware
```

3. Actualizar listas:
```bash
apt update
```

### E. Instalar paquetes básicos de red y administración
```bash
apt install net-tools isc-dhcp-client sudo -y
```

### F. Instalar entorno gráfico XFCE
```bash
apt install task-xfce-desktop lightdm -y
```
*(se elige LightDM como display manager)*

### G. Reiniciar la máquina
```bash
reboot
```
Al iniciar, aparece la pantalla de login gráfica y el escritorio XFCE completo.

## 3. Resultado final
- La VM tiene conectividad a Internet.
- El sistema cuenta con herramientas básicas (`sudo`, `dhclient`, `ifconfig`).
- Se instaló entorno gráfico XFCE funcional.
- Debian ya muestra escritorio, carpetas y menú completo.
