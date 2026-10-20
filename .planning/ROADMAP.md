# Roadmap: Sistema de Irrigação Automatizada por EC (Fertirrigação)

**Project:** Sistema de Irrigação Automatizada por EC  
**Core Value:** Manter a condutividade elétrica (EC) da solução de irrigação na faixa ideal, acionando automaticamente a válvula de água para fertirrigação — sem intervenção manual  
**Mode:** mvp  
**Total phases:** 1 (initial); more phases can be added as the project evolves

---

### Phase 1: EC Sensing & RS-485 Communication to CLP
**Goal:** Read EC values from the RS-485/Modbus RTU sensor, transmit them to the CLP, and detect communication failures — the foundation everything else builds on.
**Mode:** mvp
**Success Criteria:**
1. Sensor EC leitura valores de condutividade elétrica da água com sucesso
2. Dados de EC transmitidos ao CLP via barramento RS-485/Modbus RTU
3. Falha de comunicação com o sensor é detectada pelo CLP
4. Requisitos SENS-01, SENS-02, COMM-01, COMM-02 cobertos e verificados

**Plans:** 3 plans

Plans:
- [ ] 01-01-PLAN.md — Walking skeleton end-to-end do relatório Etapa 1 (8 itens em nível mínimo)
- [ ] 01-02-PLAN.md — Especificação técnica aprofundada (CLP, módulos RS-485, sensor EC, alimentação)
- [ ] 01-03-PLAN.md — Estratégia de automação, diagrama de interligação, mapeamento Modbus, dúvidas e viabilidade

**Requirements mapped:** SENS-01, SENS-02, COMM-01, COMM-02

---

### Future Phases (placeholder — to be defined)

| Phase | Name (proposed) | Goal | Requirements |
|-------|----------------|------|--------------|
| 2 | Control Logic — Valve Open/Close Based on EC | CLP decide abertura/fecho da válvula com base na leitura de EC | CTRL-01, VALV-01, VALV-02 |
| 3 | Safety — Comm Failure → Safe State | Válvula assume condição segura quando comunicação com sensor falha | CTRL-02 |
| 4 | Demo Integration — Real Plant + Water | Sistema operando com planta e água reais na feira do SENAI | DEMO-01 |

**Coverage:**
- v1 requirements: 9 total
- Mapped to phases: 4 (SENS-01, SENS-02, COMM-01, COMM-02)
- Unmapped: 5 (CTRL-01, CTRL-02, VALV-01, VALV-02, DEMO-01) — assigned to future phases above

---

*Roadmap created: 20 de outubro de 2026*
*Last updated: 20 de outubro de 2026 — Phase 1 defined for discuss-phase*
