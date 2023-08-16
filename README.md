# CONTINGENCIAS
### Crear usuarios para conexión remota 
Para que los usuarios conectados en la red puedan acceder al escritorio remoto, deben existir los usuarios en Active Directory.
#### Los pasos para crear usuarios o verificar que existen los usuarios:
* Paso 1: Ingresar  al Administrador del Servidor y luego al centro de administración de active directory
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/928cb60e-19a8-452c-8930-745f2085e507)
  
* Paso 2: Crear Nuevo en Centro de Administración de Active Directory.
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/d3b71add-03cd-4c03-a7e6-24a94be0b3a1)
  
* Paso 3: Completar los campos marcados en los cuadros rojos:
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/372966d6-0505-4418-8c6a-fddfe8119108)
  
* Paso 4: Vincular al usuario como miembro de un grupo(ver el proceso en orden de las siguientes capturas de pantalla):
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/91e9b1fc-1c38-430c-a77f-f5bc336263a2)
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/ac123d19-77e4-4a62-b56f-df670ba7c39a)
  
  En este punto, una vez identificado el grupo de conexión remota, agregar y dar aceptar a las ventanas.
  
  ![image](https://github.com/vasga-floze/contingecias-nyc/assets/72711545/b4328287-d4e9-47a2-9a8d-429a9d459438)

  Ya se debería poder hacer el ingreso con ese usuario usando el escritorio remoto. REPETIR EL PROCESO PARA TODOS LOS USUARIOS NECESARIOS.




