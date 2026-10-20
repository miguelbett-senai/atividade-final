# Phase 1: EC Sensing & RS-485 Communication to CLP - Research

**Researched:** 20 de outubro de 2026
**Domain:** Automação industrial - leitura de sensor de EC via Modbus RTU/RS-485 e comunicação com CLP Siemens S7-1200
**Confidence:** MEDIUM (hardware/viabilidade confirmados parcialmente; registro Modbus e pinagem do sensor pendentes com o fornecedor)

## Summary

A viabilidade técnica da Phase 1 é **alta**: o CLP Siemens S7-1200 1214C DC/DC/DC não possui RS-485 embarcado, mas suporta Modbus RTU master por meio do módulo de comunicação **CM 1241 RS422/RS485** (6ES7241-1CH32-0XB0) ou da placa **CB 1241 RS485** (6ES7241-1CH30-1XB0) encaixada na CPU, usando as instruções **Modbus_Comm_Load** e **Modbus_Master** do TIA Portal (biblioteca Modbus RTU integrada). O TIA Portal V20 e o PLCSIM V20 estão instalados na máquina de desenvolvimento [VERIFIED: filesystem], o que permite simular a comunicação antes da montagem física.

O sensor de solo (EC 0-20000 µS/cm, umidade, temperatura, 3 hastes inox, IP68, cabo 3 m, alimentação 5-30 V DC) é típico dos sensores "soil moisture/temperature/EC" com Modbus RTU, cujo mapa de registradores é bem conhecido na literatura: registrador 0 = temperatura (INT16, 0.01 °C), 1 = umidade (UINT16, 0.1 %RH ou 0.01?), 2 = EC (UINT16, 1 µS/cm), lidos com FC03/FC04. **No entanto, o mapa exato do modelo AliExpress adquirido não foi fornecido pelo vendedor** e deve ser solicitado (dúvida 6). O mesmo vale para a pinagem dos fios (dúvida 7).

Um segundo sensor (ar, umidade/temperatura) pode compartilhar o mesmo barramento RS-485, desde que receba um endereço Modbus diferente (ex.: solo = 1, ar = 2); o barramento suporta até 32 dispositivos por segmento sem repetidor (até 247 endereços com repetidores) [CITED: Siemens datasheet CM 1241]. A detecção de falha de comunicação é viável via bits ERROR/DONE/BUSY e STATUS da Modbus_Master (códigos 80C8 timeout, 8380 CRC, 8383 endereço inválido etc.) combinados com um temporizador watchdog no CLP.

**Primary recommendation:** usar o **CB 1241 RS485** (mais barato/compacto, encaixe direto na CPU, terminação 300 Ω interna opcional) ou o **CM 1241 RS422/485** se for necessário módulo removível; configurar Modbus RTU a 9600 bps, 8N1, endereço 1 para o sensor de solo (e 2 para o sensor de ar, se incluído), ler 3 registradores (temp, umidade, EC) com FC03 (MODE 0, DATA_ADDR 40001, DATA_LEN 3), com RESP_TO = 1000 ms e retries configuráveis; declarar falha de comunicação após N tentativas sem resposta e sinalizar para a lógica de segurança (Phase 3).

<user_constraints>
## User Constraints (from CONTEXT.md)

### Locked Decisions
- CLP model: Siemens S7-1200 1214C DC/DC/DC (disponível no grupo).
- EC purpose: fertirrigação (condutividade → dosagem de nutrientes).
- Phase 1 deliverable: planejamento, pesquisa de componentes, verificação de viabilidade técnica (sem programa do CLP pronto nem montagem completa, conforme Etapa 1 PDF).
- CLP program: não exigido na Phase 1.
- Full assembly: não exigido na Phase 1.
- Variáveis de corte: definidas por planta/cultura (ajustadas posteriormente).
- Sensor de solo: umidade, temperatura e EC; 3 hastes inox; IP68; cabo 3 m; Modbus RTU RS-485 / 0-10 V / 4-20 mA.
- Sensor EC faixa: 0-20000 µS/cm, precisão ±3 %.
- RS-485 típico: 4800 ou 9600 bps, 8N1, endereço padrão 1.
- Alimentação RS-485: 5-30 V DC.
- Segundo sensor (ar): necessário para umidade/temperatura do ar; mesmo barramento, endereço Modbus diferente.

