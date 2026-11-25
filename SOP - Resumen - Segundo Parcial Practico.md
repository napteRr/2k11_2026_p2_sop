# Resumen — SOP: Segundo Parcial Práctico (UTN FRC)

> **Ficha Panorámica:** Síntesis integral de todos los temas de la **parte práctica del 2.º Parcial de Sistemas Operativos** (cátedra Ing. Clarisa Stefanich), elaborada a partir de las guías, diapositivas, ejercitación de shellscripts y modelos reales de parcial en UV.

---

## Nivel 1: Visión General y Mapa de Ejes

El Segundo Parcial Práctico evalúa el dominio de la administración de procesos y automatización en Linux desde la línea de comandos. Se organiza en **6 ejes operativos**:

1. **Gestión de procesos y prioridades:** inspección con `ps`, modos interactivos (*foreground* / *background*), suspensión, reanudación (`jobs`, `fg`, `bg`), terminación (`kill`) y ajuste de prioridades (`PRI`, `NI`, `nice`, `renice`).
2. **Permisos y copias de seguridad:** control de acceso con `chmod` (numérico, simbólico y absoluto) y empaquetado/extracción segura con `tar`.
3. **Programación de tareas:** planificación periódica con `cron` / `crontab` (y puntual con `at`).
4. **Pipes, redireccionamiento y filtros:** encadenamiento (`|`), escritura (`>`, `>>`) y extracción/filtrado con `head`, `tail`, `grep`, `tr` y `cut`.
5. **Variables y comandos de sesión:** asignación directa y por sustitución de comandos, variables `PWD` y `HOME`, y comandos `who`, `pwd`, `echo`.
6. **Programación de Shellscripts:** entrada (`read`), evaluación (`test` / `[ ]`), aritmética (`expr`), estructuras de control (`if`, `for`, `while`, `until`), parámetros posicionales (`$0`, `$1..$n`, `$#`) y las distintas formas de ejecución.

---

## Nivel 2: Desarrollo Analítico por Ejes Temáticos

### Eje 1: Procesos, Modos de Ejecución, Señales y Prioridades

#### 1.1 Modos de ejecución interactivos
| Modo | Cómo se lanza | Comportamiento del prompt y la terminal |
|---|---|---|
| **Foreground** | `$ comando` | El sistema devuelve el prompt **solo cuando termina** el comando. El usuario debe esperar a que finalice para seguir trabajando. |
| **Background** | `$ comando &` | El sistema devuelve el prompt **inmediatamente** y muestra el **PID**. El proceso corre en segundo plano en paralelo; al terminar, su salida interrumpe la pantalla salvo que se redireccione (`$ comando > salida &` *(en diapositivas de cátedra figura `$ comando& > salida`)*). |

#### 1.2 Comando `ps` (estado e información de procesos)
| Comando | Qué lista | Campos / Detalle clave |
|---|---|---|
| `$ ps` | Procesos lanzados desde la **terminal actual** del usuario. | `PID` (ID del proceso), `TTY` (terminal), `TIME` (tiempo de CPU usado), `CMD` (comando). |
| `$ ps -a` | Procesos lanzados desde **todas las terminales**. | Amplía el alcance a las demás sesiones de terminal. |
| `$ ps -e` | Agrega procesos que **no fueron lanzados desde alguna terminal** (demonios / sistema). | En la columna `TTY` aparece **`?`**, que significa exactamente que **el proceso no fue lanzado desde alguna terminal**. |
| `$ ps -f` | Agrega campos con **información adicional** (formato completo; combinable como `ps -af` o `ps -aef`). | Suma `UID` (usuario), `PPID` (PID del proceso padre), `C` (uso del procesador) y `STIME` (hora/fecha de inicio). |
| `$ ps -l` | Formato largo (combinable como `ps -al`). | Permite visualizar los valores de prioridad **`PRI`** y **`NI`**, además de `F`, `S`, `UID`, `PID`, `PPID`, `C`, `ADDR`, `SZ`, `WCHAN`, `TTY`, `TIME`, `CMD`. |
| `$ ps -aux` | Listado completo estilo BSD con información adicional de todos los procesos en ejecución. | Usado en la guía junto con paginación: `$ ps -aux \| more -4 +2`. |

