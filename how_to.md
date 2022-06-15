# How To 

0. Descargar el RTSP Simple Server como lo explica en el README.md 

1. Levantar el server 
```bash
./rtsp-simple-server
```

2. Abrir Wireshark para capturar los paquetes en la interfaz correspondiente

```bash
sudo wireshark
```
3. Abrir OBS Studio y comenzar el Streaming 

## Conexión de dos clientes en simultáneo

4. Conectar ambos clientes con el mismo comando 

```bash
ffplay rtmp://$SERVER_IP:1935/mystream 
```

5. Ver que se estableció la conexión con
```bash
curl -s http://localhost:9998/metrics
curl -s http://localhost:9997/v1/rtmpconns/list | jq .
curl -s -v -X POST http://localhost:9997/v1/rtmpconns/kick/306880971
```
 
## Ver grabación 
6. Reproducir la grabación que se almacenó en la siguiente carpeta:
```bash
cd rec
ffplay file.mp4
```
## Stream de video 

7. Publicar un video 
```bash
ffmpeg -stream_loop -re -i /mnt/d/Descargas/demo.mp4 -c:v libx264 -c:a aac -f flv rtmp://$SERVER_IP:1935/mystream
```

8. Consumir el video desde el cliente
```bash
ffplay rtmp://$SERVER_IP:1935/mystream 
```

## Ver Métricas 

9. Entrar a la página de métricas 
```bash
http://$SERVER_IP:9998/metrics
```

## Wireshark 
10. Ya se capturaron los paquetes y se puede ver:
- Intercambio RTMP ( Handshake, publishStream )
- Codecs ( H.264 para video, HE-AAC para audio ) 

Ir a la sección STATISTICS -> CONVERSATIONS -> IPV4 O TCP -> BITS PER SECOND. Se puede ver:
- Ancho de banda Total 
- Ancho de banda por Cliente 

## RTSP - Codecs

11. Publicar un Stream con H.264 y HE-AAC 
```bash
ffmpeg -re -i demo.mp4 -c:v libx264 -c:a aac -f rtsp rtsp://$SERVER_IP:8554/mystream 
```

12. Ver los Codecs en Wireshark 

13. Publicar un Stream con H.265 y HE-AAC  
```bash
ffmpeg -re -i demo.mp4 -c:v libx265 -c:a aac -f rtsp rtsp://$SERVER_IP:8554/mystream 
```
14. Ver los Codecs en Wireshark 

## HLS 

15. Con la extensión de Chrome "Play HLS m3u8" instalada (o desde Firefox) acceder a: 
```bash
http://$SERVER_IP:8888/mystream
```

16. En Wireshark se pueden ver los paquetes HTTP que usa el protocolo

17. Acceder desde un celular en la misma LAN desde Chrome a:  
```bash
http://$PRIVATE_IP:8888/mystream
```

## Acceder a un stream remoto 

18. Configurar una instancia EC2 en AWS como se expica en aws_config.md

19. Publicar el stream `remotestream` de alguna de las formas vistas previamente

20. Consumir el stream 
```bash
ffplay rtsp://$SERVER_IP:8554/remotestream
```