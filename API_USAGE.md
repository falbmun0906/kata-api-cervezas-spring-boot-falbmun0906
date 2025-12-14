# Documentación de uso de la API de Cervezas

Este documento recoge el proceso de desarrollo y los ejemplos de uso de la API REST creada para la gestión de cervezas, cerveceras, categorías y estilos, siguiendo los requisitos de la kata.

Comencé creando la estructura básica del proyecto con Spring Boot y configurando la conexión a la base de datos MySQL/MariaDB, asegurando que la aplicación pudiera arrancar correctamente y acceder a las tablas proporcionadas en los scripts SQL.

A continuación, implementé las entidades JPA correspondientes a las tablas principales: `Beer`, `Brewery`, `Category` y `Style`. Para simplificar el código y evitar la escritura manual de getters y setters, utilicé Lombok en todas las entidades.

Después, creé los repositorios JPA para cada entidad, permitiendo el acceso y manipulación de los datos de forma sencilla mediante Spring Data JPA.

Posteriormente, desarrollé los controladores REST para exponer los endpoints definidos en el enunciado. Cada controlador implementa los métodos necesarios para cumplir con las operaciones CRUD básicas y la consulta de los recursos relacionados.

A continuación se muestran ejemplos de uso de la API para cada uno de los endpoints principales:

## Ejemplos de peticiones

### Obtener todas las cervezas

```
GET /beers
```
Respuesta: 200 OK
```json
[
  {
    "id": 1,
    "name": "Pale Ale",
    "abv": 5.2,
    "ibu": 40,
    "srm": 10,
    "ebc": 20,
    "style_id": 1,
    "brewery_id": 1,
    "category_id": 1,
    "description": "Cerveza rubia refrescante."
  },
  ...
]
```

### Añadir una cerveza

```
POST /beer
Content-Type: application/json

{
  "name": "IPA",
  "abv": 6.5,
  "ibu": 60,
  "srm": 12,
  "ebc": 24,
  "style_id": 2,
  "brewery_id": 1,
  "category_id": 1,
  "description": "India Pale Ale aromática."
}
```
Respuesta: 200 OK

### Obtener una cerveza por id

```
GET /beer/1
```
Respuesta: 200 OK
```json
{
  "id": 1,
  "name": "Pale Ale",
  "abv": 5.2,
  "ibu": 40,
  "srm": 10,
  "ebc": 20,
  "style_id": 1,
  "brewery_id": 1,
  "category_id": 1,
  "description": "Cerveza rubia refrescante."
}
```

### Modificar una cerveza

El endpoint PUT permite tanto actualizaciones totales como parciales. Solo es necesario enviar los campos que se desean modificar, y el resto permanecerán inalterados.

Actualización total:
```
PUT /beer/1
Content-Type: application/json

{
  "name": "Pale Ale Actualizada",
  "abv": 5.5,
  "ibu": 42,
  "srm": 11,
  "ebc": 22,
  "style_id": 1,
  "brewery_id": 1,
  "category_id": 1,
  "description": "Actualización de la descripción."
}
```
Respuesta: 200 OK

Actualización parcial (solo modificar el nombre):
```
PUT /beer/1
Content-Type: application/json

{
  "name": "Pale Ale Mejorada"
}
```
Respuesta: 200 OK

El método PATCH también está disponible y funciona de la misma forma:
```
PATCH /beer/1
Content-Type: application/json

{
  "abv": 6.0
}
```
Respuesta: 200 OK

### Eliminar una cerveza

```
DELETE /beer/1
```
Respuesta: 204 No Content

### Listar todas las cerveceras

```
GET /breweries
```
Respuesta: 200 OK

### Obtener una cervecera por id

```
GET /brewerie/1
```
Respuesta: 200 OK

### Listar todas las categorías

```
GET /categories
```
Respuesta: 200 OK

### Obtener una categoría por id

```
GET /categorie/1
```
Respuesta: 200 OK

### Listar todos los estilos

```
GET /styles
```
Respuesta: 200 OK

### Obtener un estilo por id

```
GET /style/1
```
Respuesta: 200 OK

Cada endpoint ha sido probado utilizando herramientas como Postman y curl para verificar su funcionamiento y la correcta gestión de los datos en la base de datos.

## Pruebas automatizadas

He creado una colección de Postman (`cervezas-api.postman_collection.json`) que contiene todos los tests necesarios para verificar el correcto funcionamiento de la API. Esta colección puede ejecutarse de forma automatizada utilizando Newman.

### Instalación de Newman

Para ejecutar los tests desde la línea de comandos, primero instalé Newman globalmente:

```bash
npm install -g newman
```

### Ejecución de tests

Para ejecutar todos los tests de la colección:

```bash
newman run cervezas-api.postman_collection.json
```

### Generación de reportes HTML

Para generar un reporte HTML profesional con los resultados de los tests, instalé el reporter htmlextra:

```bash
npm install -g newman-reporter-htmlextra
```

Y ejecuté los tests con la generación del reporte:

```bash
newman run cervezas-api.postman_collection.json -r htmlextra --reporter-htmlextra-export test-results.html --reporter-htmlextra-title "API de Cervezas - Resultados de Tests" --reporter-htmlextra-darkTheme
```

Este comando genera un archivo `test-results.html` con un reporte detallado y visualmente atractivo que incluye:

- Resumen general de la ejecución
- Estadísticas de tests exitosos/fallidos
- Detalles de cada petición realizada
- Tiempos de respuesta
- Cuerpos de peticiones y respuestas
- Tema oscuro/claro configurable

Los resultados de los tests muestran que todos los endpoints funcionan correctamente:

- Todas las cervezas: 200 OK
- Añadir cerveza: 200 OK
- Consultar cerveza por ID: 200 OK
- Modificar cerveza: 200 OK
- Eliminar cerveza: 204 No Content
- Todas las operaciones de lectura para cerveceras, categorías y estilos: 200 OK

### Evidencias:

En este enlace, se pueden ver las evidencias de la ejecución de los tests y el reporte generado: [Evidencias de Tests](https://falbmun0906.github.io/kata-api-cervezas-spring-boot-falbmun0906/test-results)