#### 1.3 Suspender, Reanudar y Eliminar procesos (`jobs`, `fg`, `bg`, `kill`)

| Acción | En **Foreground** | En **Background** | Observaciones de cátedra |
|---|---|---|---|
| **Suspender** *(detener temporariamente)* | **`Ctrl + Z`** | **`$ kill -2 PID`** | El PID se ve al lanzar con `&` o ejecutando `ps`. |
| **Reanudar en Foreground** | **`$ fg [n°_tarea]`** | **`$ fg [n°_tarea]`** | Usa **n.º de tarea** (NO PID), obtenido con `jobs`. Si hay una sola tarea, el número puede omitirse. |
| **Reanudar en Background** | **`$ bg [n°_tarea]`** | **`$ bg [n°_tarea]`** | Reanuda en segundo plano; en `jobs` su estado pasa de `stopped` a `running`. |
| **Eliminar** *(terminar / matar)* | **`Ctrl + C`** | **`$ kill -9 PID`** | `kill PID` (sin `-9`) puede fallar; **`kill -9 PID`** fuerza la terminación y ningún proceso puede ignorarla. |

> **Nota técnica importante (`kill -2` vs Linux real):**
> En las guías, diapositivas y parciales de la cátedra, se enseña y evalúa **`kill -2 PID`** para **suspender/detener temporariamente** un proceso en *background*. En un sistema Linux estándar, la señal `-2` (`SIGINT`) interrumpe/finaliza el proceso (equivalente a `Ctrl+C`), mientras que `-19` (`SIGSTOP`) / `-20` (`SIGTSTP`) lo suspenden. **Para responder en el parcial de SOP**, usar **`kill -2 PID`** según lo establecido por la cátedra.

**Comando `$ jobs`:**
- Lista los procesos que están detenidos temporariamente en ese momento y los que estuvieron detenidos temporariamente antes.
- Muestra 3 columnas: **`n° de tarea`**, **`estado`** (`stopped`/`Detenido` o `running`/`Ejecutando`) y **`nombre del comando`**.
- **Semejanza y diferencia `fg` vs `bg`:** ambos reanudan un proceso suspendido; `fg` lo reanuda en modo *foreground* y `bg` en modo *background*.

#### 1.4 Valores `PRI` y `NI`, y comandos `nice` y `renice`

- **Valores por defecto:** **`NI = 0`** y **`PRI = 80`**.
- **Qué es cada uno:**
  - **`PRI`** representa la prioridad efectiva del proceso en el planificador. **Varía en sentido inverso a la prioridad**: a menor `PRI`, **mayor** prioridad (más tiempo de procesador); a mayor `PRI`, **menor** prioridad.
  - **`NI`** (*nice*) es el valor que modifica directamente el usuario para alterar la prioridad. Al cambiar `NI`, se recalcula automáticamente `PRI`:
    $$\text{PRI} = 80 + \text{NI}$$
    *(Ej.: `NI = 2` → `PRI = 82`, disminuyó la prioridad; `NI = -3` → `PRI = 77`, aumentó la prioridad).*

| Comando | Cuándo se usa | Sintaxis | Qué muestra / efecto |
|---|---|---|---|
| **`nice`** | Para **lanzar** un proceso nuevo con una prioridad distinta a la por defecto. | `$ nice NI comando` *(en Linux GNU también `nice -n NI comando`)* | Inicia el comando con el `NI` indicado. |
| **`renice`** | Para **cambiar** la prioridad de un proceso que **ya inició su ejecución**. | `$ renice NI PID` | Recibe el **PID** (no el n.º de tarea). Devuelve `Old priority X New priority Y` (muestra los valores de **`NI`**). |

**Permisos sobre prioridades (regla fija de parcial):**
- **Usuario común:** **solo puede decrementar (disminuir)** la prioridad de **sus propios procesos** (es decir, **aumentar** `NI`, por ejemplo de `0` a `3`). No puede incrementarla ni tocar procesos ajenos.
- **Superusuario (`root`):** puede **incrementar y decrementar** la prioridad de **cualquier proceso**.
- *Ejemplo clásico:* `renice 4 456` con salida `Old priority 10  New priority 4` → `NI` bajó de `10` a `4`, por lo tanto **se incrementó la prioridad** del proceso `456`, lo cual **solo pudo ejecutarlo el superusuario**.

