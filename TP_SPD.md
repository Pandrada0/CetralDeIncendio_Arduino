#  __Sistema de Control de Incendios__

## Resumen del proyecto. 
__El proyecto que van a ver a continuacion tiene como fin detectar la temperatur y validar si se trata de un incendio. 
Esta temperatura la podemos ir manipulando manualmente atravez de nuestro sensor de temperatura(TMP36). Tambien atravez de nuestro 
control remoto IR podremos cambiar la estacion del año, la cual sera visible en nuestro LCD. Al detectar altas temperaturas,
el sistema nos va a informar que eso se debe a un posible incendio, nuestro sistema reconoce estas altas temperaturas y acciona 
sobre ellas. Mostrando un mensaje de alerta en nuestra pantalla, prendiendo luces de emergencia (leds) y por ultimo activando
nuestro servo.__

## __LINK DE NUESTRO PROYECTO__ 

*https://www.tinkercad.com/things/2biwFEkgaqP-proyecto-arduino-tp/editel?sharecode=mEKRhrUSIB1Xx4QhVbxixccj9bI0TIjpVwD_kPZZNTA




## El circuito esta compuesto con los siguientes componentes:

* Arduino Uno
* LCD 16x2
* Sensor Temperatura(TMP36)
* Sensor IR
* Control Remoto IR
* Micro Servomoto
* 2 led
* 3 Resistencias de (220oh)
* 2 Placas de prueba


![proyectofinal](https://github.com/Pandrada0/TP_SPD/assets/108838669/8ede3f32-fcee-47c2-b1fa-b89beff27c29)



## __Importamos nuestras librerias__
```
#include <Servo.h>
#include <IRremote.h>
#include <LiquidCrystal.h>
```

## __NuestroS objetos__

```
LiquidCrystal lcd (2,3,4,5,6,7);
Servo miServo;
```
* Creamos el objeto LCD el cual le asignamos los pines a los cuales los conectamos en el arduino.
* Creamos el objeto miServo el cual luego utilizaremos en nuestro codigo.

## __Nuestro SETUP__

```
  IrReceiver.begin(IR, DISABLE_LED_FEEDBACK);
  lcd.begin(16,2);
  miServo.attach(pinServo);
  miServo.write(0);
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
```

* Asignamos nuestro sensor IR
* Asignamos el tamaño de nuestro LCD
* Asignamos el pin del arduino donde esta conectado el servo
* Le damos una posicion inicial al servo
* Configuramos las luces de emergencia(leds)



## __Nuestras Funciones__

__Las funciones tiene el objetivo de hacer nuestro codigo las legible y ordenado.
Cada una de ellas tiene una funcion dentro de nuestro codigo.__

```
void mostrarTMP(int temperatura);
```
* Muestra la temperatura recibida del sensor en el LCD

```
void detectarIncendio(int temperatura,int estacion);
```
* Interpreta segun la temperatura y la estacion si existe la posibilidad de un incendio

```
void mostrarEstacion(int boton);
```
* Esta funcion se encaga de mostrar en la pantalla la estacion segun el boton de control IR

```
void encenderServo();
```
* Esta funcion se encarga de prender el servo cuando se detecta un incendio

```
void encenderLed();
```
* Esta funcion se encarga de prender los leds cuando se detecta un incendio


## __Nuestro LOOP__
```
 temperatura = map(analogRead(TMP),0,1023,-50,450);  
  mostrarTMP(temperatura);
  
  if (IrReceiver.decode()){ 
    switch(IrReceiver.decodedIRData.decodedRawData){
      case boton1:
        mostrarEstacion(1);
        flag = 1;
      	break;
      case boton2:
      	mostrarEstacion(2);
        flag = 2;
        break;
      case boton3:
      	mostrarEstacion(3);
        flag = 3;;
        break;
      case boton4:
     	mostrarEstacion(4);
        flag = 4;
        break; 
     }
  }
  IrReceiver.resume();
  detectarIncendio(temperatura,flag);
```

Nuestro loop se encarga de mostrar la temperatura en el momento que la lee llamando a la funcion __mostrarTMP__. 
Luego decodifica lo recibido atravez del sensor IR, dependiendo del boton llamando a la funcion __mostrarEstacion__.

* Opcion1: Verano
* Opcion2: Otoño
* Opcion3: Invierno
* Opcion4: Primavera

![botones](https://github.com/Pandrada0/TP_SPD/assets/108838669/4570bcbc-1d2b-4b6e-a983-d52db321a85f)


Dependiendo la estacion del año tenemos un margen de temperatura antes de detectar un posible incendio. 
A continuacion mostraremos el maximo de temperatura por estacion del año.

![temperatura](https://github.com/Pandrada0/TP_SPD/assets/108838669/34af19b7-9679-4957-8f41-174fa192a392)

* Verano: 42 grados
* Otoño: 25 grados
* Invierno: 20 grados
* Primavera: 30 grados



# __MENSAJE ALERTA INCENDIO__

![incendio](https://github.com/Pandrada0/TP_SPD/assets/108838669/e2bd0d0a-ebea-4671-8751-5f69507f6a59)


# __DIAGRAMA ESQUEMATICO DEL CIRCUITO__


![ESQUEMA](https://github.com/Pandrada0/TP_SPD/assets/108838669/3ea10dd4-ee0c-4844-ae53-3161b9c91dbf)





