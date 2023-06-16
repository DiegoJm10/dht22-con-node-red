# Sensor de DHT22 con node-red

## Instrucciones 
1. Colocar bloque ```mqqtt in```.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/bloquemqtt.png?raw=true)

2. Configurar el bloque con el puerto mqtt con el ip ```52.57.167.175``` como se muestra en la imagen.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/Ipmqtt.png?raw=true)

3. Colocar el bloque json y configurarlo como se muestra en la imagen.

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/JSON.png?raw=true)

4. Colocamos dos bloques function y lo configuramos con el siguente codigo.

```
msg.payload = msg.payload.TEMPERATURA;
msg.topic = "TEMPERATURA";
return msg;
```

```
msg.payload = msg.payload.HUMEDAD;
msg.topic = "HUMEDAD";
return msg;
```

5. Colocamos los bloques de chart y gauge.

## Resultados

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/Resultado.png?raw=true)

![](https://github.com/DiegoJm10/dht22-con-node-red/blob/main/Resultados2.png?raw=true)