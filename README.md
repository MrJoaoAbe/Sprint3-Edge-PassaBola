# Sprint3-Edge-PassaBola

## Sistema de monitoramento de batimentos cardíacos para atletas

### Batimentos Cardiacos e o esporte
Medir os batimentos por minuto (BPM) do coração é fundamental para atletas, pois fornece informações diretas sobre o desempenho do sistema cardiovascular. Durante os treinos, o monitoramento da frequência cardíaca ajuda a identificar se o esforço está dentro da zona ideal para o objetivo — seja queimar gordura, melhorar o condicionamento aeróbico ou aumentar a resistência. Além disso, acompanhar o BPM em repouso pode indicar evolução no condicionamento físico: atletas bem treinados costumam apresentar uma frequência mais baixa em repouso, sinal de maior eficiência do coração.

### Descrição do Trabalho
O trabalho consiste em criar um dispositivo que **monitora o BPM** de uma atleta enviando os dados para uma **página WEB**, facilitando a medição através de um **gráfico de linhas**. Além disto possui um sensor de excesso de BPM, se ultrapassar este limite ativa uma **led Vermelha** e um **sensor sonoro**. Se o BPM estiver controlado apenas a **led Verde** ficará acesa.

### Testes 
- O projeto foi testado uma parte no Wokwi
- Mas foi projetado em um arduino de forma presencial

### Tecnologias Utilizadas
- Arduino IDE
- Postman
- Javascript
- Tinkercad
- Wokwi
- Visual Studio Code
- Node-RED
- GitHub

## 🎥 Link para Vídeo