### the agent's Discretion
Nenhuma área de discrição exercida - todas as dúvidas deixadas para resolução do usuário/professor (ver Open Questions).

### Deferred Ideas (OUT OF SCOPE)
Nenhuma ideia adiada além do escopo da Phase 1 (controle de válvula, segurança de falha e demo ficam para Phases 2-4).
</user_constraints>

<phase_requirements>
## Phase Requirements

| ID | Description | Research Support |
|----|-------------|------------------|
| SENS-01 | Medir a condutividade elétrica (EC) da água com sensor RS-485/Modbus RTU | Sensor de solo com 3 hastes inox, IP68, cabo 3 m, EC 0-20000 µS/cm; leitura via FC03/FC04 no registrador 2 (típico) [CITED: sensor manuals]. |
| SENS-02 | Fornecer leitura de EC com faixa de medição adequada à fertirrigação | Faixa 0-20000 µS/cm cobre soluções nutritivas típicas (0-3000 µS/cm uso comum); precisão ±3 % (0-10000) / ±5 % (10000-20000). |
| COMM-01 | Transmitir os dados do sensor EC ao CLP via barramento RS-485/Modbus RTU | CLP S7-1200 + CM/CB 1241 + Modbus_Comm_Load/Modbus_Master; barramento 2 fios half-duplex, até 32 escravos/segmento. |
| COMM-02 | Detectar falha de comunicação com o sensor EC | Bits ERROR/DONE/BUSY + STATUS (80C8, 8380-8388) + watchdog cíclico no CLP. |
</phase_requirements>

## Architectural Responsibility Map

| Capability | Primary Tier | Secondary Tier | Rationale |
|------------|-------------|----------------|-----------|
| Leitura física do solo (EC/umidade/temp) | Sensor (campo) | — | Transdutor no ponto de medição |
| Conversão A/D e protocolo Modbus RTU | Sensor (escravo) | — | Sensor atua como escravo Modbus |
| Mestre Modbus / polling | CLP S7-1200 (CPU) | Módulo CM/CB 1241 | CPU executa Modbus_Master; módulo só faz a ponte física |
| Armazenamento das leituras (DB) | CLP (memória de trabalho) | — | Tags/DB não otimizado para DATA_PTR |
| Detecção de falha de comunicação | CLP (OB cíclico) | — | Watchdog + status da Modbus_Master |
| Sinalização de falha (saída digital/LED) | CLP (DO) | Painel | Fora do escopo da Phase 1 (Phase 3) |
| Acionamento da válvula | CLP (DO) | — | Phase 2/3 |

## Standard Stack

### Core (hardware/software)

| Componente | Modelo/Versão | Finalidade | Por que padrão |
|------------|---------------|------------|----------------|
| CLP | Siemens S7-1200 CPU 1214C DC/DC/DC (6ES7214-1AG40-0XB0) | Controlador principal | Disponível no grupo; 14 DI / 10 DO / 2 AI; 100 KB memória de trabalho; suporta até 3 módulos de comunicação + 1 placa CB [CITED: Siemens catalog] |
| Módulo RS-485 (opção A) | Siemens CM 1241 RS422/RS485 (6ES7241-1CH32-0XB0) | Interface RS-485 removível | Modbus RTU master/slave integrado; até 32 escravos/segmento; cabo até 1000 m [CITED: Siemens datasheet] |
| Placa RS-485 (opção B) | Siemens CB 1241 RS485 (6ES7241-1CH30-1XB0) | Interface RS-485 encaixada na CPU | Mais compacta (15×62×63 mm, 29 g), sem consumo 24 V, terminação 300 Ω interna opcional (pinos 7-9), bias interno [CITED: Siemens CB 1241 specs] |
| Software de engenharia | TIA Portal V20 (instalado) | Programação e configuração | Versão compatível com Modbus_Comm_Load/Modbus_Master modernos [VERIFIED: filesystem C:\Program Files\Siemens\Automation\Portal V20] |
| Simulador | PLCSIM V20 (instalado) | Teste sem hardware | Valida lógica de polling/falha [VERIFIED: filesystem C:\Program Files\Siemens\Automation\PLCSIM_V20] |
| Sensor de solo (EC) | Sensor RS-485/Modbus RTU, 3 hastes inox, IP68, cabo 3 m | Medição de EC/umidade/temp | Atende SENS-01/02; faixa 0-20000 µS/cm [CITED: AliExpress spec] |
| Sensor de ar (opcional) | Sensor RS-485 de umidade/temperatura do ar | Segundo ponto de medição | Mesmo barramento, endereço diferente [CONTEXT] |
| Fonte de alimentação | 24 V DC (a dimensionar) | Alimentação do CLP e sensores | Sensor aceita 5-30 V DC [CITED: AliExpress spec] |

