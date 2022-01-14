# Patryk Rozenkranc
# Projekt na technike mikroprocesorową

## Temat projektu

-Miernik odległości (odległość od przeszkody będzie się wyświetlać na wyświetlaczu)

## Lista elementów

Wymagane elementy:

-Arduino UNO 

-Płytka stykowa z przewodami połączeniowymi

-Moduł wyświetlacza LCD 1602 2x16

-Ultradźwiękowy czujnik odległości HC-SR04

## Sposób działania 

Podzas zbliżania się obiektu do czujnika na wyświetlaczu będzie zmieniała się odległość (maksymlna odległość 200cm, minimalna 5cm)

## Kod 

#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#define pin_nadajnik 6  //Definicja pinu, do którego podłączamy nadajnik (pin TRIG)
#define pin_odbiornik 7 //Definicja pinu, do którego podłączamy odbiornik (pin ECHO)
LiquidCrystal_I2C lcd(0x27, 16, 2);

int odleglosc;          //odległość
long czas_impulsu = 0;  //czas trwania impulsu
 
void setup()
{
 lcd.init(); //Inicjalizacja pracy LCD
 lcd.backlight();
 pinMode(pin_nadajnik, OUTPUT);
 pinMode(pin_odbiornik, INPUT);

}
 
void loop()
{
 digitalWrite(pin_nadajnik, HIGH); //ustawienie stanu wysokiego
 delayMicroseconds(10); //Czas trwania 10us
 digitalWrite(pin_nadajnik, LOW); //ustawienie stanu niskiego
 
 czas_impulsu = pulseIn(pin_odbiornik, HIGH); //Czas trwania impulsu
 odleglosc = czas_impulsu/58; //Wyznaczenie odległości w cm (wielkość 58 wynika z dokomentacji technicznej)

//Zabezpieczenie przed przekroczeniem zakresu pomiarowego
  if ( odleglosc < 5 || odleglosc> 200 )
  {
   lcd.setCursor(0,0);
   lcd.print("Pomiar niedostepny");
  }
  
  else
  {
 lcd.setCursor(0,0);
   lcd.print("Odleglosc: ");
   lcd.setCursor(11,1);
   lcd.print(odleglosc);
   lcd.print("cm");
  }
   delay(1000);
   lcd.clear();


}