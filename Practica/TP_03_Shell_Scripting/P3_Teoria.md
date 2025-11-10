# Shell Scripting - Síntesis Teórica

## ¿Qué es Shell Scripting?

**Shell** es un intérprete de comandos interactivo que permite programar scripts para automatizar tareas en sistemas Unix/Linux.

### Características principales
- No requiere compilación (interpretado)
- Ideal para manipular archivos y procesos
- Funciona en cualquier sistema *nix
- Se puede probar directamente en la terminal

### Shell más común: Bash
En la materia usamos **bash** (Bourne Again Shell), que es la shell por defecto en la mayoría de distribuciones Linux.

---

## Estructura de un Script

```bash
#!/bin/bash
# Comentarios comienzan con #
# Primera línea (shebang) indica el intérprete

echo "Hola mundo"
```

### Ejecución
```bash
# Dar permisos de ejecución
chmod +x script.sh

# Ejecutar
./script.sh
# o
bash script.sh
```

---

## Variables

### Declaración y uso
```bash
# Crear variable (SIN espacios alrededor del =)
NOMBRE="Juan"
edad=25

# Acceder con $
echo $NOMBRE
echo ${edad}  # Con llaves evita ambigüedades
```

### Tipos de datos
- **Strings**: Cadenas de texto
- **Arreglos**: Colecciones de valores

```bash
# Arreglo
numeros=(1 2 3 4 5)
echo ${numeros[0]}      # Primer elemento
echo ${numeros[@]}      # Todos los elementos
echo ${#numeros[@]}     # Longitud del arreglo
```

### Comillas
- **Dobles (`"`)**: Permiten interpolación de variables
- **Simples (`'`)**: Texto literal, no interpolan

```bash
nombre="Ana"
echo "Hola $nombre"   # Hola Ana
echo 'Hola $nombre'   # Hola $nombre
```

---

## Comandos Importantes

### Entrada/Salida
```bash
echo "texto"           # Imprimir
read variable          # Leer desde teclado
cat archivo            # Mostrar contenido
```

### Búsqueda y procesamiento
```bash
grep "patron" archivo  # Buscar texto
find /ruta -name "*.txt"  # Buscar archivos
cut -d: -f1            # Cortar columnas
wc -l                  # Contar líneas
```

### Reemplazo de comandos
Ejecutar un comando y usar su salida:
```bash
archivos=$(ls)
fecha=`date`
usuarios=$(cat /etc/passwd | cut -d: -f1)
```

---

## Redirecciones y Pipes

### Redirecciones
```bash
comando > archivo      # Sobrescribe archivo
comando >> archivo     # Agrega al final
comando 2> archivo     # Redirige errores
comando < archivo      # Lee desde archivo
```

### Pipes (|)
Conectan la salida de un comando con la entrada de otro:
```bash
cat archivo | grep "hola" | wc -l
ls -l | cut -d' ' -f1 | sort | uniq
```

---

## Estructuras de Control

### IF - Condicional
```bash
if [ condicion ]; then
    # código
elif [ otra_condicion ]; then
    # código
else
    # código
fi
```

### CASE - Selección múltiple
```bash
case $variable in
    "opcion1")
        # código
        ;;
    "opcion2")
        # código
        ;;
    *)
        # default
        ;;
esac
```

### FOR - Iteración
```bash
# Estilo foreach
for item in valor1 valor2 valor3; do
    echo $item
done

# Estilo C
for ((i=0; i<10; i++)); do
    echo $i
done

# Iterar archivos
for archivo in $(ls); do
    echo $archivo
done
```

### WHILE - Bucle condicional
```bash
while [ condicion ]; do
    # código
done
```

### SELECT - Menú
```bash
select opcion in "Opción 1" "Opción 2" "Salir"; do
    case $opcion in
        "Salir")
            break
            ;;
        *)
            echo "Seleccionaste: $opcion"
            ;;
    esac
done
```

---

## Comparaciones

### Strings
```bash
[ "$a" = "$b" ]     # Igualdad
[ "$a" != "$b" ]    # Desigualdad
[ -z "$a" ]         # String vacío
[ -n "$a" ]         # String no vacío
```

### Números
```bash
[ $a -eq $b ]       # Igual
[ $a -ne $b ]       # Distinto
[ $a -gt $b ]       # Mayor que
[ $a -ge $b ]       # Mayor o igual
[ $a -lt $b ]       # Menor que
[ $a -le $b ]       # Menor o igual
```

### Archivos
```bash
[ -e archivo ]      # Existe
[ -f archivo ]      # Es archivo regular
[ -d archivo ]      # Es directorio
[ -r archivo ]      # Tiene permiso lectura
[ -w archivo ]      # Tiene permiso escritura
[ -x archivo ]      # Tiene permiso ejecución
```

### Operadores lógicos
```bash
[ cond1 ] && [ cond2 ]    # AND
[ cond1 ] || [ cond2 ]    # OR
```

---

## Argumentos y Variables Especiales

```bash
$0      # Nombre del script
$1, $2  # Primer, segundo argumento
$#      # Cantidad de argumentos
$*      # Todos los argumentos
$@      # Todos los argumentos (mejor para iteración)
$?      # Código de retorno del último comando
$HOME   # Directorio home del usuario
$USER   # Nombre del usuario
```

### Ejemplo
```bash
#!/bin/bash
if [ $# -ne 2 ]; then
    echo "Uso: $0 arg1 arg2"
    exit 1
fi

echo "Primer arg: $1"
echo "Segundo arg: $2"
exit 0
```

---

## Funciones

```bash
# Declaración
mi_funcion() {
    local variable_local="valor"
    echo "Argumento 1: $1"
    echo "Argumento 2: $2"
    return 0  # Valor entre 0-255
}

# Llamada
mi_funcion "hola" "mundo"
resultado=$?  # Capturar valor de retorno
```

### Alcance de variables
- **Global**: Por defecto todas las variables
- **Local**: Usar `local` dentro de funciones
- **Export**: Hacer visible a procesos hijos

```bash
export MI_VARIABLE="visible en subprocesos"
```

---

## Control de Flujo

### Exit
```bash
exit 0    # Éxito
exit 1    # Error
```

### Break y Continue
```bash
while true; do
    if [ condicion ]; then
        break      # Sale del bucle
    fi
    if [ otra ]; then
        continue   # Salta a siguiente iteración
    fi
done
```

---

## Comandos Útiles para Scripts

```bash
expr 5 + 3          # Evaluar expresiones matemáticas
test -f archivo     # Equivalente a [ -f archivo ]
let i++             # Incrementar variable
sleep 5             # Pausar 5 segundos
tr a-z A-Z          # Transformar caracteres
sort                # Ordenar líneas
uniq                # Eliminar duplicados
sed                 # Editor de stream
awk                 # Procesamiento de texto
```

---

## Buenas Prácticas

1. Siempre usar shebang: `#!/bin/bash`
2. Comentar el código
3. Validar argumentos recibidos
4. Usar comillas en variables: `"$variable"`
5. Verificar que archivos existan antes de usarlos
6. Usar nombres descriptivos de variables
7. Manejar códigos de salida apropiadamente
8. Usar `local` en funciones para variables locales