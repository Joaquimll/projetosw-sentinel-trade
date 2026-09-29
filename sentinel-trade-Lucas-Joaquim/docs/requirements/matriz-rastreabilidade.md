# Matriz de Rastreabilidade: SentinelTrade

Liga cada requisito ao caso de uso do diagrama, às regras de negócio, à implementação prevista e ao teste. As colunas de implementação e teste indicam os módulos e arquivos **previstos**. Atualize o status e os caminhos conforme o código for sendo escrito.

## 1. Casos de uso (numeração para o diagrama)

| ID | Caso de uso | Ator(es) |
|----|-------------|----------|
| UC-01 | Autenticar com MFA | Investidor, Operador de Compliance |
| UC-02 | Acompanhar Cotações | Investidor, Provedor de Cotações |
| UC-03 | Consultar Posição e Carteira | Investidor |
| UC-04 | Consultar Histórico de Ordens | Investidor |
| UC-05 | Emitir Ordem | Investidor |
| UC-06 | Emitir Ordem de Compra (especialização de UC-05) | Investidor |
| UC-07 | Emitir Ordem de Venda (especialização de UC-05) | Investidor |
| UC-08 | Cancelar Ordem Pendente | Investidor, Bolsa Simulada |
| UC-09 | Validar Risco Pré-Trade (incluído em UC-05) | Sistema |
| UC-10 | Transmitir Ordem à Bolsa (incluído em UC-05) | Bolsa Simulada |
| UC-11 | Registrar Trilha de Auditoria (incluído em UC-05 e UC-08) | Sistema |
| UC-12 | Consultar Trilha de Auditoria | Operador de Compliance |
| UC-13 | Acionar Circuit Breaker (estende UC-10 e UC-02) | Sistema |
| UC-14 | Receber Notificações | Investidor |
| UC-15 | Gerenciar Contas e Limites | Operador de Compliance |

## 2. Requisitos Funcionais

| Requisito | Caso(s) de uso | Regras de negócio | Implementação (prevista) | Teste (previsto) | Status |
|-----------|----------------|-------------------|--------------------------|------------------|--------|
| RF-01 | UC-01 | RN-01, RN-17 | `src/sentinel_trade/auth/` | `tests/test_auth.py` | Pendente |
| RF-02 | UC-03, UC-15 | RN-07, RN-15, RN-18 | `src/sentinel_trade/accounts/` | `tests/test_accounts.py` | Pendente |
| RF-03 | UC-02 | RN-12 | `src/sentinel_trade/quotes/` | `tests/test_quotes.py` | Pendente |
| RF-04 | UC-05, UC-06, UC-07 | RN-05, RN-06, RN-11, RN-18 | `src/sentinel_trade/orders/` | `tests/test_orders.py` | Pendente |
| RF-05 | UC-09 | RN-02, RN-03, RN-04, RN-07, RN-08, RN-12 | `src/sentinel_trade/risk/` | `tests/test_risk.py` | Pendente |
| RF-06 | UC-04, UC-08 | RN-08, RN-09, RN-15 | `src/sentinel_trade/orders/` | `tests/test_orders.py` | Pendente |
| RF-07 | UC-10 | RN-10, RN-13 | `src/sentinel_trade/exchange/` | `tests/test_exchange.py` | Pendente |
| RF-08 | UC-05, UC-10 | RN-10 | `src/sentinel_trade/orders/` | `tests/test_order_lifecycle.py` | Pendente |
| RF-09 | UC-14 | RN-16 | `src/sentinel_trade/notifications/` | `tests/test_notifications.py` | Pendente |
| RF-10 | UC-11 | RN-14, RN-17 | `src/sentinel_trade/audit/` | `tests/test_audit.py` | Pendente |
| RF-11 | UC-12 | RN-14, RN-15 | `src/sentinel_trade/audit/` | `tests/test_audit_query.py` | Pendente |

## 3. Requisitos Não Funcionais

| Requisito | Caso(s) de uso | Regras de negócio | Implementação (prevista) | Teste (previsto) | Status |
|-----------|----------------|-------------------|--------------------------|------------------|--------|
| RNF-01 | Transversal | RN-01 | Configuração de TLS e cifragem de campos sensíveis | Verificação manual (scanner de TLS) e `tests/test_crypto.py` | Pendente |
| RNF-02 | UC-05, UC-09 | RN-02, RN-03 | `src/sentinel_trade/risk/` | Teste de carga (`tests/perf/`) | Pendente |
| RNF-03 | Transversal | RN-13 | Infraestrutura (réplicas e balanceamento) | Monitoramento e teste de queda de instância | Pendente |
| RNF-04 | UC-05 | RN-11 | `src/sentinel_trade/orders/` | `tests/test_idempotency.py` | Pendente |
| RNF-05 | UC-11 | RN-14 | `src/sentinel_trade/audit/` e regras do banco | `tests/test_audit_immutability.py` | Pendente |
| RNF-06 | UC-05, UC-08 | RN-08 | Camada de transações do banco | `tests/test_concurrency.py` | Pendente |
| RNF-07 | Transversal | - | Arquitetura de serviços e mensageria | Teste com instância adicional | Pendente |
| RNF-08 | UC-13 | RN-13 | `src/sentinel_trade/exchange/` e `quotes/` | `tests/test_circuit_breaker.py` | Pendente |
| RNF-09 | Transversal | - | `docs/architecture/` (checklist de conformidade) | Revisão documental | Pendente |
| RNF-10 | Transversal | - | Infraestrutura (backup e recuperação) | Simulação de falha | Pendente |
| RNF-11 | UC-03, UC-12 | RN-15 | Mascaramento em logs e controle de acesso | `tests/test_privacy.py` | Pendente |
| RNF-12 | UC-13 | RN-13 | Logs estruturados, métricas e alertas | Verificação manual do alerta | Pendente |
