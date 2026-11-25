# Modelo de Parcial y Active Recall — SOP 2.º Parcial Teórico (UTN FRC)

> **Base oficial:** Construido a partir del examen real **`Segundo Parcial 2k11` (`parcial p2 sop napter.pdf` / `parcial en html.txt`, Preguntas 1 a 16)** —con sus devoluciones reales de cátedra— y ampliado con preguntas de **Active Recall** sobre los temas vigentes del 2.º Parcial Teórico: **Unidad 3 (Partes 3, 4, 5 y 6)** y **Unidad 4 (Seguridad)**. *(No entran Parte 2, Memoria ni Docker).*

---

## Parte A — Revisión Comentada del Parcial Real (`parcial p2 sop napter.pdf`, Preguntas 1 a 16)

### Pregunta 1 *(2.00 pts — Multiple Choice)*
**Enunciado:** *¿CUÁL DE LAS SIGUIENTES ES UNA CARACTERÍSTICA DE LOS MENSAJES?*
- a. Alternancia estricta
- b. Exclusión mutua con espera ocupada *(marcada en el intento → 0.00 pts)*
- c. Sincronización
- d. Desactivación de interrupciones

> **Respuesta Correcta:** **`c. Sincronización`**
> **Por qué / Trampa:** La *alternancia estricta* y la *desactivación de interrupciones* son técnicas de exclusión mutua **CON espera ocupada**. Los **mensajes** son mecanismos **SIN espera ocupada**, y en la presentación de cátedra (*Unidad 3 Parte 5*) se destacan sus primitivas `Send` y `Receive` como **primitivas de sincronización** (además de la comunicación sincrónica).

---

### Pregunta 2 *(2.00 pts — Selección Múltiple)*
**Enunciado:** *¿Cuáles son mecanismos de comunicación entre procesos?*
- a. Paso de mensajes
- b. Pipes (Tuberías)
- c. Monitores
- d. Memoria compartida
- e. Sección crítica
- f. Interrupciones de reloj
- g. Semáforos
- h. Planificación por prioridades

> **Respuestas Correctas:** **`a` (Paso de mensajes), `b` (Pipes / Tuberías), `c` (Monitores), `d` (Memoria compartida) y `g` (Semáforos)** *(en el intento se marcaron erróneamente `e`, `f`, `h` y se omitieron `b`, `c`)*.
> **Por qué / Trampa:**
> - **Sección crítica (`e`)** NO es un mecanismo de comunicación; es la *porción de código* donde un proceso accede a datos compartidos.
> - **Interrupciones de reloj (`f`)** y **Planificación por prioridades (`h`)** pertenecen a **planificación de procesos** (Parte 6), no a comunicación (IPC).

---

### Pregunta 3 *(3.00 pts — Selección Múltiple)*
**Enunciado:** *En entorno multihilo, señale las verdaderas:*
- a. Cada hilo posee su propio espacio de direcciones separado
- b. Cada hilo debe ejecutarse en un procesador diferente obligatoriamente
- c. El planificador del kernel nunca ve a los hilos
- d. Un proceso puede tener uno o más hilos
- e. Residen en el mismo espacio de direcciones
- f. Tienen acceso a los mismos datos del proceso
- g. Si un hilo se bloquea, los demás nunca pueden ejecutarse
- h. Los hilos comparten el estado y los recursos del proceso

> **Respuestas Correctas:** **`d`, `e`, `f` y `h`**.
> **Por qué / Trampa:**
> - `a` es falsa: los hilos de un mismo proceso **comparten** el espacio de direcciones.
> - `b` es falsa: varios hilos pueden alternar en **un solo procesador** (concurrencia).
> - `c` y `g` son falsas como regla general del entorno multihilo porque describen limitaciones exclusivas de los **ULT**, no de los **KLT** (donde el kernel sí planifica cada hilo y si uno se bloquea puede ejecutar otro del mismo proceso).

---

