<div style="font-size: 12px; line-height: 1.35;">

--------------------------------
# Base de datos: Precios Claros - Base SEPA
--------------------------------

SEPA (Sistema Electrónico de Publicidad de Precios Argentinos) reúne los precios de comercios minoristas (grandes establecimientos) de más de 70 mil productos en todo el país, lo que de forma agregada genera una base diaria de aproximadamente 12 millones de registros. https://www.preciosclaros.gob.ar/terminos_y_condiciones.html Contactanos por cualquier tipo consulta, sugerencia y/o reclamo (ej: datos provistos por comercios no fidedignos) al e-mail soportesepa@comercio.gob.ar

</div>

<div style="font-size: 12px; line-height: 1.35;">

## Archivos del paquete

| Nombre | Requerido | Descripción |
|---|---:|---|
| `comercio.csv` | Sí | Datos fundamentales del comercio y las banderas que lo componen. El archivo tendrá tantas filas como banderas tenga el comercio. A su vez, establece la fecha de la última actualización del esquema de datos y la versión de la especificación bajo el cual se publica el paquete. |
| `sucursales.csv` | Sí | Lista de las sucursales del comercio. Los portales web de comercio electrónico deben informarse como una sucursal. |
| `productos.csv` | Sí | Lista de los precios de productos comercializados por el comercio. |

</div>

<div style="font-size: 12px; line-height: 1.35;">

## `comercio.csv`

| Campo | Requerido | Descripción |
|---|---:|---|
| `id_comercio` | Sí | El campo `id_comercio` es un identificador único otorgado por la Secretaría de Comercio al momento de dar de alta el mismo. Ejemplo: `"123"` |
| `id_bandera` | Sí | El campo `id_bandera` es un identificador único de una bandera (unidad de negocio, nombre comercial o marca independiente) perteneciente al comercio. En el caso de que el comercio tenga una sola bandera, el `id_bandera` es el número `"1"`. En el caso de que el comercio tenga más de una bandera, el primer id es `"1"` y los siguientes son incrementales (`"1"`, `"2"`, `"3"`, etc.). Ejemplo: `"1"` |
| `comercio_cuit` | Sí | El campo `comercio_cuit` es un número que identifica unívocamente al comercio ante la Administración Federal de Ingresos Públicos (A.F.I.P.). Deberá expresarse como un número sin guiones (C.U.I.T.: Clave Única de Identificación Tributaria). Ejemplo: `"30123456787"` |
| `comercio_razon_social` | Sí | El campo `comercio_razon_social` es un texto que contiene a la razón social del comercio registrada ante la Administración Federal de Ingresos Públicos (AFIP). Ejemplo: `"Supermercado S.R.L."` |
| `comercio_bandera_nombre` | Sí | El campo `comercio_bandera_nombre` es un texto que contiene el nombre comercial utilizado por el comercio para identificar un tipo de formato de comercialización propio. En el caso de que el comercio tenga una sola bandera, el `comercio_bandera_nombre` es igual al nombre de fantasía del comercio. Ejemplo: `"El Rey del Super"` |
| `comercio_bandera_url` | Sí | El campo `comercio_bandera_url` contiene un link a la propiedad digital principal de la bandera. La URL debe incluir el protocolo de conexión. Ejemplo: `"http://www.el-rey-del-super.com.ar"` |
| `comercio_ultima_actualizacion` | Sí | El campo `comercio_ultima_actualizacion` es un texto que contiene la fecha y hora de la última actualización de los archivos del paquete SEPA. El texto debe seguir el estándar ISO 8601 que respeta el siguiente formato: `YYYY-MM-DDTHH:MM:SS[.mmmmmmm][+HH:MM]`. Ejemplo: `"2016-02-29T14:04:00-03:00"` |
| `comercio_version_sepa` | Sí | El campo `comercio_version_sepa` es un número que indica la versión del modelo de datos del paquete SEPA bajo la cual se publican los 3 archivos CSV. Ejemplo: `"1.0"` |

---

## `sucursales.csv`

