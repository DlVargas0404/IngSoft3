# Facturación de Pasteles — tres arquitecturas

El mismo sistema (API REST de una pastelería: productos, clientes, facturación con control de inventario, anulación de
facturas y reporte mensual) reestructurado en **tres arquitecturas independientes**. Cada carpeta es un proyecto Maven
completo y funcional: se compila, se prueba y se ejecuta por separado.

| Carpeta | Arquitectura | Idea central |
|---|---|---|
| [`arquitectura-mvc/`](arquitectura-mvc) | Modelo – Vista – Controlador | El **modelo** es rico (entidades con reglas + servicios), la **vista** es la representación JSON (DTOs) y el **controlador** solo traduce HTTP. |
| [`arquitectura-capas/`](arquitectura-capas) | N capas estricta | `presentation → business → dataaccess / integration`. Cada capa solo conoce a la inferior (verificado por un test). Entidades anémicas; las reglas viven en la capa de negocio. |
| [`arquitectura-poo/`](arquitectura-poo) | Orientada a objetos | Jerarquía de herencia (`Cliente → Persona → EntidadActivable → EntidadBase`), interfaces (`Vendible`, `Anulable`...), clases abstractas y polimorfismo (reglas de venta intercambiables). |

## Qué NO cambia entre las tres
API REST (mismas rutas, JSON, códigos HTTP y roles), esquema de base de datos (`db/schema.sql`), reglas de negocio,
integración con `authcore-service` y la prueba de extremo a extremo `ApiIntegrationTest` (idéntica en las tres).

## Ejecutar cualquiera
```bash
cd arquitectura-mvc        # o arquitectura-capas / arquitectura-poo
mvn test                   # pruebas
mvn spring-boot:run        # http://localhost:8080  (H2 en memoria + authcore simulado)
```
```bash
curl -X POST localhost:8080/api/productos -H "Authorization: Bearer admin-token" -H "Content-Type: application/json" \
  -d '{"codigoUnico":"TOR-001","nombre":"Torta de chocolate","idCategoria":1,"precio":45000,"stock":10}'
```
Requisitos: JDK 21 y Maven 3.9+. Cada proyecto tiene su propio README con la arquitectura, estructura de paquetes,
endpoints, roles y reglas de negocio.
