# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>

<details>
<summary>Tarea #981</summary>

# **Realizar un script en bash que desglose las caracteristicas de un archivo**

<details>
<summary>Código: desglose.sh</summary>

```bash
#!/bin/bash
# Tarea #981: script en bash que desglosa las características de un archivo
# Autor: Fabrizio Soldi Esparza
#
# Uso:
#   bash desglose.sh archivo [otro_archivo ...]
# Ejemplos:
#   bash desglose.sh menu.sh
#   bash desglose.sh /bin/ls /etc/passwd ~/Downloads

# ---------- Colores (solo si la salida es una terminal) ----------
if [ -t 1 ]; then
    VERDE="\033[1;32m"; ROJO="\033[1;31m"; AMARILLO="\033[1;33m"
    CYAN="\033[1;36m";  NORMAL="\033[0m"
else
    VERDE=""; ROJO=""; AMARILLO=""; CYAN=""; NORMAL=""
fi

# ---------- Funciones de apoyo ----------
titulo () {
    echo -e "\n${CYAN}--- $1 ---${NORMAL}"
}

dato () {
    # Alinea por número de letras (printf %-28s cuenta bytes y
    # las letras con acento ocupan 2, así que desalinea)
    local etiqueta="$1:"
    local espacios=$(( 30 - ${#etiqueta} ))
    (( espacios < 1 )) && espacios=1
    printf "  %s%*s%s\n" "$etiqueta" "$espacios" "" "$2"
}

si_no () {
    # Recibe una condición de test y responde "sí" o "no"
    if test "$@"; then echo "sí"; else echo "no"; fi
}

fecha () {
    # Convierte segundos desde 1970 a dd/mm/aaaa hh:mm:ss
    if [ "$1" -eq 0 ] 2>/dev/null; then
        echo "no disponible"
    else
        date -d "@$1" "+%d/%m/%Y %H:%M:%S"
    fi
}

tipo_en_espanol () {
    case "$1" in
        "regular file")           echo "archivo regular" ;;
        "regular empty file")     echo "archivo regular (vacío)" ;;
        "directory")              echo "directorio" ;;
        "symbolic link")          echo "enlace simbólico" ;;
        "character special file") echo "dispositivo de caracteres" ;;
        "block special file")     echo "dispositivo de bloques" ;;
        "fifo")                   echo "tubería con nombre (FIFO)" ;;
        "socket")                 echo "socket" ;;
        *)                        echo "$1" ;;
    esac
}

explica_permisos () {
    # Recibe 3 letras (por ejemplo rw-) y las traduce
    local p="$1" lista=()
    [ "${p:0:1}" = "r" ] && lista+=("lectura")
    [ "${p:1:1}" = "w" ] && lista+=("escritura")
    case "${p:2:1}" in
        x|s|t) lista+=("ejecución") ;;
    esac
    if [ ${#lista[@]} -eq 0 ]; then
        echo "${p} (ninguno)"
    else
        local texto
        texto=$(IFS=,; echo "${lista[*]}")
        echo "${p} (${texto//,/, })"
    fi
}

# ---------- Función principal: desglosa un archivo ----------
desglosar () {
    local archivo="$1"

    # -L también acepta enlaces simbólicos rotos
    if [ ! -e "$archivo" ] && [ ! -L "$archivo" ]; then
        echo -e "\n${ROJO}ERROR: '${archivo}' no existe.${NORMAL}"
        return 1
    fi

    echo -e "\n${VERDE}==================================================${NORMAL}"
    echo -e "${VERDE} Desglose de: ${archivo}${NORMAL}"
    echo -e "${VERDE}==================================================${NORMAL}"

    # ----- 1. Nombre y ubicación -----
    local nombre ruta carpeta sin_punto extension
    nombre=$(basename -- "$archivo")
    ruta=$(realpath -s -- "$archivo")
    carpeta=$(dirname -- "$ruta")
    sin_punto="${nombre#.}"                  # .bashrc no tiene extensión
    if [[ "$sin_punto" == *.* ]]; then
        extension=".${sin_punto##*.}"
    else
        extension="(sin extensión)"
    fi

    titulo "Nombre y ubicación"
    dato "Nombre" "$nombre"
    dato "Extensión" "$extension"
    dato "Ruta absoluta" "$ruta"
    dato "Carpeta" "$carpeta"
    dato "¿Es oculto?" "$(si_no "${nombre:0:1}" = ".")"

    # ----- 2. Tipo -----
    # LC_ALL=C para que stat y file respondan siempre en inglés
    # y la traducción de tipo_en_espanol funcione en cualquier idioma
    local tipo
    tipo=$(LC_ALL=C stat -c '%F' -- "$archivo")

    titulo "Tipo"
    dato "Tipo de archivo" "$(tipo_en_espanol "$tipo")"
    dato "Contenido (file)" "$(LC_ALL=C file -b -- "$archivo")"
    dato "Tipo MIME" "$(file -b --mime-type -- "$archivo")"
    if [ -L "$archivo" ]; then
        dato "Apunta a" "$(readlink -- "$archivo")"
        dato "¿El destino existe?" "$(si_no -e "$archivo")"
    fi

    # ----- 3. Tamaño -----
    local bytes bloques tam_bloque
    bytes=$(stat -c '%s' -- "$archivo")
    bloques=$(stat -c '%b' -- "$archivo")
    tam_bloque=$(stat -c '%B' -- "$archivo")

    titulo "Tamaño"
    if [ "$bytes" -ge 1024 ]; then
        dato "Tamaño" "${bytes} bytes ($(numfmt --to=iec "$bytes"))"
    else
        dato "Tamaño" "${bytes} bytes"
    fi
    dato "Espacio usado en disco" "$(numfmt --to=iec $(( bloques * tam_bloque ))) (${bloques} bloques de ${tam_bloque} bytes)"
    if [ -d "$archivo" ] && [ ! -L "$archivo" ]; then
        dato "Elementos dentro" "$(ls -A -- "$archivo" 2>/dev/null | wc -l)"
        dato "Tamaño total del contenido" "$(du -sh -- "$archivo" 2>/dev/null | cut -f1)"
    fi

    # ----- 4. Permisos -----
    local simbolicos octal especiales=()
    simbolicos=$(stat -c '%A' -- "$archivo")
    octal=$(stat -c '%a' -- "$archivo")
    if [ ${#octal} -eq 4 ]; then
        local e=${octal:0:1}
        (( e & 4 )) && especiales+=("setuid")
        (( e & 2 )) && especiales+=("setgid")
        (( e & 1 )) && especiales+=("sticky bit")
    fi

    titulo "Permisos"
    dato "Permisos" "${simbolicos} (octal ${octal})"
    dato "Dueño" "$(explica_permisos "${simbolicos:1:3}")"
    dato "Grupo" "$(explica_permisos "${simbolicos:4:3}")"
    dato "Otros" "$(explica_permisos "${simbolicos:7:3}")"
    if [ ${#especiales[@]} -gt 0 ]; then
        dato "Permisos especiales" "${especiales[*]}"
    fi
    dato "Usuario que consulta" "$(whoami)"
    dato "¿Puedes leerlo?" "$(si_no -r "$archivo")"
    dato "¿Puedes modificarlo?" "$(si_no -w "$archivo")"
    dato "¿Puedes ejecutarlo?" "$(si_no -x "$archivo")"

    # ----- 5. Propietario -----
    titulo "Propietario"
    dato "Usuario dueño" "$(stat -c '%U (UID %u)' -- "$archivo")"
    dato "Grupo dueño" "$(stat -c '%G (GID %g)' -- "$archivo")"

    # ----- 6. Fechas -----
    titulo "Fechas"
    dato "Creación" "$(fecha "$(stat -c '%W' -- "$archivo")")"
    dato "Última modificación" "$(fecha "$(stat -c '%Y' -- "$archivo")")"
    dato "Último acceso" "$(fecha "$(stat -c '%X' -- "$archivo")")"
    dato "Último cambio de atributos" "$(fecha "$(stat -c '%Z' -- "$archivo")")"

    # ----- 7. Sistema de archivos -----
    titulo "Sistema de archivos"
    dato "Inodo" "$(stat -c '%i' -- "$archivo")"
    dato "Enlaces duros" "$(stat -c '%h' -- "$archivo")"
    dato "Punto de montaje" "$(stat -c '%m' -- "$archivo" 2>/dev/null)"
    dato "Tipo de sistema de archivos" "$(df --output=fstype -- "$archivo" 2>/dev/null | tail -n 1 | tr -d ' ')"

    # ----- 8. Contenido (solo archivos de texto) -----
    if [ -f "$archivo" ] && [ -r "$archivo" ]; then
        local mime
        mime=$(file -b --mime-type -- "$archivo")
        if [[ "$mime" == text/* ]]; then
            titulo "Contenido (texto)"
            dato "Líneas" "$(wc -l < "$archivo")"
            dato "Palabras" "$(wc -w < "$archivo")"
            dato "Caracteres" "$(wc -m < "$archivo")"
            dato "Codificación" "$(file -b --mime-encoding -- "$archivo")"
            dato "Primera línea" "$(head -n 1 -- "$archivo" | cut -c 1-50)"
        fi

        # ----- 9. Huellas digitales -----
        titulo "Huellas digitales (checksums)"
        dato "MD5" "$(md5sum -- "$archivo" | cut -d ' ' -f 1)"
        dato "SHA-256" "$(sha256sum -- "$archivo" | cut -d ' ' -f 1)"
    fi
}

# ---------- Programa principal ----------
if [ $# -eq 0 ] || [ "$1" = "-h" ] || [ "$1" = "--help" ]; then
    echo "Uso: bash $0 archivo [otro_archivo ...]"
    echo "Muestra el desglose de las características de cada archivo:"
    echo "nombre, tipo, tamaño, permisos, propietario, fechas, inodo y contenido."
    [ $# -eq 0 ] && exit 1 || exit 0
fi

errores=0
for archivo in "$@"; do
    desglosar "$archivo" || errores=$(( errores + 1 ))
done
echo

if [ $errores -gt 0 ]; then
    echo -e "${AMARILLO}${errores} archivo(s) no se pudieron desglosar.${NORMAL}"
    exit 1
fi
exit 0
```

