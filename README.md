# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>

<details>
<summary>Tarea #982</summary>

# **Crear un gif que explique algún concepto**

<details>
<summary>GIF: ¿Qué es una manzana?</summary>

![¿Qué es una manzana?](tarea-982/manzana.gif)

</details>

<details>
<summary>Qué explica</summary>

El GIF explica el concepto de **manzana** en 6 pasos:

1. **¿Qué es?** El fruto del manzano (*Malus domestica*), un árbol de la familia de las rosáceas, igual que la rosa, la pera y la fresa. Es un fruto de tipo pomo: carnoso y con las semillas en el centro.
2. **Sus partes:** tallo (pedúnculo), piel, pulpa, corazón, semillas y cáliz (restos de la flor), sobre un corte de la manzana.
3. **¿De dónde sale?** Flor en primavera → polinización por abejas → crece el fruto pequeño y verde → manzana madura en verano y otoño.
4. **¿Qué tiene por dentro?** En 100 g de manzana cruda con piel: 86% agua, 10% azúcares naturales, 2% fibra y 2% otros nutrientes; unas 52 kcal. Fuente: USDA FoodData Central.
5. **Datos curiosos:** hay más de 7,500 variedades (Red Delicious, Granny Smith, Golden Delicious...) y su antepasado silvestre viene de Asia Central (Kazajistán).
6. **En resumen:** la manzana es el fruto del manzano; nace de una flor, guarda sus semillas en el corazón y casi toda ella es agua.

</details>

<details>
<summary>Cómo se hizo</summary>

- El GIF se generó con Python y la librería Pillow: el script dibuja cada cuadro (manzana, corte, flor, abeja, barras) con figuras geométricas y texto, y al final los une en un GIF animado.
- Para regenerarlo: `pip install pillow` y luego `python3 generar_gif.py`.
- Datos técnicos: 800×500 píxeles, 209 cuadros, ~59 segundos, se repite en bucle.

