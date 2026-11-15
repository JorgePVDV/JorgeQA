# Análisis de logs y base de datos de una app de taxis

Proyecto del **Sprint 7** de mi formación en pruebas de software (QA). Trabajé sobre una aplicación de taxis en dos frentes: **logs de servidor desde la consola** y **consultas SQL** sobre una base de datos de viajes en taxi de Chicago.

**Autor:** Jorge Alberto Paez Vargas · QA Engineer
**Estado:** entregado y aprobado

---

## Qué demuestra este proyecto

- Investigar un incidente leyendo logs de un servidor remoto.
- Filtrar y ordenar información con comandos de consola.
- Consultar una base de datos relacional con SQL para responder preguntas de negocio.
- Documentar los comandos usados, las consultas y los resultados de forma que otra persona pueda reproducirlos.

## Herramientas

| Área | Herramienta |
|---|---|
| Consola | Terminal, SSH, `grep`, redirección de salida, `mkdir` |
| Logs | Registros de servidor Apache (`logs/2019/12`) |
| Base de datos | PostgreSQL |
| SQL | `COUNT`, `DISTINCT`, `GROUP BY`, `HAVING`, `CASE`, `JOIN`, `ORDER BY` |
| Documentación | Google Docs |

---

## Parte 1 · Consola y logs

Los logs estaban en un servidor remoto, en la carpeta `logs/2019/12`, y no se sabía en qué día ocurrió el error.

### Ejercicio 1 · Buscar solicitudes por dirección IP

**Objetivo:** encontrar las solicitudes que llegaron desde direcciones IP con el patrón `233.201`, revisando todos los archivos del mes.

**Comando usado:**

```bash
grep -r ".233.201" logs/2019/12
```

`-r` hace que `grep` busque dentro de todos los archivos de la carpeta, así no hace falta saber de antemano qué día falló. Cada resultado muestra el archivo y la línea del log, por ejemplo:

```
137.189.233.201 - - [11/12/2019:21:55:33 +0000] "PATCH /users HTTP/1.1" 401 3763
```

### Ejercicio 2 · Separar los logs del error por tipo

**Objetivo:** el sistema falló el 30/12/2019 con errores **400** y **500**. Había que guardar esos logs en archivos independientes.

**Pasos que seguí:**

1. Crear en el directorio de inicio del servidor la carpeta `bug1` y, dentro, la carpeta `events`.
2. Guardar en `bug1/main.txt` todas las solicitudes del periodo del error.
3. Separar de `main.txt` los registros con error 400 en `events/400.txt` y los de error 500 en `events/500.txt`.

**Estructura resultante:**

```
~/
└── bug1/
    ├── main.txt
    └── events/
        ├── 400.txt
        └── 500.txt
```

Los comandos exactos y el contenido de los archivos están en el [documento completo](#documento-completo).

---

## Parte 2 · Base de datos (SQL)

### Datos

Base de datos de viajes en taxi de Chicago:

| Tabla | Contenido |
|---|---|
| `neighborhoods` | Barrios de la ciudad (`neighborhood_id`, `name`) |
| `cabs` | Taxis y su compañía (`cab_id`, `vehicle_id`, `company_name`) |
| `trips` | Viajes: inicio, fin, duración, distancia, origen y destino |
| `weather_records` | Clima por hora: `ts`, `temperature`, `description` |

`trips` y `weather_records` no tienen una llave en común. Se relacionan por la hora: `trips.start_ts` con `weather_records.ts`.

### Ejercicio 1 · ¿Cuántos taxis hay realmente?

El plan era tener 10 550 vehículos y llegaron quejas de que no alcanzaban. Conté los automóviles de la tabla `cabs`, teniendo en cuenta que un mismo automóvil podría pertenecer a distintas compañías.

### Ejercicio 2 · Compañías con pocos automóviles

Conté los automóviles por compañía y devolví solo las de **menos de 100**, ordenadas de mayor a menor. Para filtrar sobre un resultado agregado usé `HAVING` en lugar de `WHERE`. Los campos se llaman `cnt` y `company_name`.

### Ejercicio 3 · Coeficiente del costo del viaje según el clima

El coeficiente vale 1 con buen clima y 2 con lluvia o tormenta, y se sospechaba un error en su cálculo. Con `CASE` clasifiqué cada hora de `weather_records` como `Bad` (si `description` contiene «rain» o «storm») o `Good` (el resto). Resultado: dos campos, `ts` y `weather_conditions`, para el periodo del 05/11/2017 00:00 al 06/11/2017 00:00.

### Ejercicio 4 · Número de viajes por compañía

Tras una actualización, las compañías reportaban ganancias distintas a las de la app. Para revisar si el problema estaba en el conteo de viajes, uní `cabs` con `trips` y conté los viajes de cada compañía los días **15 y 16 de noviembre de 2017**, ordenados de mayor a menor por `trips_amount`.

---

## Documento completo

El detalle de cada ejercicio, con los comandos, las consultas SQL, los resultados y las capturas, está en el documento que entregué:

👉 [Ver documento del proyecto](https://docs.google.com/document/d/1s5KazD0ko8KRXzInXQOFvweXhzsh_I8l/edit?usp=sharing)

## Qué aprendí

- Que `grep -r` ahorra tiempo cuando no sabes en qué archivo está el problema.
- A separar un incidente en archivos por tipo de error para que el equipo de desarrollo lo analice más fácil.
- A usar `HAVING` para filtrar resultados agregados y `CASE` para clasificar datos.
- A relacionar tablas que no tienen una llave directa, usando la hora como vínculo.

## Contacto

- GitHub: [@JorgePVDV](https://github.com/JorgePVDV)
- Portafolio: https://jorgepvdv.github.io/JorgeQA/
