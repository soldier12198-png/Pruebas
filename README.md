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

- ls -a
- find / -user bandit7 -group bandit6 -size 33c
- cat /var/lib/dpkg/info/bandit7.password

</details>

<details>
<summary>Script 7</summary>

- ls
- cat | data.txt grep "millionth"

</details>

<details>
<summary>Script 8</summary>

- ls
- sort data.txt | uniq -u

</details>

<details>
<summary>Script 9</summary>

- ls
- string data.txt | greep=

</details>

<details>
<summary>Script 10</summary>

- ls
- cat data.txt
- base64 -d data.txt

</details>

<details>
<summary>Script 11</summary>

- ls
- cat data.txt
- cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'

</details>

<details>
<summary>Script 12</summary>

- ls
- cat data.txt
- mkdir /tmp/fabot
- cp data.txt /tmp/fabot
- cd /tmp/fabot
- xxd -r
- xxd -r data.txt > data_original
- ls
- file data_original
- mv data_original data.gz
- ls
- gunzip data.gz
- ls
- file data
- bzip2 -d data
- ls
- file data.out
- mv data.out data.gz
- ls
- gunzip data.gz
- ls
- file data.out 
- tar -xvf data
- ls
- file data5.bin 
- tar -xvf data5.bin
- file data6.bin
- bzip2 -d data6.bin
- ls
- file data6.bin.out
- tar -xvf data6.bin.out
- file data8.bin
- mv data8.bin data8.gz
- gunzip data8.gz
- ls
- file data8
- cat data8

</details>

<details>
<summary>Script 13/Script 14</summary>

**LINUX TERMINAL** 

- ls
- cat sshkey.private
- exit
- echo "-----BEGIN OPENSSH PRIVATE KEY-----
(...)
-----END OPENSSH PRIVATE KEY-----" > private.key
- cat private.key 
- ls -la
- chmod 700 private.key
- ssh bandit14@bandit.labs.overthewire.org -p 2220 -i private.key
- cat /etc/bandit_pass/bandit14
- Así nos da la contraseña "aaWecNkG4FhxJQxz07uiwzVP6bJiYS65"

</details>

<details>
<summary>Script 14</summary>

**LINUX TERMINAL** 

- man nc
- nc localhost 30000
- aaWecNkG4FhxJQxz07uiwzVP6bJiYS65

</details>

<details>
<summary>Script 15</summary>

- man s_client
- openssl s_client -connect localhost:30001
- pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
- Así nos da la contraseña "akS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V"

</details>

<details>
<summary>Script 16</summary>

- nmap -p 31000-32000 localhost
- echo "kS0Hf0u5HiXFwKMKFqXvPdOTNGGa0X8V" | openssl s_client -connect localhost:31790 -quiet
- nano /tmp/private.key
- scp -P 2220 bandit16@bandit.labs.overthewire.org:/tmp/private.hey .
- chmod 600 private.key
- ssh -i private.key bandit17@bandit.labs.overthewire.org -p 2220

</details>

<details>
<summary>Script 17/18</summary>

- ls -l
- diff passwords.old passwords.new
- ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"

</details>

<details>
<summary>Script 19</summary>

- ls -l
- ./bandit20-do cat /etc/bandit_pass/bandit20

</details>

<details>
<summary>Script 20</summary>

- echo "0qXahG8ZjOVMN9Ghs7i0WsCfZyXOUbYO" | nc -lp 1234
- ./suconnect 1234

</details>

<details>
<summary>Script 21</summary>

- ls /etc/cron.d
- cat /etc/cron . d/cronjob_bandit22
- cat /usr/bin/cronjob_bandit22.sh
- cat /tmp/t706lds9S0RqQh9aMcz6ShpAoZKF7fgv

</details>
</details>


