# Sprint3-Edge-PassaBola

## Sistema de monitoramento de batimentos cardíacos para atletas

### Batimentos Cardiacos e o esporte
Medir os batimentos por minuto (BPM) do coração é fundamental para atletas, pois fornece informações diretas sobre o desempenho do sistema cardiovascular. Durante os treinos, o monitoramento da frequência cardíaca ajuda a identificar se o esforço está dentro da zona ideal para o objetivo — seja queimar gordura, melhorar o condicionamento aeróbico ou aumentar a resistência. Além disso, acompanhar o BPM em repouso pode indicar evolução no condicionamento físico: atletas bem treinados costumam apresentar uma frequência mais baixa em repouso, sinal de maior eficiência do coração.

### Descrição do Trabalho
O trabalho consiste em criar um dispositivo que **monitora o BPM** de uma atleta enviando os dados para uma **página WEB**, facilitando a medição através de um **gráfico de linhas**. Além disto possui um sensor de excesso de BPM, se ultrapassar este limite ativa uma **led Vermelha** e um **sensor sonoro**. Se o BPM estiver controlado apenas a **led Verde** ficará acesa.

### Testes 
- O projeto foi testado uma parte no Wokwi
- Foi projetado em um esp32 de forma presencial

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

## Como iniciar um servidor AWS - EC2

### Site AWS - EC2
- Entre no link - https://aws.amazon.com/pt/ec2/,
- Inicie uma instancia ,
- Dê um nome à maquina virtual,
- Selecione Ubuntu como imagem,
- E o tipo t3 como instancia,
- Crie uma par de chaves no formato **PPK**,
- E indique o quanto de memória é necessário na MV.
- Em seguida vá em editar regras de entrada e configure as seguintes portas:
**1883, 1026, 4041, 8666, 27017 e o ICMP para IPV4**
![Portas a serem liberadas](/PORTAS_A_SEREM_LIBERADAS_EC2.png "Portas a serem liberadas")
- **SALVE O IP**

### PuTTY
- Dentro do PUTTY insira o **IP** e a **CHAVE**
![Exemplo de como deve ser preenchido o IP](/puTTY%20inicial.png "Exemplo de como deve ser preenchido o IP")
![Exemplo de como deve ser preenchido a chave](/puTTY%20chave.png.png "Exemplo de como deve ser preenchido a Chave")
- Após isso clique em OPEN e siga os seguintes passos para iniciar o BROCKER
  - sudo apt update 
  - sudo apt-get install net-tools 
  - ifconfig 
  - sudo apt install git
  - sudo apt update
  - sudo apt install apt-transport-https ca-certificates curl software-properties-common
  - curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
  - sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
  - sudo apt update
  - apt-cache policy docker-ce
  - sudo apt install docker-ce
  - sudo systemctl status docker
  - git clone https://github.com/fabiocabrini/fiware
  - cd fiware
  - sudo docker compose up -d 
  - sudo docker stats 

### Postman
- Baixe este arquivo JSON → https://github.com/fabiocabrini/fiware/blob/main/FIWARE%20Descomplicado.postman_collection.json 
- Abra o arquivo JSON dentro do **POSTMAN**
- Substitua o placeholder **URL** pelo **IP** do servidor
- Faça isso para os três arquivos **(GET)** presentes no **POSTMAN**
![Exemplo HEALTHCHECK](/POSTMAN.png "Exemplo de Healthcheck")

### NODE-RED
![NODERED](/NODERED.png "NodeRED")
- BLOCO 1 / MQTT IN
  - Servidor = IP:1883
  - Tópico = /TEF/device001/attrs/p
- BLOCO 2 / JSON
- BLOCO 3 / WRITE FILE
  - Caminho = bpm.txt
  - Ação = Sobrescrever Arquivo
- BLOCO 4 / CHANGE
  - NOME = BPM
  - msg.payload
  - msg.payload.BPM
- BLOCO 5 / CHANGE
  - NOME = timestamp
  - msg.payload
  - msg.payload.timestamp
- BLOCO 6 / HTTP IN
  - Método = GET
  - URL = /bpm
- BLOCO 7 / READ FILE
  - bpm.txt
- BLOCO 8 / HTTP RESPONSE

### Materiais Utilizados
- ESP32
- Sensor de Pulso (Pulse Sensor) → conectado ao pino A0
- LED Verde (pino 10) → indica estado normal
- LED Vermelho (pino 9) → indica alerta
- Buzzer piezoelétrico (pino 11) → emite alarme sonoro
- Resistores (220Ω) para os LEDs
- Módulo RTC DS3231 (com bateria CR2032 para manter hora mesmo desligado)
- Protoboard
- Jumpers (macho-macho / macho-fêmea)
- Fonte de alimentação USB do Arduino (ou powerbank)

