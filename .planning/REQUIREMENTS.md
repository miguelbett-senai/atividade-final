# Requirements: Horta Automática SENAI

**Defined:** 2026-10-20
**Core Value:** Irrigar automaticamente com base na umidade real do solo, mantendo as plantas hidratadas sem intervenção manual — e demonstrando esse controle em tempo real na feira do SENAI.

## v1 Requirements

Requirements for initial release. Each maps to roadmap phases.

### SENS — Sensores

- [ ] **SENS-01**: CLP lê sinal analógico do sensor de umidade do solo ASAIR AM2305B
- [ ] **SENS-02**: Sistema converte leitura analógica em percentual de umidade do solo
- [ ] **SENS-03**: Leitura de umidade é atualizada no mínimo a cada 5 segundos

### CTRL — Controle (CLP)

- [ ] **CTRL-01**: CLP aciona válvula proporcional pneumática quando umidade está abaixo do limiar mínimo
- [ ] **CTRL-02**: CLP desliga válvula proporcional quando umidade atinge o limiar máximo (alvo)
- [ ] **CTRL-03**: CLP controla até 3 zonas de irrigação de forma independente
- [ ] **CTRL-04**: CLP exibe umidade atual e estado da válvula no painel/HMI

### IRRG — Irrigação

- [ ] **IRRG-01**: Válvula proporcional pneumática regula fluxo de água para o aspersor
- [ ] **IRRG-02**: Aspersor irriga a zona com água e ar de alta pressão quando válvula está aberta
- [ ] **IRRG-03**: Sistema não irriga quando umidade está acima do limiar máximo (evita encharcamento)

### IFCE — Interface / Operação

- [ ] **IFCE-01**: Usuário pode definir limiares mínimo e máximo de umidade via painel do CLP
- [ ] **IFCE-02**: Usuário pode ligar/desligar o sistema manualmente via chave ou botão no CLP
- [ ] **IFCE-03**: Indicador visual mostra se a irrigação está ativa ou parada

### SEG — Segurança

- [ ] **SEG-01**: Sistema desliga a válvula em caso de perda de sinal do sensor (falha segura)
- [ ] **SEG-02**: Alarme ou indicação visual quando sensor está desconectado ou fora de faixa

## v2 Requirements

Deferred to future release. Tracked but not in current roadmap.

### APP — Aplicativo / Conectividade

- **APP-01**: Controle e monitoramento via aplicativo móvel
- **APP-02**: Histórico de umidade e irrigação em nuvem
- **APP-03**: Notificações push quando sistema irriga ou falha

### ZONES — Mais zonas

- **ZONES-01**: Suporte a 4 ou mais zonas de irrigação independentes
- **ZONES-02**: Diferentes tipos de planta com limiares próprios por zona

### WATER — Gestão de água

- **WATER-01**: Reservatório automático com nível de água monitorado
- **WATER-02**: Sistema de reuso ou captação de água da chuva

## Out of Scope

Explicitly excluded. Documented to prevent scope creep.

| Feature | Reason |
|---------|--------|
| Irrigação por app móvel ou nuvem | Fora do escopo da feira; foco no CLP local e demonstração ao vivo |
| Mais de 3 zonas | Limita complexidade para viabilizar entrega na feira |
| Bomba de alta vazão independente | Escopo restrito ao aspersor água/ar com fonte externa |
| Sistema de reuso de água | Não é requisito da feira; adiciona complexidade hidráulica |
| Controle climático (temperatura, luminosidade) | Não está no escopo de irrigação do solo |

## Traceability

Which phases cover which requirements. Updated during roadmap creation.

| Requirement | Phase | Status |
|-------------|-------|--------|
| SENS-01 | Phase 1 | Pending |
| SENS-02 | Phase 1 | Pending |
| SENS-03 | Phase 1 | Pending |
| CTRL-01 | Phase 2 | Pending |
| CTRL-02 | Phase 2 | Pending |
| CTRL-03 | Phase 3 | Pending |
| CTRL-04 | Phase 3 | Pending |
| IRRG-01 | Phase 2 | Pending |
| IRRG-02 | Phase 2 | Pending |
| IRRG-03 | Phase 2 | Pending |
| IFCE-01 | Phase 3 | Pending |
| IFCE-02 | Phase 3 | Pending |
| IFCE-03 | Phase 3 | Pending |
| SEG-01 | Phase 1 | Pending |
| SEG-02 | Phase 1 | Pending |

**Coverage:**
- v1 requirements: 15 total
- Mapped to phases: 15
- Unmapped: 0 ✓

---
*Requirements defined: 2026-10-20*
*Last updated: 2026-10-20 after initial definition*