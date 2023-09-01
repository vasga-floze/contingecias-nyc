# CONTINGENCIAS
## COMO CREAR USUARIOS PARA CONEXION REMOTA
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
- Se tiene una plantilla de excel que en la primera hoja contiene el desglose de los fardos facturados. Se debe identificar las líneas que corresponden a cada CCF, para ir realizando las ordenes de compra por cada CCF. (Por ahora se debe filtrar por cada artículo y fecha, para comparar con los CCF que sí esten cuadrando).
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/3bde2e79-4481-4770-9464-0462987139b8)
- Luego de haber identificado todas las líneas que corresponden a un CCF se debe completar la información en la segunda hoja(LOTE), se van a modificar las columnas que en su encabezado se han rellenado con color amarillo. La información que se va a reemplazar en dichas columnas está en la primera hoja.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/99800265-2b86-40a3-9c07-f3a7690bd546)
- Al finalizar la hoja LOTE, se debe abrir TSQL y buscar en la base de datos: SOFTLAND, una tabla con el nombre: **CANNYSHOP.LOTE**, para insertar los registros directamente de la hoja de excel (sin los encabezados) a la tabla de la base de datos. **Es posible que aparezca un error de duplicados al insertar, pero no es problema, solo se puede dar en aceptar a la ventana.**
- ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/4cff4416-b6d3-4b07-833f-40e1f91c3787)
- Luego de haber insertado los lotes en la BD, se puede proceder con el registro de compras en Softland.
- Las compras se registran en el módulo de compras > Operaciones > Ordenes:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/673bca73-96f7-4a36-b903-c8583fef17c6)
- Hacer click en nuevo
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/9ef188f5-75ae-45d9-bb41-31a277bd2d06)
- Los campos marcados en la siguiente captura son los que se deben modificar, guardar y cerrar la ventana:
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/df2e97b0-42ad-4eaa-81f2-e6d331f423d2)
- Ahora se debe regresar al excel que se estaba trabajando e ir a modificar la tercera hoja (ORDEN_COMPRA_LINEA). Se van a modificar las columnas que en su encabezado se han rellenado con color amarillo. La información que se va a reemplazar en dichas columnas está en la primera hoja.
- Al finalizar la hoja ORDEN_COMPRA_LINEA, se debe abrir TSQL y buscar en la base de datos: SOFTLAND, una tabla con el nombre: **CANNYSHOP.ORDEN_COMPRA_LINEA**, para insertar los registros directamente de la hoja de excel (sin los encabezados) a la tabla de la base de datos.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/7cedb469-2325-4b3f-a13f-de7d12821a9a)
- Ahora hay que volver a Softland y abrir nuevamente la orden de compra que se está trabajando.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/a80870c3-da73-45d5-b8d9-97f793f53779)
-  Ir a la ficha de Líneas, donde ya debería cargar las líneas que se han insertado desde la base de datos.
>  ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/41da6132-63b6-4a91-9f60-e6f66e9d3e5f)
- Se debe verificar en la ficha de **Montos** que estos cuadren con el CCF.
> ![image](https://github.com/vasga-floze/contingencias-nyc/assets/72711545/2f54d9eb-436e-408d-86bc-a390e7c5f55e)



















