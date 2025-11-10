# Shell Scripting - Guía de Ejercicios

## Ejercicios Básicos

### Ejercicio 3: Primer script
Crear un script que:
- Solicite nombre y apellido
- Muestre fecha y hora actual
- Muestre usuario y directorio actual

```bash
#!/bin/bash
echo "Introduzca su nombre y apellido:"
read nombre apellido
echo "Fecha y hora actual:"
date
echo "Su apellido y nombre es:"
echo "$apellido $nombre"
echo "Su usuario es: $(whoami)"
echo "Su directorio actual es: $(pwd)"
```

**Conceptos clave:**
- `read`: leer entrada del usuario
- `$(comando)`: sustitución de comandos
- Permisos de ejecución: `chmod +x script.sh`

### Ejercicio 12: Operaciones matemáticas

**a)** Script que lea 2 números y calcule suma, resta, multiplicación y mayor:
```bash
#!/bin/bash
read -p "Ingrese primer número: " num1
read -p "Ingrese segundo número: " num2

suma=$((num1 + num2))
resta=$((num1 - num2))
mult=$((num1 * num2))

echo "Suma: $suma"
echo "Resta: $resta"
echo "Multiplicación: $mult"

if [ $num1 -gt $num2 ]; then
    echo "Mayor: $num1"
else
    echo "Mayor: $num2"
fi
```

**b)** Modificar para recibir números como parámetros:
```bash
#!/bin/bash
if [ $# -ne 2 ]; then
    echo "Error: se requieren 2 parámetros"
    exit 1
fi

suma=$(($1 + $2))
echo "Suma: $suma"
# ... resto de operaciones
```

**c)** Calculadora con operación como parámetro:
```bash
#!/bin/bash
if [ $# -ne 3 ]; then
    echo "Uso: $0 num1 operacion num2"
    exit 1
fi

case $2 in
    "+") echo $(($1 + $3));;
    "-") echo $(($1 - $3));;
    "*") echo $(($1 * $3));;
    "%") echo $(($1 % $3));;
    *) echo "Operación inválida";;
esac
```

## Estructuras de Control

### Ejercicio 13a: Números del 1 al 100 con cuadrados
```bash
#!/bin/bash
for ((i=1; i<=100; i++)); do
    cuadrado=$((i * i))
    echo "$i - $cuadrado"
done
```

### Ejercicio 13b: Menú de opciones
```bash
#!/bin/bash
select opcion in "Listar" "DondeEstoy" "QuienEsta" "Salir"; do
    case $opcion in
        "Listar")
            ls
            ;;
        "DondeEstoy")
            pwd
            ;;
        "QuienEsta")
            who
            ;;
        "Salir")
            exit 0
            ;;
    esac
done
```

### Ejercicio 13c: Verificar archivo/directorio
```bash
#!/bin/bash
if [ $# -ne 1 ]; then
    echo "Uso: $0 nombre_archivo"
    exit 1
fi

if [ -e "$1" ]; then
    if [ -d "$1" ]; then
        echo "$1 es un directorio"
    elif [ -f "$1" ]; then
        echo "$1 es un archivo"
    fi
else
    echo "$1 no existe. Creando directorio..."
    mkdir "$1"
fi
```

## Manejo de Archivos

### Ejercicio 14: Renombrar archivos
```bash
#!/bin/bash
if [ $# -ne 3 ]; then
    echo "Uso: $0 directorio -a|-b CADENA"
    exit 1
fi

directorio=$1
opcion=$2
cadena=$3

for archivo in "$directorio"/*; do
    if [ -f "$archivo" ]; then
        nombre=$(basename "$archivo")
        case $opcion in
            "-a")
                mv "$archivo" "${directorio}/${nombre}${cadena}"
                ;;
            "-b")
                mv "$archivo" "${directorio}/${cadena}${nombre}"
                ;;
        esac
    fi
done
```

### Ejercicio 16: Reporte de archivos por extensión
```bash
#!/bin/bash
if [ $# -ne 1 ]; then
    echo "Uso: $0 extension"
    exit 1
fi

extension=$1
echo "Usuario - Cantidad" > reporte.txt

for usuario in $(cut -d: -f1 /etc/passwd); do
    home=$(eval echo ~$usuario)
    if [ -d "$home" ]; then
        cantidad=$(find "$home" -name "*.$extension" 2>/dev/null | wc -l)
        echo "$usuario - $cantidad" >> reporte.txt
    fi
done
```

### Ejercicio 17: Transformar nombres (tr)
```bash
#!/bin/bash
for archivo in *; do
    # Intercambiar mayúsculas/minúsculas y eliminar 'a' y 'A'
    nuevo=$(echo "$archivo" | tr 'a-zA-Z' 'A-Za-z' | tr -d 'aA')
    echo "$nuevo"
done
```

## Arreglos

### Ejercicio 20: Implementar Pila (Stack)
```bash
#!/bin/bash
pila=()

push() {
    pila+=("$1")
}

pop() {
    if [ ${#pila[@]} -eq 0 ]; then
        echo "Pila vacía"
        return 1
    fi
    unset pila[${#pila[@]}-1]
}

length() {
    echo ${#pila[@]}
}

print() {
    echo "Elementos: ${pila[@]}"
}

# Usar las funciones
for i in {1..10}; do
    push $i
done

pop
pop
pop

echo "Longitud: $(length)"
print
```

