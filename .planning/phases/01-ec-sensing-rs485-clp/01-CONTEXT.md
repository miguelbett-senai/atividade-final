# CONTEXT.md — Phase 1: EC Sensing & RS-485 Communication to CLP

**Project:** Sistema de Irrigação Automatizada por EC (Fertirrigação) — Grupo 4, UC de Automação Industrial (Miguel Bett et al.)
**Phase:** 1 — EC Sensing & RS-485 Communication to CLP
**Status:** Decisions captured for planning; open items listed as dúvidas/necessidades (relatório item 8)

---

## Domain

Phase 1 delivers: read EC (and related sensor values) from RS-485/Modbus RTU soil sensor(s), transmit to the CLP, and detect communication failures. Output of this phase is a plan + component research + technical viability check — **not** the CLP program and **not** the full assembly (per Etapa 1 guidelines).

---

## Decisions Locked (carried into planning)

| Decision | Value | Source |
|----------|-------|--------|
| CLP model | Siemens S7-1200 1214C DC/DC/DC | User |
| EC purpose | Fertirrigação (condutividade → dosagem de nutrientes) | User |
| Phase 1 deliverable | Planejamento, pesquisa de componentes, verificação de viabilidade técnica | User + Etapa 1 PDF |
| CLP program | Not required in Phase 1 | Etapa 1 PDF |
| Full assembly | Not required in Phase 1 | Etapa 1 PDF |
| Variáveis de corte | Definidas por planta/cultura (ajustadas posteriormente) | User |
| Sensor de solo | Umidade, temperatura e EC; 3 hastes inox; IP68; cabo 3m; Modbus RTU RS-485 / 0-10V / 4-20mA | AliExpress links |
| Sensor EC faixa | 0–20000 µS/cm, precisão ±3% | AliExpress spec |
| RS485 típico | 4800 ou 9600 bps, 8N1, endereço padrão 1 | AliExpress spec |
| Alimentação RS485 | 5–30 V DC | AliExpress spec |
| Segundo sensor (ar) | Necessário para umidade/temperatura do ar; mesmo barramento, endereço Modbus diferente | Mermaid diagram + analysis |

---

## Open Items — Dúvidas e Necessidades (Relatório item 8)

The following gray areas were deliberately left **unanswered** for Phase 1. They belong in the "Dúvidas e necessidades" section of the Etapa 1 deliverable and must be raised before construction:

1. **Tempo de varredura (scan time)** — A cada quanto o CLP lê os sensores? (1s / 5s / 10s / 30s / outro) — não definido.
2. **Critério de falha de comunicação** — Quantas tentativas sem resposta antes de declarar falha e fechar a válvula? — não definido.
3. **Configuração do barramento Modbus** — Baudrate (9600 vs 4800), paridade, endereço — não confirmado com o vendedor/professor.
4. **Segundo sensor (ar)** — Incluir na Phase 1 ou deferir para fase futura? — não decidido.
5. **Limites de sanidade das leituras** — Valores mínimo/máximo aceitáveis para umidade, EC e temperatura — dependem da planta/cultura, ainda não escolhida.
6. **Tabela de registradores Modbus** — Não informada pelo vendedor; deve ser solicitada. (AliExpress spec pendente)
7. **Pinagem exata dos fios do sensor** — Não informada; deve ser solicitada ao vendedor. (AliExpress spec pendente)
8. **Dimensões, peso e tempo de resposta do sensor** — Não informados. (AliExpress spec pendente)
9. **Modelo da válvula** — O grupo possui algumas válvulas; ainda é preciso definir qual usar (tensão, acionamento, vazão, pressão, conexões hidráulicas).
10. **Disponibilidade de equipamentos no laboratório** — Compressor, motor trifásico, inversor de frequência, tanque, válvula, CLP — verificar com o professor (viabilidade de laboratório).

---

## Canonical Refs (downstream agents must read before planning)

| Ref | Full relative path | Notes |
|-----|-------------------|-------|
| PROJECT.md | `.planning/PROJECT.md` | Project context, core value, constraints |
| REQUIREMENTS.md | `.planning/REQUIREMENTS.md` | v1 requirements SENS-01/02, COMM-01/02 mapped to Phase 1 |
| ROADMAP.md | `.planning/ROADMAP.md` | Phase 1 goal and success criteria |
| Etapa 1 PDF (extracted) | `pdf_extract.txt` | Full text of the AUIN final projects definition document |
| Sensor AliExpress (EC/umidade/temp) | `https://pt.aliexpress.com/item/1005010754579465.html` | Soil sensor spec source |
| Sensor/CLP AliExpress (CLP 1214C) | `https://pt.aliexpress.com/item/1005012413969180.html` | CLP and related spec source |
| Manual reference | `https://manuals.plus/asin/B07VNFDQRJ.pdf` | General reference manual |

---

## Code Context (reusable assets / patterns)

- No existing codebase — greenfield project. No reusable assets identified.
- No `.planning/codebase/` maps exist.
- No prior CONTEXT.md files (first phase).

---

## Requirements Mapped to Phase 1

| Requirement | Status |
|-------------|--------|
| SENS-01: Medir EC com sensor RS-485/Modbus RTU | Mapped to Phase 1 |
| SENS-02: Fornecer leitura de EC com faixa adequada | Mapped to Phase 1 |
| COMM-01: Transmitir dados ao CLP via RS-485/Modbus RTU | Mapped to Phase 1 |
| COMM-02: Detectar falha de comunicação com o sensor | Mapped to Phase 1 |

---

## Next Step

Run `/gsd-plan-phase 1` to create the plan for Phase 1 using these locked decisions and open items.

---
*Last updated: 20 de outubro de 2026 after Phase 1 discussion*
