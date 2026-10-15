# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>


<details>
<summary>Tarea #993</summary>

# **Hacer un menú en bash para ejecutar scripts**

<details>
<summary>Cómo ejecutarlo</summary>

- chmod +x menu.sh
- ./menu.sh

</details>

<details>
<summary>Código: menu.sh</summary>

```bash
#!/bin/bash
# menu.sh - Tarea #993
# Menú para elegir qué script ejecutar.

# Carpeta donde está este archivo
DIR="$(cd "$(dirname "$0")" && pwd)"

while true
do
    echo ""
    echo "===================================="
    echo "           MENÚ DE SCRIPTS          "
    echo "===================================="
    echo "1) Crear árbol de directorios"
    echo "2) Hola mundo"
    echo "3) Saludo usando variables"
    echo "4) Salir"
    echo "===================================="
    read -p "Elige una opción [1-4]: " opcion

    case $opcion in
        1)
            echo "--- Árbol de directorios ---"
            bash "$DIR/arbol.sh"
            ;;
        2)
            echo "--- Hola mundo ---"
            bash "$DIR/holamundo.sh"
            ;;
        3)
            echo "--- Saludo con variables ---"
            bash "$DIR/saludo.sh"
            ;;
        4)
            echo "¡Hasta luego!"
            exit 0
            ;;
        *)
            echo "Opción no válida, elige un número del 1 al 4."
            ;;
    esac

    echo ""
    read -p "Presiona Enter para volver al menú..."
    clear
done
```

</details>

<details>
<summary>Grabación en asciinema</summary>

[![asciicast](https://asciinema.org/a/TU_ID.svg)](https://asciinema.org/a/TU_ID)

</details>
</details>