### Pregunta 4 *(2.00 pts — Multiple Choice)*
**Enunciado:** *MULTIHILO SIGNIFICA CAPACIDAD DE UN S.O. PARA MANTENER:*
- a. Varios hilos de ejecución dentro de un mismo proceso
- b. Varios procesos dentro un mismo hilo de ejecución
- c. Un proceso de ejecución por hilo
- d. Varios procesos, donde cada uno de ellos tiene un único hilo de ejecución
- e. Un hilo de ejecución por proceso

> **Respuesta Correcta:** **`a. Varios hilos de ejecución dentro de un mismo proceso`** *(2.00/2.00 en el intento)*.

---

### Pregunta 5 *(2.00 pts — Cazabobos)*
**Enunciado:** *¿CUÁL DE ESTAS AFIRMACIONES DEL ENTORNO DE MULTIHILOS DE UN PROCESO NO ES CORRECTA?*
- a. Los elementos de los procesos tienen un espacio de direcciones virtuales que contiene la imagen del proceso *(marcada erróneamente → 0.00 pts)*
- b. Un proceso se define como una unidad de asignación de recursos
- c. Acceso a memoria y recursos del proceso, compartidos con el resto de los hilos de todos los procesos
- d. Acceso protegido a los procesadores, otros procesos, archivos y recursos de Entrada/Salida

> **Respuesta Correcta (la afirmación INCORRECTA):** **`c`**
> **Trampa cazabobos:** Los hilos comparten memoria y recursos únicamente con el resto de los hilos **del MISMO proceso**, **NO** de *todos los procesos* (los procesos distintos están aislados entre sí).

---

### Pregunta 6 *(2.00 pts — Multiple Choice)*
**Enunciado:** *¿Cuál de los siguientes NO es un estado principal de un hilo?*
- a. Listo
- b. Ejecución
- c. Terminado/Finalizado (como estado principal permanente)
- d. Bloqueado

> **Respuesta Correcta:** **`c. Terminado/Finalizado`** *(2.00/2.00 en el intento — los 3 estados principales de un hilo según Unidad 3 Parte 3 son Listo, Ejecución y Bloqueado)*.

---

### Pregunta 7 *(3.00 pts — Selección Múltiple)*
**Enunciado:** *Sobre \*clusters\*, indique las verdaderas:*
- a. Solo un nodo ejecuta el núcleo y el resto permanece inactivo
- b. No requieren red para comunicarse entre nodos
- c. Crean la ilusión de ser una única máquina
- d. Son nodos interconectados por una red de alta velocidad
- e. Comparten siempre la misma memoria principal física
- f. Proporcionan escalabilidad y alta disponibilidad

> **Respuestas Correctas:** **`c`, `d` y `f`** *(3.00/3.00 en el intento)*.
> **Detalle clave:** Los *clusters* tienen memoria **distribuida** (no comparten la misma memoria física, a diferencia de SMP) y se comunican por red de alta velocidad.

---

### Pregunta 8 *(2.00 pts — Multiple Choice)*
**Enunciado:** *¿Cuál es una característica clave de la arquitectura de micronúcleo?*
- a. Es incompatible con entornos distribuidos
- b. Todos los servicios del S.O. residen en el núcleo por rendimiento
- c. Solo las funciones esenciales permanecen en el núcleo; muchos servicios se ejecutan en modo usuario
- d. No utiliza paso de mensajes entre procesos/servicios

> **Respuesta Correcta:** **`c`** *(2.00/2.00 en el intento)*.

---

### Pregunta 9 *(3.00 pts — Selección Múltiple)*
**Enunciado:** *Seleccione los requisitos clásicos de seguridad:*
- a. Rendimiento máximo · b. Escalabilidad · c. Integridad · d. Disponibilidad · e. Redundancia · f. Confidencialidad

> **Respuestas Correctas:** **`c. Integridad`, `d. Disponibilidad`, `f. Confidencialidad`** *(3.00/3.00 en el intento; el 4.º requisito clásico del temario es Autenticidad)*.

---

