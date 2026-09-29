# Visão do Projeto: SentinelTrade

**Disciplina:** Projeto de Software, Universidade Presbiteriana Mackenzie (FCI)
**Cliente fictício:** Orion Capital
**Integrantes:** Lucas dos Santos Bartolomeu, Joaquim Lange Lima Amaral

## 1. Contexto

A Orion Capital é uma corretora fictícia que opera uma plataforma digital de negociação de ações, ETFs e fundos imobiliários (FIIs). Hoje suas operações dependem de sistemas pouco integrados, o que dificulta a rastreabilidade das ordens, o controle de risco e a auditoria.

Em um sistema de trade, decisões erradas, atrasadas ou não auditáveis geram impacto financeiro, regulatório e reputacional.

## 2. Visão

O **SentinelTrade** é uma plataforma distribuída de negociação financeira, segura, escalável e tolerante a falhas. Ela permite que investidores autorizados acompanhem cotações, mantenham sua carteira, enviem ordens de compra e venda e consultem o histórico de operações, com controle de risco e trilha de auditoria em todas as etapas.

## 3. Objetivos

- Autenticar usuários com múltiplo fator (MFA).
- Manter dados de investidores, contas, carteiras, ativos e limites financeiros.
- Receber cotações em tempo quase real de um provedor externo.
- Permitir o envio, o cancelamento e a consulta de ordens de compra e venda.
- Validar saldo, posição em carteira, limite de risco e situação do mercado antes de transmitir cada ordem.
- Integrar-se a uma Bolsa/Corretora simulada e acompanhar o ciclo de vida das ordens.
- Registrar logs de auditoria imutáveis.
- Notificar o investidor sobre execução, rejeição, cancelamento ou falha.
- Operar com mecanismos de recuperação, indisponibilidade controlada e prevenção de duplicidade de ordens.

## 4. Escopo

**Dentro do escopo**
- Especificação e modelagem do sistema (requisitos, casos de uso e demais artefatos de projeto).
- Operação com ativos, contas e cotações **simulados**.

**Fora do escopo**
- Integração com bolsa real.
- Uso de dinheiro real.

## 5. Atores

| Ator | Tipo | Papel |
|------|------|-------|
| Investidor | Primário | Acompanha cotações, consulta carteira, envia e cancela ordens, recebe notificações. |
| Operador de Compliance | Primário | Consulta a trilha de auditoria e apoia o controle de conformidade. |
| Provedor de Cotações | Sistema externo | Fornece cotações simuladas ao sistema. |
| Bolsa Simulada | Sistema externo | Recebe as ordens validadas e retorna execução e liquidação. |

## 6. Principais qualidades esperadas

- **Segurança:** MFA, TLS 1.3 em trânsito e AES-256 em repouso.
- **Desempenho:** validação de risco e submissão de ordem em até 100 ms sob carga nominal.
- **Disponibilidade e resiliência:** 99,9% no horário do pregão simulado, com redundância, degradação graciosa (circuit breaker) e recuperação de desastres.
- **Consistência:** operações financeiras com propriedades ACID e idempotência nas APIs de negociação.
- **Auditabilidade:** logs append-only e imutáveis, com rastreabilidade de ponta a ponta.
- **Escalabilidade:** crescimento horizontal sem intervenção manual no núcleo de negócios.

## 7. Documentos relacionados

- Requisitos funcionais e não funcionais: `docs/requirements/`
- Diagramas UML: `docs/diagrams/`
