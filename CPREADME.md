# Projeto de Monitoramento de Luminosidade Vinheria Agnello com Arduino

Projeto feito para monitoramento de luminosidade em vinheria, tendo como proposito determinar se a condição de luz esta ok, em estado de alarme, ou em problema.

## Componentes utilizados

- Arduino
- Sensor LDR
- LEDs
- Buzzer
- Resistor
- Breadboard e fios de conexão

## Funcionalidade

O código Arduino incluído neste projeto realiza as seguintes ações:

- Faz a leitura da luminosidade através do LDR
- Controla três LEDs (verde, amarelo e vermelho) para indicar o estado da luminosidade.
- Ativa um buzzer com uma frequência de 1000 Hz para sinalizar condições de Alerta e Problema.
- Duração do som do buzzer: 3 segundos.
- Pausa do buzzer após o som: 1 segundo.
- Reativa o buzzer após a pausa, se necessário.

## Configuração

Antes de iniciar o projeto, você deve montar o circuito conforme as instruções fornecidas no código Arduino e conectar todos os componentes de acordo com o esquema.

## Uso

1. Faça o upload do código Arduino para o seu dispositivo Arduino.
2. Alimente o sistema e observe o comportamento dos LEDs e do buzzer de acordo com a luminosidade ambiente.
3. A interpretação dos LEDs é a seguinte:
   - LED verde: Luminosidade até 25%
   - LED amarelo: Luminosidade entre 25% e 50%
   - LED vermelho: Luminosidade acima de 50%
4. O buzzer será ativado nas condições de Alerta e Problema.
5. Os LEDs indicarão o estado atual da luminosidade enquanto o programa estiver em execução.

## Projeto se encontra finalizado, feito por Sergio Cristian.

## Codigo abaixo.

const int ldrPin = A0;
const int greenLedPin = 2;
const int yellowLedPin = 3;
const int redLedPin = 4;
const int buzzerPin = 5;

int ldrValue = 0;
int thresholdGreen = 256;
int thresholdYellow = 512;
unsigned long buzzerStartTime = 0;
const unsigned long buzzerDuration = 3000;
const unsigned long buzzerPauseDuration = 1000;
bool buzzerOn = false; 

void setup() {
  pinMode(greenLedPin, OUTPUT);
  pinMode(yellowLedPin, OUTPUT);
  pinMode(redLedPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
}

void loop() {
  ldrValue = analogRead(ldrPin);

  if (ldrValue <= thresholdGreen) {
   
    digitalWrite(greenLedPin, HIGH);
    digitalWrite(yellowLedPin, LOW);
    digitalWrite(redLedPin, LOW);
    noTone(buzzerPin);
    buzzerOn = false;
  } else if (ldrValue <= thresholdYellow) {
    
    digitalWrite(greenLedPin, LOW);
    digitalWrite(yellowLedPin, HIGH);
    digitalWrite(redLedPin, LOW);
    
    if (!buzzerOn) {
      tone(buzzerPin, 1000);
      buzzerStartTime = millis();
      buzzerOn = true;
    }
    
    if (millis() - buzzerStartTime >= buzzerDuration) {
      noTone(buzzerPin);  
      if (millis() - buzzerStartTime >= buzzerDuration + buzzerPauseDuration) {
        buzzerOn = false;  
      }
    }
  } else {
    
    digitalWrite(greenLedPin, LOW);
    digitalWrite(yellowLedPin, LOW);
    digitalWrite(redLedPin, HIGH);
    
    if (!buzzerOn) {
      tone(buzzerPin, 1000);  
      buzzerStartTime = millis();
      buzzerOn = true;
    }
    
    if (millis() - buzzerStartTime >= buzzerDuration) {
      noTone(buzzerPin);  
      if (millis() - buzzerStartTime >= buzzerDuration + buzzerPauseDuration) {
        buzzerOn = false; 
      }
    }
  }
}