---

### Eje 2: Permisos (`chmod`) y Empaquetado (`tar`)

#### 2.1 Comando `chmod` (3 modos de operación)
Modifica los permisos de acceso (`r` lectura, `w` escritura, `x` ejecución) sobre un archivo o directorio. Solo pueden ejecutarlo el **dueño** del archivo o el **superusuario**.

| Modo | Sintaxis | Cómo funciona | Qué pasa con los permisos no nombrados |
|---|---|---|---|
| **Numérico** | `$ chmod XYZ archivo` | Octal de 3 cifras en orden **Dueño (`u`) - Grupo (`g`) - Otros (`o`)**. Cada permiso vale `r=4`, `w=2`, `x=1` (`1` concedido, `0` denegado). | Asigna **solo** los permisos indicados en cada categoría; **los restantes quedan denegados** en las 3 categorías. |
| **Simbólico** | `$ chmod [u\|g\|o][+\|-][r\|w\|x],... arch` | `+` agrega permisos, `-` quita permisos al nivel indicado (`u` dueño, `g` grupo, `o` otros). | **Todos los restantes permisos quedan sin cambios** (tanto en ese nivel como en los demás). |
| **Absoluto** | `$ chmod [u\|g\|o]=[r\|w\|x],... arch` | El signo `=` fija **exactamente** esos permisos para el nivel especificado. | En el/los niveles indicados **los restantes permisos quedan denegados automáticamente**. En los niveles **no mencionados** quedan **sin cambios**. |

**Ejemplos paso a paso:**
- **Numérico:** Dueño todo (`rwx` = `111` = `7`), grupo lectura y ejecución (`r-x` = `101` = `5`), otros lectura y escritura (`rw-` = `110` = `6`) → `$ chmod 756 d1/a1`.
- **Simbólico:** Agregar `rw` a otros y `x` a dueño y grupo sin cambiar nada más → `$ chmod o+rw,ug+x ejemplo3`.
- **Absoluto:** Asignar *solo* `rw` a otros y *solo* `x` al dueño (el grupo no se toca) → `$ chmod o=rw,u=x ejemplo3` (en `o` se niega `x`, en `u` se niegan `rw`, `g` queda igual).
- *(Adicional visto en clase)*: `$ chmod +x prog1` (agrega permiso de ejecución) y `$ chmod +s archivo` (activa bit SUID).

#### 2.2 Comando `tar` (Empaquetado y Respaldos)
Empaqueta archivos y directorios en un único archivo de respaldo con extensión `.tar`.

| Operación | Sintaxis | Significado de opciones |
|---|---|---|
| **Crear** respaldo | `$ tar cvf respaldo.tar dire1 dire2 dire3` | `c`: crea el archivo de respaldo y empaqueta las copias.<br>`v`: *(opcional)* muestra por pantalla los archivos procesados.<br>`f`: *(obligatoria)* indica que el **primer argumento** es el archivo de respaldo. |
| **Visualizar** contenido | `$ tar tvf respaldo.tar` | `t`: lista el contenido del `.tar` con un formato similar a `ls -l`. |
| **Extraer** respaldo | `$ tar xvf ../respaldo.tar` | `x`: extrae las copias de seguridad contenidas en el archivo. |

> **Precaución obligatoria al extraer (`tar xvf`):**
> Al extraer un `.tar`, los archivos se vuelcan al **directorio actual sobreescribiendo** archivos existentes con igual nombre. Siempre deben ejecutarse estos **3 pasos**:
> ```bash
> $ mkdir destino
> $ cd destino
> $ tar xvf ../respaldo.tar
> ```

---

### Eje 3: Programación de Tareas (`cron`, `crontab` y `at`)

#### 3.1 Diferencia entre `cron`, `crontab` y `at`
- **`cron`:** programa **demonio** que se ejecuta en segundo plano desde el arranque del sistema (no controlado por el usuario). Se activa **cada minuto**, verifica si hay tareas programadas para ese instante y, de haberlas, las ejecuta y envía la salida por **correo electrónico interno de Linux** al usuario que las encargó.
- **`crontab`:** es la **tabla** (y el comando para cargarla/administrarla) que almacena las tareas programadas periódicamente.
- **`at`:** permite programar una tarea para que se ejecute **por única vez** (sin periodicidad).
  ```bash
  $ at 17:12 <Enter>
  who <Enter>
  <Ctrl + D>
  ```