| Campo | Requerido | Descripción |
|---|---:|---|
| `id_comercio` | Sí | El campo `id_comercio` es un identificador único otorgado por la Secretaría de Comercio Interior al momento de dar de alta el mismo. Ejemplo: `"123"` |
| `id_bandera` | Sí | El campo `id_bandera` es un identificador único de una bandera (unidad de negocio, nombre comercial o marca independiente) perteneciente al comercio. En el caso de que el comercio tenga una sola bandera, el `id_bandera` es el número `"1"`. En el caso de que el comercio tenga más de una bandera el primer id es `"1"` y los siguientes son incrementales (`"1"`, `"2"`, `"3"`, etc.). Ejemplo: `"1"` |
| `id_sucursal` | Sí | El campo `id_sucursal` es un identificador único de la sucursal. El mismo es un código interno propio del comercio y debe ser estable a lo largo del tiempo. Lo indicado en este apartado también será de aplicación para las sucursales web. Ejemplo: `"7"` |
| `sucursales_nombre` | Sí | El campo `sucursales_nombre` es un nombre por el cual se reconoce a la sucursal. Ejemplo: `"Sucursal Pompeya"` o `"Minimercado La Nueva Argentina"` |
| `sucursales_tipo` | Sí | El campo `sucursales_tipo` es una categorización de la sucursal según las características de su línea de cajas, definido por la cantidad de líneas de caja: **Hipermercado**: más de 15 cajas. **Supermercado**: entre 4 y 15 cajas. **Autoservicio**: entre 1 y 3 cajas. **Tradicional**: Mostrador sin línea de caja. **Web**. Ejemplo: `"Hipermercado"` |
| `sucursales_calle` | Sí | El campo `sucursales_calle` es el nombre de la calle donde se encuentra la puerta principal de la sucursal. El campo se debe encontrar normalizado (sólo debe incluir el nombre de la calle, sin número, sin pisos, sin observaciones, etc.). Para la sucursal web, indicar el nombre del sitio web de compra online. Ejemplo: `"La Nueva Argentina Online"` |
| `sucursales_numero` | Sí | El campo `sucursales_numero` es la numeración válida de la puerta principal de la sucursal (sólo debe incluir un número, sin calle, sin piso, sin observaciones, etc.). En caso de que el domicilio no posea número este campo quedará vacío. Ejemplo: `"250"`. Para la sucursal web, este campo quedará vacío. |
| `sucursales_latitud` | Sí | El campo `sucursales_latitud` corresponde a la latitud geográfica de la puerta principal de la sucursal, expresada de acuerdo al sistema de referencia WGS84. Este valor se expresará en números, redondeando en seis decimales y utilizando punto (`"."`) como separador de decimales. Ejemplo: `"-34,603722"`. Para la sucursal web, este campo quedará vacío. |
| `sucursales_longitud` | Sí | El campo `sucursales_longitud` corresponde a la longitud geográfica de la puerta principal de la sucursal, expresada de acuerdo al sistema de referencia WGS84. Este valor se expresará en números, redondeando en seis decimales y utilizando punto (`"."`) como separador de decimales. Ejemplo: `"-58,381592"`. Para la sucursal web, este campo quedará vacío. |
| `sucursales_observaciones` | No | El campo `sucursales_observaciones` es un texto que contiene observaciones adicionales sobre la localización de la sucursal, cuando estas sean necesarias o útiles para su fácil localización. Ejemplo: una sucursal dentro de un centro comercial podría incluir aquí indicaciones como el número de local, el piso, el nombre del centro comercial y otras (`"Local nro. 123 – Piso 4 - Centro Comercial Nuevo Shopping - Contiguo al local A"`). Este es un campo de texto libre, pero se recomienda utilizar guiones medios `"-"` para separar las indicaciones. Para la sucursal web, este campo debe ser completado con la página web de la bandera. |
| `sucursales_barrio` | No | El campo `sucursales_barrio` contiene el nombre del barrio o subdivisión de la localidad donde se encuentre ubicada la sucursal. Ejemplo: `"Villa Lugano"`. Para la/s sucursal/es web, también se deberá especificar el barrio correspondiente. |
| `sucursales_codigo_postal` | Sí | El campo `sucursales_codigo_postal` es un número que corresponde al código postal de la sucursal, sin letras. Ejemplo: `1426` (en lugar de `"C1426BMD"`). Para la/s sucursal/es web, también se deberá especificar el código correspondiente. |
| `sucursales_localidad` | Sí | El campo `sucursales_localidad` contiene la localidad donde se encuentra ubicada la sucursal. Ejemplo: `"Mar de Ajó"`. Para la/s sucursal/es web, también se deberá especificar la localidad correspondiente. |
| `sucursales_provincia` | Sí | El campo `sucursales_provincia` contiene la provincia donde se encuentra ubicada la sucursal. La nomenclatura a utilizar será la indicada por la norma ISO 3166-2. Ejemplo: `"AR-B"` identifica a la provincia de Buenos Aires. Para la/s sucursal/es web, también se deberá especificar la provincia correspondiente. |
| `sucursales_lunes_horario_atencion` | Sí | El campo `sucursales_lunes_horario_atencion` describe los horarios de atención de la sucursal el día lunes. La hora será expresada de 00:00 a 24:00 HS atendiendo al siguiente formato `HH:mm`. En los casos que el horario de atención sea corrido se completará hora de apertura a hora de cierre siguiendo el siguiente formato `"HH:mm a HH:mm"`. Para los casos en que se trabaje en horario cortado se separarán los turnos por un espacio guión medio espacio (`" - "`), es decir, `"HH:mm a HH:mm - HH:mm a HH:mm"`. En los casos que el comercio se encuentre cerrado el día lunes se debe completar el campo con la palabra `"cerrado"`. Las horas deben tener CINCO (5) dígitos en formato `HH:mm`. No se deben incluir espacios dentro del texto que especifica una hora y los números que tengan menos de DOS (2) dígitos deben estar precedidos por un cero (`"08"` en lugar de `"8"`). Los valores nulos o faltantes se interpretarán como errores en la información. Para la sucursal web, este campo quedará vacío. |
| `sucursales_martes_horario_atencion` | Sí | Mismo criterio de formato y validación que `sucursales_lunes_horario_atencion`, aplicado al día martes. |
| `sucursales_miercoles_horario_atencion` | Sí | Mismo criterio de formato y validación que `sucursales_lunes_horario_atencion`, aplicado al día miércoles. |
| `sucursales_jueves_horario_atencion` | Sí | Mismo criterio de formato y validación que `sucursales_lunes_horario_atencion`, aplicado al día jueves. |
| `sucursales_viernes_horario_atencion` | Sí | Mismo criterio de formato y validación que `sucursales_lunes_horario_atencion`, aplicado al día viernes. |
| `sucursales_sabado_horario_atencion` | Sí | Mismo criterio de formato y validación que `sucursales_lunes_horario_atencion`, aplicado al día sábado. |
| `sucursales_domingo_horario_atencion` | Sí | Mismo criterio de formato y validación que `sucursales_lunes_horario_atencion`, aplicado al día domingo. |
---

