---
phase: "01"
slug: "ec-sensing-rs485-clp"
status: draft
nyquist_compliant: false
wave_0_complete: false
created: "2026-10-20"
---

# Phase 01 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | Nenhum (projeto de hardware/documentação) — validação por checklist + simulação PLCSIM |
| **Config file** | N/A |
| **Quick run command** | Revisão manual da seção alterada do documento |
| **Full suite command** | Checklist Etapa 1 + revisão do professor |
| **Estimated runtime** | ~ manual |

---

## Sampling Rate

- **After every task commit:** Revisão manual do documento/seção alterada.
- **After every plan wave:** Revisão cruzada do grupo + professor.
- **Before `/gsd-verify-work`:** Checklist Etapa 1 completo + viabilidade de laboratório confirmada.
- **Max feedback latency:** N/A (validação documental)

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Threat Ref | Secure Behavior | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|------------|-----------------|-----------|-------------------|-------------|--------|
| 01-01-01 | 01 | 1 | SENS-01 | — | N/A | Manual (simulação) | PLCSIM: Modbus_Master DONE=TRUE e EC no DB | ❌ W0 | ⬜ pending |
| 01-01-02 | 01 | 1 | SENS-02 | — | N/A | Documental | Verificar datasheet do sensor vs. faixa de fertirrigação | ❌ W0 | ⬜ pending |
| 01-01-03 | 01 | 1 | COMM-01 | — | N/A | Manual (simulação/bancada) | PLCSIM + simulador de escravo Modbus; ou bancada com sensor real | ❌ W0 | ⬜ pending |
| 01-01-04 | 01 | 1 | COMM-02 | — | N/A | Manual (simulação) | Desconectar sensor / endereço errado → ERROR=TRUE (80C8) e CommOK=FALSE | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] Simulador de escravo Modbus para PLCSIM (ou sensor real em bancada) — cobre SENS-01/COMM-01/COMM-02
- [ ] Datasheet/tabela Modbus do sensor (solicitar ao vendedor) — cobre SENS-01/SENS-02
- [ ] Confirmar disponibilidade do CLP e módulo RS-485 no laboratório — cobre COMM-01

*If none: "Existing infrastructure covers all phase requirements."*

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Ler EC do sensor RS-485/Modbus RTU | SENS-01 | Hardware/simulação | Configurar Modbus_Master no TIA Portal, executar PLCSIM, verificar DONE e valor de EC no DB |
| Faixa 0-20000 µS/cm adequada | SENS-02 | Documental | Comparar datasheet do sensor com faixa de EC da fertirrigação |
| Transmitir dados ao CLP via RS-485/Modbus RTU | COMM-01 | Hardware/simulação | Verificar recepção do CLP via PLCSIM ou bancada real |
| Detectar falha de comunicação | COMM-02 | Hardware/simulação | Desconectar sensor/endereço errado, verificar ERROR=TRUE (80C8) e CommOK=FALSE |

*If none: "All phase behaviors have automated verification."*

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < N/A
- [ ] `nyquist_compliant: true` set in frontmatter

**Approval:** pending