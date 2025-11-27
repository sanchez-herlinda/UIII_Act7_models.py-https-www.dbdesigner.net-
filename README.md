# UIII_Act7_models.py-https-www.dbdesigner.net-

<img width="1352" height="627" alt="image" src="https://github.com/user-attachments/assets/c85aa8de-46ae-4211-b4e6-cd117f228586" />


**Sistema de Gestión de Tienda de Mascotas**

| Entidad | Atributos | Tipo de Campo |
|---|---|---|
| **Animal_Tienda** | id_animal, nombre_animal, especie, raza, fecha_nacimiento, genero, precio_venta, estado_salud, fecha_ingreso, chip_identificacion, vacunas_aplicadas | INT, VARCHAR(100), VARCHAR(50), VARCHAR(50), DATE, CHAR(1), DECIMAL(10,2), VARCHAR(50), DATE, VARCHAR(50), TEXT |
| **Producto_Mascota** | id_producto, nombre_producto, descripcion, precio_venta, stock, id_categoria, id_proveedor, para_especie, tamano_animal, marca, fecha_vencimiento | INT, VARCHAR(255), TEXT, DECIMAL(10,2), INT, INT, INT, VARCHAR(50), VARCHAR(50), VARCHAR(100), DATE |
| **Categoria_Mascota** | id_categoria, nombre_categoria, descripcion_categoria, es_comida, es_juguete, aplica_para_especie | INT, VARCHAR(100), TEXT, BOOLEAN, BOOLEAN, VARCHAR(100) |
| **Cliente_Mascota** | id_cliente, nombre, apellido, telefono, email, direccion, fecha_registro, num_mascotas, tipo_mascota_preferido | INT, VARCHAR(100), VARCHAR(100), VARCHAR(20), VARCHAR(100), VARCHAR(255), DATE, INT, VARCHAR(50) |
| **Venta_Tienda_Mascota** | id_venta, fecha_venta, id_cliente, id_empleado, total_venta, metodo_pago, descuento_aplicado, numero_ticket, estado_venta | INT, DATETIME, INT, INT, DECIMAL(10,2), VARCHAR(50), DECIMAL(5,2), VARCHAR(50), VARCHAR(50) |
| **Detalle_Venta_Tienda_Mascota** | id_detalle, id_venta, id_producto, cantidad, precio_unitario_venta, subtotal, iva_aplicado, id_animal_vendido | INT, INT, INT, INT, DECIMAL(10,2), DECIMAL(10,2), DECIMAL(5,2), INT |
| **Empleado_Tienda_Mascota** | id_empleado, nombre, apellido, cargo, dni, fecha_contratacion, salario, turno, telefono, email | INT, VARCHAR(100), VARCHAR(100), VARCHAR(50), VARCHAR(20), DATE, DECIMAL(10,2), VARCHAR(50), VARCHAR(20), VARCHAR(100) |

**Relaciones:**
*   Producto_Mascota (id_categoria) -> Categoria_Mascota (id_categoria) (Muchos a Uno)
*   Venta_Tienda_Mascota (id_cliente) -> Cliente_Mascota (id_cliente) (Muchos a Uno)
*   Venta_Tienda_Mascota (id_empleado) -> Empleado_Tienda_Mascota (id_empleado) (Muchos a Uno)
*   Detalle_Venta_Tienda_Mascota (id_venta) -> Venta_Tienda_Mascota (id_venta) (Muchos a Uno)
*   Detalle_Venta_Tienda_Mascota (id_producto) -> Producto_Mascota (id_producto) (Muchos a Uno)
*   Detalle_Venta_Tienda_Mascota (id_animal_vendido) -> Animal_Tienda (id_animal) (Muchos a Uno)

---
