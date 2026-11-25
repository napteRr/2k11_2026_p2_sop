# Modelo de Parcial y Active Recall — SOP 2.º Parcial Práctico (UTN FRC)

> **Base oficial:** Construido a partir de las preguntas prácticas reales del **`Segundo Parcial 2k11` (`parcial p2 sop napter.pdf` / `parcial en html.txt`, Preguntas 17 a 29)** y ampliado con casos de **Active Recall** que cubren el 100 % del temario práctico 2026 (`tar`, `chmod`, procesos/prioridades, `cron`/`crontab`, pipes/filtros y shellscripts con `if`, `for`, `while`, `until`, parámetros posicionales y formas de ejecución).

---

## Parte A — Revisión Comentada del Parcial Real (`parcial p2 sop napter.pdf`, Preguntas 17 a 29)

### Pregunta 17 *(2.00 pts)*
**Enunciado:** *Al ejecutar `chmod u=rx lista`*
> **Respuesta Correcta:** **Se asignan los permisos de lectura y ejecución al dueño del archivo. El permiso de escritura para el dueño queda automáticamente denegado** *(y los permisos de grupo y otros quedan sin cambios — Modo Absoluto `=`)* *(2.00/2.00 en el intento)*.

---

### Pregunta 18 *(2.00 pts — Cazabobos)*
**Enunciado:** *Al ejecutar `chmod u+rx lista`*
- En el intento se marcó erróneamente *"y el permiso de escritura queda automáticamente denegado"* → **0.00 pts**.
> **Respuesta Correcta:** **Se asignan los permisos de lectura y ejecución al dueño. Todos los restantes permisos quedan sin cambios** *(Modo Simbólico `+` nunca deniega otros permisos)*.

---

### Pregunta 19 *(2.00 pts — Cazabobos)*
**Enunciado:** *Al ejecutar `chmod 654 archivo`*
- En el intento se marcó erróneamente *"Todos los restantes permisos quedan sin cambios"* → **0.00 pts**.
> **Respuesta Correcta:** **Se asignan sólo permisos de lectura y escritura al dueño (`6=110`), sólo permisos de lectura y ejecución al grupo (`5=101`), y sólo permiso de lectura a los otros (`4=100`).**
> **Por qué / Trampa:** El **Modo Numérico** sobrescribe las 3 ternas completas; los bits en `0` quedan **denegados**, nunca "sin cambios".

---

### Pregunta 20 *(2.00 pts)*
**Enunciado:**
```text
renice 5 712
Old priority 0 New priority 5
```
> **Respuesta Correcta:** **Decrementa en 5 unidades la prioridad de un proceso cuyo PID es 712** *(2.00/2.00; trampa común: confundir PID con número de tarea, o pensar que al subir el número sube la prioridad)*.

---

### Pregunta 21 *(2.00 pts)*
**Enunciado:** *Para detener temporalmente un proceso que está ejecutándose en modo foreground*
> **Respuesta Correcta:** **Presionamos `ctrl z`** *(2.00/2.00)*.

---

### Pregunta 22 *(2.00 pts)*
**Enunciado:** *El comando `renice`*
> **Respuesta Correcta:** **Puede ser empleado por cualquier usuario para decrementar la prioridad de sus procesos en ejecución** *(2.00/2.00; solo el superusuario puede incrementar la prioridad o tocar procesos ajenos)*.

---

### Pregunta 23 *(2.00 pts)*
**Enunciado:** *Para reanudar proceso modo foreground debo ejecutar:*
> **Respuesta Correcta:** **`fg número de tarea`** *(2.00/2.00; averiguado con `jobs`, NO el PID)*.

---

### Pregunta 24 *(2.00 pts)*
**Enunciado:** *Si aumenta el valor nice (`NI`) entonces:*
> **Respuesta Correcta:** **Se incrementa el valor `PRI` y disminuye la prioridad de un proceso** *(2.00/2.00; recordar $\text{PRI} = 80 + \text{NI}$)*.

---

### Pregunta 25 *(20.00 pts — Desarrollo Shellscript)*
**Enunciado:** *Diseñar un shellscript que le agregue a `n` archivos un listado de los procesos (el listado deberá incluir los valores asociados a la prioridad de un proceso), dicho procedimiento deberá repetirse mientras la cantidad de archivos ingresados sea menor a la cantidad total de archivos que debe ingresar el usuario.*

