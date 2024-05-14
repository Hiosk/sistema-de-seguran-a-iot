# Sistema de segurança
> Feito por Leonardo de Oliveira, Pedro Lourenço e Pedro Henrique

## Lista To-do

- [x] Implementação do display LCD
- [x] Implementação do teclado 4x4
- [x] Desenvolvimento da leitura do teclado 4x4
- [x] Desenvolvimento do sistema de senha inicial
- [x] Implementação do micro servo
- [ ] Desenvolvimento do sistema das três tipos de senha
- [ ] Desenvolvimento da interface gráfica, cadastro e alerta silencioso
- [ ] Desenvolvimento do sistema de cadastro de senha usando a master
- [ ] Desenvolvimento do sistema de alerta utilizando o sistema da senha de alerta

## Componentes utilizados

- [Arduino UNO R3] - Microcontrolador
- [Teclado 4x4]
- [Display LCD 16x2]
- [Micro servo]
- [Resistor 220u]

## Código fonte

```c
#include <Servo.h>
#include <Keypad.h>
#include <Wire.h> 
#include <LiquidCrystal.h>

LiquidCrystal lcd(13, 12, 11, 10, 9, 8);

const int pinoServo = 15;
Servo s;

long first = 0;
long second = 0;
double total = 0;

char customKey;
const byte ROWS = 4;
const byte COLS = 4;

char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {7,6,5,4};
byte colPins[COLS] = {3,2,1,0};

Keypad customKeypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS); 

String senhaAtual = "";
String senhaCorreta = "";

void setup()
{
  s.attach(pinoServo);
  s.write(90);
lcd.begin(16, 2);
lcd.setCursor(0,0);
  lcd.print("Sistema de");
  lcd.setCursor(0,1);
  lcd.print("seguranca");
delay(4000);
lcd.clear();
lcd.setCursor(0, 0);
}


void loop()
{

  customKey = customKeypad.getKey();
  switch(customKey) 
  {
  case '0' ... '9':
    lcd.setCursor(0,0);
    senhaAtual = senhaAtual + customKey;
    lcd.print(senhaAtual);
    break;
  case 'C':
    lcd.setCursor(0,0);
    senhaAtual = "";
    lcd.clear();
    break;
  case 'A':
    verificaSenha();
  }
  
}

void verificaSenha()
{
  	if (senhaCorreta == "") {
      	senhaCorreta = senhaAtual;
    	lcd.clear();
        lcd.print("Senha atualizada.");
      	delay(3000);
      	lcd.setCursor(0,0);
        senhaAtual = "";
        lcd.clear();
    } else {
      if (senhaAtual == senhaCorreta)
      {
          lcd.clear();
          s.write(0);
          lcd.print("Acesso concedido.");
          delay(3000);
          s.write(90);
          lcd.clear();
      }
      else
      {
          lcd.clear();
          lcd.print("Acesso negado.");
          delay(3000);
          lcd.clear();
      }
    }
}
```
