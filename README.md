# Simple Streaming Service

TPE para la materia Redes de Información

# RTSP Simple Server

rtsp-simple-server es un servidor listo para su uso y libre de dependencias que permite a los usuarios publicar y reproducir streamings en tiempo real de audio y video a través de múltiples protocolos:

(TODO: Poner links faltantes)
- [RTSP (Real Time Streaming Protocol)](https://en.wikipedia.org/wiki/Real_Time_Streaming_Protocol)
- [RTMP (Real Time Messaging Protocol)](https://en.wikipedia.org/wiki/Real-Time_Messaging_Protocol)
- [HLS](https://en.wikipedia.org/wiki/HTTP_Live_Streaming)

![Supported Protocols](./resources/rtsp_ss_protocols.png)

El repositorio se encuentra en [GitHub](https://github.com/aler9/rtsp-simple-server).

(TODO: Completar información de años del proyecto y contribuciones)
El proyecto lleva 1 año y medio, y sigue en mantenimiento.

![Github Contributions](./resources/rtsp_ss_github_contributions.png)

# Requisitos

## Docker

Para instalar Docker en Ubuntu, seguir la [guía de la documentación oficial](https://docs.docker.com/engine/install/ubuntu/).

# Configuración

(TODO: Cargar configuración)

# Instalación

## Docker

En la carpeta `root`, ejecutar la siguiente línea

```bash
docker run --rm -it -e RTSP_PROTOCOLS=tcp \
-v $PWD/config.docker.yml:/rtsp-simple-server.yml \
-v $PWD/rec/:/rec/ \
-p 8554:8554 -p 1935:1935 -p 8888:8888 \
--name rtsp-server aler9/rtsp-simple-server
```

## Local

Primero descargamos la release oficial del servidor.

```bash
wget https://github.com/aler9/rtsp-simple-server/releases/download/v0.19.1/rtsp-simple-server_v0.19.1_linux_amd64.tar.gz -O rtsp.tar.gz
```

La descomprimimos y reemplazamos el archivo de config.

```bash
tar xvz -f rtsp.tar.gz
rm rtsp.tar.gz
cp config.yml rtsp-simple-server.yml
```

Creamos la carpeta para las grabaciones.

```bash
mkdir rec
```

Luego, iniciamos el servidor.

```bash
./rtsp-simple-server
```

## AWS 

El servidor se puede levantar en una instancia EC2. 
Para intrucciones de configuración ver `aws_config.md`

# Uso

De ahora en adelante, haremos referencia a la IP del servidor como `$SERVER_IP`.

## Publicar

Para publicar un stream de audio y video, podremos utilizar el protocolo RTMP o RTSP.

### OBS

En este caso, publicaremos por RTMP.

Desde OBS, en la configuración `Settings`, en la sección de `Stream`, usaremos el Servicio `Custom` con el Servidor `rtmp://$SERVER_IP:1935/` y como llave para el stream, la que queramos. En este caso usaremos `mystream`. Es importante recordar la clave ya que se usará luego para acceder al stream.

![OBS Config](./resources/obs_config.png)

Luego, podemos iniciar el streaming, y se verá cuando se conectó al servidor en la parte inferior de OBS.

![OBS Streaming](./resources/obs_streaming.png)

Se puede observar el cuadrado verde y la sección de `LIVE` indicando el tiempo de streaming.

### FFMPEG

Desde la herramienta `ffmpeg`, también podemos streamear. 
Un ejemplo es stremear un video en loop. Para el ejemplo, usaremos un video llamado `demo.mp4`.

En este caso, podríamos publicar por RTMP, pero vamos a publicar por RTSP.

```bash
ffmpeg -stream_loop -1 -re -i ./demo.mp4 -c copy -f rtsp -y rtsp://$SERVER_IP:8554/mystream
```

Si funciona correctamente, veremos una salida como la siguiente:

![FFMPEG Streaming](./resources/ffmpeg_streaming.png)

En la imagen podemos notar que se estará enviando con codec de video h264 y de audio mp3.

## Reproducir

Para reproducir el streaming, tendremos distintas formas de hacerlo.

### RTMP

Para acceder al streaming, podemos hacerlo mediante `RTMP`. Un caso de uso es con la herramienta `ffplay`, la cual utiliza las librerías de `ffmpeg`.

```bash
ffplay rtmp://$SERVER_IP:1935/mystream
```

También podemos a través de `RTSP`.

```bash
ffplay rtsp://$SERVER_IP:8554/mystream
```

Al correr cualqueira de los comandos anteriores, se reproducirá el stream.

![FFPlay Playing](./resources/ffplay_stream.gif)

### HLS

Accediendo a `http://$SERVER_IP:8888/mystream`, obtenemos el archivo `.m3u8`, que nos permite reproducir el streaming. Una forma de probarlo es con la siguiente [App](https://hls-js.netlify.app/demo/). Usando el URL anterior:

![HLS Player](./resources/hls_player.gif)

Algunos navegadores traen por un reproductor, con lo cual accediendo a la URL anterior, se comienza a reproducir el stream. Firefox es uno de ellos.

# Grabación de la Sesión 

Las grabaciones de las sesiones, se almacenan en la carpeta `rec`, en formato `mp4`.

![Grabaciones](./resources/recordings.png)

# Codecs

Podemos observar los codecs utilizados durante el intercambio del protocolo `RTMP` o `RTSP` con la herramienta `Wireshark`.

## Audio

### Audio HE-AAC

Como codec de Audio se utiliza HE-AAC (High-Efficiency Advanced Audio Coding). Es un formato de compresión de audio digital. Su uso está optimizado para velocidades de transmisión bajas o muy bajas.

![Wireshark](./resources/wireshark_rtmp_audio_he-aac.png)

## Video

### H.264

Como codec de Video se utiliza H.264 o MPEG-4 AVC (Advanced Video Coding). Es un formato de codificación de video para grabar y distribuir señales de vídeo.  Suministra imágenes de alta calidad sin consumir demasiado ancho de banda.

![Wireshark](./resources/wireshark_rtmp_video_h264.png)

# Intercambio de paquetes

## RTMP

En `Wireshark` podemos seguir una trama `RTMP` para observar el intercambio de paquetes entre cliente y servidor.

Se puede ver el Handshake RTMP y la conexión al stream `eugestream`

![Wireshark](./resources/wireshark_rtmp_handshake.png)

Se transfiere la información de Audio y Video
![Wireshark](./resources/wireshark_rtmp_audio_video.png)