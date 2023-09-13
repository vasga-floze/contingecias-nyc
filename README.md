# CONTINGENCIAS
## Script para reporte de ventas de libro de consumidor final. (Para nuevo formato de Hacienda)
```SELECT FECHA 'FECHA DE EMISIÓN',REPLACE(DOC_FISCAL,'-','') RESOLUCION,CLAVE_REFERENCIA_DE SERIE,REPLACE(CLAVE_DE,'-','') 'NÚMERO DE DOCUMENTO (DEL)',
REPLACE(CLAVE_DE,'-','') 'NÚMERO DE DOCUMENTO (AL)',TOTAL_FACTURA 'VENTAS GRAVADAS LOCALES'
 FROM CANNYSHOP.FACTURA WHERE FECHA BETWEEN '20230801' AND '20230831'
 ORDER BY FECHA,USUARIO,RESOLUCION

 --SELECT FECHA,MIN(DOC_FISCAL) DESDE,MAX(DOC_FISCAL) HASTA,SUM(TOTAL_IMPUESTO1) IVA,SUM(TOTAL_MERCADERIA) NETO,SUM(TOTAL_FACTURA) TOTAL
 --FROM CANNYSHOP.FACTURA WHERE FECHA BETWEEN '20230801' AND '20230831'
 --GROUP BY FECHA ,USUARIO
 --ORDER BY 1
```
## COMO ACTUALIZAR UN CIERRE POR FALTA DE INGRESO DE LA VENTA (PARA CORREGIR DIFERENCIA)
##### Consultar la tabla CIERRE_DET_PAGO :
Cambiar el nombre de la base de datos por la base correcta [T_SANMIGUEL1]. En el where se debe modificar la fecha (NO cambiar la hora).  La fecha debe ser del día que se desea corregir.
```
--En el where se debe modificar la fecha (NO cambiar la hora)
SELECT TOP (1000) * 
FROM [T_SANMIGUEL1].[CONSNY].[CIERRE_DET_PAGO] --Cambiar [T_SANMIGUEL1] por el nombre que tenga la base de datos de la tienda
WHERE CreateDate BETWEEN '2023-09-12 00:00:00.000' AND '2023-09-12 23:59:00.000';
```
##### Actualizar la tabla: CIERRE_DET_PAGO, el campo: DIFERENCIA
* Cambiar el nombre de la base de datos por la base correcta [T_SANMIGUEL1]. En el where se debe modificar la fecha (NO cambiar la hora). La fecha debe ser del día que se desea corregir.
* PONERLE EL WHERE A LA CONSULTA - Y CAMBIAR EL VALOR QUE SE DESEA PARA LA DIFERENCIA
```
--Modificarle la diferencia a la cantidad que se necesita. 
UPDATE [T_SANMIGUEL1].[CONSNY].[CIERRE_DET_PAGO] SET DIFERENCIA = 0.00000000 
WHERE CreateDate BETWEEN '2023-09-12 00:00:00.000' AND '2023-09-13 00:00:00.000';
```
##### Actualizar la tabla: CIERRE_DET_PAGO, el campo: TOTAL_USUARIO
```
-- NO OLVIDAR EL WHERE EN LA CONSULTA -> 
UPDATE [T_SANMIGUEL1].[CONSNY].[CIERRE_DET_PAGO] SET TOTAL_USUARIO = 721.43000000 
WHERE CreateDate BETWEEN '2023-09-12 00:00:00.000' AND '2023-09-12 23:59:00.000';

```
> Ejemplo: (Los datos marcados en rojo son los que se deben modificar de acuerdo a las necesidades de la tienda)
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/e64fc879-1f09-44e2-91bd-ea3fe1304bb2)

#### Consultar la tabla CIERRE_POS:
```
SELECT TOP (1000) *
FROM [T_SANMIGUEL1].[CONSNY].[CIERRE_POS] 
WHERE FECHA_HORA BETWEEN '2023-09-12 00:00:00.000' AND '2023-09-12 23:59:00.000';
```

```
UPDATE [T_SANMIGUEL1].[CONSNY].[CIERRE_POS] SET TOTAL_DIFERENCIA = 0.00000000  WHERE FECHA_HORA BETWEEN '2023-09-12 00:00:00.000' AND '2023-09-12 23:59:00.000';
```
> Ejemplo: (Los datos marcados en rojo son los que se deben modificar de acuerdo a las necesidades de la tienda)
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/905aee52-ef25-41b1-9647-e5f1cc11ae1b)

