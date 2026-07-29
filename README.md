# Justificación del Diseño de la Base de Datos

La base de datos fue diseñada siguiendo los principios de normalización e integridad referencial con el propósito de administrar de manera eficiente la información de un concesionario de vehículos. El modelo permite gestionar el inventario de vehículos, los clientes, los vendedores, las ventas y los servicios de mantenimiento, evitando la duplicidad de datos y garantizando la consistencia de la información.

## Descripción de las tablas

### Vehículo

Esta tabla almacena toda la información relacionada con los vehículos disponibles o vendidos por el concesionario.

**Campos:**

* **id_vehiculo (PK):** Identificador único del vehículo.
* **vin (UNIQUE):** Número de serie único del vehículo.
* **marca:** Fabricante del vehículo.
* **modelo:** Modelo del vehículo.
* **anio:** Año de fabricación.
* **precio:** Precio de venta.
* **color:** Color del vehículo.
* **tipo_combustible:** Tipo de combustible (gasolina, diésel, híbrido, eléctrico, etc.).
* **tipo_transmision:** Tipo de transmisión (manual o automática).
* **estado:** Disponibilidad del vehículo (disponible o no disponible).

**Justificación:** El VIN se establece como único para evitar registros duplicados y asegurar la identificación inequívoca de cada vehículo. El campo **estado** permite controlar la disponibilidad del inventario después de cada venta.

---

### Cliente

Almacena la información de los clientes que realizan compras o solicitan servicios de mantenimiento.

**Campos:**

* **id_cliente (PK):** Identificador único del cliente.
* **nombre:** Nombre completo del cliente.
* **telefono:** Número telefónico de contacto.
* **correo_electronico:** Dirección de correo electrónico.
* **direccion:** Dirección de residencia.

**Justificación:** Centralizar la información del cliente permite mantener un historial de compras y servicios sin duplicar registros.

---

### Vendedor

Contiene la información de los empleados responsables de realizar las ventas.

**Campos:**

* **id_vendedor (PK):** Identificador del vendedor.
* **numero_empleado (UNIQUE):** Código único asignado al empleado.
* **nombre:** Nombre del vendedor.
* **fecha_contratacion:** Fecha de ingreso a la empresa.

**Justificación:** Cada vendedor posee un número de empleado único que facilita su identificación y el seguimiento de su desempeño comercial.

---

### Venta

Registra la información general de cada transacción realizada.

**Campos:**

* **id_venta (PK):** Identificador de la venta.
* **id_cliente (FK):** Cliente que realizó la compra.
* **id_vendedor (FK):** Vendedor responsable de la venta.
* **fecha_venta:** Fecha de la transacción.
* **total:** Valor total de la venta.
* **metodo_pago:** Método de pago utilizado.

**Justificación:** La separación de la información de la venta permite registrar correctamente la relación entre clientes, vendedores y vehículos, además de conservar el historial de transacciones.

---

### Detalle_Venta

Tabla intermedia que relaciona las ventas con los vehículos vendidos.

**Campos:**

* **id_detalle_venta (PK):** Identificador del detalle.
* **id_venta (FK):** Venta asociada.
* **id_vehiculo (FK):** Vehículo vendido.
* **precio_unitario:** Precio del vehículo al momento de la venta.
* **descuento:** Descuento aplicado, si existe.

**Justificación:** Se implementa esta tabla para resolver la relación entre ventas y vehículos, permitiendo que una venta pueda incluir uno o varios vehículos y conservando el precio registrado en el momento de la transacción.

---

### Mantenimiento

Registra todos los servicios realizados a los vehículos.

**Campos:**

* **id_mantenimiento (PK):** Identificador del servicio.
* **id_vehiculo (FK):** Vehículo que recibe el mantenimiento.
* **id_cliente (FK, NULL):** Cliente propietario del vehículo, cuando corresponda.
* **tipo_servicio:** Tipo de mantenimiento realizado.
* **fecha_servicio:** Fecha del servicio.
* **costo:** Valor del mantenimiento.
* **descripcion:** Observaciones o detalles del servicio.

**Justificación:** Se permite que el campo **id_cliente** sea opcional para registrar mantenimientos realizados a vehículos del inventario que aún no han sido vendidos.

## Relaciones entre las tablas

* Un **cliente** puede realizar muchas **ventas** (1:N).
* Un **vendedor** puede registrar muchas **ventas** (1:N).
* Una **venta** puede incluir uno o varios **vehículos**, mediante la tabla **Detalle_Venta**.
* Un **vehículo** puede aparecer en registros de mantenimiento durante su ciclo de vida.
* Un **cliente** puede solicitar múltiples servicios de mantenimiento.

## Restricciones implementadas

* El **VIN** del vehículo debe ser único.
* El **número de empleado** es único para cada vendedor.
* Todas las claves foráneas garantizan la integridad referencial entre las tablas.
* Al registrar una venta, el estado del vehículo cambia a **No disponible** para evitar ventas duplicadas.
* El campo **id_cliente** en la tabla **Mantenimiento** admite valores nulos cuando el servicio corresponde a un vehículo del inventario sin propietario.

## Conclusión

La estructura propuesta facilita la administración integral del concesionario, permitiendo controlar el inventario de vehículos, registrar las ventas, gestionar la información de clientes y vendedores, y mantener un historial completo de los servicios de mantenimiento. El diseño prioriza la integridad de los datos, la reducción de redundancias y la escalabilidad para futuras ampliaciones del sistema.
