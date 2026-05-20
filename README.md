# API de Registro de Productos

API REST para gestionar un catálogo de productos con autenticación JWT, operaciones CRUD completas y gestión de categorías. Especificación construida con **OpenAPI 3.0.3**.

---

## Descripción general

Este proyecto define la especificación de una API que permite:

- **Registrar usuarios** y autenticarlos mediante tokens JWT.
- **Crear, leer, actualizar y eliminar productos** con soporte para paginación, filtros y búsqueda.
- **Organizar productos en categorías** con su propio ciclo de vida CRUD.

La especificación está contenida en el archivo `product-registry-api.yaml` y puede importarse en cualquier herramienta compatible con OpenAPI (Swagger Editor, Postman, Redocly, etc.).

---

## Estructura de la API

### Base URLs

| Entorno    | URL                                            |
|------------|------------------------------------------------|
| Producción | `https://api.productos.example.com/v1`         |
| Staging    | `https://staging-api.productos.example.com/v1` |

### Endpoints

#### Autenticación (públicos)

| Método | Ruta             | Descripción                |
|--------|------------------|----------------------------|
| POST   | `/auth/register` | Registrar un nuevo usuario |
| POST   | `/auth/login`    | Iniciar sesión             |

#### Productos (requieren autenticación)

| Método | Ruta                    | Descripción                          |
|--------|-------------------------|--------------------------------------|
| GET    | `/productos`            | Listar productos (paginado + filtros)|
| POST   | `/productos`            | Crear un producto                    |
| GET    | `/productos/{id}`       | Obtener un producto por ID           |
| PUT    | `/productos/{id}`       | Actualizar un producto completo      |
| PATCH  | `/productos/{id}`       | Actualizar campos específicos        |
| DELETE | `/productos/{id}`       | Eliminar un producto                 |

#### Categorías (requieren autenticación)

| Método | Ruta                    | Descripción              |
|--------|-------------------------|--------------------------|
| GET    | `/categorias`           | Listar categorías        |
| POST   | `/categorias`           | Crear una categoría      |
| PUT    | `/categorias/{id}`      | Actualizar una categoría |
| DELETE | `/categorias/{id}`      | Eliminar una categoría   |

---

## Autenticación

La API utiliza **JWT (JSON Web Token)** con esquema Bearer. El flujo es:

1. Registrarse o iniciar sesión mediante los endpoints de `/auth`.
2. Obtener el `token` de la respuesta.
3. Incluirlo en todas las solicitudes siguientes en el header:

```
Authorization: Bearer <token>
```

El token tiene una expiración configurable (por defecto 3600 segundos).

---

## Modelos principales

### Usuario

| Campo           | Tipo     | Descripción                         |
|-----------------|----------|-------------------------------------|
| id              | UUID     | Identificador único                 |
| nombre          | string   | Nombre completo                     |
| email           | string   | Correo electrónico (único)          |
| rol             | enum     | `admin`, `editor` o `lector`        |
| fecha_creacion  | datetime | Fecha de registro                   |

### Producto

| Campo               | Tipo     | Descripción                          |
|---------------------|----------|--------------------------------------|
| id                  | UUID     | Identificador único                  |
| nombre              | string   | Nombre del producto (máx. 200 car.)  |
| descripcion         | string   | Descripción detallada (máx. 2000)    |
| precio              | double   | Precio unitario (≥ 0)               |
| stock               | integer  | Unidades disponibles (≥ 0)          |
| sku                 | string   | Código único de inventario           |
| categoria_id        | UUID     | Referencia a la categoría            |
| imagenes            | array    | Hasta 10 imágenes (url + alt)        |
| creado_por          | UUID     | Usuario que creó el producto         |
| fecha_creacion      | datetime | Fecha de creación                    |
| fecha_actualizacion | datetime | Última modificación                  |

### Categoría

| Campo           | Tipo     | Descripción                          |
|-----------------|----------|--------------------------------------|
| id              | UUID     | Identificador único                  |
| nombre          | string   | Nombre de la categoría (máx. 100)    |
| descripcion     | string   | Descripción opcional (máx. 500)      |
| fecha_creacion  | datetime | Fecha de creación                    |

---

## Filtros y paginación

El endpoint `GET /productos` soporta los siguientes parámetros de query:

| Parámetro     | Tipo    | Default           | Descripción                            |
|---------------|---------|--------------------|----------------------------------------|
| pagina        | integer | 1                  | Número de página                       |
| limite        | integer | 20 (máx. 100)     | Resultados por página                  |
| categoria_id  | UUID    | —                  | Filtrar por categoría                  |
| busqueda      | string  | —                  | Búsqueda por nombre o descripción      |
| precio_min    | double  | —                  | Precio mínimo                          |
| precio_max    | double  | —                  | Precio máximo                          |
| ordenar_por   | enum    | `fecha_creacion`   | Campo de ordenamiento                  |
| orden         | enum    | `desc`             | Dirección: `asc` o `desc`             |

La respuesta paginada incluye metadatos:

```json
{
  "datos": [ ... ],
  "paginacion": {
    "pagina_actual": 1,
    "total_paginas": 5,
    "total_resultados": 98,
    "limite": 20
  }
}
```

---

## Manejo de errores

Todas las respuestas de error siguen un formato consistente:

```json
{
  "codigo": 400,
  "mensaje": "El campo 'nombre' es obligatorio.",
  "detalles": [
    {
      "campo": "nombre",
      "error": "Este campo no puede estar vacío."
    }
  ]
}
```

| Código | Significado                                        |
|--------|----------------------------------------------------|
| 400    | Solicitud inválida — datos faltantes o incorrectos |
| 401    | No autenticado — token ausente o expirado          |
| 404    | Recurso no encontrado                              |
| 409    | Conflicto — recurso duplicado (ej. email)          |

---

## Cómo usar la especificación

### Swagger Editor (online)

1. Ir a [editor.swagger.io](https://editor.swagger.io).
2. Menú **File → Import File** y seleccionar `product-registry-api.yaml`.
3. Explorar y probar los endpoints desde la interfaz.

### Postman

1. Abrir Postman → **Import**.
2. Seleccionar el archivo `.yaml`.
3. Se generará automáticamente una colección con todos los endpoints.

### Generación de código

Con [OpenAPI Generator](https://openapi-generator.tech/) se pueden generar clientes y servidores en múltiples lenguajes:

```bash
# Ejemplo: generar un servidor en Node.js
openapi-generator-cli generate \
  -i product-registry-api.yaml \
  -g nodejs-express-server \
  -o ./server

# Ejemplo: generar un cliente en Python
openapi-generator-cli generate \
  -i product-registry-api.yaml \
  -g python \
  -o ./client-python
```

---

## Validaciones destacadas

- **Password**: mínimo 8 caracteres, al menos una mayúscula, una minúscula y un número.
- **SKU**: patrón `^[A-Z0-9\-]{3,30}$` (solo mayúsculas, números y guiones).
- **Imágenes**: máximo 10 por producto.
- **Paginación**: límite entre 1 y 100 resultados por página.
- **IDs**: formato UUID v4 en todos los identificadores.

---

## Tecnologías y estándares

- **OpenAPI 3.0.3** — Especificación de la API.
- **JWT (RFC 7519)** — Autenticación stateless.
- **UUID v4** — Identificadores únicos.
- **ISO 8601** — Formato de fechas.

---

## Licencia

Este proyecto se distribuye bajo la licencia MIT. Consultar el archivo `LICENSE` para más detalles.
