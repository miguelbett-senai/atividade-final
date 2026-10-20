# Requirements: Sistema de Irrigação Automatizada por EC (Fertirrigação)

**Defined:** 20 de outubro de 2026
**Core Value:** Manter a condutividade elétrica (EC) da solução de irrigação na faixa ideal, acionando automaticamente a válvula de água para fertirrigação — sem intervenção manual.

## v1 Requirements

Requirements for the initial release. Each maps to a roadmap phase (traceability populated when phases are defined).

### Sensors (SENS)

- [ ] **SENS-01**: Medir a condutividade elétrica (EC) da água com sensor RS-485/Modbus RTU
- [ ] **SENS-02**: Fornecer leitura de EC com faixa de medição adequada à fertirrigação

### Communication (COMM)

- [ ] **COMM-01**: Transmitir os dados do sensor EC ao CLP via barramento RS-485/Modbus RTU
- [ ] **COMM-02**: Detectar falha de comunicação com o sensor EC

### Control (CTRL)

- [ ] **CTRL-01**: Receber a leitura de EC no CLP e decidir a abertura ou fecho da válvula
- [ ] **CTRL-02**: Colocar a válvula em condição segura quando houver falha de comunicação com o sensor

### Actuation / Valve (VALV)

- [ ] **VALV-01**: Abrir a válvula de água quando EC estiver abaixo do valor mínimo (adicionar água/nutriente)
- [ ] **VALV-02**: Fechar a válvula quando EC atingir o valor alvo

### Demo (DEMO)

- [ ] **DEMO-01**: Operar o sistema com planta e água reais na feira de tecnologia do SENAI

## v2 Requirements

Deferred to a future release. Tracked but not in the current scope.

### pH Control

- **PH-01**: Controlar o pH da solução de irrigação além da EC

### Scheduling

- **SCHED-01**: Irrigação por programação de horário independente do sensor

### Supervisory

- **SUP-01**: Interface gráfica / painel supervisório para acompanhamento remoto

## Out of Scope

Explicitly excluded to prevent scope creep.

| Feature | Reason |
|---------|--------|
| Bomba de injeção de fertilizante separada | A v1 usa a válvula de água disponível; bomba é um custo/complexidade a adicionar depois |
| Controle de pH | Foco inicial é apenas a EC; pH pode entrar em v2 |
| Irrigação por horário (sem sensor) | O controle é por leitura de EC, não por temporizador |
| Interface gráfica / supervisório | A demonstração da feira é local no protótipo |

## Traceability

Which phases cover which requirements. Updated when roadmap phases are defined.

| Requirement | Phase | Status |
|-------------|-------|--------|
| SENS-01 | — | Pending |
| SENS-02 | — | Pending |
| COMM-01 | — | Pending |
| COMM-02 | — | Pending |
| CTRL-01 | — | Pending |
| CTRL-02 | — | Pending |
| VALV-01 | — | Pending |
| VALV-02 | — | Pending |
| DEMO-01 | — | Pending |

**Coverage:**
- v1 requirements: 9 total
- Mapped to phases: 0 (phases/roadmap not yet defined)
- Unmapped: 9 ⚠️

---
*Requirements defined: 20 de outubro de 2026*
*Last updated: 20 de outubro de 2026 after initial definition*
