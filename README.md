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
- *Y al final lo ordenar por la columna D (cantidad) de menor a mayor*
  