**Solución que obtuvo 20.00/20.00 en el parcial:**
```bash
echo "Cantidad de archivos: "
read cantot
caning=1
while [ $caning -le $cantot ]; do
    echo "Ingrese arg. de un archivo: "
    read archivo
    if [ -f "$archivo" ]; then
        ps -l >> $archivo && echo "$caning/$cantot $archivo enlistado con procesos"
        caning=`expr $caning + 1`
    else
        echo "No es un archivo"
    fi
done
```

> **Claves de Rúbrica de 10 (20/20 pts):**
> 1. Usar **`ps -l`** (o `ps -al`) porque es la opción que incluye **`PRI` y `NI`**.
> 2. Usar **`>>`** para **agregar** sin sobrescribir.
> 3. Respetar la estructura pedida: **`while`** (también válido iniciando `caning=0` y `while [ $caning -lt $cantot ]`, que refleja literalmente *"mientras sea menor"*).
> 4. Validar **`if [ -f "$archivo" ]`** antes de agregar.
> 5. Incrementar `caning=` `` `expr $caning + 1` `` **únicamente dentro del `then`** (si ingresa un directorio o archivo inexistente en el `else`, no cuenta como válido).

---

### Pregunta 26 *(2.00 pts)*
**Enunciado:** *Para visualizar un listado de todos los procesos que están o estuvieron detenidos temporariamente, todos los lunes y viernes durante septiembre, octubre y noviembre a las 9:15 hs, la periodicidad debe expresarse de la siguiente manera:*
> **Respuesta Correcta:** **`15 09 * 9-11 1,5 jobs`** *(2.00/2.00; formato `min hora día_mes mes día_semana comando`)*.

---

### Pregunta 27 *(2.00 pts)*
**Enunciado:** *El comando `renice`*
> **Respuesta Correcta:** **Permite modificar la prioridad de un proceso cuya ejecución ya se inició** *(2.00/2.00; a diferencia de `nice`, que lo lanza desde cero con prioridad distinta)*.

---

### Pregunta 28 *(2.00 pts — Cazabobos)*
**Enunciado:** *`crontab` es:*
- En el intento se marcó erróneamente *"un demonio que se comunica mediante el correo electrónico de Linux con los usuarios..."* → **0.00 pts**.
> **Respuesta Correcta:** **Una tabla que contiene información sobre las actividades programadas para que se ejecuten con cierta periodicidad.**
> **Por qué / Trampa:** El **demonio** que se ejecuta cada minuto y envía correo interno es **`cron`**; **`crontab`** es la **tabla**.

---

### Pregunta 29 *(2.00 pts — Orden invertido en `chmod`)*
**Enunciado:** *Para asignar en el archivo `report`: Sólo permiso lectura para los otros, sólo permisos de ejecución y lectura para el dueño, sólo permiso de escritura y lectura para el grupo (todos los restantes permisos deben quedar denegados) debe ejecutarse:*
> **Respuesta Correcta:** **`chmod 564 report`** *(2.00/2.00)*.
> **Por qué / Trampa:** El enunciado nombra primero *otros* (`r--`=4), luego *dueño* (`r-x`=5) y luego *grupo* (`rw-`=6), pero `chmod` numérico exige orden **`u g o`** → `564`.

---

## Parte B — Simulacro Complementario de Active Recall (Temario Práctico Completo 2026)

### Bloque 1 — Procesos, `ps`, `nice`/`renice`, Señales y `tar`

1. **[Caso Práctico — Background]** Se lanza `$ find / -name dire/grupo&` y el sistema devuelve `456`.
   - a) ¿Cómo suspende temporariamente su ejecución?
   - b) Si luego `jobs` muestra `1 stopped find`, ¿cómo lo reanuda en *background* y cómo lo reanudarías en *foreground*?
   - c) ¿Cómo lo elimina de manera forzosa sin posibilidad de que el proceso ignore la señal?
2. **[Cazabobos — `renice` y `PRI`/`NI`]** Un proceso suspendido muestra `PRI=80` y `NI=0` en `ps -l`. Luego ejecutamos `renice -3 456` y posteriormente `renice 4 456`.
   - a) ¿Qué salida `Old priority / New priority` devuelve cada comando, cuánto pasa a valer `PRI` en cada paso y si subió o bajó la prioridad?
   - b) ¿Puede un usuario común ejecutar ambos comandos? Justifique.
3. **[Cazabobos — `ps -e`]** ¿Qué significa que en la columna `TTY` del comando `ps -e` aparezca el carácter `?`?
4. **[Caso Práctico — `tar`]** Cree un archivo de respaldo `respaldo.tar` de los directorios `dire1`, `dire2` y `dire3`, liste su contenido y **extraiga las copias tomando las precauciones necesarias**.

