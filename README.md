# 🌡️ Projeto ESP32 + AHT10 – Monitoramento de Temperatura e Umidade

Este projeto utiliza um **ESP32** e o sensor **AHT10** para medir temperatura e umidade em tempo real, enviando os dados para o **Blynk**.  
Inclui também uma página HTML personalizada para visualização limpa e moderna do código.

---

## 🚀 Funcionalidades

- Leitura de **temperatura** e **umidade** (AHT10)
- Envio dos dados via **Blynk**
- Interface HTML elegante com:
  - 🌙 Tema claro/escuro  
  - 🔍 Zoom no código  
  - 📸 Exportação em PNG  
  - 📋 Copiar código com 1 clique  

---

## 🖥️ Pré-visualização do projeto online

🔗 **Acesse aqui:** (https://github.com/mirillo27982/visualizador-de-c-digo.git)

---

## 📦 Tecnologias e bibliotecas usadas

- ESP32  
- AHT10  
- Biblioteca Adafruit  
- WiFi.h  
- Blynk  
- HTML / CSS / JavaScript  
- html2canvas  

---

<!DOCTYPE html>
<html lang="pt-BR">

<head>
  <meta http-equiv="Content-Type" content="text/html; charset=windows-1252">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Visualizador de Código - Murillo Alves</title>

  <!-- Fontes -->
  <link href="./Visualizador de Código - Murillo Alves_files/css2" rel="stylesheet">

  <style>
    :root {
      --bg-dark: linear-gradient(135deg, #121212, #1a1a1a);
      --text-light: #e0e0e0;
      --code-bg-dark: #1e1e1e;
      --toolbar-bg-dark: #2a2a2a;
      --btn-bg-dark: #3a3a3a;
      --btn-hover-dark: #505050;
      --accent-color: #aaff66;
      --description-bg-dark: #223024;
      --description-text-dark: #c8ffdb;

      --bg-light: linear-gradient(135deg, #f6f6f6, #eaeaea);
      --text-dark: #1a1a1a;
      --code-bg-light: #ffffff;
      --toolbar-bg-light: #e0e0e0;
      --btn-bg-light: #cccccc;
      --btn-hover-light: #bbbbbb;
      --description-bg-light: #e3f7dc;
      --description-text-light: #2a3a21;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: 'Inter', sans-serif;
      color: var(--text-light);
      background: var(--bg-dark);
      transition: background 0.3s, color 0.3s;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
      padding: 20px;
    }

    body.light {
      background: var(--bg-light);
      color: var(--text-dark);
    }

    .top-bar {
      width: 100%;
      max-width: 900px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }

    .logo-container img {
      height: 45px;
      transition: transform 0.5s, filter 0.3s;
    }

    .logo-container img:hover {
      transform: scale(1.1);
    }

    .theme-toggle {
      display: flex;
      align-items: center;
      cursor: pointer;
      font-weight: 600;
      font-size: 14px;
      border: none;
      padding: 7px 12px;
      border-radius: 6px;
      transition: all 0.3s;
      user-select: none;
      background: var(--btn-bg-dark);
      color: var(--text-light);
    }

    body.light .theme-toggle {
      background: var(--btn-bg-light);
      color: var(--text-dark);
    }

    .theme-toggle span {
      margin-left: 8px;
    }

    h1 {
      font-weight: 600;
      font-size: 28px;
      margin-bottom: 5px;
      color: var(--accent-color);
      text-shadow: 0 0 10px rgba(170, 255, 102, 0.4);
      transition: color 0.3s, text-shadow 0.3s;
    }

    body.light h1 {
      color: var(--text-dark);
      text-shadow: none;
    }

    p.subtitle {
      margin: 0 0 30px;
      font-size: 15px;
      color: #888;
    }

    body.light p.subtitle {
      color: #555;
    }

    .description-box {
      max-width: 900px;
      width: 90vw;
      background: var(--description-bg-dark);
      color: var(--description-text-dark);
      border-radius: 14px;
      padding: 20px;
      box-shadow: 0 0 20px rgba(170, 255, 102, 0.2);
      font-size: 16px;
      margin-bottom: 40px;
      transition: all 0.3s;
    }

    body.light .description-box {
      background: var(--description-bg-light);
      color: var(--description-text-light);
      box-shadow: 0 0 15px rgba(0, 128, 0, 0.2);
    }
    .container {
      max-width: 900px;
      width: 90vw;
      background: var(--code-bg-dark);
      border-radius: 14px;
      padding: 20px;
      box-shadow: 0 0 25px rgba(170, 255, 102, 0.3);
      border: 1px solid #333;
      opacity: 0;
      transform: translateY(20px);
      animation: fadeInUp 0.7s forwards;
      transition: all 0.3s;
      position: relative;
    }

    @keyframes fadeInUp {
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .toolbar {
      display: flex;
      justify-content: flex-end;
      gap: 10px;
      background: var(--toolbar-bg-dark);
      padding: 10px 15px;
      border-radius: 8px;
      margin-bottom: 15px;
      border: 1px solid #333;
      position: sticky;
      top: 10px;
      z-index: 10;
      transition: all 0.3s;
    }

    pre {
      text-align: left;
      margin: 0;
      padding: 20px;
      border-radius: 12px;
      overflow-x: auto;
      border: 1px solid #333;
      box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.3);
      font-family: 'Fira Code', monospace;
      font-size: 15px;
      line-height: 1.5em;
      white-space: pre;
      background: transparent;
      color: var(--description-text-dark);
    }
  </style>
</head>

<body>

  <div class="top-bar">
    <div class="logo-container">
      <img src="./Visualizador de Código - Murillo Alves_files/SENAI_São_Paulo_logo.png" alt="Logo SENAI">
    </div>
    <button class="theme-toggle" id="theme-toggle">&#9728;&#65039;<span>Tema Claro</span></button>
  </div>

  <h1>Desenvolvido por "Murillo Alves"</h1>
  <p class="subtitle">nas aulas de Sistemas Embarcados</p>

  <div class="description-box">
    Este projeto utiliza um sensor AHT10 para medir temperatura e umidade, enviando os dados para o app Blynk via ESP32. Permite monitoramento remoto em tempo real.
  </div>

  <div class="container" id="code-container">

    <div class="toolbar">
      <button id="copy-btn">&#128203; Copiar Código</button>
      <button id="export-png-btn">&#128444; Exportar PNG</button>
    </div>

    <pre><code id="code-content">
#define BLYNK_TEMPLATE_ID "TMPL2IJ0DFQyk"
#define BLYNK_TEMPLATE_NAME "AHT10"
#define BLYNK_AUTH_TOKEN "4e5AIKGIx146aUn5QJH4wdDOVzCdEw5j"

#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <WiFiClient.h>
#include <BlynkSimpleEsp32.h>
#include <Adafruit_AHT10.h>

Adafruit_AHT10 aht;
Adafruit_Sensor *aht_humidity, *aht_temp;

char ssid[] = "senai109";
char pass[] = "senai109";

BlynkTimer timer;

float umidade;
float temperatura;

void EnviaValor() {
  Blynk.virtualWrite(V0, umidade);
  Blynk.virtualWrite(V1, temperatura);
}

void setup() {
  Serial.begin(115200);
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  timer.setInterval(1000L, EnviaValor);

  if (!aht.begin()) {
    while (1) delay(10);
  }

  aht_temp = aht.getTemperatureSensor();
  aht_humidity = aht.getHumiditySensor();
}

void loop() {
  sensors_event_t humidity;
  sensors_event_t temp;

  aht_humidity->getEvent(&humidity);
  aht_temp->getEvent(&temp);

  umidade = humidity.relative_humidity;
  temperatura = temp.temperature;

  Blynk.run();
  timer.run();
}
    </code></pre>

  </div>

  <script src="./Visualizador de Código - Murillo Alves_files/html2canvas.min.js"></script>
  <script>
    const body = document.body;
    const themeToggleBtn = document.getElementById('theme-toggle');
    const copyBtn = document.getElementById('copy-btn');
    const exportBtn = document.getElementById('export-png-btn');
    const codeContent = document.getElementById('code-content');
    const codeContainer = document.getElementById('code-container');

    themeToggleBtn.addEventListener('click', () => {
      body.classList.toggle('light');
    });

    copyBtn.addEventListener('click', () => {
      navigator.clipboard.writeText(codeContent.innerText);
    });

    exportBtn.addEventListener('click', () => {
      html2canvas(codeContainer).then(canvas => {
        const link = document.createElement('a');
        link.download = 'codigo_murillo.png';
        link.href = canvas.toDataURL();
        link.click();
      });
    });
  </script>

</body>
</html>

---

## 👨‍💻 Desenvolvido por

**Murillo Alves**  
Aluno de Mecatronica – SENAI  