![descarga](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/e6a78b4c-f4c7-4f00-a62a-3cd858fabc63)

# COMO CREAR USUARIOS PARA CONEXION REMOTA
Para que los usuarios conectados en la red puedan acceder al escritorio remoto, deben existir los usuarios en Active Directory.
#### Los pasos para crear usuarios o verificar que existen los usuarios:
* Paso 1: Ingresar  al Administrador del Servidor y luego al centro de administración de active directory
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/1643eb4a-0bc7-4a23-8055-871bda4e74a6)

* Paso 2: Crear Nuevo en Centro de Administración de Active Directory.
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/8598ea9b-6eea-4339-a834-014d93c59f31)

* Paso 3: Completar los campos marcados en los cuadros rojos:
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/726d25ae-7ff4-4b94-916b-93c644c474c1)

  
* Paso 4: Vincular al usuario como miembro de un grupo(ver el proceso en orden de las siguientes capturas de pantalla):
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/1053aacc-7dc0-43bc-8b35-61ae854d1f77)
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/ee4a0db9-bcf5-44f6-a7c8-c743254679f1)

  En este punto, una vez identificado el grupo de conexión remota, agregar y dar aceptar a las ventanas.
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/4c097cd7-eff8-460a-a0c8-cf3a98cc3494)


 Ya se debería poder hacer el ingreso con ese usuario usando el escritorio remoto. REPETIR EL PROCESO PARA TODOS LOS USUARIOS NECESARIOS.


## COMO CARGAR ARCHIVOS PARA SINCRONIZACION DE TIENDAS
Para poder hacer la carga de datos para la sincronización, es necesario tener:
  - La computadora HP color azul (Antes asignada a Pablo, debe ser en esa porque ahí están instalados los programas que se usan).
  - El excel con la información extraída de las tiendas. (Eduardo saca esa información de las tiendas, Pablo revisa que esté bien)
  - Tener abierto el programa que se usa para la carga de ese excel. El programa se encuentra en la siguiente ruta: ``` Documentos > Visual Studio 2010 > Projects > BULKTEST > BULKTEST > bin > Debug```
    ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/a9c1cc56-dbaf-4dbf-853b-f6d05e67ce4e)

  - Se abre ese ejecutable que está señalando en la captura anterior y se configura de la siguiente manera:
    
    ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/94d663c9-cc1c-4856-b506-abb0911b22b1)

Normalmente el Excel que se va a cargar tiene 4 hojas: **DP, DPL, PP y AP**. (*Para poder cargarlo se debe tener cerrado el Excel, porque a veces se bloquea*).

 #### Cada una tiene la estructura de una tabla de la base de datos (Prefijo la Hoja de Excel y Nombre de la tabla):
> * DP=DOCUMENTO_POS ---> Esta debe ser SIEMPRE la primera hoja en cargarse
> * DPL=DOC_POS_LINEA
> * PP=PAGO_POS
> * AP=AUXILIAR_POS

  Se va cargando una hoja a la vez, por ejemplo:
  
 > ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/74bacd9d-4f31-4972-acb5-8f9be5c3c550)
 - Luego se debe dar clic en el botón cargar para que habilite el campo para la tabla en la que se va a cargar.
 - Se debe relacionar con la información de prefijo y tabla:
     * DP=CONSNY.DOCUMENTO_POS
     * DPL=CONSNY.DOC_POS_LINEA
     * PP=CONSNY.PAGO_POS
     * AP=CONSNY.AUXILIAR_POS
 - Y luego dar clic en procesar.
> ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/99ea9e51-6e01-45c3-8e48-f3e8bad4078a)
 - Esto no se debe tocar (lo marcado en el cuadro rojo):
> ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/3a4ff644-28db-4465-a227-bb7c01c2002c)
-  Luego se debe hacer el resto de hojas siguiendo el mismo procedimiento, realizado en esta sección. Siempre con el esquema CONSNY.
- **Nota: Cuando son todas las tiendas, DPL se tarda hasta 10 minutos**