<details>
<summary><strong>Rúbrica de 10 — Bloque 1</strong></summary>

1. **a)** Según la cátedra de SOP, para suspender un proceso en *background*: **`kill -2 456`** *(nota: en Linux estándar sería `kill -19 456`, en parcial de SOP responder `kill -2 PID`)*. **b)** En *background*: **`bg 1`**; en *foreground*: **`fg 1`** (usan el n.º de tarea `1` de `jobs`). **c)** Eliminar forzoso: **`kill -9 456`**.
2. **a)** Con `renice -3 456`: `Old priority: 0  New priority: -3` → $\text{PRI} = 80 - 3 = \mathbf{77}$ (**incrementó** la prioridad en 3 unidades). Con `renice 4 456`: `Old priority: -3  New priority: 4` → $\text{PRI} = 80 + 4 = \mathbf{84}$ (**disminuyó** la prioridad). **b)** **No**: el usuario común solo puede decrementar la prioridad de sus propios procesos; para `renice -3 456` (incrementar prioridad) es obligatorio ser **superusuario (`root`)**. El segundo sí podría hacerlo un usuario común sobre su propio proceso porque `NI` sube de `-3` a `4`.
3. Indica que **el proceso no fue lanzado desde alguna terminal** (demonio o proceso de sistema). *(No responder "no puede identificarse")*.
4. Crear: **`tar cvf respaldo.tar dire1 dire2 dire3`**. Listar: **`tar tvf respaldo.tar`**. Extraer con precaución (evitar sobreescritura en el directorio actual):
   ```bash
   mkdir destino
   cd destino
   tar xvf ../respaldo.tar
   ```

</details>

---

### Bloque 2 — `cron`/`crontab`, `at`, Pipes, Redireccionamiento y Filtros

5. **[Aplicación — `crontab` vs. `at`]** Escriba los comandos para resolver cada consigna:
   - a) Visualizar los usuarios conectados **hoy a las 17:12 por única vez**.
   - b) Guardar (agregando) un listado con información completa de todos los procesos activos (incluyendo procesos sin terminal) todos los **miércoles de septiembre a las 18:30 hs** en `~/procesos.log`.
   - c) Interpretar la línea de crontab: `45 18 */3 8,10,12 * ps -aef` y `10 12 * * 0 who`.
6. **[Pipes y Filtros — `head`, `tail`, `grep`, `tr`, `cut`]**
   - a) A partir de un archivo `comandos`, mostrar **únicamente las líneas 5 a 8** usando `head` y `tail`.
   - b) Contar cuántos **archivos regulares con permiso de lectura para el grupo** hay en el directorio actual usando `ls -l`, `grep` y `wc`.
   - c) Explicar por qué se intercala **`tr -s " "`** antes de **`cut -d' ' -f...`** al procesar salidas de `ls -l`, `ps -l` o `wc -l`.

<details>
<summary><strong>Rúbrica de 10 — Bloque 2</strong></summary>

5. **a)** Con `at` (única vez):
   ```bash
   $ at 17:12
   who
   # presionar Ctrl+D
   ```
   **b)** Con `vi archivo` escribir `30 18 * 9 3 ps -aef >> $HOME/procesos.log` (guardar con `Esc ZZ`) y cargar con `$ crontab archivo`.
   **c)** `45 18 */3 8,10,12 * ps -aef`: ejecuta `ps -aef` **cada 3 días durante agosto, octubre y diciembre a las 18:45 hs**. `10 12 * * 0 who`: lista usuarios conectados **todos los domingos a las 12:10 hs** (`0` y `7` = domingo).
6. **a)** **`head -8 comandos | tail -4`** (las líneas 5, 6, 7 y 8 son las 4 últimas de las 8 primeras: $8 - 5 + 1 = 4$).
   **b)** **`ls -l | grep '^-...r' | wc -l`** (carácter 1 es `-` archivo regular; caracteres 2-4 dueño cualquiera `...`; carácter 5 es `r` lectura para el grupo).
   **c)** Porque `ls -l`, `ps -l` o `wc` insertan **múltiples espacios consecutivos** para alinear columnas; `cut -d' '` cuenta cada espacio como un delimitador distinto. **`tr -s " "`** compacta los espacios consecutivos en un único espacio permitiendo que `-fN` seleccione la columna exacta.

</details>

---

### Bloque 3 — Shellscripts (`if`, `for`, `while`, `until`, `read`, `test`, `expr`, Parámetros Posicionales y Ejecución)