## `productos.csv`

| Campo | Requerido | Descripción |
|---|---:|---|
| `id_comercio` | Sí | El campo `id_comercio` es un identificador único otorgado por la Secretaría de Comercio Interior al momento de dar de alta el mismo. Ejemplo: `"123"` |
| `id_bandera` | Sí | El campo `id_bandera` es un identificador único de una bandera (unidad de negocio, nombre comercial o marca independiente) perteneciente al comercio. En el caso de que el comercio tenga una sola bandera, el `id_bandera` es el número `"1"`. En caso de que el comercio tenga más de una bandera el primer id es `"1"` y los siguientes son incrementales (`"1"`, `"2"`, `"3"`, etc.). Ejemplo: `"1"` |
| `id_sucursal` | Sí | El campo `id_sucursal` es un identificador único de la sucursal. El mismo es un código interno propio del comercio y debe ser estable a lo largo del tiempo. Ejemplo: `"7"` |
| `id_producto` | Sí | El campo `id_producto` es un identificador único del producto. El mismo es el código EAN (GTIN 8 o GTIN 13), UPC-A (GTIN 12) o un código interno propio del comercio de TRECE (13) dígitos de largo, cuando ninguno de los anteriores sea de posible aplicación. En el caso de que el comercio deba utilizar un código propio para la identificación del producto, este debe ser estable en el tiempo, único para todas las sucursales del comercio y tener TRECE (13) dígitos de largo, incorporando ceros a la izquierda de ser necesario. Ejemplo: (A) `"0779027200550"` (B) `"0000000000001"` |
| `productos_ean` | Sí | El campo `productos_ean` define si el valor en el campo `id_producto` corresponde al estándar EAN (GTIN 8 o GTIN 13), UPC-A (GTIN 12) —en cuyo caso adopta el valor `"1"`— o es un identificador único del comercio para ese producto (en cuyo caso adopta el valor `"0"`). Ejemplo: `"0"` |
| `productos_descripcion` | Sí | El campo `productos_descripcion` es una descripción completa del producto, que incluye toda la información necesaria para que un Consumidor lo identifique unívocamente (descripción, marca, presentación, cantidad, unidad de medida, etc.). Ejemplo: `"Aceite oliva Freir aerosol 120ml"` |
| `productos_cantidad_presentacion` | Sí | El campo `productos_cantidad_presentacion` contiene la cantidad de producto que se vende en función de su presentación de venta en términos de su `"unidad de medida"`. En el caso de que el producto se venda a granel (Ej.: papas a granel que el Consumidor debe pesar en la balanza) la cantidad de presentación será `"1"`. Ejemplo: `"120"` |
| `productos_unidad_medida_presentacion` | Sí | El campo `productos_unidad_medida_presentacion` contiene la unidad de medida en la que se expresa la cantidad de producto vendido en sus diferentes presentaciones. Volumen: l (litro), hl (hectolitro), ml (mililitro), m3 (metro cúbico), cm3 (centímetro cúbico). Peso: kg (kilogramo), gr o g (gramo). Longitud: m (metro), dm (decímetro), cm (centímetro). Otros: unidad, paquete. Ejemplo: `"ml"` |
| `productos_marca` | Sí | El campo `productos_marca` contiene el nombre comercial de la marca del producto. Ejemplo: `"Freir"` |
| `productos_precio_lista` | Sí | El campo `productos_precio_lista` indica el valor del producto para la venta al consumidor final, sin considerar deducciones por descuentos de ningún tipo. Este valor debe representar aquel precio al que cualquier consumidor puede adquirir el producto sin ningún tipo de descuento, con cualquier medio de pago aceptado por el comercio, sin utilizar ninguna tarjeta de fidelidad, sin considerar segmentación del consumidor (Ej.: jubilado, estudiante, etc.) y sin consideraciones específicas de ningún tipo. Este valor se expresará en números, redondeando en dos decimales y utilizando punto (`"."`) como separador de decimales. Ejemplo: `"111.66"` |
| `productos_precio_referencia` | Sí | El campo `productos_precio_referencia` indica el valor del producto según la unidad de medida de referencia, es decir, precio por `"lt"` (litro), precio por `"kg"` (kilogramo) o por `"m"` (metro), entre otros (ver detalle en la descripción del campo `productos_unidad_medida`). Este valor se expresará en números, redondeando en dos decimales y utilizando punto (`"."`) como separador de decimales. Ejemplo: `"200.00"` |
| `productos_cantidad_referencia` | Sí | El campo `productos_cantidad_referencia` contiene la cantidad de producto en la que se expresa su precio según la unidad de medida de referencia. Ejemplo: `"1"` o `"100"` |
| `productos_unidad_medida_referencia` | Sí | El campo `productos_unidad_medida_referencia` contiene la unidad de medida de referencia. Volumen: l (litro), hl (hectolitro), ml (mililitro), m3 (metro cúbico), cm3 (centímetro cúbico). Peso: kg (kilogramo), gr o g (gramo). Longitud: m (metro), dm (decímetro), cm (centímetro). Otros: unidad, paquete. Ejemplo: `"l"` |
| `productos_precio_unitario_promo1` | No | El campo `productos_precio_unitario_promo1` indica el precio de la unidad de producto sujeta a la promoción número 1, indicado en la Disposición como `"Precio unitario correspondiente a promoción de alcance general"`. Este precio sólo hace referencia a aquellos descuentos que se aplican al producto sin estar sujetos a segmentación del consumidor (Ej.: jubilados, estudiantes, empleado Empresa `"X"` etc.), medio de pago que utilice o a las tarjetas de fidelización que pueda poseer. En este campo se permitirá consignar precios con descuentos (A) por producto o (B) por categoría de producto o (C) por marca o (D) por cantidad, todas condiciones excluyentes entre sí. Este valor se expresará en números, redondeando en dos decimales y utilizando punto (`"."`) como separador de decimales. Ejemplo: `"73.70"` |
| `productos_leyenda_promo1` | No | El campo `productos_leyenda_promo1` es un texto que describe la promoción vigente que da lugar al precio indicado en el campo `productos_precio_unitario_promo1`. La misma debe incluir la fecha de vigencia de la promoción y el stock de unidades disponibles en caso de corresponder. Ejemplo: `"3x2 - Lleva 3 productos y paga 2 - Vigencia: Desde el 24/12/16 Hasta el 31/12/16 - Stock: Hasta agotar las 100.000 unidades"` |
| `productos_precio_unitario_promo2` | No | El campo `productos_precio_unitario_promo2` indica el valor de la unidad de producto sujeta a la promoción número 2, indicado en la Disposición como `"Precio unitario correspondiente a promoción especial"`. En este campo se permitirá consignar precios con descuentos que se aplican al producto por una o más condiciones de alcance general (por producto, por categoría de producto, por marca o por cantidad) o particular (tarjeta de fidelidad, medios de pago, segmentación del consumidor -jubilados, estudiantes, etc.-). Este valor se expresará en números, redondeando en dos decimales y utilizando punto (`"."`) como separador de decimales. Ejemplo: `"78.16"` |
| `productos_leyenda_promo2` | No | El campo `productos_leyenda_promo2` es un texto que describe la promoción vigente que da lugar al precio producido por `productos_precio_unitario_promo2`. La misma debe incluir la fecha de vigencia, las condiciones a cumplir para efectivizar el precio informado como `"Especial"` y el stock de unidades disponibles en caso de corresponder. Ejemplo: `"50% de descuento en la segunda unidad, más 15% de descuento con tarjeta fidelidad". Vigencia: Desde el 24/12/2016 Hasta el 31/12/2016. Stock: Hasta agotar las 100.000 unidades.` |

</div>


<div style="font-size: 12px; line-height: 1.35;">

## Campos compartidos entre archivos

| Campo | comercio.csv | sucursales.csv | productos.csv |
|---|---:|---:|---:|
| `id_comercio` | ✓ | ✓ | ✓ |
| `id_bandera` | ✓ | ✓ | ✓ |
| `id_sucursal` |  | ✓ | ✓ |

</div>

# *Códigos de provincia*

```tsv

sucursales_provincia	provincia
0	AR-C	CABA
1	AR-B	Buenos Aires
2	AR-K	Catamarca
3	AR-H	Chaco
4	AR-U	Chubut
5	AR-X	Córdoba
6	AR-W	Corrientes
7	AR-E	Entre Ríos
8	AR-P	Formosa
9	AR-Y	Jujuy
10	AR-L	La Pampa
11	AR-F	La Rioja
12	AR-M	Mendoza
13	AR-N	Misiones
14	AR-Q	Neuquén
15	AR-R	Río Negro
16	AR-A	Salta
17	AR-J	San Juan
18	AR-D	San Luis
19	AR-Z	Santa Cruz
20	AR-S	Santa Fe
21	AR-G	Santiago del Estero
22	AR-V	Tierra del Fuego
23	AR-T	Tucumán
```