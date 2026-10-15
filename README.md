# Pruebas

<details>
<summary>????</summary>
**ME LA SÚPER PELAN TODOS ALV** 
</details>

<details>
<summary>Tarea #992</summary>

# **Realizar los 18 bash scripts que vienen en el PDF shell_linux.pdf que comparti en el canal de tallerso-recursos, subirlos a su repositorio. Para hacer durante la clase. **

<details>
<summary>Script 1</summary>

- echo "Hola mundo"

</details>

<details>
<summary>Script 2</summary>

- SALUDO="Hola mundo"

- echo -n "Este script te dice: "
- echo ${SALUDO}

</details>

<details>
<summary>Script 3</summary>

- una_variable=666
- MI_NOMBRE="Richard"
- NOMBRES="Iñigo Asier Sten Roberto Pello"
- BOOLEANO=true

- echo "Echemos un ojo a las variables "
- echo "Un número: ${una_variable}"
- echo "Un nombre: ${MI_NOMBRE}"
- echo "Un grupo de nombres: ${NOMBRES}"
- echo "Un booleano: ${BOOLEANO}"

- echo "Has invocado el script ${0} y el primer argumento es: ${1}"
- echo "Me has pasado $# argumentos"
- echo "PID del último proceso en segundo plano: ${!} y los parámetros: $@"
- echo "Mi directorio actual: ${PWD} y mi usuario ${UID}"

</details>

<details>
<summary>Script 4</summary>

- asociaciones[0]="Gruslin"
- asociaciones[1]="Hackresi"
- asociaciones[2]="NavarradotNET"
- asociaciones[3]="Riberlug"

- partidos=("UPN" "PSN" "CDN" "IUN" "Nafarroa BAI" "RCN")
- numeros=(15 23 45 42 23 1337 23 666 69)

- echo "${asociaciones[0]} es una asociación, ${partidos[1]} un partido"

- echo "Tamaño de asociaciones: ${#asociaciones[@]}"
- echo "Tamaño de partidos: ${#partidos[@]}"

- echo "Todos los números: ${numeros[@]}"


</details>

<details>
<summary>Script 5</summary>

- NOMBRE="Navarrux Live edition"
- echo "${NOMBRE}, una parte: ${NOMBRE:9} y otra: ${NOMBRE:9:4}"

- SINVALOR=
- echo "Variable SINVALOR: ${SINVALOR:-31337} y ${VACIO:-Miguel Indurain}"
- echo "La variable SINVALOR sigue sin valor: '${SINVALOR}'"

- echo "Variable SINVALOR: ${SINVALOR:=31337} y ${VACIO:=Miguel Indurain}"
- echo "Ahora SINVALOR sí tiene valor: ${SINVALOR} y VACIO: ${VACIO}"

- PROGRAMA='sniffit'
- echo "Valor de PROGRAMA: ${PROGRAMA:+tcpdump} y ahora ${PROGRAMA}"

- ( echo "Valor de: ${EJEMPLO:?la variable EJEMPLO no tiene valor}" )
- EJEMPLO='Roberto'
- echo "Valor de: ${EJEMPLO:?la variable EJEMPLO no tiene valor} y luego ${EJEMPLO}" 

</details>

<details>
<summary>Script 6</summary>

- VALOR1=23
- VALOR2=45
- RESULTADO=`expr ${VALOR1} + ${VALOR2}`
- echo "Resultado: ${RESULTADO}"
- RESULTADO=`expr ${VALOR1} + ${VALOR2} + 3`
- echo "Resultado: ${RESULTADO}"
- VALOR1=5
- VALOR2=4
- echo "${VALOR1} y ${VALOR2}"
- RESULTADO=$[${VALOR1} + ${VALOR2} + 2]
- echo "Ahora: ${VALOR1} + ${VALOR2} + 2 = ${RESULTADO}"
- RESULTADO=$[${VALOR1} + $[${VALOR2} * 3]]
- echo "Ahora: ${VALOR1} + ${VALOR2} * 3 = ${RESULTADO}"
- RESULTADO=$(( VALOR1 * VALOR2 ))
- echo "Con \$(( )): ${VALOR1} * ${VALOR2} = ${RESULTADO}"
- echo "Resto: 17 % 5 = $(( 17 % 5 ))"
- echo "Potencia: 2 ** 10 = $(( 2 ** 10 ))"