#### Despues de haber cargado la información se debe hacer la comprobación
- Para ello se debe ejecutar el script: 
```
USE EXIMP600;

DECLARE @StartDate DATETIME = '2023-08-01 00:00';
DECLARE @EndDate DATETIME = '2023-08-31 23:59';

SELECT
    CASE
        WHEN E.TDA = 'BCSM' THEN 'SM4'
        ELSE E.TDA
    END AS TDA,
    E.FECHA,
    CONVERT(DECIMAL(18, 2), SUM(TOTAL_PAGAR)) MONTO,
    E.CAJA,
    'TICKET' AS TIPO
FROM
(
    SELECT consny.DOCUMENTO_POS.DOCUMENTO,
           LEFT(consny.DOCUMENTO_POS.DOCUMENTO, 3) AS TDA,
           consny.DOCUMENTO_POS.TIPO,
           consny.DOCUMENTO_POS.CAJA,
           consny.DOCUMENTO_POS.CORRELATIVO,
           RIGHT(consny.DOCUMENTO_POS.DOCUMENTO, 6) AS NUMERO,
           CASE consny.DOCUMENTO_POS.TIPO
               WHEN 'D' THEN TOTAL_PAGAR * -1
               ELSE TOTAL_PAGAR
           END AS TOTAL_PAGAR,
           CASE
               WHEN consny.DOCUMENTO_POS.documento LIKE '%R1%' THEN 'R1'
               WHEN consny.DOCUMENTO_POS.documento LIKE '%R2%' THEN 'R2'
               WHEN consny.DOCUMENTO_POS.documento LIKE '%R3%' THEN 'R3'
               WHEN consny.DOCUMENTO_POS.documento LIKE '%R4%' THEN 'R4'
               WHEN consny.DOCUMENTO_POS.documento LIKE '%R5%' THEN 'R5'
               ELSE 'FAC'
           END AS RESOLUCION,
           CONVERT(DATE, consny.DOCUMENTO_POS.FCH_HORA_COBRO) AS FECHA,
           consny.DOCUMENTO_POS.CAJERO AS TIENDA,
           consny.CAJA_POS.BODEGA,
           consny.DOCUMENTO_POS.NUM_CIERRE,
           LEFT(consny.CAJA_POS.BODEGA, 1) AS EMPRESA,
           MONTH(consny.DOCUMENTO_POS.FCH_HORA_COBRO) AS MES
    FROM consny.DOCUMENTO_POS
    INNER JOIN consny.CAJA_POS ON consny.DOCUMENTO_POS.CAJA = consny.CAJA_POS.CAJA AND consny.DOCUMENTO_POS.CAJA_COBRO = consny.CAJA_POS.CAJA
    WHERE (consny.DOCUMENTO_POS.FCH_HORA_COBRO BETWEEN @StartDate AND @EndDate) AND (consny.DOCUMENTO_POS.ESTADO_COBRO = 'C')
) AS E
GROUP BY
    CASE
        WHEN E.TDA = 'BCSM' THEN 'SM4'
        ELSE E.TDA
    END,
    E.FECHA,
    e.caja

UNION ALL

SELECT USUARIOBODEGA.PREFIJODOC, CUADRO_VENTA.FECHA, CUADRO_VENTA.MONTO_SISTEMA, CUADRO_VENTA.CAJA, 'CUADRO'
FROM CUADRO_VENTA
INNER JOIN USUARIOBODEGA ON CUADRO_VENTA.BODEGA = USUARIOBODEGA.BODEGA AND CUADRO_VENTA.CAJA = USUARIOBODEGA.CAJA
WHERE (CUADRO_VENTA.FECHA BETWEEN @StartDate AND @EndDate);
```
- En el script hay que cambiar los rangos de fecha del primer día al último día del mes que se está cargando.
- Una vez ejecutada la query se debe copiar con encabezados el resultado desde SQL a Excel.
- Se debe armar una tabla dinámica con esos datos, se agrega:
  -  Tienda y fecha como Filas.
  -  Tipo como columna.
  -  Monto como valores.
- Luego se cambia el formato tabular:
> * ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/e76779a3-3f25-4d9f-b735-f53c1784fa7a)
> * Y que no muestre totales.
> * ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/11ed029a-505c-46ae-a6ff-803f4b4e81a4)
> * También dar la opción de repetir todas las etiquetas:
> * ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/e6e6bce8-4db4-43ba-9315-5ebb7f5e42d1)
- Luego para verificar se filtra por las tiendas que se han cargado, y se debe verificar que las columnas cuadro y ticket coincidan en la información.
- Si en la columna de ticket falta el dato, quiere decir que esa información no se ha sacado de la tienda todavía.


## COMO CORREGIR ERROR 
  * ### Falló el SetLocation: No se ha podido encontrar la tabla 'R_ASIENT'
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/a92cebd8-a26b-49d5-9eb0-f17278c01fde)

  #### Contexto: Puede suceder al querer previsualizar un reporte que se desea imprimir.
