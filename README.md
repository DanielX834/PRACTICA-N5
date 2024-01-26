# PRACTICA-5-DHT-22-CON-SENSOR-ULTRASONICO-CON-LCD
Este repositorio muestra como podemos programar una ESP32 con el sensor DHT11, el sensor ultrasonico HC-SR04 y un display LCD.

## Introducción
### Descripción
La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un sensor (```DTH22```) para adquirir temperatura y humedad del entorno, tambien un sensor (```HC-SR04```) para obtener el registro de distancia y un display ```LCD``` para mostrar los resultados obtenidos, asi como tambien mi nombre y mi carrera; Cabe aclarar que esta practica se usara un simulador llamado [WOKWI](https://https://wokwi.com/).

## Material Necesario
Para realizar esta practica necesitas lo siguiente:
- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP32
- Sensor DHT22
- Sensor HC-SR04
- Display LCD 16x2 (I2C)

## Instrucciones
### Requisitos previos
Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).

### Instrucciones de preparación de entorno 
1. Abrir la terminal de programación y colocar la siguente programación:
```
#include "DHTesp.h"
#include <LiquidCrystal_I2C.h>
#define I2C_ADDR    0x27
#define LCD_COLUMNS 20
#define LCD_LINES   4

const int DHT_PIN = 15;
const int Trigger = 4;   //Pin digital 2 para el Trigger del sensor
const int Echo = 2;   //Pin digital 3 para el Echo del sensor

DHTesp dhtSensor;

LiquidCrystal_I2C lcd(I2C_ADDR, LCD_COLUMNS, LCD_LINES);

void setup() {

  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  pinMode(Trigger, OUTPUT); //pin como salida
  pinMode(Echo, INPUT);  //pin como entrada
  digitalWrite(Trigger, LOW);//Inicializamos el pin con 0
  lcd.init();
  lcd.backlight();

}

void loop() {

  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  Serial.println("Temp: " + String(data.temperature, 1) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  Serial.println("---");
  
  long t; //timepo que demora en llegar el eco
  long d; //distancia en centimetros

  digitalWrite(Trigger, HIGH);
  delayMicroseconds(10);          //Enviamos un pulso de 10us
  digitalWrite(Trigger, LOW);
  
  t = pulseIn(Echo, HIGH); //obtenemos el ancho del pulso
  d = t/59;             //escalamos el tiempo a una distancia en cm
  
  Serial.print("Distancia: ");
  Serial.print(d);      //Enviamos serialmente el valor de la distancia
  Serial.print("cm");
  Serial.println();

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("  Temp: " + String(data.temperature, 1) + "\xDF"+"C  ");
  lcd.setCursor(0, 1);
  lcd.print(" Humidity: " + String(data.humidity, 1) + "% ");
  delay(2000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Distancia: " + String(d) + "cm");
  delay(2000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Daniel Armenta");
  lcd.setCursor(0,1);
  lcd.print("Ing. Electrico");
  delay(2000);
}
```
2. Instalar la libreria de **DHT sensor library for ESPx** como se muestra en la siguente imagen.

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/1Libreria.jpg?raw=true)

3. Instalar la libreria de **LiquidCrystal I2C** como se muestra en la siguente imagen.

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/2LibreriaL2C.jpg?raw=true)

4. Hacer la conexion de **DHT11** con la **ESP32** como se muestra en la siguente imagen.

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/3Conexion.jpg?raw=true)


5. Hacer la conexion de **HC-SR04** con la **ESP32** como se muestra en la siguente imagen.

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/4ConexionHC-SR04.jpg?raw=true)

6. Hacer la conexion del **LCD 16x2 (I2C)** con la **ESP32** como se muestra en la siguente imagen.

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/5ConexionL2C.jpg?raw=true)

### Instrucciónes de operación
1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.
3. Visualizar los datos en la pantalla LCD.
4. Colocar la distancia dando *doble click* al sensor **HC-SR04** 
5. Colocar la temperatura y humedad dando *doble click* al sensor **DHT11**

## Resultados

Una vez terminado iniciamos simulacion y se observaran los valores en la lcd.

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/6Resultado1.jpg?raw=true)

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/7Resultado2.jpg?raw=true)

![](https://github.com/DanielX834/PRACTICA-N5/blob/main/8Resultado3.jpg?raw=true)

# Créditos
Desarrollado por Ing. Daniel Armenta
