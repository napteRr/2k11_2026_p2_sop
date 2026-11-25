# SOP — Resúmenes y Modelos de Parcial (2.º Parcial - UTN FRC)

Material de estudio completo en formato **Markdown** (compatible directamente con GitHub) y cuestionario interactivo para el **Segundo Parcial de Sistemas Operativos**.

## Cuestionario Interactivo (Simulacro Online)

> **[Abrir Cuestionario Interactivo — Segundo Parcial SOP](https://napterr.github.io/2k11_2026_p2_sop/)**
>
> Simulacro y modo repaso estilo UV (Parcial Real 2k11, otros parciales reales UV, Teoría U3–U4, Práctica y 20 temas a desarrollar con rúbrica). Funciona directo desde el navegador (PC o celular) y también podés usar [`cuestionario_parcial2_sop.html`](cuestionario_parcial2_sop.html) de forma 100% offline.

---

## Contenidos

### Resúmenes Multinivel (Teoría — Alcance: U3 Partes 3, 4, 5 y 6 + U4 Seguridad; no entran Memoria ni Docker)
- [**Unidad 3: Administración de Procesos (Partes 3, 4, 5 y 6)**](SOP%20-%20Resumen%20Multinivel%20-%20Unidad%203%20-%20Procesos.md)
  - *Parte 3:* Hilos (*threads*), Monohilo vs. Multihilo, Estados, RPC y ULT vs. KLT.
  - *Parte 4:* Procesamiento paralelo, Maestro/Esclavo, SMP, *Clusters* y Arquitectura Micronúcleo.
  - *Parte 5:* Concurrencia, Condiciones de competencia, Exclusión Mutua, Productor-Consumidor, Semáforos, Monitores y Paso de Mensajes.
  - *Parte 6:* Planificadores (Largo, Mediano y Corto plazo), Criterios y Algoritmos (FCFS, SJF, SRTN, Round Robin, Prioridad, Colas Múltiples y Garantizada).
- [**Unidad 4: Seguridad en Sistemas Operativos**](SOP%20-%20Resumen%20Multinivel%20-%20Unidad%204%20-%20Seguridad.md)
  - Requisitos y Amenazas, Ataques Pasivos vs. Activos, Protección y Matriz de Acceso (ACL vs. Capacidades), Aderezo/*Salt* (`crypt(3)`), Estrategias de contraseñas, `/etc/passwd`, `/etc/group`, `/etc/shadow`, Auditoría/Detección de Intrusos, *Malware* y Antivirus.

### Resumen Práctico (Comandos, Permisos, Cron y Shellscripts)
- [**Resumen — Segundo Parcial Práctico**](SOP%20-%20Resumen%20-%20Segundo%20Parcial%20Practico.md) — Gestión de procesos (`ps`, `jobs`, `fg`, `bg`, `kill`, `nice`/`renice`), permisos y empaquetado (`chmod`, `tar`), tareas programadas (`cron`, `crontab`, `at`), pipes/filtros (`head`, `tail`, `grep`, `tr`, `cut`), variables y *shellscripts* (`if`, `for`, `while`, `until`, `$0`–`$#`).

### Modelos de Parcial (Active Recall — basados en `Segundo Parcial 2k11` / Napter + Guía 2026)
- [**Modelo de Parcial Teórico**](SOP%20-%20Modelo%20de%20Parcial%20-%20Teorico.md) — Revisión completa de las Preguntas 1–16 del parcial real con sus trampas y rúbricas de 10 + banco complementario de U3 (Partes 3–6) y U4.
- [**Modelo de Parcial Práctico**](SOP%20-%20Modelo%20de%20Parcial%20-%20Practico.md) — Revisión completa de las Preguntas 17–29 del parcial real + simulacro práctico completo (`ps`, `nice`/`renice`, `jobs`/`fg`/`bg`/`kill`, `tar`, `chmod`, `crontab`/`at`, pipes/filtros y shellscripts).

---

## Estructura de Imágenes (`img/`)

Todas las capturas de ejemplo extraídas de los PDFs oficiales están organizadas por unidad y parte y enlazadas con rutas relativas (`img/...`) para funcionar sin modificaciones al subir la carpeta a GitHub:

- `img/unidad3_parte3/` (`traza_e_imagen_proceso.png`, `procesos_e_hilos_so.png`, `entorno_monohilo.png`, `entorno_multihilo.png`, `rpc_un_hilo.png`, `rpc_dos_hilos.png`, `hilos_ult.png`, `hilos_klt.png`)
- `img/unidad3_parte4/` (`procesamiento_paralelo_clasificacion.png`, `arquitectura_maestro_esclavo.png`, `arquitectura_smp.png`, `cluster_nodos.png`, `arquitectura_por_capas.png`, `arquitectura_micronucleo.png`)
- `img/unidad3_parte5/` (`procesos_interactuantes.png`, `multiprogramacion.png`, `multiprocesamiento.png`, `exclusion_mutua_esquema.png`, `productor_consumidor_codigo.png`, `semaforos_sleep_wakeup.png`, `mensaje_formato.png`, `mensaje_fragmentacion.png`, `mensaje_ack_retransmision.png`)
- `img/unidad3_parte6/` (`tipos_planificacion_estados.png`, `round_robin_cola.png`, `planificacion_prioridad.png`, `colas_multiples_ejemplo.png`, `planificacion_garantizada.png`)
- `img/unidad4_parte_unica/` (`amenazas_y_ataques.png`, `matriz_de_acceso.png`, `contrasena_carga_aderezo.png`, `contrasena_verificacion_aderezo.png`, `politica_contrasenas_robustez.png`, `etc_passwd_ejemplo.png`, `etc_group_ejemplo.png`, `etc_shadow_ejemplo.png`, `software_malicioso_clasificacion.png`)
