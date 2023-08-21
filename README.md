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
 - Se debe relacionar con la información de prefijo y tabla mencionada anteriormente. Y luego dar clic en procesar.
> ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/99ea9e51-6e01-45c3-8e48-f3e8bad4078a)
 - Esto no se debe tocar (lo marcado en el cuadro rojo):
> ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/3a4ff644-28db-4465-a227-bb7c01c2002c)
-  Luego se debe hacer el resto de hojas siguiendo el mismo procedimiento, realizado en esta sección. Siempre con el esquema CONSNY.
- **Nota: Cuando son todas las tiendas, DPL se tarda hasta 10 minutos**

#### Despues de haber cargado la información se debe hacer la comprobación

