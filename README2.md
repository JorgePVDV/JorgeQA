# Pruebas de API · Urban Grocers

Proyecto del **Sprint 4** de mi formación en pruebas de software (QA). Analicé dos requisitos de la API de **Urban Grocers** (trabajo con kits y servicio de entrega «Order and Go»), diseñé casos de prueba, los ejecuté con **Postman** y reporté los errores encontrados en **Jira**.

**Autor:** Jorge Alberto Paez Vargas · QA Engineer

---

## Resumen de resultados

| | Casos | Aprobados | Fallidos | Defectos en Jira |
|---|:---:|:---:|:---:|:---:|
| **Requisito 1 · Kits** | 17 | 11 | 6 | 6 |
| **Requisito 2 · Servicio de entrega** | 17 | 9 | 8 | 8 |
| **Total** | **34** | **20** | **14** | **14** |

Cada caso fallido tiene su propio informe de errores en Jira (`S4-1` a `S4-14`).

## Herramientas

| Herramienta | Uso |
|---|---|
| **Postman** | Envío de solicitudes y validación de respuestas |
| **Jira** | Informes de errores |
| **apiDoc** | Endpoints, métodos, parámetros y cuerpos de ejemplo |
| **Requisitos del backend** | Reglas de negocio y criterios de aceptación |
| **Google Sheets / Excel** | Documentación de casos de prueba y resultados |

## Técnicas de diseño de pruebas

- Casos **positivos** y **negativos**
- **Clases de equivalencia**
- **Valores límite**
- Validación de **códigos de estado** y **mensajes de error**

---

## Requisito 1 · Trabajar con los kits

**Qué se probó:** creación de kits y adición de productos a un kit.

| Endpoint | Método |
|---|---|
| `/api/v1/kits` | POST |
| `/api/v1/kits/:id/products` | POST |

**Cobertura de los 17 casos:**

- **Cantidad de productos por kit** (valores límite): 29, 30 y 31 productos. El máximo permitido es 30.
- **Nombre del kit** (valores límite): 1, 2, 3, 14, 15 y 16 caracteres. El máximo permitido es 15.
- **Caracteres no latinos** en el nombre del kit (por ejemplo, cirílico).
- **Campos obligatorios** ausentes en el cuerpo de la solicitud.
- **IDs inexistentes:** producto que no existe y kit que no existe.
- **Estructura incorrecta** del array `productsList`.
- **Datos existentes:** revisión de que ningún kit supere los 30 productos.

### Defectos encontrados (Requisito 1)

| ID | Resumen | Esperado | Obtenido |
|---|---|---|---|
| S4-1 | Se puede crear un kit con nombre de 1 carácter | 400 Bad Request | 201 Created |
| S4-2 | Se puede crear un kit con nombre de 16 caracteres | 400 Bad Request | 201 Created |
| S4-3 | Se acepta un nombre con caracteres no latinos | 400 Bad Request | 201 Created |
| S4-4 | Se crea un kit sin completar los campos obligatorios de producto | 400 Bad Request | 201 Created |
| S4-5 | Se puede agregar un producto con ID inexistente a un kit | Error | Se agrega |
| S4-6 | El kit 1 «Para pícnics» contiene 39 productos y supera el límite de 30 | Máximo 30 | 39 productos |

---

## Requisito 2 · Servicio de entrega «Order and Go»

**Qué se probó:** el cálculo de la entrega según tres campos numéricos obligatorios.

| Endpoint | Método |
|---|---|
| `/order-and-go/v1/delivery` | POST |

```json
{
  "deliveryTime": 9,
  "productsCount": 10,
  "productsWeight": 11
}
```

**Reglas del requisito:**

- Si falta un campo, tiene un tipo incorrecto o el cuerpo es inválido, la respuesta debe ser **400 Bad Request**.
- Si los valores no permiten calcular la entrega, la respuesta es **200 OK** con `"isItPossibleToDeliver": false`.

**Cobertura de los 17 casos:**

- **Campos ausentes:** sin `productsCount`, sin `productsWeight` y sin `deliveryTime`.
- **`deliveryTime`** (valores límite del horario): 7, 8, 9, 21, 22 y 23.
- **Tipo de dato incorrecto:** texto en lugar de número en los tres campos.
- **Estructura de JSON inválida.**
- **`productsCount`** (valores límite): 14, 15 y 16 productos, con el cálculo del costo de entrega.

### Defectos encontrados (Requisito 2)

| ID | Resumen | Esperado | Obtenido |
|---|---|---|---|
| S4-7 | La solicitud sin `productsCount` se procesa | 400 Bad Request | 200 OK |
| S4-8 | La solicitud sin `productsWeight` se procesa | 400 Bad Request | 200 OK |
| S4-9 | La solicitud sin `deliveryTime` se procesa | 400 Bad Request | 200 OK |
| S4-10 | `deliveryTime` = 7 (fuera de horario) permite la entrega | `isItPossibleToDeliver: false` | `true` |
| S4-11 | `deliveryTime` = 23 (fuera de horario) permite la entrega | `isItPossibleToDeliver: false` | `true` |
| S4-12 | `deliveryTime` con texto («ocho») se acepta | 400 Bad Request | 200 OK |
| S4-13 | `productsWeight` con texto («once») se acepta | 400 Bad Request | 200 OK |
| S4-14 | `productsCount` con texto («Diez») se acepta | 400 Bad Request | 200 OK |

---

## Estructura de un caso de prueba

Cada caso de la hoja de cálculo incluye:

| Campo | Contenido |
|---|---|
| Caso de prueba | Qué se quiere verificar |
| Datos de la prueba | Método, endpoint y cuerpo de la solicitud |
| Pasos | Cómo ejecutarlo en Postman |
| Resultado esperado | Código de estado y mensaje según los requisitos |
| Resultado obtenido | Lo que devolvió la API |
| Estatus | `Passed` o `Failed` |
| Enlace al defecto | Informe de errores en Jira |

## Archivos

- 📄 [Casos de prueba y resultados (Excel)](casos-de-prueba-sprint4.xlsx)

## Qué aprendí

- A derivar casos de prueba a partir de requisitos, usando clases de equivalencia y valores límite.
- A distinguir un error del cliente (400) de uno del servidor (500) y a validar tanto el código como el mensaje.
- A detectar fallos de validación: la API acepta datos que debería rechazar.
- A redactar informes de errores con pasos, resultado esperado y real, y a enlazarlos con los casos que los originaron.

## Contacto

- GitHub: [@JorgePVDV](https://github.com/JorgePVDV)
- Portafolio: https://jorgepvdv.github.io/JorgeQA/