### Bibliotecas utilizadas no código**
- PulseSensorPlayground.h → leitura do sensor de batimentos
- ArduinoJson.h → criação de JSON para envio ao Node-RED
- Wire.h → comunicação I2C
- RTClib.h → leitura do relógio de tempo real DS3231
- WiFi.h → Para conectar à rede
- PubSubClient.h → Para conectar ao servidor

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

- Placa -> DOIT ESP32 DEVKIT V1
`
**Configure o código para conectar no servidor**
````cpp
const char* default_SSID = "Wokwi-GUEST"; // Nome da rede Wi-Fi
const char* default_PASSWORD = ""; // Senha da rede Wi-Fi
const char* default_BROKER_MQTT = "3.144.236.56"; // IP do Broker MQTT
const int default_BROKER_PORT = 1883; // Porta do Broker MQTT
const char* default_TOPICO_SUBSCRIBE = "/TEF/device001/cmd"; // Tópico MQTT de escuta
const char* default_TOPICO_PUBLISH_1 = "/TEF/device001/attrs"; // Tópico MQTT de envio de informações para Broker
const char* default_TOPICO_PUBLISH_2 = "/TEF/device001/attrs/p"; // Tópico MQTT de envio de informações para Broker
const char* default_ID_MQTT = "fiware_001"; // ID MQTT
const int default_D4 = 2; // Pino do LED onboard
// Declaração da variável para o prefixo do tópico
const char* topicPrefix = "device001";

// Variáveis para configurações editáveis
char* SSID = const_cast<char*>(default_SSID);
char* PASSWORD = const_cast<char*>(default_PASSWORD);
char* BROKER_MQTT = const_cast<char*>(default_BROKER_MQTT);
int BROKER_PORT = default_BROKER_PORT;
char* TOPICO_SUBSCRIBE = const_cast<char*>(default_TOPICO_SUBSCRIBE);
char* TOPICO_PUBLISH_1 = const_cast<char*>(default_TOPICO_PUBLISH_1);
char* TOPICO_PUBLISH_2 = const_cast<char*>(default_TOPICO_PUBLISH_2);
char* ID_MQTT = const_cast<char*>(default_ID_MQTT);
````

**Carregue o código**

- Abra o Serial Monitor para verificar os JSONs enviados para o Node-RED.

### Integrantes do Grupo
- Eduardo Santiago Bassan — RM: 561474
- Vitor Fernandes dos Santos — RM: 566275
- Henry Andrade Browne — RM: 562622
- João Victor de Souza Abe — RM: 561446

### Código ESP32 / Versão FINAL – Monitor Cardíaco

```cpp
#include <ArduinoJson.h> 
#include <RTClib.h>
#include <Wire.h>
//Server
#include <WiFi.h>
#include <PubSubClient.h>

// Bibliotecas para conectar ao servidor
const char* default_SSID = "Wokwi-GUEST"; // Nome da rede Wi-Fi
const char* default_PASSWORD = ""; // Senha da rede Wi-Fi
const char* default_BROKER_MQTT = "3.144.236.56"; // IP do Broker MQTT
const int default_BROKER_PORT = 1883; // Porta do Broker MQTT
const char* default_TOPICO_SUBSCRIBE = "/TEF/device001/cmd"; // Tópico MQTT de escuta
const char* default_TOPICO_PUBLISH_1 = "/TEF/device001/attrs"; // Tópico MQTT de envio de informações para Broker
const char* default_TOPICO_PUBLISH_2 = "/TEF/device001/attrs/p"; // Tópico MQTT de envio de informações para Broker
const char* default_ID_MQTT = "fiware_001"; // ID MQTT
const int default_D4 = 2; // Pino do LED onboard
// Declaração da variável para o prefixo do tópico
const char* topicPrefix = "device001";

// Variáveis para configurações editáveis
char* SSID = const_cast<char*>(default_SSID);
char* PASSWORD = const_cast<char*>(default_PASSWORD);
char* BROKER_MQTT = const_cast<char*>(default_BROKER_MQTT);
int BROKER_PORT = default_BROKER_PORT;
char* TOPICO_SUBSCRIBE = const_cast<char*>(default_TOPICO_SUBSCRIBE);
char* TOPICO_PUBLISH_1 = const_cast<char*>(default_TOPICO_PUBLISH_1);
char* TOPICO_PUBLISH_2 = const_cast<char*>(default_TOPICO_PUBLISH_2);
char* ID_MQTT = const_cast<char*>(default_ID_MQTT);
int D4 = default_D4;

WiFiClient espClient;
PubSubClient MQTT(espClient);
char EstadoSaida = '0';