### Ejercicio 21: Productoria
```bash
#!/bin/bash
num=(10 3 5 7 9 3 5 4)

productoria() {
    resultado=1
    for numero in "${num[@]}"; do
        resultado=$((resultado * numero))
    done
    echo $resultado
}

echo "Productoria: $(productoria)"
```

### Ejercicio 22: Pares e Impares
```bash
#!/bin/bash
numeros=(1 2 3 4 5 6 7 8 9 10)
impares=0

for num in "${numeros[@]}"; do
    if [ $((num % 2)) -eq 0 ]; then
        echo "Par: $num"
    else
        ((impares++))
    fi
done

echo "Cantidad de impares: $impares"
```

### Ejercicio 23: Suma de vectores
```bash
#!/bin/bash
vector1=(1 80 65 35 2)
vector2=(5 98 3 41 8)

for i in "${!vector1[@]}"; do
    suma=$((vector1[i] + vector2[i]))
    echo "La suma de los elementos de la posición $i es $suma"
done
```

## Funciones y Validaciones

### Ejercicio 24: Usuarios del grupo "users"
```bash
#!/bin/bash
usuarios=()

# Llenar arreglo con usuarios del grupo
for user in $(grep "users" /etc/group | cut -d: -f4 | tr ',' ' '); do
    usuarios+=("$user")
done

case $1 in
    "-b")
        if [ -n "${usuarios[$2]}" ]; then
            echo "${usuarios[$2]}"
        else
            echo "Error: posición inválida"
        fi
        ;;
    "-l")
        echo ${#usuarios[@]}
        ;;
    "-i")
        echo "${usuarios[@]}"
        ;;
esac
```

### Ejercicio 25: Validar archivos en posiciones impares
```bash
#!/bin/bash
if [ $# -eq 0 ]; then
    echo "Error: debe enviar al menos un parámetro"
    exit 1
fi

inexistentes=0

for i in $(seq 1 2 $#); do
    archivo=${!i}
    if [ -e "$archivo" ]; then
        if [ -d "$archivo" ]; then
            echo "$archivo es un directorio"
        elif [ -f "$archivo" ]; then
            echo "$archivo es un archivo"
        fi
    else
        ((inexistentes++))
    fi
done

echo "Archivos/directorios inexistentes: $inexistentes"
```

### Ejercicio 27: Contar archivos con permisos
```bash
#!/bin/bash
if [ $# -ne 1 ]; then
    echo "Uso: $0 directorio"
    exit 1
fi

if [ ! -d "$1" ]; then
    echo "Error: el directorio no existe"
    exit 4
fi

lectura=0
escritura=0

for archivo in "$1"/*; do
    if [ -f "$archivo" ]; then
        [ -r "$archivo" ] && ((lectura++))
        [ -w "$archivo" ] && ((escritura++))
    fi
done

echo "Archivos con lectura: $lectura"
echo "Archivos con escritura: $escritura"
```

## Ejercicios Avanzados

### Ejercicio 18: Monitorear login de usuario
```bash
#!/bin/bash
if [ $# -ne 1 ]; then
    echo "Uso: $0 nombre_usuario"
    exit 1
fi

usuario=$1

while true; do
    if who | grep -q "^$usuario "; then
        echo "Usuario $usuario logueado en el sistema"
        exit 0
    fi
    sleep 10
done
```

### Ejercicio 30: Set (Conjunto) - conceptos básicos
```bash
#!/bin/bash
set_data=()

initialize() {
    set_data=()
}

add() {
    # Verificar si ya existe
    for elemento in "${set_data[@]}"; do
        if [ "$elemento" = "$1" ]; then
            return 1  # Ya existe
        fi
    done
    set_data+=("$1")
    return 0
}

contains() {
    for elemento in "${set_data[@]}"; do
        if [ "$elemento" = "$1" ]; then
            return 0  # Existe
        fi
    done
    return 1  # No existe
}

print() {
    for elemento in "${set_data[@]}"; do
        echo "$elemento"
    done
}
```

## Comandos Útiles en Ejercicios

### Comando `cut`
Corta columnas o campos de un texto:
```bash
cut -d: -f1 /etc/passwd        # Campo 1, delimitador :
cut -d, -f1,3 archivo.csv      # Campos 1 y 3
echo "hola mundo" | cut -d' ' -f2  # Segunda palabra
```

### Comando `tr`
Transforma o elimina caracteres:
```bash
echo "hola" | tr 'a-z' 'A-Z'   # Mayúsculas
echo "hola" | tr -d 'a'        # Elimina 'a'
echo "hola" | tr 'hl' 'HL'     # Reemplaza h->H, l->L
```

### Comando `find`
Busca archivos:
```bash
find /home -name "*.doc"       # Archivos .doc
find . -type f                 # Solo archivos
find . -type d                 # Solo directorios
find . -type l                 # Enlaces simbólicos
```

### Comando `grep`
Busca patrones:
```bash
grep "texto" archivo           # Busca "texto"
grep -v "texto" archivo        # Líneas que NO contienen "texto"
grep -i "texto" archivo        # Case insensitive
who | grep "usuario"           # Buscar en salida de comando
```

## Consejos para Resolver Ejercicios

1. **Validar parámetros**: Siempre verificar `$#`
2. **Usar comillas**: En variables con espacios: `"$variable"`
3. **Testar condiciones**: Archivos, directorios, permisos
4. **Manejar errores**: Usar `exit` con códigos apropiados
5. **Modularizar**: Usar funciones para código repetitivo
6. **Debugear**: Usar `bash -x script.sh` o `echo` para rastrear valores