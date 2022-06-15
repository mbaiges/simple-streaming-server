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

## Conexión de dos clientes en simultáneo

3. Conectar ambos clientes con el mismo comando 

```bash
ffplay rtmp://$SERVER_IP:1935/mystream 
```
 
## Ver grabación 
4. Reproducir la grabación que se almacenó en la siguiente carpeta:
```bash
cd rec
ffplay file.mp4
```
## Stream de video 

5. Publicar un video 
```bash
ffmpeg -stream_loop -re -i /mnt/d/Descargas/demo.mp4 -c:v libx264 -c:a aac -f flv rtmp://$SERVER_IP:1935/mystream
```

6. Consumir el video desde el cliente
```bash
ffplay rtmp://$SERVER_IP:1935/mystream 
```

## Ver Métricas 

7. Entrar a la página de métricas 
```bash
http://$SERVER_IP:9998/metrics
```

## Wireshark 
8. Ya se capturaron los paquetes y se puede ver:
- Intercambio RTMP ( Handshake, publishStream )
- Codecs ( H.264 para video, HE-AAC para audio ) 

Ir a la sección STATISTICS -> CONVERSATIONS -> IPV4 O TCP -> BIT PER SECOND. Se puede ver:
- Ancho de banda Total 
- Ancho de banda por Cliente 


## HLS 

9. Con la extensión de Chrome "Play HLS m3u8" instalada (o desde Firefox) acceder a: 
```bash
http://$SERVER_IP:8888/mystream
```
10. En Wireshark se pueden ver los paquetes HTTP que usa el protocolo

## Codecs 