</details>

<details>
<summary>Cómo ejecutarlo</summary>

- `bash desglose.sh archivo`
- También acepta varios archivos a la vez: `bash desglose.sh menu.sh /bin/ls /etc/passwd`
- Sin argumentos muestra cómo se usa y termina con código de salida 1.

</details>

<details>
<summary>Qué muestra</summary>

- **Nombre y ubicación:** nombre, extensión, ruta absoluta, carpeta y si es oculto.
- **Tipo:** archivo regular, directorio, enlace simbólico, dispositivo, FIFO o socket; qué contiene según `file` y su tipo MIME. Si es enlace, a dónde apunta y si el destino existe.
- **Tamaño:** en bytes y legible (K, M, G), y el espacio real que ocupa en disco. En carpetas, cuántos elementos tiene.
- **Permisos:** en letras y en octal, desglosados para dueño, grupo y otros; permisos especiales (setuid, setgid, sticky bit) y si el usuario actual puede leer, modificar o ejecutar el archivo.
- **Propietario:** usuario y grupo dueños con su UID y GID.
- **Fechas:** creación, última modificación, último acceso y último cambio de atributos.
- **Sistema de archivos:** inodo, enlaces duros, punto de montaje y tipo de sistema de archivos.
- **Contenido:** en archivos de texto, líneas, palabras, caracteres, codificación y primera línea.
- **Huellas digitales:** MD5 y SHA-256 del archivo.

