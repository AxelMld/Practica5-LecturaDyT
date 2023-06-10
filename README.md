# **PRÁCTICA 5 Lectura de distancia y temperatura**

## Para está práctica se utilizó 

* Módulo ESP32
* Sensor Ultrasónico de distancia 
* Sensor DHT22
* LCD 16x2

## Para la simulación nos dirigimos a la página  

woki : https://wokwi.com

#### Dashboard de página a utilizar 
![](https://github.com/AxelMld/Practica-3-Sensor-Ultrasonico-/blob/main/dash.png?raw=true)

## Pasos a Seguir 

1. Seleccionar el módulo ESP32.
2. Escoger los dispositivos a utilizar anteriormente mensionados. 
3. Realizar las conexiones como se muestra en el apartado "conexión".
4. Agregar las librerias mensiadas en el apartado Librerías 




## **Conexión**

![](https://github.com/AxelMld/Practica4/blob/main/Conexion.png?raw=true)


## **Librerías**
1. LiquidCrystal I2C
2. DHT sensor library for ESPx

## Código utilizado 


```

#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4


// pines y seccion del sensor de distancia 

const int Trigger = 13;   //Pin digital 2 para el Trigger del sensor
const int Echo = 12;   //Pin digital 3 para el Echo del sensor

//para la pantalla lcd
LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

//pines y seccion del sensor DHT22
const int DHT_PIN = 15;

DHTesp dhtSensor;


void setup() {

  Serial.begin(9600);//iniciailzamos la comunicación


  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
 
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0

  lcd.init();
  lcd.backlight();
}

void loop()
{

  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

 TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();
  delay(1000);          //Hacemos una pausa de 100ms


  lcd.setCursor(0, 0);
  lcd.print("Axel M          ");
  lcd.setCursor(0, 1);
  lcd.print("Distancia: " + String(d) + "cm");
  delay(2000);

  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  lcd.print("Wokwi Online IoT");


}


```


# **Resultados**

## 1. Resultado lectura de distancia  

![](https://github.com/AxelMld/Practica4/blob/main/resultado%201.png?raw=true)

## 2. Resultado lectura de temperatura y humedad. 




# Creditos 

**Axel Miranda.**