### Pregunta 10 *(2.00 pts — Multiple Choice)*
**Enunciado:** *¿Cuál es la mejor descripción de la amenaza llamada "Interceptación"?*
- a. Una parte no autorizada consigue acceso al sistema o a sus datos
- b. Se destruye un componente del sistema, como un disco o canal de comunicación *(Interrupción)*
- c. Un elemento no autorizado modifica un componente del sistema *(Modificación)*
- d. Un elemento no autorizado inserta registros o mensajes falsos en el sistema *(Fabricación)*

> **Respuesta Correcta:** **`a`** *(2.00/2.00 en el intento)*.

---

### Pregunta 11 *(12.00 pts — Desarrollo / Rúbrica Estricta)*
**Enunciado:** *Explica detalladamente el funcionamiento y la función del "aderezo" (semilla/salt) al momento de cargar y verificar una contraseña.*

- **Errores sancionados en el intento real (4.00/12.00):**
  1. Hablar de *"encriptación"* en vez de **función hash criptográfica unidireccional**.
  2. Afirmar que *"se guarda la contraseña junto al aderezo"* (se guarda el **hash** junto al aderezo, nunca la contraseña).
  3. Omitir que la semilla es generada **aleatoriamente** y guardada en **texto plano**.
  4. Omitir la mecánica de **recálculo y comparación** al verificar, y el fin principal: **evitar hashes idénticos ante contraseñas duplicadas**.

<details>
<summary><strong>Respuesta Esperada — Rúbrica de 10 (12/12 pts)</strong></summary>

1. **Qué es el aderezo (*salt* / semilla):** es un valor generado de forma **aleatoria** por el sistema (12 bits en el esquema clásico con `crypt(3)`) que se almacena **en texto plano** junto al ID de usuario y al **hash** resultante en el archivo de contraseñas (nunca se almacena la contraseña en texto plano).
2. **Función principal del aderezo:**
   - **Evitar hashes idénticos ante contraseñas duplicadas:** si dos usuarios eligen la misma clave, al combinarse con semillas aleatorias distintas generan **hashes diferentes**, ocultando contraseñas repetidas.
   - **Incrementar la longitud efectiva** de entrada a la función hash para fortalecer la clave.
   - **Dificultar ataques por fuerza bruta con hardware dedicado (DES) y tablas precalculadas**, obligando al atacante a recalcular el hash por cada semilla distinta.
3. **Carga de una nueva contraseña:** el usuario ingresa su contraseña; el sistema genera un aderezo aleatorio, combina `(aderezo + contraseña)` y los procesa mediante una **función hash criptográfica unidireccional** (`crypt(3)`), guardando `(ID_usuario, aderezo_en_texto_plano, hash_generado)` en el archivo de contraseñas.
4. **Verificación durante el login:**
   1. Con el ID de usuario se ubica su registro en el archivo de contraseñas.
   2. Se extrae el **aderezo almacenado en texto plano**.
   3. Se combina con la **contraseña ingresada** por el usuario.
   4. Se **recalcula el hash** aplicando la misma función hash unidireccional.
   5. Se **compara** el hash recalculado contra el hash guardado en el archivo: si coinciden, el usuario autentica; si difieren, se rechaza el acceso.

</details>

---

### Pregunta 12 *(12.00 pts — Desarrollo / Rúbrica Estricta)*
**Enunciado:** *¿Qué principio se debe implementar para evitar las condiciones de competencia y cuáles son las cuatro condiciones necesarias que deben cumplirse para lograr una solución adecuada?*

- **Errores sancionados en el intento real (2.00/12.00):**
  1. Confundir exclusión mutua con "prohibir compartir memoria" (la exclusión mutua prohíbe que dos procesos estén **simultáneamente dentro de su sección crítica**).
  2. Omitir la condición de **progreso** (ningún proceso *fuera* de su SC puede bloquear a otros).
  3. Distorsionar **espera limitada** (es no esperar indefinidamente *para ingresar*, no "quedar atascado dentro").
  4. Inventar "esperar una E/S" en lugar de la regla de **independencia del hardware** (no hacer suposiciones sobre velocidades relativas ni cantidad de CPUs).

<details>
<summary><strong>Respuesta Esperada — Rúbrica de 10 (12/12 pts)</strong></summary>

