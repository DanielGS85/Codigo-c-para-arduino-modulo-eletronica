 // Dado eletronico com Arduino Uno
 // Sorteia um numero de 1 a 6 ao pressionar o botao

const int buttonPin = 10;

// Pinos adaptados conforme o ultimo print
const int segmentPins[7] = {6, 7, 8, 2, 3, 4, 5};

// 1 = segmento aceso
const byte digits[6][8] = {
// 6, 7, 8, 2, 3, 4, 5, DP
  {0, 0, 1, 1, 0, 0, 0, 0}, // 1
  {1, 1, 0, 1, 1, 0, 1, 0}, // 2
  {0, 1, 1, 1, 1, 0, 1, 0}, // 3
  {0, 0, 1, 1, 0, 1, 1, 0}, // 4
  {0, 1, 1, 0, 1, 1, 1, 0}, // 5
  {1, 1, 1, 0, 1, 1, 1, 0}, // 6
};
void setup() {
  // Configura os segmentos como saida
  for (int i = 0; i < 7; i++) {
    pinMode(segmentPins[i], OUTPUT);
    digitalWrite(segmentPins[i], LOW);
  }

  // Botao com resistor externo de pull-down
  pinMode(buttonPin, INPUT);

  // Inicializa o sorteio
  randomSeed(analogRead(A0));

  Serial.begin(9600);
  Serial.println("Dado eletronico pronto!");

  // Exibe o numero inicial
  exibirNumero(1);
}

void exibirNumero(int numero) {
  for (int i = 0; i < 7; i++) {
    digitalWrite(segmentPins[i], digits[numero - 1][i]);
  }
}

void loop() {
  // HIGH significa que o botao foi pressionado
  if (digitalRead(buttonPin) == HIGH) {
    delay(50);

    // Confirma o pressionamento
    if (digitalRead(buttonPin) == HIGH) {
      int numero = random(1, 7);

      exibirNumero(numero);

      Serial.print("Resultado do dado: ");
      Serial.println(numero);

      // Aguarda o botao ser solto
      while (digitalRead(buttonPin) == HIGH) {
        delay(10);
      }

      delay(50);
    }
  }
}