#### 3.2 Procedimiento y estructura de `crontab`
1. Crear o editar un archivo con la definición de la tarea: `$ vi archivo` (guardar en `vi` con `Esc` + `ZZ`).
2. Cargar el archivo en la tabla crontab: `$ crontab archivo` *(para verla: `crontab -l`; para borrarla: `crontab -r`)*.

**Formato de cada línea (5 campos temporales + comando):**
```text
min   hora   día_del_mes   mes   día_de_la_semana   comando
0-59  0-23      1-31       1-12     0-6 o 1-7
```
*(El domingo puede representarse como **`0`** o **`7`**; lunes=`1`, martes=`2`, miércoles=`3`, jueves=`4`, viernes=`5`, sábado=`6`).*

**Operadores admitidos en cada campo:**
- `*`: todos los valores posibles (cada minuto, cada hora, todos los días, etc.).
- `a,b`: lista de valores puntuales (ej. `8,10,12` = agosto, octubre y diciembre; `1,5` = lunes y viernes).
- `a-b`: rango continuo (ej. `1-5` = del día 1 al 5; `2-4` = de martes a jueves).
- `*/n`: intervalo cada `n` unidades (ej. `*/30` en minutos = cada 30 minutos; `*/3` en día del mes = cada 3 días).
- `inicio/n`: cada `n` unidades a partir de `inicio` (ej. en guía de cátedra: `* 17/2 * 12 2-4 who` = de martes a jueves cada 2 horas desde las 17 hs en diciembre; en cron estándar `0 17-23/2`).

**Ejemplos de parcial:**
- Todos los viernes a las 13:10 ver usuarios conectados → `10 13 * * 5 who`
- Todos los lunes de noviembre a las 13:40 → `40 13 * 11 1 who`
- Cinco primeros días de cada mes a las 8:40 listar procesos → `40 8 1-5 * * ps -aef`
- Cada 3 días en agosto, octubre y diciembre a las 18:45 → `45 18 */3 8,10,12 * ps -aef`
- Lunes y viernes de septiembre, octubre y noviembre a las 9:15 listar trabajos detenidos → `15 09 * 9-11 1,5 jobs`

---

### Eje 4: Pipes, Redireccionamiento y Filtros de Texto

#### 4.1 Pipes (`|`) y Redireccionamiento (`>`, `>>`)
- **Pipe o tubería (`comando1 | comando2`):** envía la salida estándar de `comando1` como **entrada** de `comando2` en lugar de mostrarla por pantalla. Permite aplicar filtros (`grep`, `head`, `tail`, `cut`, `tr`, `wc`, `more`) sobre salidas de otros comandos (ej. `ls /bin | grep '^l'`).
- **Redireccionamiento de salida:**
  - **`>`** (`comando > ruta/archivo`): si el archivo no existe, **lo crea**; si existe, **lo sobreescribe sin pedir confirmación** (también `$ cat > archivo` + texto + `Ctrl+D`).
  - **`>>`** (`comando >> ruta/archivo`): si el archivo no existe, **lo crea**; si existe, **agrega** la salida al final del contenido existente sin sobreescribirlo.

#### 4.2 Filtros: `head`, `tail`, `grep`, `tr`, `cut`