Para evitar las **condiciones de competencia** (donde el resultado final depende del orden de ejecución al leer/escribir memoria compartida), se debe implementar el principio de **Exclusión Mutua**, garantizando que solo un proceso acceda a la **sección crítica** a la vez y cumpliendo las **4 condiciones necesarias**:
1. **Exclusión mutua:** no puede haber dos procesos **simultáneamente dentro de sus secciones críticas**.
2. **Progreso:** ningún proceso que se ejecute **fuera de su sección crítica** puede bloquear a otros procesos.
3. **Espera limitada:** ningún proceso debe **esperar indefinidamente (eternamente) para ingresar** a su sección crítica (evita la inanición).
4. **Independencia del hardware:** no deben hacerse suposiciones sobre las **velocidades relativas** de los procesos ni sobre la **cantidad de procesadores (CPUs)** del sistema.

</details>

---

### Pregunta 13 *(2.00 pts — Multiple Choice)*
**Enunciado:** *¿CUÁL ES EL PLANIFICADOR QUE LE ASIGNA UN QUANTUM DE TIEMPO A CADA PROCESO PARA UTILIZAR LA CPU?*
> **Respuesta Correcta:** **`a. De ronda circular o Round Robin`** *(2.00/2.00 en el intento)*.

---

### Pregunta 14 *(2.00 pts — Cálculo de Colas Múltiples)*
**Enunciado:** *UN PROCESO REQUIERE 40 QUANTUMS PARA SU EJECUCIÓN. SI SE ESTÁ IMPLEMENTANDO EL ALGORITMO DE PLANIFICACIÓN DE PROCESOS DE COLAS MÚLTIPLES. DICHO PROCESO PASARÁ DEL ESTADO DE LISTO A EJECUCIÓN:*
- a. 1 vez · b. 12 veces · c. 5 veces · d. 40 veces *(marcada erróneamente → 0.00 pts)* · e. 6 veces · f. 10 veces

> **Respuesta Correcta:** **`e. 6 veces`**
> **Cálculo exacto:** En Colas Múltiples, cada descenso duplica los quantums asignados ($1, 2, 4, 8, 16, 32 \dots$):
> - 1.ª pasada (cola 1): $1$ quantum (acum. $1$)
> - 2.ª pasada (cola 2): $2$ quantums (acum. $3$)
> - 3.ª pasada (cola 3): $4$ quantums (acum. $7$)
> - 4.ª pasada (cola 4): $8$ quantums (acum. $15$)
> - 5.ª pasada (cola 5): $16$ quantums (acum. $31$)
> - 6.ª pasada (cola 6): hasta $32$ quantums (solo usa $9$ para llegar a $40$) → **6 veces pasa de Listo a Ejecución**.

---

### Pregunta 15 *(2.00 pts — Multiple Choice)*
**Enunciado:** *¿Qué criterio está orientado principalmente a procesos interactivos?*
- a. Minimizar el número de colas de prioridad *(marcada erróneamente → 0.00 pts)* · b. Minimizar la fragmentación externa · c. Maximizar el tamaño del quantum · d. Minimizar el tiempo de respuesta

> **Respuesta Correcta:** **`d. Minimizar el tiempo de respuesta`** *(en lotes el criterio análogo es minimizar el tiempo de retorno)*.

---

### Pregunta 16 *(3.00 pts — Selección Múltiple)*
**Enunciado:** *Sobre la planificación por prioridad, seleccione las correctas:*
- a. Sin envejecimiento puede presentarse inanición de procesos
- b. La prioridad puede asignarse de forma estática o dinámica
- c. Siempre utiliza colas múltiples con realimentación *(marcada erróneamente)*
- d. Garantiza automáticamente 1/N de CPU para cada usuario *(corresponde a Planificación Garantizada)*

> **Respuestas Correctas:** **`a` y `b`**.

---

## Parte B — Simulacro Complementario de Active Recall (Temas de U3 Partes 3–6 y U4 no incluidos arriba)

### Bloque 1 — Unidad 3 Partes 3 y 4 (Hilos, SMP, Clusters y Micronúcleo)