</details>

<details>
<summary>Explicación</summary>

- **stat:** es el comando principal. Con `stat -c` y un formato se saca cada dato por separado: `%s` tamaño, `%A` y `%a` permisos, `%U` dueño, `%i` inodo, `%h` enlaces, `%W %Y %X %Z` fechas en segundos, que la función `fecha` convierte con `date -d @segundos`.
- **file:** identifica el contenido real del archivo sin importar su extensión.
- **test:** `-e`, `-L`, `-f`, `-d`, `-r`, `-w`, `-x` revisan si existe, si es enlace, si es archivo o carpeta y qué puede hacer el usuario con él.
- **Expansión de variables:** `${nombre#.}`, `${sin_punto##*.}` y `${simbolicos:1:3}` recortan cadenas para sacar la extensión y separar los permisos de dueño, grupo y otros.
- **Funciones:** `titulo`, `dato`, `si_no`, `fecha`, `tipo_en_espanol` y `explica_permisos` evitan repetir código; `desglosar` hace todo el trabajo para un archivo.
- **for y case:** el `for` recorre todos los archivos recibidos y el `case` traduce el tipo de archivo al español.
- **LC_ALL=C:** obliga a `stat` y `file` a responder en inglés para que la traducción funcione aunque Ubuntu esté en español.
- **Código de salida:** termina con 0 si todo salió bien y con 1 si algún archivo no existe.

</details>

</details>

