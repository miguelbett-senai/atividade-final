# Roadmap: Horta Automática SENAI

**Project:** Horta Automática SENAI — Sistema de Irrigação Automatizada
**Core Value:** Irrigar automaticamente com base na umidade real do solo, mantendo as plantas hidratadas sem intervenção manual — e demonstrando esse controle em tempo real na feira do SENAI.
**Mode:** mvp

## Phase Summary

| # | Phase | Goal | Requirements | Success Criteria |
|---|-------|------|--------------|------------------|
| 1 | Sensores & Segurança | Ler umidade do solo com sensor ASAIR AM2305B e garantir falha segura | SENS-01, SENS-02, SENS-03, SEG-01, SEG-02 | 4 |
| 2 | Válvula & Irrigação | Acionar válvula proporcional pneumática e irrigar com aspersor | CTRL-01, CTRL-02, IRRG-01, IRRG-02, IRRG-03 | 5 |
| 3 | Multizona & Interface | Controlar até 3 zonas e exibir status no painel do CLP | CTRL-03, CTRL-04, IFCE-01, IFCE-02, IFCE-03 | 5 |
| 4 | Integração & Demo | Integrar tudo e validar demonstração ao vivo na feira | Todas as v1 | 3 |

---

## Phase Details

### Phase 1: Sensores & Segurança
**Goal:** Ler umidade do solo com sensor ASAIR AM2305B e garantir falha segura  
**Mode:** mvp  
**Success Criteria**:
1. CLP lê sinal analógico do ASAIR AM2305B e converte em % de umidade
2. Leitura é atualizada a cada 5 segundos (ou mais rápido)
3. Sistema detecta sensor desconectado ou fora de faixa e indica alarme
4. Válvula é desligada automaticamente em caso de perda de sinal do sensor (falha segura)

Requirements: SENS-01, SENS-02, SENS-03, SEG-01, SEG-02

### Phase 2: Válvula & Irrigação
**Goal:** Acionar válvula proporcional pneumática e irrigar com aspersor  
**Mode:** mvp  
**Success Criteria**:
1. CLP abre a válvula proporcional quando umidade está abaixo do limiar mínimo
2. CLP fecha a válvula quando umidade atinge o limiar máximo
3. Aspersor irriga com água e ar de alta pressão quando válvula está aberta
4. Sistema não irriga quando umidade está acima do alvo (evita encharcamento)
5. Controle proporcional dosa o fluxo, não apenas abre/fecha

Requirements: CTRL-01, CTRL-02, IRRG-01, IRRG-02, IRRG-03

### Phase 3: Multizona & Interface
**Goal:** Controlar até 3 zonas e exibir status no painel do CLP  
**Mode:** mvp  
**Success Criteria**:
1. CLP gerencia até 3 zonas de irrigação independentemente
2. Painel/HMI exibe umidade atual e estado da válvula por zona
3. Usuário define limiares mínimo e máximo de umidade via painel do CLP
4. Usuário liga/desliga o sistema manualmente via chave ou botão
5. Indicador visual mostra se irrigação está ativa ou parada

Requirements: CTRL-03, CTRL-04, IFCE-01, IFCE-02, IFCE-03

### Phase 4: Integração & Demo na Feira
**Goal:** Integrar tudo e validar demonstração ao vivo na feira do SENAI  
**Mode:** mvp  
**Success Criteria**:
1. Sistema completo (sensor + CLP + válvula + aspersor) funciona integrado
2. Demonstração ao vivo: solo seco → válvula abre → aspersor irriga → solo úmido → válvula fecha
3. Sistema opera de forma estável durante toda a feira sem falhas

Requirements: Todas as v1 (SENS-01..SEG-02)

---

## Traceability Summary

| Requirement | Phase | Status |
|-------------|-------|--------|
| SENS-01 | Phase 1 | Pending |
| SENS-02 | Phase 1 | Pending |
| SENS-03 | Phase 1 | Pending |
| SEG-01 | Phase 1 | Pending |
| SEG-02 | Phase 1 | Pending |
| CTRL-01 | Phase 2 | Pending |
| CTRL-02 | Phase 2 | Pending |
| IRRG-01 | Phase 2 | Pending |
| IRRG-02 | Phase 2 | Pending |
| IRRG-03 | Phase 2 | Pending |
| CTRL-03 | Phase 3 | Pending |
| CTRL-04 | Phase 3 | Pending |
| IFCE-01 | Phase 3 | Pending |
| IFCE-02 | Phase 3 | Pending |
| IFCE-03 | Phase 3 | Pending |

**Coverage:**
- v1 requirements: 15 total
- Mapped to phases: 15
- Unmapped: 0 ✓

---
*Roadmap created: 2026-10-20*
*Last updated: 2026-10-20 after initial roadmap creation*