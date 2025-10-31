# Projeto Semáforo Offline

## Contexto

Você começou a estagiar no **Departamento de Engenharia de Trânsito** e ficou responsável por controlar o fluxo em uma via movimentada do bairro **Butantã**.  
Seu desafio foi montar e programar um **semafóro** que garanta a segurança de pedestres e veículos, seguindo a lógica de tempo de cada fase das luzes, desde a montagem dos LEDs até a programação da sequência correta.

---

## Parte 1 — Montagem Física do Semáforo

A montagem foi feita em **protoboard**, utilizando **LEDs, resistores e fios jumpers**, conectados a um **ESP32**.  
As cores e disposição seguem o padrão de um semáforo real:

| Cor do LED | Função | Pino no ESP32 | Resistência (Ω) | 
|:--|:--|:--:|:--:|
| Vermelho | Parar | 27 | 220 | 
| Verde | Siga | 33 | 220 | 
| Amarelo | Atenção | 25 | 220 | 

### Demonstração do funcionamento

[Vídeo de demonstração](https://youtube.com/shorts/HyW6deyOSlo?feature=share)

<img src='Foto semaforo.jpeg' alt='Foto do protótio físico do Semáforo' height=500>


**Descrição da montagem:**
- Cada LED está conectado a um pino digital do ESP32 por meio de um resistor de 220 Ω.  
- O terminal negativo dos LEDs está conectado ao **GND** comum da protoboard.  
- Os fios foram organizados para facilitar a identificação das cores e evitar curto-circuitos.

---

## Parte 2 — Programação e Lógica do Semáforo

A programação foi feita em **C++**, utilizando a função `millis()` para controle de tempo **sem travar o loop principal** (diferente do uso de `delay()`), e **ponteiros** para manipulação do tempo dinamicamente.

### Lógica das fases

| Fase | Cor Ativa | Duração | Descrição | 
|:--|:--|:--:|:--|
| 1 | Vermelho | 6s | Interrompe o fluxo e permite travessia de pedestres |
| 2 | Verde | 4s | Libera o trânsito de veículos |
| 3 | Amarelo | 2s | Alerta para a troca de sinal |

### Código Utilizado

```cpp
unsigned long ultimoTempoLeitura = 0;         // armazena o último tempo
unsigned long* pUltimoTempo = &ultimoTempoLeitura; // ponteiro para a variável de tempo

void setup() {
  pinMode(27, OUTPUT); // LED vermelho
  pinMode(25, OUTPUT); // LED amarelo
  pinMode(33, OUTPUT); // LED verde
}

void loop() {
  unsigned long tempoAtual = millis();           // tempo atual em ms
  unsigned long tempoDecorrido = tempoAtual - *pUltimoTempo; // tempo desde o último reset

  if (tempoDecorrido < 6000) {          // até 6s
    digitalWrite(27, HIGH);             // liga vermelho
    digitalWrite(33, LOW);              // desliga verde
    digitalWrite(25, LOW);              // desliga amarelo
  }
  else if (tempoDecorrido < 10000) {    // de 6s a 10s
    digitalWrite(27, LOW);
    digitalWrite(33, HIGH);
    digitalWrite(25, LOW);
  }
  else if (tempoDecorrido < 12000) {    // de 10s a 12s
    digitalWrite(27, LOW);
    digitalWrite(33, LOW);
    digitalWrite(25, HIGH);
  }
  else {                                // após 12s, reinicia
    *pUltimoTempo = tempoAtual;         // reinicia o tempo via ponteiro
  }
}
```
---

### Avaliação em pares

#### Avaliador 1 - Rafael Cabral

| Critério | Contempla (Pontos) | Observações do Avaliador |
|:--|:--:|:--|
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores | 3 |  |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo | 3 |  |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) | 3 |  |
| Ir além: Implementou um componente extra, fez com `millis()` ao invés de `delay()` e/ou usou ponteiros no código | 1 | Código limpo |


#### Avaliador 2 - Isabela Peçanha

| Critério | Contempla (Pontos) | Observações do Avaliador |
|:--|:--:|:--|
| Montagem física com cores corretas, boa disposição dos fios e uso adequado de resistores | 3 |  |
| Temporização adequada conforme tempos medidos com auxílio de algum instrumento externo | 3 |  |
| Código implementa corretamente as fases do semáforo e estrutura do código (variáveis representativas e comentários) | 3 |  |
| Ir além: Implementou um componente extra, fez com `millis()` ao invés de `delay()` e/ou usou ponteiros no código | 1 |  |