### Supporting

| Componente | Versão | Finalidade | Quando usar |
|------------|--------|------------|-------------|
| Resistores de terminação 120 Ω | — | Terminação do barramento RS-485 | Extremidades do barramento (obrigatório em linhas longas; em 3 m é opcional, mas recomendado) |
| Resistores de bias (pull-up/pull-down) | ~650 Ω típico | Manter nível idle definido | Quando nenhum dispositivo fornece bias (CM 1241 pode não fornecer; CB 1241 fornece bias interno) |
| Cabo blindado par trançado | AWG 24-18 | Ligação A/B/GND | RS-485 (max 1000 m) |
| Fonte 5-30 V DC dedicada ao sensor | — | Alimentação do sensor | Se não usar os 24 V do CLP |

### Alternatives Considered

| Em vez de | Poderia usar | Tradeoff |
|-----------|--------------|----------|
| CM/CB 1241 (Modbus nativo) | Conversor RS-485↔RS-232 externo + porta Freeport | Mais complexo, sem suporte Modbus nativo, maior risco de erro; não recomendado |
| Modbus RTU | 0-10 V / 4-20 mA (analógico) | O sensor também suporta analógico, mas o trabalho exige RS-485/Modbus RTU; analógico perderia multi-sensor no mesmo barramento |
| Um sensor combinado (solo) | Sensores separados por variável | Mais cabos e entradas; o combinado é mais simples |

**Installation (software):**
```
TIA Portal V20 -> Instructions -> Communication -> Modbus (RTU) -> Modbus_Comm_Load + Modbus_Master
```
Nenhum pacote npm/Python/Rust envolvido (projeto de hardware); a "instalação" é a configuração de hardware no TIA Portal.

**Version verification:** TIA Portal V20 e PLCSIM V20 verificados via filesystem [VERIFIED: filesystem C:\Program Files\Siemens\Automation\Portal V20]. Versão do firmware do CPU 1214C e do CM/CB 1241 deve ser confirmada no hardware real (a documentação indica Modbus_Comm_Load com CM 1241 apenas a partir do firmware V2.1 do módulo) [CITED: Siemens docs].

## Package Legitimacy Audit

Não aplicável: este phase não instala pacotes de software (projeto de automação/hardware). Componentes de hardware (CLP, módulos, sensores) são adquiridos via fornecedores; a legitimidade/qualidade deve ser verificada por ficha técnica e aprovação do professor (viabilidade de laboratório).

## Architecture Patterns

### System Architecture Diagram

```
[Sensor de solo EC] (escravo Modbus addr 1)  ----+
                                                  |
[Sensor de ar (opcional)] (escravo addr 2)  ------+---- RS-485 (A/B/GND, 2 fios, half-duplex)
                                                  |
                                                  v
                                    [CM 1241 / CB 1241 RS-485]  (módulo na CPU)
                                                  |
                                                  v
                                    [S7-1200 CPU 1214C]  -- Modbus_Master (polling cíclico)
                                                  |
                                                  v
                                    [DB de leituras] (Temp, Umidade, EC, status com)
                                                  |
                                                  v
                                    [OB cíclico / watchdog] -> flag CommOK + (Phase 3) válvula segura
```

Fluxo de dados: sensor (mede) → RS-485 → módulo → CPU (Modbus_Master lê FC03) → DB → lógica de controle (Phase 2) / detecção de falha (COMM-02).

### Recommended Project Structure (TIA Portal)