* **Solución**:
    * Buscar la carpeta que contiene el instalador de softland.
     ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/bed771c3-ba63-422e-bff7-5b3001a0fa73)
    * Buscar un exe que se llama Deploy Crystal y reinstalarlo.
      *  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/8821c87f-a698-4e2a-8e34-b8adb0143a5a)
      *  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/f51b76e6-b504-4d81-86c5-2fd4eba6c73d)
    * Probar nuevamente a previsualizar el reporte, si no reiniciar la computadora e intentar nuevamente.

## COMO HACER RETACEOS
- **Contexto inicial** , de gerencia financiera viene un archivo, que hasta que alguien de contabilidad revisa se puede procesar.
- Cada pestaña del Excel corresponde a una compra de un contenedor, se supone que todos son de NYC pero hay que revisar los encabezados porque a veces hay de Carisma.
- Alguien de contabilidad lo revisa y corrige, luego lo regresa con los valores de los gastos corregidos.
- Hasta ese punto los nombres de los paquetes van en inglés y sin código.
- Lo primero es identificar qué es, colocarle el código que le corresponde con su respectiva descripción.
- Se puede tomar como base o idea el último correo correcto(mayo) enviado por Pablo desde la bandeja de salida.  Ahí están unas cosas que hacen unos cálculos para validar lo que de contabilidad mandan, aunque la validación se hace con un porcentaje, no es infalible pero da un parámetro.
- La información de esos artículos está en la compañía NYC (no confundir con NEwYORK)... Si se encontrara algo que no está ahí, hay que crearle código nuevo.
- Lo recibido de contabilidad se pone en la hoja RESUMEN del archivo anual, para que esté consolidado porque de repente hay que cruzarlo con contabilidad y es mejor ya tenerlo así, y si todo está bien lo voy separando en hojas individuales según el número de retaceo. parecido a como gerencia financiera lo manda, pero con otro orden y en español
- En el archivo se puede ver que van hojas de colores por grupo. Cada grupo es un mes. Hay que ver los encabezados de cada hoja para ver que tengan el período correcto y el número de factura, ese número sale del encabezado del archivo que manda gerencia financiera.
- Para crear un nuevo mes, sale más práctico copiar alguna de las hojas que ya está y se limpia para volverlo a llenar con la nueva información.
> - *Un tip: borrar el contenido debajo del encabezado de la tabla sin llegar al total.*
- *Y al final ordenar por la columna D (cantidad) de menor a mayor*

## COMO GENERAR REPORTE PARA CUMPLIMIENTO


