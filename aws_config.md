# AWS 
En caso de desear correr el server SRS en una instancia de EC2 se pueden seguir los siguientes pasos

## Configuración de EC2 

1. Dentro de la consola elegimos Lanzar una Instancia EC2 
![EC2_1](./resources/aws/ec2_1.png)

2. Configuramos el nombre 
![EC2_2](./resources/aws/ec2_2.png)

3. Elegimos una AMI de Ubuntu 
![EC2_3](./resources/aws/ec2_3.png)

4. Elegimos el Tipo de Instancia  
![EC2_4](./resources/aws/ec2_4.png)

5. Creamos un par de keys para encriptar 
![EC2_5](./resources/aws/ec2_5.png)

6. Configuramos el tráfico SSH
![EC2_6](./resources/aws/ec2_6.png)

<!-- Queda pendiente de acá para abajo (UDP abre varios puertos) -->

7. Configuramos los Security Groups para RTMP, Web App, HLS, RTSP? 
![EC2_7](./resources/aws/ec2_7.png)

8. Ciframos con una llave de KMS 
![EC2_9](./resources/aws/ec2_9.png)

9. Habilitar Hibernate Behaviour
![EC2_10](./resources/aws/ec2_10.png)

10. Ejecutar el server rtsp-simple-server por User Data
![EC2_11](./resources/aws/ec2_11.png)