```
Project
├── Device (S7-1200 1214C)
│   ├── Hardware config: CPU + CM 1241/CB 1241
│   └── Tag table: CommOK, EC_raw, Temp_raw, Hum_raw, CommErrorCount
├── Program blocks
│   ├── OB100 (Startup): pulso inicial para Modbus_Comm_Load
│   ├── OB1 (cíclico): sequenciador de polling + watchdog
│   └── FB/FC: ReadSensor (Modbus_Master), CommWatchdog
├── Data blocks
│   ├── DB_Sensor (non-optimized, para DATA_PTR)
│   └── DB_Comm (status/contadores)
└── HMI/SCADA: fora do escopo v1
```

### Pattern 1: Configuração da porta (Modbus_Comm_Load)

**What:** configura o módulo CB/CM para Modbus RTU (baud, paridade, timeout). Deve ser chamado uma vez no startup (OB100) ou na primeira varredura.

**When to use:** sempre, antes de qualquer Modbus_Master.

**Example (parâmetros):**
```
Modbus_Comm_Load(
  REQ       := FirstScan,          // pulso de subida
  PORT      := "CM_1241_RS485",    // hardware ID do módulo
  BAUD      := 9600,               // 4800 ou 9600 (confirmar com sensor)
  PARITY    := 0,                  // 0=None, 1=Odd, 2=Even (sensor típico 8N1 => 0)
  FLOW_CTRL := 0,                  // sem flow control (RS-485)
  RTS_ON_DLY:= 0,
  RTS_OFF_DLY:= 0,
  RESP_TO   := 1000,               // ms (5..65535)
  MB_DB     := Modbus_Master_DB,   // instância do Modbus_Master
  DONE      => PortConfigured,
  ERROR     => PortConfigError,
  STATUS    => PortStatus
);
```
Notas: o parâmetro estático MODE da instância Modbus_Comm_Load deve ser 4 (half-duplex RS-485, 2 fios) [CITED: Siemens manual]. BAUD válido: 300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 76800, 115200. PARITY: 0=None, 1=Odd, 2=Even.

### Pattern 2: Leitura dos registradores (Modbus_Master, FC03)

**What:** lê N registradores de holding do escravo (sensor). Para ler temp/umidade/EC, DATA_LEN=3.

**When to use:** a cada ciclo de polling, para cada sensor do barramento.

**Example:**
```
Modbus_Master(
  REQ       := PollTrigger,         // pulso (não manter true continuamente)
  MB_ADDR   := 1,                  // endereço do sensor de solo
  MODE      := 0,                  // MODE 0 + addr 40001..49999 = FC03 Read Holding Registers
  DATA_ADDR := 40001,              // registrador 0 do sensor (40001 Modbus = 0x0000)
  DATA_LEN  := 3,                  // temp, umidade, EC
  DATA_PTR  := DB_Sensor,          // DB não otimizado (array de WORD)
  DONE      => ReadDone,
  BUSY      => ReadBusy,
  ERROR     => ReadError,
  STATUS    => ReadStatus
);
```
Mapeamento esperado (típico, confirmar com o fornecedor):
- DATA_PTR[0] = temperatura (INT16, escala 0.01 °C ou 0.1 °C conforme sensor)
- DATA_PTR[1] = umidade (UINT16, escala 0.1 %RH ou 0.01)
- DATA_PTR[2] = EC (UINT16, 1 µS/cm)

Para FC04 (Read Input Registers), MODE=0 com DATA_ADDR 30001..39999.

### Pattern 3: Multi-escravo no mesmo barramento

**What:** alterar MB_ADDR entre duas leituras consecutivas (solo=1, ar=2). RS-485 é half-duplex: escrever e ler não podem ocorrer simultaneamente; sequenciar.

**When to use:** quando o segundo sensor for incluído.

### Pattern 4: Detecção de falha de comunicação

**What:** combinar (a) ERROR/STATUS da Modbus_Master, (b) ausência de DONE dentro do tempo esperado, (c) watchdog: se não houver leitura bem-sucedida há mais de T segundos, CommOK = FALSE.

**When to use:** sempre (COMM-02).

**Códigos de status relevantes [CITED: Siemens error table]:**
- 80C8: escravo não respondeu no tempo (timeout) - principal indicador de falha
- 80E1: erro de paridade
- 80E2: erro de framing
- 8380: erro de CRC
- 8381: função não suportada
- 8383: endereço de dados inválido
- 8387: escravo errado respondeu
- 8200: porta ocupada (requisição anterior ainda em andamento)

