# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>

<details>
<summary>Tarea #987</summary>

# **Construir un pyjail (reto de escape en Python)**

<details>
<summary>Paso 1 - Entorno</summary>

- Carpeta `mi-jail/` con `flag.txt` y `jail.py`
- `flag.txt`: `FLAG{primer_escape}`
- Correr: `python jail.py`

</details>

<details>
<summary>Paso 2 - Jail con eval</summary>

```python
entrada = input(">>> ")
print(eval(entrada))
```

</details>

<details>
<summary>Paso 3 - Lista negra</summary>

```python
prohibidas = ["import", "open", "os", "flag", "eval", "exec", '"', "'"]
for p in prohibidas:
    if p in entrada:
        print("Bloqueado"); exit()
print(eval(entrada))
```

</details>

<details>
<summary>Paso 4 - Quitar los builtins</summary>

```python
print(eval(entrada, {"__builtins__": {}}))
```

</details>

<details>
<summary>Paso 5 - Aislar con Docker</summary>

```dockerfile
FROM python:3.12-slim
WORKDIR /reto
COPY flag.txt jail.py ./
CMD ["python", "jail.py"]
```

- `docker build -t mi-jail .`
- `docker run -it --rm mi-jail`

</details>