</details>

<details>
<summary>Script 7</summary>

- booleano=true
- $booleano && echo "OK" || echo "no es true"
- if ! $booleano; then otrobool=true; else otrobool=false; fi
- $otrobool || echo "Ahora es falso"
- valor=6
- test $valor -le 6 && echo "Se cumple"
- nuevo=${valor}
- test ${nuevo} -eq ${valor} && echo "Son lo mismo"
- echo "Ahora ${nuevo} es lo mismo que ${valor}"

</details>

<details>
<summary>Script 8</summary>

- VARIABLE=45
-  if [ ${VARIABLE} -gt 0 ]; then
-     echo "${VARIABLE} es mayor que 0"
- fi
- if [ -e /etc/shadow ]
- then
-     echo "OK, parece que tienes un sistema con shadow pass"
- fi
- OTRA=-23
- if [ ${OTRA} -lt 0 ]
- then
-     echo "${OTRA} es menor que 0"
- else
-     echo "${OTRA} es mayor que 0"
- fi
- echo -n "Mete un valor: "
- read VALOR1
- echo -n "Mete otro valor: "
- read VALOR2
- echo "Has introducido los valores ${VALOR1} y ${VALOR2}"
- if [ "${VALOR1}" -gt "${VALOR2}" ]
- then
-     echo "${VALOR1} es mayor que ${VALOR2}"
- elif [ "${VALOR1}" -lt "${VALOR2}" ]
- then
-     echo "${VALOR1} es menor que ${VALOR2}"
- else
-     echo "${VALOR1} y ${VALOR2} son iguales"
- fi
- test -f './fichero.txt' || touch fichero.txt
- test -f './fichero.txt' && echo "El fichero fichero.txt existe"

</details>

<details>
<summary>Script 9</summary>

- if [ $# -eq 0 ]
- then
-     echo "Uso: $0 fichero"
-     exit 1
- fi
- if [ -e "$1" ] && [ -f "$1" ]
- then
-     echo "OK, el fichero existe"
- else
-     echo "NO existe"
- fi
- if [ ! -e "$1" ]
- then
-     echo "No existe"
- fi

</details>

<details>
<summary>Script 10</summary>

- NOMBRE=""
- echo -n "Dame un nombre: "
- read NOMBRE
- case ${NOMBRE} in
-    iñigo)
-        echo "${NOMBRE} dice: me dedico a Navarrux"
-        ;;
-    sten)
-        echo "${NOMBRE} dice: tengo un blog friki"
-        ;;
-    asier)
-        echo "${NOMBRE} dice: .NET me gusta"
-        ;;
-    pello)
-        echo "${NOMBRE} dice: el shell mola"
-        ;;
-    *)
-        echo "A ${NOMBRE} no lo conozco"
-        ;;
- esac

</details>

<details>
<summary>Script 11</summary>

- echo "FOR simple: "
- for i in a b c d e f g h i
- do
-    echo "letra: $i"
- done
- NOMBRES="Iñigo Sten Asier Pello Roberto"
- echo "FOR simple para recorrer una lista: "
- echo "Participantes en la 4party: "
- for i in ${NOMBRES}
- do
-     echo ${i}
- done
- echo "FOR con el resultado de un comando"
- for i in `cat direcciones.txt`
- do
-    echo ${i}
- done
- echo "FOR con ficheros"
- for i in *.sh
- do
-    ls -lh ${i}
- done
- echo "FOR clásico "
- for (( cont=0 ; ${cont} < 10 ; cont=`expr $cont + 1` ))
- do
-    echo "Ahora valgo> ${cont}"
- done

</details>

<details>
<summary>Script 12</summary>