| Comando | Función | Sintaxis y variantes de cátedra |
|---|---|---|
| **`head`** | Muestra las **primeras `n` líneas** de un archivo o entrada. | `$ head -n archivo` *(ej. `$ ls /dev \| head -4`)* |
| **`tail`** | Muestra las **últimas `n` líneas** (o desde la línea `n` con `+n`). | `$ tail -n archivo` / `$ tail +n archivo` |
| **Rango `head` + `tail`** | Extrae desde la línea `a` hasta la `b`. | `$ head -b archivo \| tail -(b - a + 1)`<br>*(ej. líneas 3 y 4: `$ head -4 comando10 \| tail -2`; líneas 7 a 9: `$ head -9 comando10 \| tail -3`)* |
| **`grep`** | Filtra líneas que coinciden con un patrón. | `$ grep "texto" archivo`: contiene `texto`.<br>`$ grep '^patron'`: **comienza** con `patron` (`$` = fin de línea).<br>`.` (punto) = **1 carácter cualquiera** (`^...r` = 4.º carácter es `r`; `^-.w.r` sobre `ls -l` = archivo regular con `w` dueño y `r` grupo; `^dr.x` = directorio con `rx` para grupo).<br>`$ grep -v patron archivo`: excluye las líneas que coinciden con `patron`. |
| **`tr`** | Traduce, comprime o elimina caracteres de la entrada estándar (vía pipe). | `$ ... \| tr -s " "`: **compacta múltiples espacios consecutivos en uno solo** (esencial antes de `cut -d' '` en salidas con columnas alineadas como `ls -l`, `ps -l`, `wc`, `free`).<br>Otros: `tr 'a-z' 'A-Z'` (reemplaza), `tr -d 'x'` (borra). |
| **`cut`** | Extrae columnas/campos específicos según un delimitador. | `$ cut -d'delimitador' -fn°_campos [archivo]`<br>*(ej. `$ cut -d'-' -f4 inversiones2`, `$ cut -d' ' -f1,3 notas`, `$ wc -l $di/$i \| cut -d' ' -f1`)* |

---

### Eje 5: Variables y Comandos Básicos de Sesión

#### 5.1 Creación y uso de variables
- **Asignación directa:** `nombre=valor` (**¡sin espacios alrededor del `=`!**). Ej.: `color=blanco`, `caning=0`, `b=/home/clarisa/d1/d2/d3`. Si el texto tiene espacios, usar comillas: `msg="hola mundo"`.
- **Sustitución de comandos:** asigna la salida estándar de un comando encerrándolo entre acentos graves (`` `comando` ``) o `$(comando)`:
  ```bash
  camino=`pwd`
  can=`wc -w $var10`
  resum=`expr $num1 + $num2`
  ```
- **Por teclado (`read`):** `read var` (al asignar no lleva `$`).
- **Uso / consulta:** anteponer **`$`** al nombre (`$color`, `$camino`).

#### 5.2 Variables de entorno `PWD` y `HOME`
| Variable | Qué almacena | Comportamiento durante la sesión |
|---|---|---|
| **`PWD`** | Ruta absoluta del **directorio actual** (*directorio de trabajo* / *directorio activo*). | **Cambia** cada vez que el usuario ejecuta `cd`. |
| **`HOME`** | Ruta absoluta del **directorio de conexión** (*homedirectory* / *directorio de login*, por defecto `/home/usuario`). | **Se mantiene fija** como referencia al directorio inicial del usuario (equivale a `~`). |

#### 5.3 Comandos `who`, `pwd`, `echo`
- **`who`**: lista los **usuarios conectados** actualmente al sistema, su terminal (`TTY`) y la fecha/hora de inicio de sesión. *(Nota: `whoami` devuelve solo el nombre del usuario de la sesión actual).*
- **`pwd`** (*Print Working Directory*): muestra la ruta absoluta del directorio actual (equivalente a `echo $PWD`).
- **`echo`**: imprime texto o valores de variables por salida estándar. Con la opción **`-n`** (`echo -n "Ingrese: "`) **no realiza salto de línea al final**, dejando el cursor en la misma línea para leer con `read`.

---

### Eje 6: Programación de Shellscripts

#### 6.1 Comandos internos clave: `read`, `test`, `expr`

1. **`read variable`:** detiene la ejecución esperando entrada por teclado hasta `Enter` y guarda lo ingresado en `variable`. También se usa como pausa (`read a`).
2. **`test condicion` o `[ condicion ]`:** evalúa una condición y devuelve `$? = 0` si es verdadera o distinto de `0` si es falsa. **Con `[ ... ]`, los espacios después de `[` y antes de `]` son obligatorios.**
   - **Archivos:** `-f ruta` (existe y es **archivo regular/ordinario**), `-d ruta` (existe y es **directorio**), `-e ruta` (existe), `-r` / `-w` / `-x` (permisos).
   - **Números enteros:** `-eq` (`=`), `-ne` (`!=`), `-gt` (`>`), `-ge` (`>=`), `-lt` (`<`), `-le` (`<=`). *(No usar `>` ni `<` para números porque el shell los interpreta como redirección).*
   - **Cadenas:** `[ "$a" = "$b" ]` (iguales), `[ "$a" != "$b" ]` (distintas), `-z "$a"` (vacía), `-n "$a"` (no vacía).