> ([[Link do vídeo no youtube](https://youtu.be/DaaeMRpnFRI?si=da0R8Zpry1-qpj2i)]
---

### Materiais Utilizados
- ESP32
- Sensor de Pulso (Pulse Sensor) → conectado ao pino A0
- LED Verde (pino 10) → indica estado normal
- LED Vermelho (pino 9) → indica alerta
- Buzzer piezoelétrico (pino 11) → emite alarme sonoro
- Resistores (220Ω) para os LEDs
- Módulo I2C para LCD (PCF8574) → facilita a conexão com SDA/SCL
- Módulo RTC DS3231 (com bateria CR2032 para manter hora mesmo desligado)
- Protoboard
- Jumpers (macho-macho / macho-fêmea)
- Fonte de alimentação USB do Arduino (ou powerbank)

### Bibliotecas utilizadas no código**
- PulseSensorPlayground.h → leitura do sensor de batimentos
- ArduinoJson.h → criação de JSON para envio ao Node-RED
- Wire.h → comunicação I2C
- RTClib.h → leitura do relógio de tempo real DS3231

### Como montar o projeto? 
1. **Sensor de Pulso**

- VCC → 5V do Arduino

- GND → GND do Arduino

- Sinal → A0 do Arduino

2. **LEDs**

- LED Verde → pino 10 (em série com resistor 220Ω até o GND)

- LED Vermelho → pino 9 (em série com resistor 220Ω até o GND)

3. **Buzzer**

- Pino positivo (+) → pino 11 do Arduino

- Pino negativo (–) → GND

4. **RTC DS3231**

- Conecta também via I2C (pode compartilhar os pinos com o LCD):

- GND → GND

- VCC → 5V

- SDA → A4

- SCL → A5

**Passos de Montagem**

- Coloque o Arduino UNO na protoboard ou só use os jumpers.

- Conecte o sensor de pulso (A0, 5V, GND).

- Coloque os LEDs na protoboard com resistores e conecte aos pinos 9 e 10.

- Ligue o buzzer ao pino 11 e GND.

- Conecte o RTC DS3231 também aos mesmos pinos SDA e SCL.

- Alimente tudo com o cabo USB no Arduino.

**Programação**

- No Arduino IDE, instale as bibliotecas necessárias:

- PulseSensor Playground

- ArduinoJson

- RTClib

**Carregue o código**

- Abra o Serial Monitor para verificar os JSONs enviados para o Node-RED.

### Integrantes do Grupo
- Eduardo Santiago Bassan — RM: 561474
- Vitor Fernandes dos Santos — RM: 566275
- Henry Andrade Browne — RM: 562622
- João Victor de Souza Abe — RM: 561446

### Código Arduino – Monitor Cardíaco

```cpp
#include <PulseSensorPlayground.h>
#include <ArduinoJson.h>
#include <Wire.h>
#include <RTClib.h>

#define pulsePin A0 // Pino de sinal do sensor
int ledVerde = 10;
int ledVermelha = 9;
int buzzer = 11;

// Objeto PulseSensor
PulseSensorPlayground pulseSensor;

// Variáveis BPM
int BPM = 0;
int ultimoBPM = 0;
int LimitarBpm = 150;

// Relógio
RTC_DS1307 rtc;

void setup() {
  pinMode(ledVerde, OUTPUT);
  pinMode(ledVermelha, OUTPUT);
  pinMode(buzzer, OUTPUT);

  Serial.begin(9600);

  pulseSensor.analogInput(pulsePin);
  pulseSensor.setThreshold(513); // Ajuste conforme necessário
  pulseSensor.begin();

  if (!rtc.begin()) {
    Serial.println("Erro ao iniciar RTC!");
    while (1);
  }

  // Ajusta hora do RTC na primeira vez (comente depois de setar!)
  // rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
}

void loop() {
  // Detectar Pulsação
  BPM = pulseSensor.getBeatsPerMinute();
  bool pulsoDetectado = pulseSensor.sawStartOfBeat();

  // Atualiza último BPM válido
  if (pulsoDetectado && BPM > 0) {
    ultimoBPM = BPM;
  }

  // Obter Hora
  DateTime agora = rtc.now();

  // Criar JSON
  StaticJsonDocument<256> doc;
  doc["BPM"] = ultimoBPM;

  // Formatando o Timestamp
  char timestamp[16];
  sprintf(timestamp, "%04d%02d%02d%02d%02d",
          agora.year(),
          agora.month(),
          agora.day(),
          agora.hour(),
          agora.minute());

  doc["timestamp"] = timestamp;

  String output;
  serializeJson(doc, output);
  Serial.println(output); // Enviar para o NODE-RED

  // Controle de LEDs e buzzer
  if (pulsoDetectado && ultimoBPM > 0) {
    if (ultimoBPM < LimitarBpm) {
      digitalWrite(ledVerde, HIGH);
      digitalWrite(ledVermelha, LOW);
      noTone(buzzer);
      Serial.println("Batidas controladas");
    } else {
      digitalWrite(ledVerde, LOW);
      digitalWrite(ledVermelha, HIGH);
      tone(buzzer, 400);
      Serial.println("Batidas acima do esperado!");
    }
  }

  delay(100); // Loop rápido para leituras mais constantes
}

``` 
### Código HTML / JS – Gráfico Cardíaco

```cpp
<section>
  <div id="grafico">
    <h1>Monitorar Batimentos Cardiacos</h1>

    <canvas id="chartBPM" width="600" height="300"></canvas>

    <div>
      <p>Batimentos: <span id="BPM">--</span> por Minuto</p>
      <p>Data e Hora: <span id="timestamp">--</span></p>
    </div>
  </div>
</section>

<!-- Importando o Chart.js -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<script>
  // Configuração inicial do gráfico
  const ctx = document.getElementById('chartBPM').getContext('2d');
  const chartBPM = new Chart(ctx, {
    type: 'line',
    data: {
      labels: [], // timestamps
      datasets: [{
        label: 'Batimentos por Minuto (BPM)',
        data: [], // valores BPM
        borderColor: 'red',
        backgroundColor: 'rgba(255, 99, 132, 0.2)',
        borderWidth: 2,
        tension: 0.2 // curva mais suave
      }]
    },
    options: {
      responsive: true,
      scales: {
        x: {
          title: { display: true, text: 'Tempo' }
        },
        y: {
          beginAtZero: true,
          title: { display: true, text: 'BPM' }
        }
      }
    }
  });

  // Função para buscar os dados
  function atualizarDados() {
    fetch('http://localhost:1880/dados') 
      .then(response => response.json())
      .then(data => {
        console.log("JSON recebido:", data);

        // Atualiza os elementos no HTML
        document.getElementById('BPM').textContent = data.BPM;
        document.getElementById('timestamp').textContent = data.timestamp;

        // Adiciona novos dados ao gráfico
        chartBPM.data.labels.push(data.timestamp); 
        chartBPM.data.datasets[0].data.push(data.BPM);

        // Mantém apenas os últimos 20 pontos (evitar gráfico infinito)
        if (chartBPM.data.labels.length > 20) {
          chartBPM.data.labels.shift();
          chartBPM.data.datasets[0].data.shift();
        }

        chartBPM.update();
      })
      .catch(e => {
        console.log("Falha ao buscar dados:", e);
      });
  }

  // Atualiza a cada 5 segundos
  setInterval(atualizarDados, 5000);

  // Chamada inicial
  atualizarDados();
</script>