### Anti-Patterns to Avoid
- **Manter REQ = TRUE continuamente:** a Modbus_Master dispara a cada borda; manter true pode re-disparar. Usar pulso one-shot.
- **DB otimizado para DATA_PTR:** DATA_PTR exige DB clássico (acesso não otimizado) ou área M. DB otimizado falha com 818B/818C.
- **Terminação em todos os nós:** terminar apenas nas duas extremidades do barramento; terminar no meio cria stubs e reflexões.
- **Misturar A/B:** inverter polaridade (A/B) é a causa #1 de "sem comunicação". Pin 3 do CM = A (Data+), pin 8 = B (Data-); no CB, pin 1/2 = B (Data+), pin 3/4 = A (Data-). Conferir com o sensor (que pode nomear TX+/TX-).
- **Ignorar o bias:** barramento RS-485 precisa de polarização em idle; sem ela, ruído pode gerar bytes espúrios. Preferir módulo que forneça bias (CB 1241) ou adicionar resistores externos.
- **Polling muito rápido:** intervalo mínimo recomendado entre requisições ≥ 50-100 ms para dar tempo ao escravo responder; respeitar o tempo de resposta do sensor (≤ 1 s para EC).
- **Não tratar o segundo sensor como outro endereço:** usar o mesmo endereço 1 para dois sensores causa colisão.

## Don't Hand-Roll

| Problema | Não construir | Usar | Por quê |
|----------|---------------|------|---------|
| Protocolo Modbus RTU / CRC | Parser serial próprio | Modbus_Comm_Load + Modbus_Master (TIA) | CRC-16, timing, half-duplex já tratados; erro de CRC 8380 reportado automaticamente |
| Driver RS-485 | Bit-banging em GPIO | CM/CB 1241 | Níveis elétricos, terminação, bias, isolamento 707 V |
| Conversão de endianness/escala | Cálculo manual ad-hoc | Bloco de conversão documentado (INT16→real) | Evita erro de escala; documentar fator por registrador |
| Watchdog de comunicação | Timer genérico sem reset | OB cíclico com flag de "última leitura OK" + TON | Evita falso positivo em primeira varredura |

**Key insight:** a complexidade real não está no protocolo (já resolvido pela biblioteca Siemens), mas na **integração elétrica (A/B, terminação, bias, alimentação) e na confirmação do mapa de registradores do sensor específico**.

## Common Pitfalls

### Pitfall 1: Mapa de registradores não confirmado
**What goes wrong:** ler registrador errado → valores sem sentido (ex.: EC lido como umidade).
**Why it happens:** fornecedor AliExpress não envia datasheet; mapas variam entre fabricantes.
**How to avoid:** solicitar tabela Modbus ao vendedor antes da montagem; testar com software Modbus (ex.: Modbus Poll) ou PLCSIM + simulador de escravo.
**Warning signs:** EC fora de 0-20000, umidade > 100 %, temperatura < -40.

### Pitfall 2: Paridade/baudrate incompatíveis
**What goes wrong:** erro 80E1 (paridade) ou 80C8 (timeout).
**Why it happens:** sensor padrão pode ser 4800 8N1 ou 9600 8N1; CLP configurado diferente.
**How to avoid:** confirmar baud/paridade do sensor (leitura da etiqueta/datasheet) e configurar idêntico; testar um baud por vez.

### Pitfall 3: Polaridade A/B invertida
**What goes wrong:** sem comunicação (80C8).
**Why it happens:** nomenclatura inconsistente (A/B vs TX+/TX- vs +/-).
**How to avoid:** se falhar, inverter A/B no sensor; manter GND comum.

### Pitfall 4: Falta de terminação/bias em barramento longo
**What goes wrong:** comunicação intermitente, erros de CRC (8380).
**Why it happens:** reflexões em linhas não terminadas.
**How to avoid:** terminar nas extremidades (120 Ω); habilitar bias interno (CB) ou adicionar resistores; aterrar a blindagem em um ponto.

### Pitfall 5: Alimentação do sensor instável
**What goes wrong:** reset do sensor, endereço voltando ao padrão 1, leituras erráticas.
**Why it happens:** queda de tensão no cabo longo; fonte subdimensionada.
**How to avoid:** fonte dedicada 24 V com margem; verificar consumo (≈0,5 W @ 24 V); aterrar GND comum.