- echo "WHILE en marcha"
- cont=0
- while (test ${cont} -lt 100)
- do
-    cont=`expr $cont + 10`
-    echo "Valor del contador: ${cont}"
- done
- echo "Valor final del contador: ${cont}"

</details>

<details>
<summary>Script 13/Script 14</summary>

-  echo "Estructura until"
- cont=15
- until (test ${cont} -lt 0)
- do
-     cont=`expr $cont - 1`
-     echo "Valor del contador: ${cont}"
- done
- echo "Valor final del contador: ${cont}"

</details>

<details>
<summary>Script 14</summary>

- echo "Select de distros"
- DISTROS="Debian Ubuntu Navarrux Gentoo Mandriva"
- echo "Selecciona la mejor opción por favor: ${resultado}"
- select resultado in ${DISTROS}
- do
-     (test ${#resultado} -ne 0) && break
-     echo "Selecciona la mejor opción por favor: ${resultado}"
- done
- echo "Sistema seleccionado: [${resultado}] longitud de cadena: ${#resultado}"


</details>

<details>
<summary>Script 15</summary>

- RESULTADO=0
- muestrapantalla () {
-     echo "Valores: $0> $1 y $2 y $3"
- }
- sumame () {
-     echo "Estamos en la función: ${FUNCNAME}"
-     echo "Parametros: $1 y $2"
-     RESULTADO=`expr ${1} + ${2}`
-     return 0
- }
-     echo "No ejecutes esto... "
-     boom
- }
- muestrapantalla 3 4 "epa"
- sumame 45 67 && echo "OK" || echo "Ocurrió un error"
- echo "Resultado: ${RESULTADO} $!"


</details>

<details>
<summary>Script 16</summary>

- source libreria.sh
- muestrapantalla 69 123 "epa"
- sumame 1337 3389 && echo "OK" || echo "Ocurrió un error"
- echo "Resultado: ${RESULTADO} $!"

</details>

<details>
<summary>Script 17</summary>

- lCONTADOR=0
- funcion () {
-    CONTADOR=`expr $CONTADOR + 1`
-    echo "Se ha recibido una señal: ${FUNCNAME} ${0} (van ${CONTADOR})"
-    if [ ${CONTADOR} -ge 3 ]; then
-        echo "Tres señales recibidas. Salgo."
-        exit
-    fi
- }
- rap "funcion" INT QUIT TSTP
- echo "Mi PID es $$. Pulsa Ctrl-C tres veces para salir"
- echo "(o desde otra terminal: kill $$)"
- while true
- do
-    sleep 2
-    echo "ufff qué sueño..."
- done

</details>

<details>
<summary>Script 18</summary>

- echo -e "\033[40m\033[37m Blanco \033[0m"
- echo -e "\033[40m\033[1;37m Gris claro \033[0m"
- echo -e "\033[40m\033[30m Negro \033[0m (esto es negro)"
- echo -e "\033[40m\033[1;30m Gris \033[0m"
- echo -e "\033[40m\033[31m Rojo \033[0m"
- echo -e "\033[40m\033[1;31m Rojo claro \033[0m"
- echo -e "\033[40m\033[32m Verde \033[0m"
- echo -e "\033[40m\033[1;32m Verde claro \033[0m"
- echo -e "\033[40m\033[33m Marrón \033[0m"
- echo -e "\033[40m\033[1;33m Amarillo \033[0m"
- echo -e "\033[40m\033[34m Azul \033[0m"
- echo -e "\033[40m\033[1;34m Azul claro \033[0m"
- echo -e "\033[40m\033[35m Purpura \033[0m"
- echo -e "\033[40m\033[1;35m Rosa \033[0m"
- echo -e "\033[40m\033[36m Cyan \033[0m"
- echo -e "\033[40m\033[1;36m Cyan claro \033[0m"
- echo -e "\033[42m\033[31m Navarrux v1.0 \033[0m"
- echo -e "\033[40m\033[4;33m Amarillo \033[0m"

</details>
</details>