7. **[Diseño — `for` + `if` + `grep` + `>>`]** Diseñar un shellscript **sin parámetros posicionales** que solicite al usuario un directorio; verifique que es un directorio y, por cada **archivo regular** dentro de él, le **agregue** al final el listado de los comandos de `/bin` que comienzan con la letra `r`, mostrando luego el contenido actualizado de cada archivo.
8. **[Diseño — `until` + `expr` + `cut`]** Diseñar un shellscript usando **`until`** que permita contar las líneas de `n` archivos regulares ingresados por teclado (validando que cada archivo sea regular antes de incrementar el contador) e imprima `"El archivo X tiene Y líneas"` usando `wc -l`, `tr -s " "` / `cut`.
9. **[Parámetros Posicionales — Orden Invertido y Validación `$#`]**
   - a) Escriba un shellscript `progpar3` usando parámetros posicionales que concatene dos archivos en un archivo destino **de modo que al invocarlo el usuario pase primero el destino y luego el segundo archivo y luego el primero**:
     `$ sh progpar3 nuevo3 a2 a1` *(y el resultado en `nuevo3` sea `a1` seguido de `a2`)*.
   - b) Agregue el control con `$#` para avisar si la cantidad de parámetros es incorrecta y valide con `test` que ambos archivos de entrada sean regulares.
10. **[Ejecución y Variables]**
    - a) Mencione las **3 maneras** de ejecutar un shellscript vistas en clase e indique en cuáles hace falta permiso de ejecución (`+x`) y cuál corre en el *shell actual* en vez de un *subshell*.
    - b) Explique la diferencia entre las variables de entorno **`$PWD`** y **`$HOME`** luego de ejecutar `$ cd /tmp`.
    - c) Corrija los 3 errores de sintaxis en este fragmento:
      ```bash
      if [-f $arch]
      then
          n=`expr n+1`
          prod=`expr $a * $b`
      fi
      ```

<details>
<summary><strong>Rúbrica de 10 — Bloque 3</strong></summary>

7. **Solución esperada:**
   ```bash
   echo -n "Ingrese directorio: "
   read vardir
   if [ -d "$vardir" ]
   then
       for i in `ls "$vardir"`
       do
           if [ -f "$vardir/$i" ]
           then
               ls /bin | grep '^r' >> "$vardir/$i" && echo "El nuevo contenido de $i es:"
               more "$vardir/$i"
           fi
       done
   else
       echo "$vardir no es un directorio"
   fi
   ```
   *(Clave: usar `$vardir/$i`, no solo `$i`)*.

8. **Solución esperada con `until`:**
   ```bash
   echo -n "Ingrese la cantidad de archivos: "
   read cantot
   caning=0
   until [ $caning -eq $cantot ]
   do
       echo -n "Ingrese el argumento del archivo: "
       read var
       if [ -f "$var" ]
       then
           caning=`expr $caning + 1`
           can=`wc -l "$var" | tr -s " " | cut -d' ' -f1`
           echo "El archivo $var tiene $can líneas"
       else
           echo "Argumento no válido"
       fi
   done
   ```

9. **Solución `progpar3`:**
   ```bash
   if [ $# -eq 3 ]
   then
       if [ -f "$3" ] && [ -f "$2" ]
       then
           cat "$3" "$2" > "$1"
           echo "El contenido del nuevo archivo es:"
           more "$1"
       else
           echo "Alguno de los archivos de entrada no es regular"
       fi
   else
       echo "La cantidad de parámetros ingresada no es la correcta"
   fi
   ```
   *(Nota: al invocar `sh progpar3 nuevo3 a2 a1`, `$0=progpar3`, `$1=nuevo3`, `$2=a2`, `$3=a1`; `cat $3 $2 > $1` concatena `a1` y `a2` dentro de `nuevo3`)*.

10. **a)** Las 3 formas:
    1. `$ sh script` → no requiere `+x`, corre en subshell.
    2. `$ . script` → no requiere `+x`, corre en el **shell actual** (sin subshell).
    3. `$ chmod +x script` seguido de `$ ./script` → **sí requiere `+x`**, corre en subshell.
    **b)** Tras `cd /tmp`, **`$PWD`** pasa a valer `/tmp` (directorio de trabajo actual), mientras que **`$HOME`** mantiene fija la ruta del directorio de conexión del usuario (ej. `/home/alumno`).
    **c)** Correcciones:
    1. `if [ -f "$arch" ]` (faltaban espacios obligatorios después de `[` y antes de `]`).
    2. ``n=`expr $n + 1` `` (faltaba `$` en `$n` y espacios alrededor del `+`).
    3. ``prod=`expr $a \* $b` `` (faltaba escapar `*` como `\*`).

</details>