1. **[Comparación]** Compare **ULT** vs. **KLT** respecto a: (a) quién realiza el cambio de hilo, (b) qué ocurre si un hilo realiza una llamada bloqueante al S.O. y (c) si aprovechan arquitecturas **SMP**.
2. **[Comparación]** Compare **Arquitectura Maestro/Esclavo** vs. **SMP** indicando las dos desventajas críticas de Maestro/Esclavo y los tres aspectos/desafíos de diseño propios de SMP.
3. **[Conceptual]** ¿Cuáles son las **3 funciones esenciales** que permanecen dentro del **Micronúcleo** y qué modelo de comunicación utilizan los servidores que corren en modo usuario?

<details>
<summary><strong>Respuestas Esperadas — Bloque 1</strong></summary>

1. **ULT:** (a) el cambio lo hace la biblioteca de hilos en modo usuario (sin cambio a modo núcleo); (b) si un hilo se bloquea, **se bloquean todos los hilos del proceso**; (c) **no** aprovecha SMP. **KLT:** (a) requiere cambio a modo núcleo; (b) si un hilo se bloquea, el kernel puede planificar **otro hilo del mismo proceso**; (c) **sí** planifica simultáneamente varios hilos del mismo proceso en múltiples CPUs (SMP).
2. **Maestro/Esclavo:** el S.O. corre siempre en el mismo procesador maestro → desventajas: (1) **punto único de fallo** (si falla el maestro cae todo el sistema) y (2) el maestro se vuelve un **cuello de botella**. **SMP:** el S.O. puede correr en cualquier CPU (todos simétricos con memoria y E/S compartidas) → aspectos a resolver: (1) **conflictos de planificación** (dos CPUs tomando el mismo proceso/hilo), (2) **coherencia de caché** y (3) **sincronización** de zonas de memoria compartidas.
3. Dentro del micronúcleo permanecen: (1) **Gestión de memoria a bajo nivel** (traducción página virtual → marco físico), (2) **IPC mediante paso de mensajes** (asociando puertos a procesos) y (3) **Gestión de interrupciones y E/S**. Los servicios externos corren como servidores en modo usuario bajo un modelo **cliente-servidor** por paso de mensajes.

</details>

---

### Bloque 2 — Unidad 3 Partes 5 y 6 (Sincronización, Mensajes y Planificación)

4. **[Caso Práctico — Productor/Consumidor]** En la solución con `sleep()` y `wakeup()` usando la variable compartida `cuenta`, explique paso a paso el escenario exacto en el que **ambos procesos terminan dormidos eternamente** y qué mecanismo (con sus operaciones atómicas) lo resuelve sin espera ocupada.
5. **[Caso Práctico — Mensajes]** Al enviar un mensaje entre máquinas distintas usando `ACK`, se pierde el **`ACK-2`** (el receptor sí había recibido el Mensaje 2). ¿Qué hace el emisor y cómo evita el receptor procesar dos veces el mismo dato? Además, ¿qué campo de la cabecera controla la integridad del mensaje?
6. **[Cálculo — Planificación]**
   - **a) SJF:** Dados 4 procesos con duraciones `A=8`, `B=4`, `C=2`, `D=1` min, calcule el tiempo promedio de terminación/espera en orden `A,B,C,D` vs. orden SJF (`D,C,B,A`).
   - **b) Garantizada:** Si los ratios `CPU utilizada / CPU prometida` son `A=0,30`, `B=0,50`, `C=1,20`, `D=2,00`, ¿qué proceso se ejecuta a continuación y por qué?
   - **c) Round Robin:** ¿Cuál es el rango de quantum apropiado visto en cátedra y qué problema ocurre si es demasiado chico o demasiado grande?

<details>
<summary><strong>Respuestas Esperadas — Bloque 2</strong></summary>

