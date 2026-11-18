# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>

<details>
<summary>Tarea #985</summary>

# **Realizar en Python una herramienta que permita interactuar con el modulo "os"**

<details>
<summary>Código: herramienta_os.py</summary>

```python
# Tarea #985: herramienta en Python para interactuar con el módulo "os"
# Autor: Fabrizio Soldi Esparza
#
# Uso: python3 herramienta_os.py

import os
import stat
import time


# ---------- Funciones de apoyo ----------

def limpiar_pantalla():
    # os.name vale 'posix' en Linux/macOS y 'nt' en Windows
    os.system("clear" if os.name == "posix" else "cls")


def pausa():
    input("\nPresiona Enter para volver al menú...")


def tamano_legible(bytes_):
    unidades = ["B", "KB", "MB", "GB", "TB"]
    tamano = float(bytes_)
    for unidad in unidades:
        if tamano < 1024 or unidad == unidades[-1]:
            return f"{tamano:.1f} {unidad}" if unidad != "B" else f"{int(tamano)} B"
        tamano /= 1024


def fecha(segundos):
    return time.strftime("%d/%m/%Y %H:%M:%S", time.localtime(segundos))


def pedir_ruta(mensaje):
    # expanduser convierte ~ en la carpeta personal
    return os.path.expanduser(input(mensaje).strip())


def confirmar(mensaje):
    respuesta = input(f"{mensaje} (s/n): ").strip().lower()
    return respuesta == "s"


# ---------- Opciones del menú ----------

def info_sistema():
    print("=== Información del sistema ===")
    print(f"Tipo de sistema (os.name):   {os.name}")
    if hasattr(os, "uname"):
        datos = os.uname()
        print(f"Sistema operativo:           {datos.sysname}")
        print(f"Nombre del equipo:           {datos.nodename}")
        print(f"Versión del kernel:          {datos.release}")
        print(f"Arquitectura:                {datos.machine}")
    print(f"Núcleos de CPU:              {os.cpu_count()}")
    usuario = os.getenv("USER") or os.getenv("USERNAME")
    if usuario is None:
        try:
            usuario = os.getlogin()
        except OSError:
            usuario = "desconocido"
    print(f"Usuario:                     {usuario}")
    if hasattr(os, "getuid"):
        print(f"UID del usuario:             {os.getuid()}")
    print(f"PID de este programa:        {os.getpid()}")
    print(f"PID del proceso padre:       {os.getppid()}")
    print(f"Carpeta personal:            {os.path.expanduser('~')}")
    print(f"Separador de rutas:          {os.sep}")


def carpeta_actual():
    print(f"Estás en: {os.getcwd()}")


def cambiar_carpeta():
    ruta = pedir_ruta("¿A qué carpeta quieres ir? ")
    os.chdir(ruta)
    print(f"Ahora estás en: {os.getcwd()}")


def listar_carpeta():
    ruta = pedir_ruta("Carpeta a listar (Enter = la actual): ") or "."
    elementos = sorted(os.scandir(ruta), key=lambda e: (not e.is_dir(), e.name.lower()))
    print(f"\nContenido de {os.path.abspath(ruta)}:\n")
    print(f"{'TIPO':<10}{'TAMAÑO':>10}   NOMBRE")
    for elemento in elementos:
        if elemento.is_symlink():
            tipo = "enlace"
        elif elemento.is_dir():
            tipo = "carpeta"
        else:
            tipo = "archivo"
        if elemento.is_file(follow_symlinks=False):
            tamano = tamano_legible(elemento.stat(follow_symlinks=False).st_size)
        else:
            tamano = "-"
        print(f"{tipo:<10}{tamano:>10}   {elemento.name}")
    print(f"\nTotal: {len(elementos)} elementos")


def crear_carpeta():
    ruta = pedir_ruta("Nombre de la carpeta nueva (puede ser a/b/c): ")
    os.makedirs(ruta)
    print(f"Carpeta creada: {os.path.abspath(ruta)}")


def crear_archivo():
    ruta = pedir_ruta("Nombre del archivo nuevo: ")
    texto = input("Escribe el texto que tendrá: ")
    # os.open trabaja a bajo nivel, como las llamadas al sistema de Linux.
    # O_EXCL hace que falle si el archivo ya existe, para no borrar nada.
    descriptor = os.open(ruta, os.O_WRONLY | os.O_CREAT | os.O_EXCL, 0o644)
    os.write(descriptor, (texto + "\n").encode("utf-8"))
    os.close(descriptor)
    print(f"Archivo creado: {os.path.abspath(ruta)}")


def leer_archivo():
    ruta = pedir_ruta("Archivo a leer: ")
    descriptor = os.open(ruta, os.O_RDONLY)
    contenido = os.read(descriptor, 4096)      # lee hasta 4 KB
    os.close(descriptor)
    print(f"\n--- Contenido de {ruta} (primeros 4 KB) ---")
    print(contenido.decode("utf-8", errors="replace"))


def info_archivo():
    ruta = pedir_ruta("Archivo o carpeta: ")
    datos = os.stat(ruta)
    print(f"\n=== {os.path.abspath(ruta)} ===")
    if os.path.isdir(ruta):
        tipo = "carpeta"
    elif os.path.islink(ruta):
        tipo = "enlace"
    else:
        tipo = "archivo"
    nombre, extension = os.path.splitext(os.path.basename(ruta))
    print(f"Tipo:                {tipo}")
    print(f"Extensión:           {extension or '(sin extensión)'}")
    print(f"Tamaño:              {datos.st_size} bytes ({tamano_legible(datos.st_size)})")
    print(f"Permisos:            {stat.filemode(datos.st_mode)} (octal {oct(datos.st_mode & 0o777)[2:]})")
    print(f"Puedes leer:         {'sí' if os.access(ruta, os.R_OK) else 'no'}")
    print(f"Puedes escribir:     {'sí' if os.access(ruta, os.W_OK) else 'no'}")
    print(f"Puedes ejecutar:     {'sí' if os.access(ruta, os.X_OK) else 'no'}")
    print(f"Dueño (UID/GID):     {datos.st_uid}/{datos.st_gid}")
    print(f"Inodo:               {datos.st_ino}")
    print(f"Enlaces duros:       {datos.st_nlink}")
    print(f"Última modificación: {fecha(datos.st_mtime)}")
    print(f"Último acceso:       {fecha(datos.st_atime)}")


def renombrar():
    origen = pedir_ruta("Archivo o carpeta a renombrar/mover: ")
    destino = pedir_ruta("Nuevo nombre o ruta: ")
    if os.path.exists(destino):
        print("Ya existe algo con ese nombre; no se hizo nada.")
        return
    os.rename(origen, destino)
    print(f"'{origen}' ahora es '{destino}'")


def cambiar_permisos():
    ruta = pedir_ruta("Archivo o carpeta: ")
    actual = oct(os.stat(ruta).st_mode & 0o777)[2:]
    print(f"Permisos actuales: {actual}")
    nuevos = input("Permisos nuevos en octal (ej. 755, 644): ").strip()
    os.chmod(ruta, int(nuevos, 8))
    print(f"Permisos de '{ruta}': {stat.filemode(os.stat(ruta).st_mode)}")


def eliminar_archivo():
    ruta = pedir_ruta("Archivo a eliminar: ")
    if not os.path.isfile(ruta):
        print("Eso no es un archivo (o no existe).")
        return
    if confirmar(f"¿Seguro que quieres eliminar '{ruta}'?"):
        os.remove(ruta)
        print("Archivo eliminado.")
    else:
        print("Cancelado.")


def eliminar_carpeta():
    ruta = pedir_ruta("Carpeta VACÍA a eliminar: ")
    if confirmar(f"¿Seguro que quieres eliminar '{ruta}'?"):
        os.rmdir(ruta)          # solo borra carpetas vacías
        print("Carpeta eliminada.")
    else:
        print("Cancelado.")


def recorrer_arbol():
    ruta = pedir_ruta("Carpeta a recorrer (Enter = la actual): ") or "."
    total_archivos = 0
    total_carpetas = 0
    for carpeta, subcarpetas, archivos in os.walk(ruta):
        nivel = carpeta[len(ruta):].count(os.sep)
        sangria = "    " * nivel
        print(f"{sangria}{os.path.basename(carpeta) or carpeta}/")
        for archivo in archivos:
            print(f"{sangria}    {archivo}")
        total_carpetas += len(subcarpetas)
        total_archivos += len(archivos)
    print(f"\n{total_carpetas} subcarpetas, {total_archivos} archivos")


def variables_entorno():
    nombre = input("Variable a consultar (Enter = ver las principales): ").strip()
    if nombre:
        valor = os.getenv(nombre)
        if valor is None:
            print(f"La variable {nombre} no existe.")
        else:
            print(f"{nombre} = {valor}")
    else:
        for variable in ["USER", "HOME", "SHELL", "LANG", "PWD", "PATH"]:
            print(f"{variable:<6} = {os.environ.get(variable, '(no definida)')}")
        print(f"\nHay {len(os.environ)} variables de entorno en total.")


def ejecutar_comando():
    comando = input("Comando a ejecutar (ej. ls -l, date, whoami): ").strip()
    if comando:
        codigo = os.system(comando)
        print(f"\nEl comando terminó con código {os.waitstatus_to_exitcode(codigo) if os.name == 'posix' else codigo}")


# ---------- Menú principal ----------

OPCIONES = [
    ("Información del sistema", info_sistema),
    ("Ver carpeta actual", carpeta_actual),
    ("Cambiar de carpeta", cambiar_carpeta),
    ("Listar contenido de una carpeta", listar_carpeta),
    ("Crear carpeta", crear_carpeta),
    ("Crear archivo de texto", crear_archivo),
    ("Leer archivo", leer_archivo),
    ("Información de un archivo", info_archivo),
    ("Renombrar o mover", renombrar),
    ("Cambiar permisos", cambiar_permisos),
    ("Eliminar archivo", eliminar_archivo),
    ("Eliminar carpeta vacía", eliminar_carpeta),
    ("Recorrer árbol de carpetas", recorrer_arbol),
    ("Variables de entorno", variables_entorno),
    ("Ejecutar un comando del sistema", ejecutar_comando),
]


def mostrar_menu():
    limpiar_pantalla()
    print("=" * 50)
    print("     HERRAMIENTA PARA EL MÓDULO os DE PYTHON")
    print("=" * 50)
    print(f"Carpeta actual: {os.getcwd()}\n")
    for numero, (texto, _) in enumerate(OPCIONES, start=1):
        print(f"  {numero:>2}) {texto}")
    print("   0) Salir")
    print("=" * 50)


def main():
    while True:
        mostrar_menu()
        opcion = input("Elige una opción: ").strip()

        if opcion == "0":
            print("¡Hasta luego!")
            break
        elif opcion.isdigit() and 1 <= int(opcion) <= len(OPCIONES):
            texto, funcion = OPCIONES[int(opcion) - 1]
            print(f"\n>>> {texto}\n")
            try:
                funcion()
            except FileNotFoundError:
                print("Error: no existe ese archivo o carpeta.")
            except FileExistsError:
                print("Error: ya existe un archivo o carpeta con ese nombre.")
            except PermissionError:
                print("Error: no tienes permiso para hacer eso.")
            except NotADirectoryError:
                print("Error: eso no es una carpeta.")
            except IsADirectoryError:
                print("Error: eso es una carpeta, no un archivo.")
            except ValueError:
                print("Error: el valor que escribiste no es válido.")
            except OSError as error:
                print(f"Error del sistema: {error.strerror}")
        else:
            print(f"Opción no válida: '{opcion}'. Escribe un número del 0 al {len(OPCIONES)}.")

        pausa()


if __name__ == "__main__":
    try:
        main()
    except (KeyboardInterrupt, EOFError):
        print("\n¡Hasta luego!")
```