3. **`expr`:** realiza operaciones aritméticas con enteros.
   - **Reglas obligatorias:** espacios alrededor de cada operador, capturar con backticks `` `expr ...` `` y **escapar el `*` como `\*`** para multiplicación.
   - Ejemplos: ``res=`expr $a + $b` ``, ``res=`expr $a - $b` ``, ``res=`expr $a \* $b` ``, ``res=`expr $a / $b` ``, ``caning=`expr $caning + 1` ``.

#### 6.2 Estructuras de control (`if`, `for`, `while`, `until`)

| Estructura | Lógica de ejecución | Cierre |
|---|---|---|
| **`if`** | Ejecuta un bloque si la condición es **verdadera** (y opcionalmente `else` si es falsa). | `fi` |
| **`for`** | Recorre uno por uno los elementos de una lista (habitualmente generada con `` `ls $dir` `` o `` `cut ...` ``). | `done` |
| **`while`** | Repite el bloque **mientras** la condición sea **VERDADERA** (incluye `while true` para menús). | `done` |
| **`until`** | Repite el bloque **hasta que** la condición sea **VERDADERA** (es decir, **mientras sea FALSA**). | `done` |

```bash
# 1) Estructura if ... then ... else ... fi
if [ -f "$var" ]
then
    echo "Es archivo regular"
else
    echo "No es archivo regular"
fi

# 2) Estructura for (¡atención al $dir/$i dentro del bucle!)
for i in `ls $dir`
do
    if [ -f "$dir/$i" ]
    then
        more "$dir/$i"
    fi
done

# 3) Estructura while (repite mientras sea verdadero)
caning=0
while [ $caning -lt $cantot ]
do
    caning=`expr $caning + 1`
done

# 4) Estructura until (repite mientras sea falso / hasta que se cumpla)
caning=0
until [ $caning -eq $cantot ]
do
    caning=`expr $caning + 1`
done
```

> **Atención en `for` sobre `ls $dir`:** la variable `i` recibe solo el **nombre del archivo**, no su ruta. Para operar sobre él o testearlo con `-f`/`-d` hay que anteponer el directorio: **`$dir/$i`**.

**Ejemplo completo integrador tipo parcial (Pregunta 25 real de UV):**
```bash
echo -n "Cantidad de archivos: "
read cantot
caning=0
while [ $caning -lt $cantot ]
do
    echo -n "Ingrese arg. de un archivo: "
    read archivo
    if [ -f "$archivo" ]
    then
        ps -l >> "$archivo" && echo "$archivo actualizado"
        caning=`expr $caning + 1`
    else
        echo "No es un archivo regular"
    fi
done
```

#### 6.3 Parámetros posicionales (`$0`, `$1..$n`, `$#`)
Permiten pasar los argumentos al shellscript directamente en la línea de comando al invocarlo (por lo tanto, **no llevan mensajes ni `read` pidiendo esos datos**):
- **`$0`**: nombre del shellscript ejecutado.
- **`$1`, `$2`, `$3`, ...**: 1.º, 2.º, 3.º, ... argumento ingresado después del nombre del script.
- **`$#`**: cantidad total de argumentos ingresados por el usuario (se usa para validar: `if [ $# -eq 3 ]`).

**Regla clave del orden de argumentos:** el orden en que el usuario escribe los parámetros al ejecutar depende de **en qué lugar del código** usa cada `$1`, `$2`, `$3`:
- Si `progpar` hace `cat $1 $2 > $3` → se ejecuta: `$ sh progpar dire/a1 dire/d1/a2 nuevo1`
- Si `progpar3` hace `cat $3 $2 > $1` → se ejecuta: `$ sh progpar3 nuevo3 dire/d1/a2 dire/a1` *(el destino va primero porque está en `$1`)*.

