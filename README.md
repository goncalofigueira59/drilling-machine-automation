# Automação de Máquina de Furação de Peças (Fura Baixo e Alto)

<p align="center">
  <img src="https://img.shields.io/badge/Categoria-Projeto%20Académico-blue" alt="Categoria: Projeto Académico">
  <img src="https://img.shields.io/badge/Área-Automação%20Industrial-orange" alt="Área: Automação Industrial">
  <img src="https://img.shields.io/badge/Lógica-GRAFCET-green" alt="Lógica: GRAFCET">
  <img src="https://img.shields.io/badge/Programação-Ladder-red" alt="Programação: Ladder">
  <img src="https://img.shields.io/badge/PLC-Fatek%20FBs--20MC-purple" alt="PLC: Fatek FBs-20MC">
</p>

> [Projeto Académico] Desenvolvimento da lógica de controlo (GRAFCET ) e implementação em linguagem Ladder para a automação de uma máquina de furação capaz de processar peças de duas alturas distintas. Projeto realizado no âmbito da UC de Automação Industrial do CTeSP em Instalações Elétricas e Automação.

---

## Índice

- Introdução e Objetivos]
- Descrição do Funcionamento da Máquina
  - Sensores e Atuadores
  - Processamento de Peças Altas e Baixas
- Lógica de Controlo: GRAFCET
  - Estrutura e Simbologia
  - Detalhe do GRAFCET para Peça Baixa e Peça Alta
- Implementação em Ladder
  - Ferramentas e PLC Utilizados
  - Tabela de Endereços
  - Simbologia Ladder Utilizada
  - Desenvolvimento do Programa Ladder (Conversão GRAFCET para Ladder)
- Testes e Verificação
- Conclusão e Aprendizagens
- Documentação do Projeto
- Licença

---

### 1. Introdução e Objetivos

Este projeto, desenvolvido no âmbito da Unidade Curricular de **Automação Industrial** do **CTeSP em Instalações Elétricas e Automação** da ESTGA – Universidade de Aveiro, teve como objetivo principal a conceção da lógica de controlo e a sua implementação para uma máquina de furação. As tarefas incluíram o desenvolvimento do diagrama **GRAFCET**, a programação em linguagem **Ladder** e a realização de testes para verificar o correto funcionamento do sistema.

O trabalho representou um desafio importante, permitindo a aplicação prática de conhecimentos teóricos sobre diagramas GRAFCET e a linguagem de programação Ladder, essenciais na automação industrial.

### 2. Descrição do Funcionamento da Máquina

O sistema de furação é projetado para processar peças de duas alturas diferentes (alta e baixa) de forma contínua. A furadora tem uma posição inicial na altura mais elevada, e a broca deve permanecer em rotação durante todo o movimento descendente e ascendente.

#### 2.1. Sensores e Atuadores

O controlo do sistema é realizado através de:

-   **Sensores ON-OFF (Normalmente Abertos):**
    -   `h`: Sensor de posição inicial da furadora (mais alta).
    -   `b`: Sensor de deteção de peça alta.
    -   `m`: Sensor de deteção de peça baixa.
    -   `f`: Sensor de fim de curso inferior da furadora.
    -   `p`: Sensor de presença de peça na estação de trabalho.
-   **Atuadores:**
    -   `sf`: Solenoide para controlo do movimento descendente da furadora (retorno por mola).
    -   `br`: Solenoide para controlo da rotação da broca.
-   **Cilindro Pneumático:** Utilizado para o movimento da furadora.

#### 2.2. Processamento de Peças Altas e Baixas

A máquina distingue as peças pela sua altura:

-   **Peça Alta:** O sensor `b` é acionado antes do sensor `m`.
-   **Peça Baixa:** O sensor `m` é acionado antes do sensor `b`.

O processo de furação é diferente para cada tipo de peça, conforme ilustrado no enunciado do problema. O sistema pode operar de forma ininterrupta, assumindo a substituição automática ou manual das peças no final de cada processamento.

### 3. Lógica de Controlo: GRAFCET

O diagrama GRAFCET foi elaborado no software **FluidSim 4** (versão 4.2p/1.67 Pneumatics), utilizando simbologia válida para descrever o funcionamento sequencial do sistema.

#### 3.1. Estrutura e Simbologia

O GRAFCET é composto por:

-   **Etapas (Quadrados numerados de 0 a 9):** Representam os estados do sistema.
-   **Transições (Linhas entre etapas):** Condições lógicas que, quando verdadeiras, permitem a passagem para a etapa seguinte.
-   **Ações (Retângulos à direita das etapas):** Comandos executados quando a etapa associada está ativa (ex: ligar/desligar broca, ativar/desativar solenoide de movimento).

#### 3.2. Detalhe do GRAFCET para Peça Baixa e Peça Alta

O GRAFCET foi concebido para iniciar com a broca e o solenoide de movimento ligados na posição inicial (Etapa 0), condicionado pela presença de peça (`sensor p`). A partir daí, bifurca-se em dois caminhos principais:

-   **Caminho para Peça Baixa:** Ativado pela deteção do `sensor m`. Segue uma sequência de movimentos ascendentes e descendentes específicos para este tipo de peça.
-   **Caminho para Peça Alta:** Ativado pela deteção do `sensor b`. Segue uma rotina de furação adaptada à maior altura da peça.

No final de cada ciclo de furação, a máquina retorna à Etapa 0, aguardando a substituição da peça.

### 4. Implementação em Ladder

A programação Ladder foi desenvolvida na plataforma **WinProLadder** (versão 3.30, simulador 1.2) da Fatek, tendo como autómato alvo o **Fatek FBs-20MC**. A implementação seguiu a metodologia de conversão de diagramas GRAFCET para Ladder, conforme os slides disponibilizados no e-learning da UC.

