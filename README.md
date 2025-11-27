# UIII_Act7_models.py-https-www.dbdesigner.net-

<img width="1352" height="627" alt="image" src="https://github.com/user-attachments/assets/c85aa8de-46ae-4211-b4e6-cd117f228586" />



from django.db import models


# ------------------------
#  CATEGORÍA MASCOTA
# ------------------------
class CategoriaMascota(models.Model):
    id_categoria = models.AutoField(primary_key=True)
    nombre_categoria = models.CharField(max_length=100)
    descripcion_categoria = models.TextField(blank=True, null=True)
    es_comida = models.BooleanField(default=False)
    es_juguete = models.BooleanField(default=False)
    aplica_para_especie = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre_categoria


# ------------------------
#  ANIMAL EN TIENDA
# ------------------------
class AnimalTienda(models.Model):

    GENERO_CHOICES = (
        ('M', 'Macho'),
        ('H', 'Hembra'),
    )

    id_animal = models.AutoField(primary_key=True)
    nombre_animal = models.CharField(max_length=100)
    especie = models.CharField(max_length=50)
    raza = models.CharField(max_length=50)
    fecha_nacimiento = models.DateField()
    genero = models.CharField(max_length=1, choices=GENERO_CHOICES)
    precio_venta = models.DecimalField(max_digits=10, decimal_places=2)
    estado_salud = models.CharField(max_length=50)
    fecha_ingreso = models.DateField()
    chip_identificacion = models.CharField(max_length=50)
    vacunas_aplicadas = models.TextField()

    def __str__(self):
        return f"{self.nombre_animal} ({self.especie})"


# ------------------------
#  PRODUCTO PARA MASCOTA
# ------------------------
class ProductoMascota(models.Model):
    id_producto = models.AutoField(primary_key=True)
    nombre_producto = models.CharField(max_length=255)
    descripcion = models.TextField()
    precio_venta = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()

    id_categoria = models.ForeignKey(
        CategoriaMascota,
        on_delete=models.CASCADE,
        related_name='productos'
    )

    # Suponemos que Proveedor será otra tabla más adelante
    id_proveedor = models.IntegerField()

    para_especie = models.CharField(max_length=50)
    tamano_animal = models.CharField(max_length=50)
    marca = models.CharField(max_length=100)
    fecha_vencimiento = models.DateField()

    def __str__(self):
        return self.nombre_producto


# ------------------------
#  CLIENTE
# ------------------------
class ClienteMascota(models.Model):
    id_cliente = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    telefono = models.CharField(max_length=20)
    email = models.CharField(max_length=100)
    direccion = models.CharField(max_length=255)
    fecha_registro = models.DateField()
    num_mascotas = models.IntegerField()
    tipo_mascota_preferido = models.CharField(max_length=50)

    def __str__(self):
        return f"{self.nombre} {self.apellido}"


# ------------------------
#  EMPLEADO
# ------------------------
class EmpleadoTiendaMascota(models.Model):
    id_empleado = models.AutoField(primary_key=True)
    nombre = models.CharField(max_length=100)
    apellido = models.CharField(max_length=100)
    cargo = models.CharField(max_length=50)
    dni = models.CharField(max_length=20)
    fecha_contratacion = models.DateField()
    salario = models.DecimalField(max_digits=10, decimal_places=2)
    turno = models.CharField(max_length=50)
    telefono = models.CharField(max_length=20)
    email = models.CharField(max_length=100)

    def __str__(self):
        return f"{self.nombre} {self.apellido} - {self.cargo}"


# ------------------------
#  VENTA EN TIENDA
# ------------------------
class VentaTiendaMascota(models.Model):

    METODO_PAGO_CHOICES = (
        ('EFECTIVO', 'Efectivo'),
        ('TARJETA', 'Tarjeta'),
        ('TRANSFERENCIA', 'Transferencia'),
    )

    id_venta = models.AutoField(primary_key=True)
    fecha_venta = models.DateTimeField()

    id_cliente = models.ForeignKey(
        ClienteMascota,
        on_delete=models.CASCADE,
        related_name='ventas'
    )

    id_empleado = models.ForeignKey(
        EmpleadoTiendaMascota,
        on_delete=models.CASCADE,
        related_name='ventas'
    )

    total_venta = models.DecimalField(max_digits=10, decimal_places=2)
    metodo_pago = models.CharField(max_length=50, choices=METODO_PAGO_CHOICES)
    descuento_aplicado = models.DecimalField(max_digits=5, decimal_places=2)
    numero_ticket = models.CharField(max_length=50)
    estado_venta = models.CharField(max_length=50)

    def __str__(self):
        return f"Venta #{self.id_venta} - {self.fecha_venta.date()}"


# ------------------------
#  DETALLE DE VENTA
# ------------------------
class DetalleVentaTiendaMascota(models.Model):
    id_detalle = models.AutoField(primary_key=True)

    id_venta = models.ForeignKey(
        VentaTiendaMascota,
        on_delete=models.CASCADE,
        related_name='detalles'
    )

    id_producto = models.ForeignKey(
        ProductoMascota,
        on_delete=models.CASCADE,
        related_name='ventas_detalle'
    )

    id_animal_vendido = models.ForeignKey(
        AnimalTienda,
        on_delete=models.SET_NULL,
        null=True, blank=True,
        related_name='ventas_detalle'
    )

    cantidad = models.IntegerField()
    precio_unitario_venta = models.DecimalField(max_digits=10, decimal_places=2)
    subtotal = models.DecimalField(max_digits=10, decimal_places=2)
    iva_aplicado = models.DecimalField(max_digits=5, decimal_places=2)

    def __str__(self):
        return f"Detalle {self.id_detalle} de Venta {self.id_venta.id_venta}"






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
