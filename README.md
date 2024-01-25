# P8_NR_DHT22
## INTRODUCCION

### DESCRIPCION 
.
La **Esp32**  es un dispositivo electronico en el cual podemos realizar practicas y simulaciones, en esta practica realizaremos  con  la ayuda de un servidor en linea la simulacion de una conexion wifi entre nuestra tarjeta y  **Node-red**, esta accion nos permitira visualizar en tiempo real nuestras graficas de acuerdo a los valores detectados por nuestros sensores. Cabe aclarar que para esta practica se usara un simulador llamado **https://wokwi.com/**.

### MATERIAL A OCUPAR

Para realizar esta practica ocuparemos lo siguente:

-WORKI

-Tarjeta ESP 32

-DTH22

-Node red

-Un servidor virtual que nos genere una IP

### Requisitos previos

Para poder realizar esta practica es nesesario contar con conosimientos basicos de programacion y electronica.

## INSTRUCIOES DE ELAVORACION 

1.Buscar un servidor virtual y generar una IP:
 a) Entraremos al siguiente link **(https://www.hivemq.com/mqtt/public-mqtt-broker/)** y copiaremos la direccion señalada
 ![](https://github.com/nijs17/P8_NR_DHT22/blob/main/W1.png)
 b)Abriremos el simbolo del sistema y escribimos **nslookup**  seguido del link anteriormente copiado **broker.hivemq.com** esta accion nos generara una IP, copiamos esa IP
 ![](https://github.com/nijs17/P8_NR_DHT22/blob/main/W2.png) 
 2. Armar el proyecto en node red.
 Acontinuacion se muestran los cambios que se le tiene que hacer al proyecto, en los que se encuentran pegar la IP y cambiar el nombre al tipico, elegir al leguaje de programacion que se va a convertir, programar las funciones  y editar los valores de los indicadores y sensores como se muestra en la imagen.
 ![]()
 ![]()
 ![]()

 
2. Hacer las conexiones de la tarjeta con los demas elementos, anteriormente mensionados, como se muestra en la siguente imagen. 
 ![](https://github.com/nijs17/P6_Nivel-de-agua/blob/main/l1.png)

Abrir la terminal de programación y colocar el siguente codigo
## INSTRUCCIONES DE OPERACION 


    1. Iniciar simulador.
    
    2. Realizar las conexiones mostradas en la imagen anterior.
    
    3. Mover la sencibilidad del ultrazonico para simular el nivel del agua y asi poder visualizar sus diferentes estados del detector de nivel, como se muestra en la siguiente imagen.
    
## RESUSLTADOS
Al finalizar tendremos uns detector de nivel que nos mostrara el nivel de nuestro tinaco con LEDS que indican de manera visual el porcentaje de agua que tiene nuestro deposito.
![](https://github.com/nijs17/P6_Nivel-de-agua/blob/main/l2.png)