### Pitfall 6: Endereço duplicado com segundo sensor
**What goes wrong:** colisão de respostas, 8387 (escravo errado respondeu).
**Why it happens:** ambos com endereço 1 de fábrica.
**How to avoid:** reconfigurar o endereço do segundo sensor (registrador 0x0200/512 típico) antes de colocá-lo no barramento; ou configurar um de cada vez.

### Pitfall 7: Tempo de resposta do sensor
**What goes wrong:** timeout (80C8) com RESP_TO muito curto.
**Why it happens:** EC pode ter tempo de resposta ≤ 1 s; sensor lento.
**How to avoid:** RESP_TO ≥ 1000 ms; não polir mais rápido que o tempo de resposta.

## Code Examples

### Leitura típica (Modbus RTU) - frame esperado
```
// Requisição (mestre -> sensor addr 1): FC03, reg 0, 3 words
01 03 00 00 00 03 04 08   // addr, func, startHi, startLo, qtyHi, qtyLo, CRClo, CRChi

// Resposta (sensor -> mestre): 3 words (temp, umidade, EC)
01 03 06 00 64 01 F4 03 E8  CRC  // exemplo: temp=10.0°C? (0x0064=100 -> 10.0), umidade=50.0% (0x01F4=500), EC=1000 µS/cm
```
Escala conforme manual do sensor (típico: temp 0.1 °C, umidade 0.1 %RH, EC 1 µS/cm) [CITED: sensor manuals].

### Watchdog (pseudo-lógica no OB1)
```
IF ReadDone THEN LastOK_Time := CurrentTime; CommOK := TRUE; END_IF;
IF (CurrentTime - LastOK_Time) > WatchdogTime THEN CommOK := FALSE; CommErrorCount := CommErrorCount + 1; END_IF;
```
WatchdogTime a definir (sugestão: 3× tempo de polling + margem).

## State of the Art

| Abordagem antiga | Abordagem atual | Quando mudou | Impacto |
|------------------|-----------------|--------------|---------|
| MB_COMM_LOAD / MB_MASTER (legacy) | Modbus_Comm_Load / Modbus_Master | TIA V11+ (V13 SP1+ recomendado) | Nomes e parâmetros atualizados; CM 1241 precisa firmware ≥ V2.1 |
| RS-232 point-to-point | RS-485 half-duplex multi-drop | — | Permite múltiplos sensores no mesmo barramento |
| Leitura analógica 0-10 V / 4-20 mA | Modbus RTU digital | — | Mais parâmetros por sensor, diagnóstico, multi-drop |

**Deprecated/outdated:**
- Instruções legacy MB_COMM_LOAD/MB_MASTER ainda funcionam em TIA, mas a documentação atual (V20/V21) usa Modbus_Comm_Load/Modbus_Master; preferir as novas em projetos novos.
- Uso de RS-232 para longas distâncias: limitado a ~15 m; RS-485 até 1000 m.

## Assumptions Log

| # | Claim | Section | Risk if Wrong |
|---|-------|---------|---------------|
| A1 | Mapa de registradores do sensor (reg 0 temp, 1 umidade, 2 EC) é o típico, não o do modelo adquirido | Code Examples / Patterns | Leituras erradas; mitigar solicitando datasheet |
| A2 | Escala dos registradores (temp 0.1 °C, umidade 0.1 %RH, EC 1 µS/cm) | Code Examples | Erro de escala por 10x; confirmar no datasheet |
| A3 | Baudrate/paridade padrão do sensor (9600 ou 4800, 8N1) | Standard Stack | Falha de comunicação; testar ambos |
| A4 | Número de retries padrão da Modbus_Master | Pattern 4 | Critério de falha indefinido (dúvida 2) |
| A5 | CM/CB 1241 disponíveis no laboratório/grupo | Environment | Se indisponível, usar conversor externo (menos robusto) |
| A6 | Sensor de ar será incluído na Phase 1 | Open Questions | Se deferido, barramento terá 1 escravo (simplifica) |
| A7 | Terminação 120 Ω suficiente; bias interno do CB 1241 | Architecture | Comunicação intermitente em campo |
| A8 | TIA Portal V20 suporta o firmware do módulo disponível | Environment | Pode exigir atualização de firmware |

## Open Questions

