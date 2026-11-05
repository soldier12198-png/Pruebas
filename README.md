# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>

<details>
<summary>Tarea #987</summary>

# **Construir un pyjail (reto de escape tipo CTF) en Python**

Un *pyjail* es un reto donde el jugador escribe código Python que el programa ejecuta con restricciones, y el objetivo es "escapar" de ellas para leer una flag secreta. Se construye paso a paso, reforzándolo cada vez.

<details>
<summary>Paso 1 - Preparar el entorno</summary>

Estructura de la carpeta:

- `mi-jail/`
  - `flag.txt`
  - `jail.py`

Contenido de `flag.txt`:

```
FLAG{primer_escape}
```

Se ejecuta con `python jail.py`.

</details>

<details>
<summary>Paso 2 - Jail básico con eval (puerta abierta)</summary>

`eval()` ejecuta como código cualquier texto que reciba, no solo matemáticas.

```python
print("=== Bienvenido a la cárcel ===")
print("Soy una calculadora. Escribe una operación.")
entrada = input(">>> ")
resultado = eval(entrada)
print("Resultado:", resultado)
```

Sin restricciones, el jugador lee la flag directo:

```
open("flag.txt").read()
```

</details>

<details>
<summary>Paso 3 - Barrotes: lista negra (blacklist)</summary>

Se rechaza la entrada si contiene palabras prohibidas.

```python
print("=== Bienvenido a la cárcel ===")
entrada = input(">>> ")

prohibidas = ["import", "open", "os", "flag"]

for palabra in prohibidas:
    if palabra in entrada:
        print("¡Palabra prohibida detectada!")
        exit()

print("Resultado:", eval(entrada))
```

</details>

<details>
<summary>Paso 4 - Romper la lista negra y reforzarla</summary>

La lista negra se esquiva escribiendo las palabras en pedazos, porque el filtro busca el texto exacto:

```
eval("op"+"en")("fl"+"ag.txt").read()
```

Se refuerza prohibiendo también `eval`, `exec` y las comillas:

```python
prohibidas = ["import", "open", "os", "flag", "eval", "exec", '"', "'"]
```

Conclusión: una lista negra nunca cubre todas las formas de escribir algo, así que no basta por sí sola.

</details>

<details>
<summary>Paso 5 - Quitarle las herramientas a Python</summary>

En vez de prohibir palabras, se vacía la caja de herramientas: `open`, `getattr`, `chr`, etc. dejan de existir dentro de la celda.

```python
resultado = eval(entrada, {"__builtins__": {}})
```

Con esto `2 + 3 * 10` sigue dando `32`, pero los trucos que armaban texto letra por letra fallan con `NameError`.

</details>

<details>
<summary>Paso 6 - Aislar con Docker (el muro de verdad)</summary>

Como ningún pyjail es 100% inescapable, se aísla en un contenedor para que un posible escape llegue solo a la cajita y no a la máquina real.

`Dockerfile`:

```dockerfile
FROM python:3.12-slim
WORKDIR /reto
COPY flag.txt jail.py ./
CMD ["python", "jail.py"]
```

Construir y correr:

```
docker build -t mi-jail .
docker run -it --rm mi-jail
```

</details>