int LED_VERMELHA = 4;
int LED_VERDE = 16;
int SENSOR_CARDIACO_POT = 34;
int buzzer = 14; 

int VALOR_BRUTO_SENSOR = 0;
int VALOR_CONVERTIDO_BPM = 0;

// Relógio
RTC_DS1307 rtc;

void initSerial() {
  Serial.begin(9600);
}

void reconectWiFi() {
  if (WiFi.status() == WL_CONNECTED) return;
  WiFi.begin(SSID, PASSWORD);
  while (WiFi.status() != WL_CONNECTED) {
    delay(100);
    Serial.print(".");
  }
  Serial.println("\nConectado ao Wi-Fi. IP: " + WiFi.localIP().toString());
  digitalWrite(D4, LOW);
}

void initWiFi() {
  delay(10);
  Serial.println("------Conexao WI-FI------");
  reconectWiFi();
}

void initMQTT() {
  MQTT.setServer(BROKER_MQTT, BROKER_PORT);
  MQTT.setCallback(mqtt_callback);
}

void mqtt_callback(char* topic, byte* payload, unsigned int length) {
  String msg;
  for (int i = 0; i < length; i++) msg += (char)payload[i];
  Serial.println("Mensagem recebida: " + msg);

  if (msg.equals(String(topicPrefix) + "@on|")) {
    digitalWrite(D4, HIGH);
    EstadoSaida = '1';
  }
  if (msg.equals(String(topicPrefix) + "@off|")) {
    digitalWrite(D4, LOW);
    EstadoSaida = '0';
  }
}


void reconnectMQTT() {
  while (!MQTT.connected()) {
    Serial.print("Tentando conectar ao broker...");
    if (MQTT.connect(ID_MQTT)) {
      Serial.println("Conectado!");
      MQTT.subscribe(TOPICO_SUBSCRIBE);
    } else {
      Serial.println("Falha. Tentando em 2s.");
      delay(2000);
    }
  }
}

void VerificaConexoesWiFIEMQTT() {
  if (!MQTT.connected()) reconnectMQTT();
  reconectWiFi();
}

void setup() {
  initSerial();

  pinMode(LED_VERDE, OUTPUT);
  pinMode(LED_VERMELHA, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(SENSOR_CARDIACO_POT, INPUT);

  initWiFi();
  initMQTT();

  if (!rtc.begin()) {
    Serial.println("Erro ao iniciar RTC!");
    while (1);
  }

  // Ajusta hora do RTC na primeira vez (comente depois de setar!)
  // rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
}

void loop() {
  VerificaConexoesWiFIEMQTT();
  MQTT.loop();

  // Simula a leitura do batimento cardíaco
  VALOR_BRUTO_SENSOR = analogRead(SENSOR_CARDIACO_POT);
  VALOR_CONVERTIDO_BPM = map(VALOR_BRUTO_SENSOR, 0, 4095, 0, 220); //Batimento por minuto

  // Obter Hora
  DateTime agora = rtc.now();

  //Criar JSON
  StaticJsonDocument<256> doc;
  doc["BPM"] = VALOR_CONVERTIDO_BPM;

  // Formatando o Timestamp
  char timestamp[16];
  sprintf(timestamp, "%04d%02d%02d%02d%02d",
          agora.year(),
          agora.month(),
          agora.day(),
          agora.hour(),
          agora.minute());

  doc["timestamp"] = timestamp;

char buffer[256];
serializeJson(doc, buffer);
Serial.println(buffer); // Enviar para o NODE-RED

// Mensagem de Saída
//String mensagem = "BPM: " + String(VALOR_CONVERTIDO_BPM) + " medido as " + String(timestamp);

// Publica no broker
MQTT.publish(TOPICO_PUBLISH_2, buffer);
Serial.print("Enviado: ");
Serial.println(buffer);


  // Controle de LEDs e buzzer
if (VALOR_CONVERTIDO_BPM > 0) {
  if (VALOR_CONVERTIDO_BPM < 110) {
    digitalWrite(LED_VERDE, HIGH);
    digitalWrite(LED_VERMELHA, LOW);
    noTone(buzzer);
    delay(1000);
    Serial.println("Batidas controladas");
  //  Serial.println(mensagem);
  } else {
    digitalWrite(LED_VERDE, LOW);
    digitalWrite(LED_VERMELHA, HIGH);
    tone(buzzer, 400);
    delay(1000);
    Serial.println("Batidas acima do esperado!");
  //  Serial.println(mensagem);
  }
}

  delay(1000); // Loop rápido para leituras mais constantes
}

```

### Código Arduino / Primeira versão – Monitor Cardíaco

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