1. **Tempo de varredura (scan time)** - A cada quanto o CLP lê os sensores? (1 s / 5 s / 10 s / 30 s / outro) - não definido. Recomendação de pesquisa: 1-5 s (tempo de resposta do sensor ≤ 1 s para EC; polling mais rápido não agrega). [ASSUMED]
2. **Critério de falha de comunicação** - quantas tentativas sem resposta antes de declarar falha e fechar a válvula? - não definido. Recomendação: 3 tentativas consecutivas com timeout; combinar com watchdog de 3× tempo de polling.
3. **Configuração do barramento Modbus** - baudrate (9600 vs 4800), paridade, endereço - não confirmado. Ação: confirmar com vendedor/professor; testar.
4. **Segundo sensor (ar)** - incluir na Phase 1 ou deferir? - não decidido. Impacto: se incluído, configurar endereço 2 e sequenciar leituras.
5. **Limites de sanidade das leituras** - min/max aceitáveis de umidade, EC, temperatura - dependem da planta/cultura, não escolhida. Ação: definir na Phase 2 com base na cultura.
6. **Tabela de registradores Modbus** - não informada pelo vendedor; deve ser solicitada (bloqueia confirmação do mapa).
7. **Pinagem exata dos fios do sensor** - não informada; solicitar ao vendedor (A/B, VCC, GND, e se há fio de saída analógica).
8. **Dimensões, peso e tempo de resposta do sensor** - não informados; solicitar.
9. **Modelo da válvula** - definir tensão, acionamento, vazão, pressão, conexões hidráulicas (afeta Phase 2/3).
10. **Disponibilidade de equipamentos no laboratório** - compressor, motor trifásico, inversor, tanque, válvula, CLP - verificar com o professor (viabilidade).

## Environment Availability

| Dependency | Required By | Available | Version | Fallback |
|------------|------------|-----------|---------|----------|
| TIA Portal | Programação/configuração | ✓ | V20 (Portal V20) | — |
| PLCSIM | Simulação | ✓ | V20 | Teste manual |
| CLP S7-1200 1214C DC/DC/DC | Controlador | ? (a confirmar com grupo/professor) | — | Nenhum (bloqueante) |
| CM 1241 RS422/485 ou CB 1241 RS485 | Interface RS-485 | ? (a confirmar) | — | Conversor RS-485↔RS-232 externo (menos robusto) |
| Sensor de solo EC (RS-485) | SENS-01 | ✗ (não adquirido) | — | Sensor 0-10 V/4-20 mA (perde Modbus/multi-drop) |
| Sensor de ar (umidade/temp) | Segundo ponto | ✗ (não adquirido) | — | Deferir para fase futura |
| Fonte 24 V DC | Alimentação | ? (a confirmar) | — | Fonte de bancada |
| Cabo blindado, conectores, terminadores | Fiação | ? (a confirmar) | — | — |
| Válvula de água | Phase 2/3 | ? (grupo possui algumas) | — | — |
| Tanque/reservatório, tubulação | Demo (Phase 4) | ? (laboratório) | — | — |

**Missing dependencies with no fallback:**
- CLP S7-1200 1214C (se não disponível, bloqueia tudo) - confirmar com professor.
- Módulo/placa RS-485 (CM/CB 1241) - sem ele não há Modbus RTU nativo.

**Missing dependencies with fallback:**
- Sensor de solo: pode usar saída analógica 0-10 V/4-20 mA (perde multi-drop e diagnóstico Modbus).
- Sensor de ar: pode ser deferido (Phase 1 funciona só com solo).

## Validation Architecture

Nyquist validation habilitada (config). Este é um projeto de planejamento/documentação; não há código executável. A "validação" é por critérios de verificação (checklist) e revisão de documento.

### Test Framework

| Property | Value |
|----------|-------|
| Framework | Nenhum (projeto de hardware/documentação) |
| Config file | N/A |
| Quick run command | Revisão manual do RESEARCH.md / checklist de viabilidade |
| Full suite command | Revisão do professor + checklist Etapa 1 |

### Phase Requirements → Test Map