#### 6.4 Diferentes maneras de ejecutar un shellscript
En clase se enseñan **3 formas** de ejecución:

| Forma | Comando | ¿Requiere permiso `x`? | Cómo se ejecuta |
|---|---|---|---|
| **1. Invocando al intérprete (`sh`)** | `$ sh nombre_script [args]` | **No** | Lanza un **subshell** (proceso hijo) que lee y ejecuta el script. |
| **2. En el shell actual (`.`)** | `$ . nombre_script [args]` | **No** | Ejecuta el script **dentro del shell actual** (sin subshell; las variables o `cd` persisten en la sesión). *Atención al espacio después del punto.* |
| **3. Como ejecutable directo (`./`)** | `$ chmod +x nombre_script`<br>`$ ./nombre_script [args]` | **Sí** (`chmod +x`) | Requiere anteponer `./` porque el directorio actual normalmente no está en `$PATH`. |

---

## Nivel 3: Vocabulario de Cátedra

- **PID vs. N.º de tarea:** el **PID** identifica unívocamente al proceso en el sistema operativo (se ve con `ps` y lo usan `kill` y `renice`); el **n.º de tarea** se asigna al suspender un proceso en la sesión actual (se ve con `jobs` y lo usan `fg` y `bg`).
- **`cron` vs. `crontab`:** `cron` es el **demonio** que despierta cada minuto y ejecuta las tareas enviando la salida por correo interno; `crontab` es la **tabla** (y comando) donde se registran las tareas periódicas.
- **`while` vs. `until`:** `while` itera **mientras** la condición se cumpla; `until` itera **hasta que** la condición se cumpla.
- **Modo simbólico vs. Modo absoluto en `chmod`:** `+`/`-` toca **solo** los permisos mencionados y deja todo lo demás intacto; `=` fija los permisos del nivel nombrado y **niega automáticamente** los demás permisos de ese nivel, sin tocar los otros niveles.

---

## Nivel 4: Trampas Clásicas de Parcial ("Cazabobos") y Checklist

1. **Orden cambiado en `chmod` numérico:** los enunciados suelen pedir *"solo lectura para otros, ejecución y lectura para dueño, escritura y lectura para grupo"*. Siempre reordenar a **`u g o`** (`564`, no `456`).
2. **Confundir `chmod u=rx` con `chmod u+rx`:** con `=`, `w` del dueño queda denegado; con `+`, `w` del dueño queda sin cambios; con `chmod 654`, las 3 categorías quedan fijadas exactamente (no "sin cambios").
3. **Sentido inverso de `NI`/`PRI` y permisos de `renice`:** si `Old priority 9 New priority 6`, el número bajó pero **la prioridad AUMENTÓ** (y un usuario común **NO** puede hacerlo; solo `root`).
4. **Confundir `PID` con `n° de tarea`:** `renice` y `kill` usan **PID** (`ps`); `fg` y `bg` usan **n.º de tarea** (`jobs`).
5. **`TTY = ?` en `ps -e`:** la respuesta correcta de cátedra es *"el proceso no fue lanzado desde alguna terminal"* (no "no puede identificarse").
6. **`at` vs. `crontab`:** `at` es **por única vez**, nunca periódico. Y `crontab` es una **tabla**, no un demonio.
7. **Extraer un `.tar` directo en el directorio actual:** siempre recordar `mkdir destino` → `cd destino` → `tar xvf ../respaldo.tar`.
8. **Errores de sintaxis en `expr` y `[ ]`:**
   - Mal: ``narch=`expr narch-1` `` (falta `$` y faltan espacios) o `expr 5 * 3` (sin `\`).
   - Bien: ``narch=`expr $narch - 1` `` y ``prod=`expr 5 \* 3` ``.
   - Mal: `if [-f $var]` (sin espacios dentro de corchetes). Bien: `if [ -f "$var" ]`.
9. **Olvidar la ruta en `for i in \`ls $dir\``:** testear `[ -f $i ]` falla si no estás parado dentro de `$dir`; usar **`[ -f "$dir/$i" ]`**.
10. **Incrementar el contador fuera del `if` en `while`/`until`:** si el archivo ingresado no existe o es directorio (`else`), **no** se debe incrementar `caning`, así el ciclo exige `n` archivos válidos.