```python
# Tarea #982: GIF que explica un concepto: ¿Qué es una manzana?
# Autor: Fabrizio Soldi Esparza
# Genera manzana.gif dibujando cada cuadro con Pillow.
# Uso: python3 generar_gif.py

import math
from PIL import Image, ImageDraw, ImageFont

W, H = 800, 500          # tamaño final del GIF
S = 2                    # se dibuja al doble y se reduce (bordes suaves)

FUENTE = "/usr/share/fonts/truetype/dejavu/DejaVuSans.ttf"
FUENTE_NEGRITA = "/usr/share/fonts/truetype/dejavu/DejaVuSans-Bold.ttf"

FONDO = (255, 248, 240)
TINTA = (45, 35, 30)
GRIS = (125, 115, 105)
LINEA = (225, 210, 195)
ROJO, ROJO_OSC, BRILLO = (214, 40, 40), (150, 22, 28), (245, 120, 110)
VERDE, VERDE_OSC = (95, 165, 60), (55, 115, 40)
VERDE_M, VERDE_M_OSC, VERDE_M_BRI = (140, 200, 70), (85, 140, 35), (195, 230, 140)
AMAR_M, AMAR_M_OSC, AMAR_M_BRI = (240, 205, 60), (185, 145, 25), (252, 238, 160)
CAFE = (105, 65, 35)
PULPA, CORAZON, SEMILLA = (250, 236, 200), (236, 212, 160), (85, 45, 20)
AMARILLO, AZUL, NARANJA = (240, 195, 45), (70, 140, 210), (240, 150, 50)
ROSA, ROSA_OSC = (250, 218, 228), (225, 160, 182)

_fuentes = {}


def fuente(tam, negrita=False):
    if (tam, negrita) not in _fuentes:
        ruta = FUENTE_NEGRITA if negrita else FUENTE
        _fuentes[(tam, negrita)] = ImageFont.truetype(ruta, int(tam * S))
    return _fuentes[(tam, negrita)]


def mezcla(color, p):
    """Color que aparece poco a poco desde el fondo (p de 0 a 1)."""
    p = max(0.0, min(1.0, p))
    return tuple(int(FONDO[i] + (color[i] - FONDO[i]) * p) for i in range(3))


def P(x, y):
    return (x * S, y * S)


def C(x0, y0, x1, y1):
    return [x0 * S, y0 * S, x1 * S, y1 * S]


def avance(k, i):
    """Qué tan visible está el elemento i cuando la escena va en k."""
    return max(0.0, min(1.0, k - i))


def suave(t):
    return 1 - (1 - t) ** 3


def rebote(t):
    c = 1.70158
    return 1 + (c + 1) * (t - 1) ** 3 + c * (t - 1) ** 2


def texto(d, x, y, s, tam, color=TINTA, negrita=False, p=1, anchor="la"):
    if p > 0:
        d.text(P(x, y), s, font=fuente(tam, negrita), fill=mezcla(color, p), anchor=anchor)


# ---------- Dibujos ----------

def cuerpo(d, cx, cy, r, color, margen=0.0):
    m = margen
    d.ellipse(C(cx - 1.0 * r + m, cy - 0.85 * r + m, cx + 0.35 * r - m, cy + 0.9 * r - m), fill=color)
    d.ellipse(C(cx - 0.35 * r + m, cy - 0.85 * r + m, cx + 1.0 * r - m, cy + 0.9 * r - m), fill=color)


def tallo(d, cx, cy, r, p):
    a, b, c = (cx, cy - 0.5 * r), (cx - 0.03 * r, cy - 0.9 * r), (cx + 0.12 * r, cy - 1.07 * r)
    puntos = []
    for i in range(16):
        t = i / 15
        x = (1 - t) ** 2 * a[0] + 2 * (1 - t) * t * b[0] + t ** 2 * c[0]
        y = (1 - t) ** 2 * a[1] + 2 * (1 - t) * t * b[1] + t ** 2 * c[1]
        puntos.append(P(x, y))
    d.line(puntos, fill=mezcla(CAFE, p), width=max(2, int(0.085 * r * S)), joint="curve")


def hoja(d, x, y, largo, ancho, angulo, p):
    contorno = [(t / 20 * largo, ancho / 2 * math.sin(math.pi * t / 20)) for t in range(21)]
    contorno += [(t / 20 * largo, -ancho / 2 * math.sin(math.pi * t / 20)) for t in range(20, -1, -1)]
    ca, sa = math.cos(math.radians(angulo)), math.sin(math.radians(angulo))
    d.polygon([P(x + px * ca - py * sa, y + px * sa + py * ca) for px, py in contorno], fill=mezcla(VERDE, p))
    d.line([P(x, y), P(x + 0.9 * largo * ca, y + 0.9 * largo * sa)],
           fill=mezcla(VERDE_OSC, p), width=max(1, int(0.012 * largo * S * 3)))


def manzana(d, cx, cy, r, p=1, color=ROJO, oscuro=ROJO_OSC, brillo=BRILLO, con_hoja=True):
    if p <= 0 or r < 2:
        return
    tallo(d, cx, cy, r, p)
    if con_hoja:
        hoja(d, cx + 0.07 * r, cy - 0.9 * r, 0.6 * r, 0.26 * r, -28, p)
    cuerpo(d, cx, cy, r, mezcla(oscuro, p))
    cuerpo(d, cx, cy, r, mezcla(color, p), margen=0.04 * r)
    d.ellipse(C(cx - 0.68 * r, cy - 0.42 * r, cx - 0.42 * r, cy - 0.02 * r), fill=mezcla(brillo, p))


def semilla(d, x, y, a, b, angulo, p):
    ca, sa = math.cos(math.radians(angulo)), math.sin(math.radians(angulo))
    puntos = []
    for i in range(30):
        t = 2 * math.pi * i / 30
        px = a * math.cos(t) * (0.5 + 0.5 * (1 + math.sin(t)) / 2)
        py = b * math.sin(t)
        puntos.append(P(x + px * ca - py * sa, y + px * sa + py * ca))
    d.polygon(puntos, fill=mezcla(SEMILLA, p))


def corte(d, cx, cy, r, p=1):
    tallo(d, cx, cy, r, p)
    cuerpo(d, cx, cy, r, mezcla(ROJO_OSC, p))
    cuerpo(d, cx, cy, r, mezcla(ROJO, p), margen=0.035 * r)
    cuerpo(d, cx, cy, r, mezcla(PULPA, p), margen=0.075 * r)
    d.ellipse(C(cx - 0.24 * r, cy - 0.4 * r, cx + 0.24 * r, cy + 0.42 * r), fill=mezcla(CORAZON, p))
    for sx, sy, ang in [(-0.1, -0.1, 18), (0.1, -0.1, -18), (-0.1, 0.17, -18), (0.1, 0.17, 18)]:
        semilla(d, cx + sx * r, cy + sy * r, 0.07 * r, 0.12 * r, ang, p)
    for ang in (-60, -20, 20, 60):          # cáliz
        a = math.radians(ang + 90)
        d.line([P(cx, cy + 0.68 * r), P(cx + 0.09 * r * math.cos(a), cy + 0.68 * r + 0.09 * r * math.sin(a))],
               fill=mezcla(CAFE, p), width=max(1, int(0.025 * r * S)))


def flor(d, cx, cy, r, p):
    for i in range(5):
        a = -math.pi / 2 + i * 2 * math.pi / 5
        x, y = cx + 0.55 * r * math.cos(a), cy + 0.55 * r * math.sin(a)
        d.ellipse(C(x - 0.5 * r, y - 0.5 * r, x + 0.5 * r, y + 0.5 * r),
                  fill=mezcla(ROSA, p), outline=mezcla(ROSA_OSC, p), width=S * 2)
    d.ellipse(C(cx - 0.32 * r, cy - 0.32 * r, cx + 0.32 * r, cy + 0.32 * r), fill=mezcla(AMARILLO, p))


def abeja(d, cx, cy, r, p):
    ala = mezcla((205, 228, 248), p)
    d.ellipse(C(cx - 0.55 * r, cy - 1.15 * r, cx + 0.15 * r, cy - 0.3 * r), fill=ala)
    d.ellipse(C(cx - 0.05 * r, cy - 1.05 * r, cx + 0.65 * r, cy - 0.3 * r), fill=ala)
    d.ellipse(C(cx - r, cy - 0.6 * r, cx + r, cy + 0.6 * r), fill=mezcla(AMARILLO, p))
    for ox in (-0.35, 0.1, 0.55):
        h = 0.6 * r * math.sqrt(1 - ox ** 2)
        d.line([P(cx + ox * r, cy - h), P(cx + ox * r, cy + h)], fill=mezcla(TINTA, p), width=int(0.17 * r * S))
    d.ellipse(C(cx - 0.8 * r, cy - 0.2 * r, cx - 0.62 * r, cy - 0.02 * r), fill=mezcla(TINTA, p))
    d.polygon([P(cx + r * 0.97, cy - 0.12 * r), P(cx + 1.3 * r, cy), P(cx + r * 0.97, cy + 0.12 * r)],
              fill=mezcla(TINTA, p))


def flecha(d, x0, x1, y, p):
    if p <= 0:
        return
    x1p = x0 + (x1 - x0) * suave(p)
    d.line([P(x0, y), P(x1p - 6, y)], fill=mezcla(GRIS, p), width=S * 3)
    d.polygon([P(x1p, y), P(x1p - 12, y - 7), P(x1p - 12, y + 7)], fill=mezcla(GRIS, p))


def etiqueta(d, px, py, tx, ty, s, p, izquierda=False, detalle=""):
    if p <= 0:
        return
    color = mezcla(TINTA, p)
    d.line([P(px, py), P(tx, ty)], fill=color, width=S * 2)
    d.ellipse(C(px - 4, py - 4, px + 4, py + 4), fill=color)
    x, anchor = (tx - 8, "rm") if izquierda else (tx + 8, "lm")
    texto(d, x, ty, s, 19, TINTA, True, p, anchor)
    if detalle:
        texto(d, x, ty + 22, detalle, 14, GRIS, False, p, anchor)


def encabezado(d, s):
    texto(d, 40, 28, s, 28, TINTA, True)
    d.line([P(40, 72), P(760, 72)], fill=LINEA, width=S * 2)


# ---------- Escenas ----------
# Cada escena recibe k: va de 0 a n y cada elemento aparece cuando k pasa su número.

def portada(d, k):
    t = avance(k, 0)
    manzana(d, 400, 205, 100 * rebote(t) if t > 0 else 0, p=min(1, t * 3))
    texto(d, 400, 360, "¿Qué es una manzana?", 42, TINTA, True, avance(k, 1), "mm")
    texto(d, 400, 410, "Un concepto explicado en 6 pasos", 20, GRIS, False, avance(k, 2), "mm")


def que_es(d, k):
    encabezado(d, "1. ¿Qué es?")
    manzana(d, 190, 270, 95)
    texto(d, 330, 150, "Es el fruto del manzano,", 26, TINTA, True, avance(k, 0))
    texto(d, 330, 190, "un árbol llamado Malus domestica.", 20, TINTA, False, avance(k, 1))
    texto(d, 330, 240, "Es de la familia de las rosáceas:", 20, TINTA, False, avance(k, 2))
    texto(d, 330, 268, "la misma que la rosa, la pera y la fresa.", 20, GRIS, False, avance(k, 2))
    texto(d, 330, 318, "Es un fruto de tipo pomo:", 20, TINTA, False, avance(k, 3))
    texto(d, 330, 346, "carnoso y con las semillas en el centro.", 20, GRIS, False, avance(k, 3))


def partes(d, k):
    encabezado(d, "2. Sus partes")
    cx, cy, r = 400, 280, 125
    corte(d, cx, cy, r)
    etiqueta(d, cx + 0.035 * r, cy - 0.88 * r, 560, 135, "Tallo (pedúnculo)", avance(k, 0))
    etiqueta(d, cx + 0.975 * r, cy, 590, 245, "Piel", avance(k, 1), detalle="protege el fruto")
    etiqueta(d, cx + 0.55 * r, cy + 0.3 * r, 590, 345, "Pulpa", avance(k, 2), detalle="la parte que se come")
    etiqueta(d, cx - 0.2 * r, cy - 0.3 * r, 215, 190, "Corazón", avance(k, 3), izquierda=True)
    etiqueta(d, cx - 0.1 * r, cy + 0.17 * r, 215, 300, "Semillas", avance(k, 4), izquierda=True)
    etiqueta(d, cx, cy + 0.72 * r, 215, 405, "Cáliz", avance(k, 5), izquierda=True, detalle="restos de la flor")


def de_donde(d, k):
    encabezado(d, "3. ¿De dónde sale?")
    xs = [100, 295, 490, 685]
    y = 235
    flor(d, xs[0], y, 42, avance(k, 0))
    p = avance(k, 1)
    flecha(d, xs[0] + 60, xs[1] - 60, y, p)
    flor(d, xs[1] + 8, y + 18, 26, p)
    abeja(d, xs[1] - 18, y - 22, 26, p)
    p = avance(k, 2)
    flecha(d, xs[1] + 60, xs[2] - 60, y, p)
    manzana(d, xs[2], y + 8, 30, p, VERDE_M, VERDE_M_OSC, VERDE_M_BRI, con_hoja=False)
    p = avance(k, 3)
    flecha(d, xs[2] + 60, xs[3] - 60, y, p)
    manzana(d, xs[3], y + 8, 52, p)
    pasos = [("1. Flor", "sale en primavera"),
             ("2. Polinización", "gracias a las abejas"),
             ("3. Crece el fruto", "pequeño y verde"),
             ("4. Manzana madura", "en verano y otoño")]
    for i, (titulo, detalle) in enumerate(pasos):
        texto(d, xs[i], 330, titulo, 17, TINTA, True, avance(k, i), "mm")
        texto(d, xs[i], 356, detalle, 14, GRIS, False, avance(k, i), "mm")


def por_dentro(d, k):
    encabezado(d, "4. ¿Qué tiene por dentro?")
    texto(d, 400, 115, "Composición de 100 g de manzana cruda con piel", 19, GRIS, False, 1, "mm")
    partes_ = [("Agua", 86, AZUL), ("Azúcares naturales", 10, NARANJA),
               ("Fibra", 2, VERDE), ("Otros nutrientes", 2, GRIS)]
    x0, ancho, y0, alto = 80, 640, 150, 56
    lleno = ancho * suave(avance(k, 0))
    x = x0
    for nombre, porc, color in partes_:
        w = ancho * porc / 100
        visible = max(0, min(w, x0 + lleno - x))
        if visible > 0:
            d.rectangle(C(x, y0, x + visible, y0 + alto), fill=color)
        x += w
    if avance(k, 0) > 0.6:
        texto(d, x0 + 20, y0 + alto / 2, "86% agua", 22, (255, 255, 255), True, 1, "lm")
    for i, (nombre, porc, color) in enumerate(partes_):
        p = avance(k, 1 + i)
        cx = 110 if i % 2 == 0 else 430
        cy = 260 + (i // 2) * 50
        if p > 0:
            d.rectangle(C(cx, cy - 12, cx + 24, cy + 12), fill=mezcla(color, p))
            texto(d, cx + 36, cy, f"{nombre}: {porc}%", 20, TINTA, i == 0, p, "lm")
    texto(d, 400, 390, "Energía: unas 52 kcal por cada 100 g", 22, TINTA, True, avance(k, 5), "mm")
    texto(d, 400, 425, "Fuente: USDA FoodData Central", 14, GRIS, False, avance(k, 5), "mm")


def curiosos(d, k):
    encabezado(d, "5. Datos curiosos")
    p = avance(k, 0)
    manzana(d, 200, 200, 58, p)
    manzana(d, 400, 200, 58, p, VERDE_M, VERDE_M_OSC, VERDE_M_BRI)
    manzana(d, 600, 200, 58, p, AMAR_M, AMAR_M_OSC, AMAR_M_BRI)
    for x, nombre in [(200, "Red Delicious"), (400, "Granny Smith"), (600, "Golden Delicious")]:
        texto(d, x, 290, nombre, 17, GRIS, True, p, "mm")
    texto(d, 400, 350, "Hay más de 7,500 variedades en el mundo.", 21, TINTA, True, avance(k, 1), "mm")
    texto(d, 400, 395, "Su antepasado silvestre viene de Asia Central (Kazajistán).", 18, TINTA, False, avance(k, 2), "mm")


def resumen(d, k):
    encabezado(d, "6. En resumen")
    manzana(d, 400, 185, 62)
    texto(d, 400, 290, "La manzana es el fruto del manzano.", 23, TINTA, True, avance(k, 0), "mm")
    texto(d, 400, 330, "Nace de una flor, guarda sus semillas en el corazón", 20, TINTA, False, avance(k, 1), "mm")
    texto(d, 400, 360, "y protege todo con la pulpa y la piel.", 20, TINTA, False, avance(k, 1), "mm")
    texto(d, 400, 410, "Y casi toda ella (86%) es agua.", 22, ROJO, True, avance(k, 2), "mm")


ESCENAS = [(portada, 3), (que_es, 4), (partes, 6), (de_donde, 4),
           (por_dentro, 6), (curiosos, 3), (resumen, 3)]


def marco(d, indice):
    texto(d, 40, 476, "Tarea #982 · Fabrizio Soldi Esparza", 13, GRIS, False, 1, "lm")
    for i in range(len(ESCENAS)):
        x = 640 + i * 18
        relleno = ROJO if i == indice else LINEA
        d.ellipse(C(x - 5, 476 - 5, x + 5, 476 + 5), fill=relleno)


def cuadro(funcion, k, indice):
    img = Image.new("RGB", (W * S, H * S), FONDO)
    d = ImageDraw.Draw(img)
    funcion(d, k)
    marco(d, indice)
    return img.resize((W, H), Image.LANCZOS)


def main():
    cuadros, duraciones = [], []
    PASOS_ANIM, MS_ANIM, MS_FUNDIDO = 6, 70, 60

    def fundido(desde, hasta):
        for j in range(1, 5):
            cuadros.append(Image.blend(desde, hasta, j / 5))
            duraciones.append(MS_FUNDIDO)

    for indice, (funcion, n) in enumerate(ESCENAS):
        inicio = cuadro(funcion, 0, indice)
        if cuadros:
            fundido(cuadros[-1], inicio)
        cuadros.append(inicio)
        duraciones.append(400)
        for paso in range(n):
            for j in range(1, PASOS_ANIM + 1):
                cuadros.append(cuadro(funcion, paso + j / PASOS_ANIM, indice))
                duraciones.append(MS_ANIM)
            duraciones[-1] = 2600 if paso == n - 1 else 1200

    fundido(cuadros[-1], cuadros[0])

    # Una sola paleta para todos los cuadros: los colores no "parpadean"
    # entre cuadros y el archivo pesa menos
    muestras = cuadros[::8]
    tira = Image.new("RGB", (W, H * len(muestras)))
    for i, c in enumerate(muestras):
        tira.paste(c, (0, H * i))
    paleta = tira.quantize(colors=255, method=Image.Quantize.MEDIANCUT)
    finales = [c.quantize(palette=paleta, dither=Image.Dither.NONE) for c in cuadros]

    finales[0].save("manzana.gif", save_all=True, append_images=finales[1:],
                    duration=duraciones, loop=0, optimize=True)
    print(f"manzana.gif: {len(cuadros)} cuadros, {sum(duraciones) / 1000:.1f} segundos")


if __name__ == "__main__":
    main()
```

</details>

</details>
