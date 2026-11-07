# Taller: Diagramas con draw.io — RPG Manager

**Estudiante:** *Juan Esteban Pineda Gonzalez*  
**Fecha: 6/11/2026**   


## Archivos

|Diagrama|Editable|Imagen|
|-|-|-|
|Contexto (nivel 0)|`01\_contexto\_rpg.drawio`|`01\_contexto\_rpg.svg`|
|Casos de uso|`02\_casos\_uso\_rpg.drawio`|`02\_casos\_uso\_rpg.svg`|
|Actividades (2 páginas: CU-07 y CU-03)|`03\_actividades\_combate.drawio`|`03\_actividades\_cu07.svg`, `03\_actividades\_cu03.svg`|

\---

## Bloque 1 — Diagrama de contexto

!\[Diagrama de contexto](01\_contexto\_rpg.svg)

### Ejercicio 1A — Flujos que faltan

|#|Origen|Flujo de datos|Destino|
|-|-|-|-|
|F10|Jugador|credenciales de acceso|RPG Manager|
|F11|Servicio de Respaldo en la Nube|partida guardada|RPG Manager|

### Bug a corregir

* **Error 1:** "Base de datos de Personajes" no es una entidad externa. Es un almacén de datos interno del sistema y no va en el nivel 0.
* **Error 2:** "Crear Personaje" es una acción (un caso de uso). El flujo debe nombrarse con el dato que viaja, por ejemplo "datos del personaje".
* **Error 3:** Una flecha directa Jugador → Game Master no es válida. Las entidades externas no se conectan entre sí; todo dato pasa por el sistema.

\---

## Bloque 2 — Diagrama de casos de uso

!\[Diagrama de casos de uso](02\_casos\_uso\_rpg.svg)

### Ejercicio 2A — Generalización de actores

* **¿Hacia dónde apunta el triángulo vacío y por qué?** Hacia **Jugador** (el actor general). El Jugador Premium es un tipo especial de Jugador, y la generalización siempre apunta al más general.
* **¿Por qué el Jugador Premium NO necesita líneas a CU-01 … CU-10?** Porque hereda todo lo que puede hacer el Jugador por la generalización. Solo se dibuja lo que agrega: CU-13 Comprar ítem exclusivo.

### Bug a corregir

* **Error 1:** "Inventario" es un sustantivo. Un caso de uso es verbo + sustantivo: "Gestionar Inventario".
* **Error 2:** El Game Master es un actor, así que va **fuera** del límite del sistema.
* **Error 3:** La flecha «include» va del caso base al incluido: de CU-03 Crear Personaje hacia CU-04 Elegir clase, no al revés.
* **Error 4:** Los actores no se asocian entre sí. Solo se relacionan con casos de uso (o entre ellos por generalización).

\---

## Bloque 3 — Diagramas de actividades

### CU-07 Combatir en la Arena

!\[Actividades CU-07](03\_actividades\_cu07.svg)

### Ejercicio 3A — ¿Y si se acaban las pociones?

* **¿En qué carril va la decisión?** En el carril **RPG Manager**, porque es el sistema quien revisa el inventario.
* **Guarda 1:** `\[hay pociones]` → va a **Restaurar vida del Personaje**.
* **Guarda 2:** `\[sin pociones]` → va a **Informar "No tienes pociones"**, y de ahí se vuelve a **Iniciar ronda** para que el Jugador elija otra acción.

### Ejercicio 3B — CU-03 Crear Personaje

!\[Actividades CU-03](03\_actividades\_cu03.svg)

* **¿Qué caso de uso del Bloque 2 aparece "escondido" en este flujo?** **CU-04 Elegir clase**: es el paso donde el Jugador elige Guerrero, Mago o Arquero (relación «include» de CU-03).

### Bug a corregir

* **Error 1:** Un rombo con una sola salida `\[sí]`. Toda decisión debe tener guardas que cubran todos los casos (falta `\[no]`).
* **Error 2:** Un fork cuyos caminos terminan cada uno en su propio nodo final, sin join. Los caminos paralelos deben unirse en un join antes de continuar.
* **Error 3:** "Calcular daño al oponente" lo ejecuta el sistema, así que va en el carril **RPG Manager**, no en el del Jugador.

\---

## Reflexión final

1. **¿Por qué el contexto no muestra la base de datos?** Porque el nivel 0 solo muestra lo que intercambia el sistema con el exterior, tratándolo como una caja negra. La base de datos es interna y aparece desde el nivel 1.
2. **"datos del personaje" vs "Crear Personaje".** "datos del personaje" es la información que viaja (nombre, clase). "Crear Personaje" es lo que el actor quiere lograr con el sistema: una acción, o sea un caso de uso.
3. **Ejemplos propios.** «include»: *Comprar ítem exclusivo* incluye *Validar saldo*, porque siempre se valida. «extend»: *Usar código de invitación* extiende *Registrarse*, porque solo ocurre si el jugador lo tiene.
4. **¿Qué aporta el diagrama de actividades?** El orden de los pasos, las decisiones, los ciclos, el paralelismo y quién (qué carril) hace cada cosa. El de casos de uso solo dice *qué* se puede hacer.
5. **¿En qué clase vive "Calcular daño al oponente"?** En la clase **Personaje** (por ejemplo `calcularDaño(oponente)`), porque ahí están los datos que lo determinan (ataque, clase). La Arena de Combate solo coordina las rondas.
6. **PlantUML vs draw.io.** PlantUML cuando la documentación cambia seguido y se versiona en Git (los cambios se leen línea a línea). draw.io cuando hay que presentar o explicar, y se necesita control visual total.

## Autoevaluación