| Req ID | Behavior | Test Type | Automated Command | File Exists? |
|--------|----------|-----------|-------------------|-------------|
| SENS-01 | Ler EC do sensor RS-485/Modbus RTU | Manual (simulação) | PLCSIM: Modbus_Master retorna DONE=TRUE e EC no DB | ❌ Wave 0 |
| SENS-02 | Faixa 0-20000 µS/cm adequada | Documental | Verificar datasheet do sensor vs. faixa de fertirrigação | ❌ Wave 0 |
| COMM-01 | Transmitir dados ao CLP via RS-485/Modbus RTU | Manual (simulação/bancada) | PLCSIM + simulador de escravo Modbus; ou bancada com sensor real | ❌ Wave 0 |
| COMM-02 | Detectar falha de comunicação | Manual (simulação) | Desconectar sensor / endereço errado → ERROR=TRUE (80C8) e CommOK=FALSE | ❌ Wave 0 |

### Sampling Rate
- **Per task commit:** revisão manual do documento/seção alterada.
- **Per wave merge:** revisão cruzada do grupo + professor.
- **Phase gate:** checklist Etapa 1 completo + viabilidade de laboratório confirmada.

### Wave 0 Gaps
- [ ] Simulador de escravo Modbus para PLCSIM (ou sensor real em bancada) - cobre SENS-01/COMM-01/COMM-02
- [ ] Datasheet/tabela Modbus do sensor (solicitar ao vendedor) - cobre SENS-01/SENS-02
- [ ] Confirmar disponibilidade do CLP e módulo RS-485 no laboratório - cobre COMM-01

*(Se os gaps acima forem resolvidos, a infraestrutura de verificação (checklist + simulação) cobre todos os requisitos da Phase 1.)*

## Security Domain

Não aplicável como segurança de software (sem aplicação web/API). Considerações de segurança elétrica/física:
- Alimentação 24 V DC com proteção por fusível; aterramento da blindagem em um ponto.
- IP68 do sensor permite imersão, mas os conectores/emendas devem ser protegidos (caixa estanque) para manter o grau de proteção.
- Isolamento galvânico do módulo RS-485 (707 V tipo-teste) protege o CLP de surtos no barramento; ainda assim, usar DPS/supressor em instalações externas (recomendado, não obrigatório para demo em laboratório).
- Condição segura da válvula em falha de comunicação é tratada na Phase 3 (CTRL-02).

## Sources

### Primary (HIGH confidence)
- Siemens TIA Portal docs - Modbus_Comm_Load / Modbus_Master (parâmetros, códigos de erro) - docs.tia.siemens.cloud (V20/V21) [CITED]
- Siemens datasheet CM 1241 RS422/485 (6ES7241-1CH32-0XB0) - suporte a Modbus RTU master/slave, 32 escravos/segmento, 1000 m [CITED]
- Siemens CB 1241 RS485 (6ES7241-1CH30-1XB0) - pinagem, terminação 300 Ω, bias, 15×62×63 mm [CITED]
- Siemens manual "How do you establish MODBUS-RTU communication?" (CM/CB 1241) - modo half-duplex, até 32 escravos [CITED]
- Filesystem local - TIA Portal V20 e PLCSIM V20 instalados [VERIFIED: filesystem]

### Secondary (MEDIUM confidence)
- Tutoriais de integração S7-1200 Modbus RTU (industrialmonitordirect.com, 2026-05-09) - pinagem CM 1241 (pin 3 = A, pin 8 = B, pin 5 = GND), parâmetros típicos [CITED]
- Manuais de sensores de solo RS-485/Modbus RTU (infwin MEC10, DFRobot SEN0603, Farnell S-Temp&VWC&EC-02) - mapas de registradores típicos e escalas [CITED]

### Tertiary (LOW confidence)
- AliExpress - especificação do sensor (EC 0-20000 µS/cm, ±3 %, 5-30 V DC, 4800/9600 bps, 8N1, addr 1) [ASSUMED - a confirmar com o vendedor]
- Mapa de registradores "típico" (reg 0/1/2) - varia por fabricante [ASSUMED]

## Metadata

**Confidence breakdown:**
- Standard stack: HIGH - CLP/módulos/TIA verificados por documentação oficial e filesystem
- Architecture: MEDIUM - padrão Modbus RTU bem documentado; mapa de registradores do sensor específico pendente
- Pitfalls: HIGH - erros comuns de RS-485/Modbus documentados em múltiplas fontes

**Research date:** 20 de outubro de 2026
**Valid until:** 30 dias (hardware estável); revalidar se o modelo do sensor mudar