## COMO CARGAR ORDENES DE COMPRA Y EMBARQUE POR CCF
### Orden de Compra
- Se tiene una plantilla de excel que en la primera hoja contiene el desglose de los fardos facturados. Se debe identificar las líneas que corresponden a cada CCF, para ir realizando las ordenes de compra por cada CCF. (Por ahora se debe filtrar por cada artículo y fecha, para comparar con los CCF que sí esten cuadrando).
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/3bde2e79-4481-4770-9464-0462987139b8)
- Luego de haber identificado todas las líneas que corresponden a un CCF se debe completar la información en la segunda hoja(LOTE), se van a modificar las columnas que en su encabezado se han rellenado con color amarillo. La información que se va a reemplazar en dichas columnas está en la primera hoja.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/99800265-2b86-40a3-9c07-f3a7690bd546)
- Al finalizar la hoja LOTE, se debe abrir SQLSMS y buscar en la base de datos: SOFTLAND, una tabla con el nombre: **CANNYSHOP.LOTE**, para insertar los registros directamente de la hoja de excel (sin los encabezados) a la tabla de la base de datos. **Es posible que aparezca un error de duplicados al insertar, pero no es problema, solo se puede dar en aceptar a la ventana.**
- ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/4cff4416-b6d3-4b07-833f-40e1f91c3787)
- Luego de haber insertado los lotes en la BD, se puede proceder con el registro de compras en Softland.
- Las compras se registran en el módulo de compras > Operaciones > Ordenes:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/673bca73-96f7-4a36-b903-c8583fef17c6)
- Hacer click en nuevo
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/9ef188f5-75ae-45d9-bb41-31a277bd2d06)
- Los campos marcados en la siguiente captura son los que se deben modificar, guardar y cerrar la ventana:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/df2e97b0-42ad-4eaa-81f2-e6d331f423d2)
- Ahora se debe regresar al excel que se estaba trabajando e ir a modificar la tercera hoja (ORDEN_COMPRA_LINEA). Se van a modificar las columnas que en su encabezado se han rellenado con color amarillo. La información que se va a reemplazar en dichas columnas está en la primera hoja.
- Al finalizar la hoja ORDEN_COMPRA_LINEA, se debe abrir el SQLSMS y buscar en la base de datos: SOFTLAND, una tabla con el nombre: **CANNYSHOP.ORDEN_COMPRA_LINEA**, para insertar los registros directamente de la hoja de excel (sin los encabezados) a la tabla de la base de datos.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/7cedb469-2325-4b3f-a13f-de7d12821a9a)
- Ahora hay que volver a Softland y abrir nuevamente la orden de compra que se está trabajando.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/a80870c3-da73-45d5-b8d9-97f793f53779)
-  Ir a la ficha de Líneas, donde ya debería cargar las líneas que se han insertado desde la base de datos.
>  ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/41da6132-63b6-4a91-9f60-e6f66e9d3e5f)
- Se debe verificar en la ficha de **Montos** que estos cuadren con el CCF.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/2f54d9eb-436e-408d-86bc-a390e7c5f55e)
- Ahora solo faltaría aprobar la orden de compra:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/e3d5585d-feed-4a0d-9754-bb6cf3f8c606)
- Continuar
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/c4593121-1a01-4b01-ba1c-98a8be3a3f2e)
### Embarque
- Después de aprobar la orden de compra, se procede a la creación del embarque. Para ello se debe ir al módulo de compras > Operaciones > Embarques:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/cfaae31d-243b-4c58-95f7-1bdf3fdd08ae)
- Hacer click en nuevo embarque:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/6002b51a-8bd3-4f94-8ecb-0281c219e48c)
- Completar lo campos que se han marcado con el cuadro rojo, al finalizar guardar y salir:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/a69b1ca4-b1fa-46b6-88f6-98d474990524)
- Hacer clic en **cargar ordenes**:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/09703c77-18fc-4c56-9f5c-c76849bc71a3)
-  Seleccionar en la ficha de filtro **la bodega y la orden** que se está cargando, luego en la ficha de **Líneas** seleccionar todas las fichas de la orden de compra y marcarlas con el cheque, luego hacer click en cargar y salir:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/0a255bff-a0ff-453b-96f7-8e49abbc8cda)
- Continuar
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/34a80aac-d613-417a-a374-26b2bb35cd4a)
- Ahora se debe ir nuevamente al excel que se está trabajando para hacer la carga de los desgloses, pero primero se debe llenar la cuarta hoja (DET_LIN_EMBARQUE).  La información que se va a reemplazar en dichas columnas siempre está en la primera hoja.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/5f67911f-1bb1-452c-81ae-67df2c7bad3d)
- Al tener lista la hoja DET_LIN_EMBARQUE se debe llevar a SQLSMS también para buscar la tabla CANNYSHOP.DET_LIN_EMBARQUE e insertar la data:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/c99e0e3e-1ca7-4bfb-9343-c68cd43aec09)
- Volvemos a Softland y si está abierta la ventana de Embarque se cierra y se vuelve a abrir para que cargue la actualización en la ficha de los desgloses:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/642eb60d-1531-4d70-a23a-2b3bfdb2e7d9)
- Se hace el recalculo:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/e14f05e3-76da-495b-8743-aea00af7fc7f)
- Luego vamos a la ficha de Costos y en el menú superior damos click a **Refrescar costos**. (Tener cuidado que no sea el botón de aplicar a inventario). 
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/64219f85-232c-49f9-a91e-2b1980a43c10)
- Luego de **"Refrescar costos"** se debe GUARDAR y ahora se va a habilitar la ficha de Factura:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/ae98a26c-1755-4058-91e4-fe3efdbf75c6)
- Configuración general de una factura:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/2241ff98-b33d-4074-9cbc-417d59b6db76)
- Verificar que todas las lineas de desglose esten marcadas en la ficha de Lineas:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/755b4535-9bfe-4ea5-994c-fff1a5141c5e)
- Continuar
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/2d94ed40-59a1-44a3-ad91-68a7d68a6b55)
- Se verifica contrastando con los CCF, que el importe y el impuesto de IVA sea iguales. En caso de faltar o pasarse unos "centavitos" puede hacerse un ajuste, sumando o restando a algunas cantidades, las que sean más grandes de preferencia:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/8d780f6b-1038-4694-a2e9-9e3a11b6c186)
- Se regresa a la ficha general para modificar los montos e igualar al CCF (guardar y salir):
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/2f85fef0-e678-4a05-93d5-23612c6ef3c8)
- Procede aplicar al inventario...



































