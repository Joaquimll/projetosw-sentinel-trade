# Requisitos Funcionais e Não Funcionais: SentinelTrade

**Disciplina:** Projeto de Software, Universidade Presbiteriana Mackenzie (FCI)
**Projeto:** Exercício Integrador, Sistema SentinelTrade (Orion Capital)
**Integrantes:** Lucas dos Santos Bartolomeu, Joaquim Lange Lima Amaral

Este documento estabelece o kick-off formal de requisitos de engenharia de software para o sistema SentinelTrade da corretora Orion Capital, baseado no padrão acadêmico e profissional adotado pela FCI / Mackenzie.

## 1. Requisitos Funcionais (RF)

Os requisitos funcionais definem as capacidades, comportamentos e funcionalidades específicas que a plataforma distribuída de trade deve executar.

| ID | RF | Descrição |
|----|----|-----------|
| RF-01 | Autenticação Multifator (MFA) | O sistema deve autenticar investidores e operadores por meio de credenciais seguras complementadas obrigatoriamente por MFA (TOTP/SMS/E-mail) antes de autorizar qualquer operação. |
| RF-02 | Gestão de Contas e Custódia | O sistema deve manter e atualizar o cadastro de investidores, contas gráficas, saldos em moeda corrente, limites operacionais e posições de custódia de ativos (ações, ETFs e FIIs). A gestão de contas e limites é feita por operador autorizado, e o investidor deve poder consultar seu saldo e sua carteira. |
| RF-03 | Ingestão de Cotações em Quase Real-Time | O sistema deve consumir, processar e disseminar cotações simuladas provenientes de um provedor de mercado externo, com atualização contínua e latência de até 1 segundo entre o recebimento e a disponibilização ao investidor. |
| RF-04 | Envio de Ordens de Compra e Venda | O sistema deve permitir a submissão de ordens de compra e venda de ativos, especificando ticker, tipo de ordem (a mercado/limitada), quantidade e preço limite (quando a ordem for limitada). |
| RF-05 | Validação Pré-Trade de Risco e Saldo | O sistema deve validar compulsoriamente saldo financeiro (em compras), posição de custódia do ativo (em vendas), limites de exposição e status do pregão antes da transmissão da ordem. |
| RF-06 | Cancelamento, Consulta e Histórico de Ordens | O sistema deve permitir a solicitação de cancelamento de ordens pendentes, a consulta detalhada do estado atual de qualquer ordem do investidor e a consulta do histórico de operações. |
| RF-07 | Integração com Bolsa Simulada | O sistema deve encaminhar ordens validadas para a API/serviço da Bolsa/Corretora simulada e processar os retornos assíncronos de execução e liquidação. |
| RF-08 | Rastreamento do Ciclo de Vida da Ordem | O sistema deve gerenciar e persistir a transição formal dos estados da ordem (Criada, Validada, Enviada, Parcialmente Executada, Executada, Rejeitada ou Cancelada). |
| RF-09 | Notificação em Tempo Real ao Investidor | O sistema deve notificar o investidor imediatamente via interface gráfica ou mensageria sobre confirmação de envio, execução, rejeição, cancelamento, falha ou oscilação severa. |
| RF-10 | Auditoria Imutável e Rastreabilidade | O sistema deve gravar registros de auditoria append-only e imutáveis para cada tentativa de login, validação de risco, despacho de ordem e alteração de saldo ou custódia. |
| RF-11 | Consulta da Trilha de Auditoria | O sistema deve permitir que o operador de compliance consulte e filtre a trilha de auditoria (por usuário, ordem, tipo de evento e período), sem possibilidade de alterar os registros. |

## 2. Requisitos Não Funcionais (RNF)

Os requisitos não funcionais estabelecem as restrições arquiteturais, critérios de qualidade, desempenho, segurança e resiliência da infraestrutura financeira.

| ID | RNF | Descrição |
|----|-----|-----------|
| RNF-01 | Segurança e Criptografia | O sistema deve aplicar criptografia em trânsito com TLS 1.3 em todos os endpoints e criptografia AES-256 para dados sensíveis em repouso no banco de dados. |
| RNF-02 | Desempenho e Baixa Latência | O tempo de processamento para validação de risco pré-trade e submissão da ordem não deve ultrapassar 100 ms sob condições nominais de carga. |
| RNF-03 | Alta Disponibilidade | O sistema deve manter disponibilidade de 99,9% durante os horários de operação do pregão simulado, operando com redundância ativa. |
| RNF-04 | Idempotência e Prevenção de Duplicidade | O sistema deve garantir tratamento idempotente nas APIs de negociação por meio de chaves exclusivas de requisição (Idempotency-Key), impedindo ordens duplicadas em caso de retry de rede. |
| RNF-05 | Imutabilidade dos Logs | Os logs de auditoria e transações financeiras devem ser persistidos em armazenamento com política de integridade estrita (append-only), impedindo alteração física ou exclusão acidental ou maliciosa. |
| RNF-06 | Consistência Transacional (ACID) | Operações financeiras que envolvam retenção de saldo e reserva de custódia de ativos devem respeitar estritamente as propriedades ACID, prevenindo condições de corrida e saldo negativo. |
| RNF-07 | Escalabilidade Horizontal | A arquitetura de serviços distribuídos e mensageria deve suportar acréscimo de instâncias sem requerer intervenção manual no núcleo de negócios. |
| RNF-08 | Tolerância a Falhas e Indisponibilidade Controlada | Em caso de perda de conexão com o provedor de cotações ou com a Bolsa simulada, o sistema deve adotar degradação graciosa (circuit breaker), rejeitando novas ordens sem corromper o estado atual. |
| RNF-09 | Conformidade Regulatória Financeira | O projeto arquitetural deve respeitar as diretrizes de governança de tecnologia em serviços financeiros baseadas em normas de mercado (como CVM / B3) e boas práticas de cibersegurança (ISO 27001). |
| RNF-10 | Retenção e Recuperabilidade | O sistema deve garantir mecanismos de recuperação de desastres (RPO igual a zero para dados transacionais e RTO inferior a 60 segundos para restabelecimento de serviços). |
| RNF-11 | Proteção de Dados Pessoais (LGPD) | O sistema deve tratar os dados pessoais de investidores conforme a LGPD, coletando apenas o necessário, restringindo o acesso por perfil e mascarando dados sensíveis em logs e telas. |
| RNF-12 | Observabilidade e Monitoramento | O sistema deve expor logs estruturados, métricas (latência, taxa de erro, filas) e alertas para falhas de integração, permitindo diagnosticar incidentes sem acesso direto ao banco de dados. |