4. El Consumidor lee `cuenta == 0` y es interrumpido por el planificador *antes* de ejecutar `sleep()`. El Productor inserta un ítem, sube `cuenta` a `1` y llama `wakeup(consumidor)`, pero como el Consumidor no estaba dormido, **la señal `wakeup` se pierde**. El Productor llena el buffer hasta `cuenta == N` y hace `sleep()`. Luego el Consumidor retoma en `sleep()` y también se duerme → **bloqueo mutuo eterno**. Se resuelve con **Semáforos** (operaciones atómicas `DOWN` y `UP`, que acumulan el incremento de `S` para no perder despertares).
5. Al no recibir `ACK-2`, el emisor **retransmite el mensaje 2**. El receptor inspecciona el **`Nro. de Secuencia`** en la cabecera, detecta que ya recibió ese mensaje (**duplicado**), descarta la copia repetida y reenvía `ACK-2`. La integridad se controla con el **`C.R.C.` (*Código de Redundancia Cíclica*)**.
6. **a) SJF:** Orden `A,B,C,D`: $(8+12+14+15)/4 = 49/4 = \mathbf{12{,}25\text{ min}}$. Orden `D,C,B,A` (SJF): $(1+3+7+15)/4 = 26/4 = \mathbf{6{,}5\text{ min}}$.
   **b) Garantizada:** Se ejecuta el **Proceso A (`0,30`)** porque tiene el menor cociente, lo que significa que es el que **menos CPU utilizó respecto de la prometida ($1/N$)**.
   **c) Round Robin:** Quantum apropiado: **20 a 50 mseg**. Muy pequeño → demasiados **cambios de contexto**; muy grande → aumenta el **tiempo de respuesta** (tiende a FCFS).

</details>

---

### Bloque 3 — Unidad 4 (Seguridad, Accesos, Auditoría y Malware)

7. **[Comparación]** Diferencie **ataques pasivos** (y cómo se previenen) de los **4 tipos de ataques activos** sobre redes/comunicaciones.
8. **[Conceptual]** ¿Cómo se implementa en la práctica una **Matriz de Acceso** grande y dispersa? Diferencie descomposición por filas vs. por columnas.
9. **[Cazabobos]** Compare **verificación reactiva** vs. **verificación proactiva** de contraseñas, y **Requiere anfitrión** vs. **Independiente / Se replica** en la clasificación de *software malicioso* (*Puerta secreta, Bomba lógica, Troyano, Virus, Gusano, Zombie*).
10. **[Práctica/Linux]** ¿Qué diferencia hay entre ejecutar `$ su usuario` y `$ su - usuario`, y para qué sirven `gpasswd` y `newgrp`? Además, enumere los **5 elementos** de un registro de auditoría y las **4 estrategias** de detección de los antivirus.

<details>
<summary><strong>Respuestas Esperadas — Bloque 3</strong></summary>

7. **Pasivos** (acceden sin alterar los recursos: *revelación del contenido* y *análisis de tráfico*) → se previenen con **cifrado de datos**. **Activos** (alteran o inventan flujo): *enmascaramiento/suplantación*, *reenvío*, *modificación de mensajes* y *denegación de servicios (DoS)*.
8. Descomposición por **columnas → Listas de Control de Acceso (ACL)** asociadas a cada objeto/archivo (qué usuarios tienen qué derechos). Descomposición por **filas → Etiquetas de Capacidades** asociadas a cada usuario/sujeto (a qué objetos puede acceder).
9. **Reactiva:** el sistema intenta crackear claves ya guardadas y cancela las débiles (inconveniente: quedan vulnerables hasta detectarse). **Proactiva:** valida la fortaleza **en el momento** de elegir la clave antes de aceptarla. **Malware:** requieren programa anfitrión → *Puerta secreta, Bomba lógica, Troyano, Virus*; independientes → *Gusano, Zombie*; **se replican** → *Virus, Gusano, Zombie*.
10. `su - usuario` cambia al *homedirectory* y entorno del usuario destino; `su usuario` (sin `-`) inicia sesión pero mantiene el directorio actual. `gpasswd` configura contraseña a un grupo (en `/etc/gshadow`) para que un usuario pueda unirse temporalmente con `newgrp`. **5 elementos de auditoría:** *sujeto, acción, objeto, utilización de recursos, sello de tiempo*. **4 estrategias antivirus:** *firmas de virus, detección heurística, detección por comportamiento, caja de arena (sandbox)*.

</details>
