# Práctica 4.4 (semana del 23 al 27 Enero): deployment of an architecture EFS-EC2-MultiAZ in the CLoud (AWS)

## IP de las máquinas:
## Linux01: 54.242.84.1
## Linux02: 54.161.26.52

## IP del balanceador: 100.25.160.245/balancer-manager

1. Montamos los Grupos de Seguridad de las máquinas EFS y EC2

   1. Clicamos sobre EC2 > Security Groups
   2. Creamos los siguientes grupos de seguridad con los siguientes parámetros:

        ![img](Practica4-4/img/Imagen1.png)

   3. Después modificaremos las reglas de entrada del grupo de seguridad abriendo el puerto SSH
   
        ![img](Practica4-4/img/Imagen2.png)

   4. Y creamos otro grupo de seguridad con los siguientes parámetros:

        ![img](Practica4-4/img/Imagen3.png)

      IMPORTANTE el origen es el grupo de seguridad creado anterior a éste.

2. Creamos las máquinas EC2

        ![img](Practica4-4/img/Imagen4.png)
        ![img](Practica4-4/img/Imagen5.png)

   1. Desplegamos la pestaña de Detalles avanzados y modificamos lo siguiente:

        ![img](Practica4-4/img/Imagen6.png)

   2. Después des eso desplegamos la instancia
   3. Posteriormente crearemos otra máquina con los mismos cambios menos este: En vez de la subred con zona de disponibilidad en “us-east-1a” será “us-east-1b”.

        ![img](Practica4-4/img/Imagen7.png)

3. Montaremos un sistema de archivos NFS

        ![img](Practica4-4/img/Imagen8.png)
        ![img](Practica4-4/img/Imagen9.png)

    1. Cuando lo hayamos creado nos iremos a Red > Administrar e cambiaremos en las zonas de disponibilidad para que escoja el grupo de seguridad “SCefs"

        ![img](Practica4-4/img/Imagen10.png)

4. Posteriormente nos conectaremos a las máquinas EC2 creadas anteriormente

    1. Introduciremos este código en la máquina Linux01:

        - sudo su
        - cd /var/www/html
        - mkdir efs-mount
  
    2. Y luego este otro cambiando IDEFS por la id de EFS:

        ![img](Practica4-4/img/Imagen11.png)

        - sudo mount -t nfs -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport “IDEFS”.efs.us-east-1.amazonaws.com:/ efs-mount

    3. Descargamos el siguiente archivo con este comando:

        - cd efs-mount
        - wget https://s3.eu-west-1.amazonaws.com/www.profesantos.cloud/Netflix.zip
        - unzip Netflix.zip

    4. Y ahora haremos lo mismo con la segunda máquina pero sin descargar el archivo anterior.
    5. Si metemos el siguiente comando, nos debe de salir el código de la siguiente página web:

        - curl localhost/efs-mount/index.html
  

        ![img](Practica4-4/img/Imagen12.png)

    6. Si queremos mostrarla en el navegador:

        Y saldrá esto: 

        ![img](Practica4-4/img/Imagen13.png)

5. Modificar el fichero de Apache

    1. Si metemos el siguiente código:

    - vim /etc/httpd/conf/httpd.conf

    2. Se modificará el fichero en:

        ![img](Practica4-4/img/Imagen14.png)

        Para sobreescribir es ESC > :w, y luego para salir es: ESC > :q!


    




