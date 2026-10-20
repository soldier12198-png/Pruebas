# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>


<details>
<summary>Tarea #993</summary>

# **Hacer un menú en bash que permita elegir la ejecución entre el script para crear un árbol de directorios, el script de hola mundo y el script de saludo usando variables y por último la opción de salir.**

<details>
<summary>menu.sh</summary>

```bash
#!/bin/bash
# Menú para elegir qué script ejecutar

while true; do
    echo ""
    echo "========= MENÚ ========="
    echo "1) Crear árbol de directorios"
    echo "2) Hola mundo"
    echo "3) Saludo con variables"
    echo "4) Salir"
    read -p "Elige una opción: " opcion

    case $opcion in
        1) bash arbol.sh ;;
        2) bash hola.sh ;;
        3) bash saludo.sh ;;
        4) echo "¡Adiós!"; break ;;
        *) echo "Opción no válida, intenta de nuevo." ;;
    esac
done
```

</details>

<details>
<summary>Ejecución</summary>

- bash menu.sh

</details>

<details>
<summary>Grabación en asciinema</summary>

[![asciicast](https://asciinema.org/a/48Yox1Tq3gvuvj9k.svg)](https://asciinema.org/a/48Yox1Tq3gvuvj9k)

</details>
</details>