</details>

<details>
<summary>Cómo ejecutarlo</summary>

- `python3 herramienta_os.py`
- Escribir el número de la opción y presionar Enter.
- Después de cada opción, presionar Enter para volver al menú. La opción 0 sale del programa.

</details>

<details>
<summary>Opciones y funciones de os que usa</summary>

| Opción | Qué hace | Funciones del módulo os |
|---|---|---|
| 1 | Información del sistema | `os.name`, `os.uname()`, `os.cpu_count()`, `os.getenv()`, `os.getuid()`, `os.getpid()`, `os.getppid()`, `os.sep` |
| 2 | Ver carpeta actual | `os.getcwd()` |
| 3 | Cambiar de carpeta | `os.chdir()` |
| 4 | Listar una carpeta | `os.scandir()` |
| 5 | Crear carpeta (también anidadas) | `os.makedirs()` |
| 6 | Crear archivo de texto | `os.open()`, `os.write()`, `os.close()` |
| 7 | Leer archivo | `os.open()`, `os.read()`, `os.close()` |
| 8 | Información de un archivo | `os.stat()`, `os.access()`, `os.path.splitext()` |
| 9 | Renombrar o mover | `os.rename()`, `os.path.exists()` |
| 10 | Cambiar permisos | `os.chmod()` |
| 11 | Eliminar archivo | `os.remove()` |
| 12 | Eliminar carpeta vacía | `os.rmdir()` |
| 13 | Recorrer árbol de carpetas | `os.walk()` |
| 14 | Variables de entorno | `os.environ`, `os.getenv()` |
| 15 | Ejecutar un comando del sistema | `os.system()` |

</details>

<details>
<summary>Explicación</summary>

- **Menú:** una lista `OPCIONES` guarda el texto y la función de cada opción; el `while True` muestra el menú, lee la opción con `input` y llama a la función que corresponde.
- **Funciones:** cada opción es una función aparte, así el programa es fácil de leer y de ampliar.
- **Bajo nivel:** las opciones 6 y 7 usan `os.open`, `os.write` y `os.read`, que trabajan con descriptores de archivo igual que las llamadas al sistema de Linux. `os.O_EXCL` hace que no se sobrescriba un archivo que ya existe.
- **Permisos:** la opción 10 convierte lo que escribe el usuario (por ejemplo `755`) de octal a número con `int(texto, 8)` y se lo pasa a `os.chmod`.
- **Seguridad:** antes de eliminar se pide confirmación, y `os.rmdir` solo borra carpetas vacías.
- **Errores:** `try/except` atrapa `FileNotFoundError`, `FileExistsError`, `PermissionError`, `NotADirectoryError`, `IsADirectoryError` y `ValueError`, y muestra un mensaje claro en lugar de que el programa truene.

</details>

</details>
