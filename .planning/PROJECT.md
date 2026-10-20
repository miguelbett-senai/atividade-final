# Horta Automática SENAI — Sistema de Irrigação Automatizada

## What This Is

Sistema automatizado de irrigação para horta, projetado para demonstração na feira de tecnologia do SENAI. Usa um CLP como controlador central, um sensor analógico de umidade do solo ASAIR AM2305B para leitura do estado do substrato, uma válvula proporcional pneumática para dosar o fluxo, e um aspersor de água com ar de alta pressão para irrigação das plantas. Escopo inicial: 1 a 3 zonas de irrigação independentes.

## Core Value

Irrigar automaticamente com base na umidade real do solo, mantendo as plantas hidratadas sem intervenção manual — e demonstrando esse controle em tempo real na feira do SENAI.

## Business Context

<!-- Opcional — projeto para feira/exibição, não monetizado. Mantido como referência institucional. -->

- **Customer**: Visitantes da feira de tecnologia do SENAI e comunidade escolar
- **Revenue model**: Não se aplica (projeto educacional/demonstrativo)
- **Success metric**: Sistema demonstra irrigação automática funcional durante a feira, com leitura de umidade e acionamento da válvula/aspersor
- **Strategy notes**: Demonstrar competência em automação industrial (CLP + sensores analógicos + pneumática proporcional)

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Ler umidade do solo via sensor ASAIR AM2305B (analógico) no CLP
- [ ] Acionar válvula proporcional pneumática com base na leitura de umidade
- [ ] Irrigar 1 a 3 zonas independentemente via aspersor de água/ar de alta pressão
- [ ] Exibir status de umidade e estado da válvula no painel do CLP
- [ ] Funcionar durante toda a feira sem falhas

### Out of Scope

- Irrigação por aplicativo móvel ou nuvem — fora do escopo da feira (foco no CLP local)
- Mais de 3 zonas de irrigação — limita a complexidade para entrega na feira
- Bomba de alta vazão ou sistema de reuso de água — escopo restrito ao aspersor água/ar

## Context

- Feira de tecnologia do SENAI; público inclui estudantes, professores e visitantes técnicos
- Demonstração ao vivo: o sistema deve ligar a válvula quando o solo estiver seco e desligar quando atingir umidade alvo
- Hardware: CLP (entrada analógica 4-20 mA ou 0-10 V), sensor ASAIR AM2305B, válvula proporcional pneumática, aspersor de água com ar comprimido de alta pressão

## Constraints

- **[Hardware]**: CLP com entrada analógica para sensor ASAIR AM2305B — sensor escolhido pelo usuário
- **[Hardware]**: Válvula proporcional pneumática — controle proporcional de fluxo, não on/off simples
- **[Hardware]**: Aspersor de água + ar de alta pressão — necessita fonte de ar comprimido
- **[Escopo]**: 1 a 3 zonas — limitado para viabilizar entrega na feira
- **[Prazo]**: Feira do SENAI — escopo enxuto, funcional para demonstração

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| CLP como controlador central | Automação industrial padrão, robusto para feira | — Pending |
| Sensor ASAIR AM2305B | Escolhido pelo usuário, leitura analógica de umidade | — Pending |
| Válvula proporcional pneumática | Controle de fluxo dosado, adequado para irrigação de precisão | — Pending |
| 1 a 3 zonas | Equilibra demonstração e complexidade para a feira | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Business Context check (if present) — customer, revenue model, success metric still accurate?
4. Audit Out of Scope — reasons still valid?
5. Update Context with current state (users, feedback, metrics)

---
*Last updated: 2026-10-20 after initialization*