#### 4.1. Ferramentas e PLC Utilizados

-   **Software GRAFCET:** FluidSim 4 (versão 4.2p/1.67 Pneumatics).
-   **Software Ladder:** WinProLadder (versão 3.30, simulador 1.2).
-   **PLC Alvo:** Fatek FBs-20MC.

#### 4.2. Tabela de Endereços

| Tipo | Endereço | Nomenclatura | Descrição |
| :--- | :------- | :----------- | :-------- |
| **Entradas** | X1 | SensorF | Sensor F: ON-OFF / Normalmente aberto |
| | X2 | SensorH | Sensor H: ON-OFF / Normalmente aberto |
| | X3 | SensorP | Sensor P: ON-OFF / Normalmente aberto |
| | X4 | SensorM | Sensor M: ON-OFF / Normalmente aberto |
| | X5 | SensorB | Sensor B: ON-OFF / Normalmente aberto |
| **Saídas** | Y1 | YV_Broca | Solenoide da Broca |
| | Y2 | YV_Solenoide | Solenoide de movimento da furadora |
| **Memórias** | M0 | M_Etapa0 | Memória Retentiva Associada à Etapa 0 do Grafcet |
| | M1 | M_Etapa1 | Memória Retentiva Associada à Etapa 1 do Grafcet |
| | M2 | M_Etapa2 | Memória Retentiva Associada à Etapa 2 do Grafcet |
| | M3 | M_Etapa3 | Memória Retentiva Associada à Etapa 3 do Grafcet |
| | M4 | M_Etapa4 | Memória Retentiva Associada à Etapa 4 do Grafcet |
| | M5 | M_Etapa5 | Memória Retentiva Associada à Etapa 5 do Grafcet |
| | M6 | M_Etapa6 | Memória Retentiva Associada à Etapa 6 do Grafcet |
| | M7 | M_Etapa7 | Memória Retentiva Associada à Etapa 7 do Grafcet |
| | M8 | M_Etapa8 | Memória Retentiva Associada à Etapa 8 do Grafcet |
| | M9 | M_Etapa9 | Memória Retentiva Associada à Etapa 9 do Grafcet |
| | M1924 | M_FirstCycleScan | Memória ativada no primeiro ciclo de scan do autómato |

#### 4.3. Simbologia Ladder Utilizada

-   **Contacto Normalmente Aberto (NO):** Verdadeiro quando o sinal físico está presente (ex: sensor ativado).
-   **Contacto Normalmente Fechado (NC):** Verdadeiro quando o sinal físico está ausente (ex: sensor inativo). Utilizado para negar o `sensor p`.
-   **Set (S) e Reset (R):** Blocos lógicos que forçam um estado (verdadeiro ou falso) a uma variável booleana, mantendo esse estado até ser alterado por outra instrução. Utilizados para ativar e desativar as memórias das etapas e as saídas.
-   **Rungs:** As linhas de programação Ladder são executadas de cima para baixo e da esquerda para a direita. As saídas são ativadas quando existe um caminho lógico verdadeiro desde o rail esquerdo até à saída.

#### 4.4. Desenvolvimento do Programa Ladder (Conversão GRAFCET para Ladder)

O programa Ladder foi estruturado seguindo a lógica do GRAFCET, com cada transição e etapa cuidadosamente implementada:

-   **Início do Ciclo (Etapa 0):** O programa inicia automaticamente com a ligação da máquina, utilizando a memória especial `M1924` (First Cycle Scan) do Fatek para ativar a Etapa 0, sem necessidade de um botão de Start permanente.
-   **Transição T1:** Ativa a Etapa 1, condicionada pela presença de peça (`sensor p`).
-   **Transição T2:** Reconhecimento de peça baixa (`sensor m`), desativa a Etapa 1 e ativa a Etapa 2.
-   **Transição T3:** Caminho alternativo para peça alta (`sensor b`), desativa a Etapa 1 e ativa a Etapa 3.
-   **Transição T4:** Início do ciclo de peça baixa (`sensor f`), desativa a solenoide de movimento (`YV_Solenoide`) para que a furadora suba.
-   **Transições T5 a T9:** Detalham as sequências de movimentos e ações para os ciclos de peça baixa e peça alta, incluindo a ativação e desativação da broca (`YV_Broca`) e do movimento da furadora (`YV_Solenoide`) em função dos sensores de posição (`h`, `f`, `b`, `m`).
-   **Transição T10 e T11:** Marcam o fim dos ciclos de peça baixa e peça alta, respetivamente, e a transição de volta para a Etapa 0, permitindo um novo ciclo.

### 5. Testes e Verificação

Foram realizados testes exaustivos para verificar o correto funcionamento do programa Ladder e a sua conformidade com o diagrama GRAFCET. Os testes incluíram a simulação da ativação de cada etapa e transição, tanto para o ciclo de peça baixa como para o de peça alta, garantindo que a máquina se comportava conforme o esperado em todas as condições.

### 6. Conclusão e Aprendizagens

Este trabalho prático foi um desafio significativo que permitiu consolidar os conhecimentos em automação industrial. A experiência de desenvolver um diagrama GRAFCET complexo e convertê-lo para programação Ladder, utilizando ferramentas de simulação como o FluidSim e o WinProLadder, foi fundamental. O projeto reforçou a importância da análise sequencial de processos e da implementação rigorosa de lógicas de controlo para sistemas industriais.

### 7. Documentação do Projeto

O relatório completo do projeto, incluindo o diagrama GRAFCET e a descrição detalhada da programação Ladder, está disponível em formato PDF na pasta `project-files/`.

### 8. Licença

Este projeto está licenciado sob a **MIT License**. Veja o ficheiro `LICENSE` para mais detalhes.
