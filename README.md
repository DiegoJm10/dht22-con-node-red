# Sensor de DHT22 con node-red

## Instrucciones 
1. Colocar bloque ```mqqtt in```.
![](https://github.com/DiegoJm10/PracticaDHT/blob/main/New%20ESP32%20Project%20-%20Wokwi%20Simulator%20-%20Google%20Chrome%2008_06_2023%2011_10_20%20p.%20m.%20(2).png?raw=true)

2. Configurar el bloque con el puerto mqtt con el ip ```52.57.167.175``` como se muestra en la imagen.

![](https://github.com/DiegoJm10/PracticaDHT/blob/main/New%20ESP32%20Project%20-%20Wokwi%20Simulator%20-%20Google%20Chrome%2008_06_2023%2011_10_20%20p.%20m.%20(2).png?raw=true)

3. Colocar el bloque json y configurarlo como se muestra en la imagen.

![](https://github.com/DiegoJm10/PracticaDHT/blob/main/New%20ESP32%20Project%20-%20Wokwi%20Simulator%20-%20Google%20Chrome%2008_06_2023%2011_10_20%20p.%20m.%20(2).png?raw=true)

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