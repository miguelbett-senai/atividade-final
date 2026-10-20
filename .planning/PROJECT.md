# Sistema de Irrigação Automatizada por EC (Fertirrigação)

## O que isto é

Sistema de automação para irrigação controlada pela condutividade elétrica (EC) da água/solução nutritiva. Sensores de EC com comunicação RS-485/Modbus RTU enviam as leituras a um CLP, que decide abrir ou fechar uma válvula de água para manter a concentração de nutrientes na faixa desejada. É um protótipo para demonstração na feira de tecnologia do SENAI, operando com planta e água reais.

Projeto do Grupo 4 — UC de Automação Industrial, Etapa 1 (Descrição do projeto e especificação dos componentes).
Integrantes: Mighel Bett, Miguel Lorenzoni, Artur e Luiz Gustavo.

## Valor central

Manter a condutividade elétrica (EC) da solução de irrigação na faixa ideal, acionando automaticamente a válvula de água para fertirrigação — sem intervenção manual.

## Requisitos

### Validados

(Nenhum ainda — entregar para validar)

### Ativos (hipóteses até serem enviados e validados)

- [ ] Medir a condutividade elétrica (EC) da água com sensor RS-485/Modbus RTU
- [ ] Enviar a leitura de EC ao CLP via barramento RS-485/Modbus RTU
- [ ] Detectar falha de comunicação com o sensor EC
- [ ] Decidir, no CLP, abertura ou fecho da válvula com base na leitura de EC (fertirrigação)
- [ ] Colocar a válvula em condição segura quando houver falha de comunicação com o sensor
- [ ] Demonstrar o sistema funcionando com planta e água reais na feira do SENAI

### Fora do escopo

- Controle de pH da solução — foco inicial é apenas a EC
- Irrigação por programação de horário (sem leitura de sensor) — o controle é por EC
- Interface gráfica / supervisório — a demonstração é local no protótipo
- Bomba de injeção de fertilizante separada — a v1 usa a válvula de água disponível

## Contexto

Trabalho acadêmico da UC de Automação Industrial (Etapa 1): descrever o projeto, especificar componentes, definir estratégia de automação, desenho inicial e dúvidas. Ainda sem acesso ao edital da feira do SENAI. Nesta etapa não é necessário entregar o programa do CLP pronto nem a montagem completa — apenas planejamento, pesquisa de componentes e verificação de viabilidade.

## Restrições

- **CLP**: já disponível no grupo; o controlador deve ser compatível com RS-485/Modbus RTU ou usar conversor
- **Sensores EC**: ainda não adquiridos — devem ser RS-485/Modbus RTU (conforme o trabalho)
- **Válvula**: o grupo possui algumas válvulas; ainda é preciso definir qual usar (tensão, acionamento, vazão, pressão, conexões hidráulicas)
- **Demo da feira**: deve funcionar com planta e água reais
- **Entrega Etapa 1**: planejamento, pesquisa de componentes e verificação de viabilidade (sem programa do CLP pronto nem montagem completa)

## Decisões-chave

| Decisão | Rationale | Outcome |
|---------|-----------|---------|
| EC como variável de controle | Permite fertirrigação baseada na concentração de nutrientes | — Pendente |
| Comunicação RS-485/Modbus RTU | Especificada no trabalho da UC | — Pendente |
| Lógica de abertura/fecho da válvula | Ainda a definir com o grupo (pensar em fertirrigação) | — Pendente |

---

*Última atualização: 20 de outubro de 2026 após inicialização*

## Evolução

Este documento evolui em transições de fase e marcos do projeto.

**Após cada transição de fase:**
1. Requisitos invalidados? → Mover para Fora do Escopo com razão
2. Requisitos validados? → Mover para Validados com referência de fase
3. Novos requisitos surgiram? → Adicionar a Ativos
4. Decisões para registrar? → Adicionar a Decisões-chave
5. "O que isto é" ainda está preciso? → Atualizar se desviado

**Após cada marco:**
1. Revisão completa de todas as seções
2. Verificação do Valor central — ainda é a prioridade correta?
3. Auditar Fora do Escopo — razões ainda válidas?
4. Atualizar Contexto com o estado atual
