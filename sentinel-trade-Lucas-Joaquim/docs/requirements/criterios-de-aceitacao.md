# Critérios de Aceitação: SentinelTrade

Formato: **Dado** (contexto), **Quando** (ação), **Então** (resultado esperado). Os valores numéricos seguem os requisitos e as regras de negócio.

## 1. Requisitos Funcionais

### RF-01: Autenticação Multifator (MFA)
- **CA-01.1** Dado um investidor com credenciais válidas, quando ele informa o código MFA correto, então o acesso é autorizado.
- **CA-01.2** Dado um investidor com credenciais válidas, quando ele informa um código MFA incorreto, então o acesso é negado e a tentativa é registrada na auditoria.
- **CA-01.3** Dado um usuário que errou o MFA 5 vezes seguidas, quando tenta novamente, então a conta está bloqueada temporariamente.

### RF-02: Gestão de Contas e Custódia
- **CA-02.1** Dado um operador autorizado, quando ele altera o limite de uma conta, então o novo limite vale para as próximas validações e a alteração é auditada.
- **CA-02.2** Dado um investidor autenticado, quando ele abre a carteira, então vê saldo e posições em custódia atualizados, apenas da própria conta.

### RF-03: Ingestão de Cotações em Quase Real-Time
- **CA-03.1** Dado que o provedor envia uma nova cotação, quando o sistema a recebe, então ela fica disponível ao investidor em até 1 segundo.

### RF-04: Envio de Ordens de Compra e Venda
- **CA-04.1** Dado um investidor autenticado e o pregão aberto, quando ele envia uma ordem limitada com ticker, quantidade e preço limite, então a ordem é criada com estado Criada.
- **CA-04.2** Dado uma ordem limitada sem preço limite, quando ela é enviada, então o sistema a recusa com mensagem de validação.
- **CA-04.3** Dado uma quantidade zero, negativa ou fracionada, quando a ordem é enviada, então o sistema a recusa.

### RF-05: Validação Pré-Trade de Risco e Saldo
- **CA-05.1** Dado um saldo insuficiente, quando o investidor envia uma ordem de compra, então a ordem é rejeitada e não é transmitida à Bolsa.
- **CA-05.2** Dado uma quantidade maior que a custódia disponível, quando o investidor envia uma ordem de venda, então a ordem é rejeitada.
- **CA-05.3** Dado o pregão fechado, quando o investidor envia qualquer ordem, então ela é rejeitada.
- **CA-05.4** Dado uma ordem que ultrapassa o limite de exposição da conta, quando ela é validada, então é rejeitada.

### RF-06: Cancelamento, Consulta e Histórico de Ordens
- **CA-06.1** Dado uma ordem pendente, quando o investidor solicita o cancelamento, então a ordem passa a Cancelada e o saldo ou a custódia reservados são liberados.
- **CA-06.2** Dado uma ordem Executada, quando o investidor tenta cancelar, então o cancelamento é negado.
- **CA-06.3** Dado um investidor autenticado, quando ele consulta o histórico, então vê suas ordens com estado, datas e valores, e nenhuma ordem de outro investidor.

### RF-07: Integração com Bolsa Simulada
- **CA-07.1** Dado uma ordem validada, quando ela é transmitida, então a Bolsa simulada a recebe e a ordem passa a Enviada.
- **CA-07.2** Dado um retorno assíncrono de execução da Bolsa, quando o sistema o processa, então o estado da ordem, o saldo e a custódia são atualizados.

### RF-08: Rastreamento do Ciclo de Vida da Ordem
- **CA-08.1** Dado uma ordem, quando o estado muda, então a nova transição é persistida com data e hora.
- **CA-08.2** Dado uma transição fora das permitidas (por exemplo, Executada → Enviada), quando ela é tentada, então o sistema a recusa.

### RF-09: Notificação em Tempo Real ao Investidor
- **CA-09.1** Dado uma ordem que muda para Enviada, Executada, Rejeitada ou Cancelada, quando a mudança ocorre, então o investidor é notificado em até 2 segundos.
- **CA-09.2** Dado uma falha no processamento da ordem, quando ela ocorre, então o investidor é notificado.

### RF-10: Auditoria Imutável e Rastreabilidade
- **CA-10.1** Dado um login, uma validação de risco, o despacho de uma ordem ou uma alteração de saldo/custódia, quando o evento ocorre, então é gravado um registro com usuário, data e hora, tipo de evento e resultado.
- **CA-10.2** Dado um registro de auditoria existente, quando alguém tenta alterá-lo ou excluí-lo, então a operação é bloqueada.

### RF-11: Consulta da Trilha de Auditoria
- **CA-11.1** Dado um operador de compliance autenticado, quando ele filtra por usuário, ordem, tipo de evento ou período, então recebe apenas os registros correspondentes, sem opção de edição.
- **CA-11.2** Dado um investidor, quando ele tenta acessar a trilha de auditoria, então o acesso é negado.

## 2. Requisitos Não Funcionais

| ID | Critério de aceitação | Como verificar |
|----|-----------------------|----------------|
| RNF-01 | Todas as conexões usam TLS 1.3, e os dados sensíveis aparecem cifrados no banco. | Scanner de TLS e inspeção direta dos campos no banco. |
| RNF-02 | A validação de risco e a submissão da ordem levam até 100 ms em 95% das requisições sob carga nominal. | Teste de carga com medição de latência. |
| RNF-03 | Disponibilidade de pelo menos 99,9% no pregão simulado, e a queda de uma instância não interrompe o serviço. | Monitoramento de uptime e derrubada manual de uma instância. |
| RNF-04 | Duas requisições com a mesma Idempotency-Key geram uma única ordem. | Teste automatizado reenviando a mesma requisição. |
| RNF-05 | Tentativas de UPDATE ou DELETE na tabela de auditoria falham. | Teste de integração no banco. |
| RNF-06 | Duas compras concorrentes que juntas excedem o saldo resultam em apenas uma aceita, e o saldo nunca fica negativo. | Teste de concorrência. |
| RNF-07 | Uma nova instância dos serviços entra em operação sem intervenção manual no núcleo de negócios, e a carga é distribuída. | Subir instância adicional e observar a distribuição. |
| RNF-08 | Com a Bolsa ou o provedor de cotações indisponível, novas ordens são rejeitadas com mensagem clara e o estado existente permanece intacto. Ao retornar o serviço, a operação normaliza. | Simular a falha das integrações. |
| RNF-09 | Existe um checklist documentado que mapeia as diretrizes de CVM/B3 e da ISO 27001 aos controles do projeto. | Revisão do documento pela equipe. |
| RNF-10 | Após a queda simulada de um serviço, ele volta em menos de 60 segundos sem perda de transações já confirmadas. | Simulação de falha e verificação de dados. |
| RNF-11 | Dados pessoais aparecem mascarados em logs e telas, e cada perfil acessa só o que lhe cabe. | Inspeção dos logs e testes de autorização. |
| RNF-12 | Logs estruturados, métricas de latência, erro e filas estão disponíveis, e falha de integração gera alerta. | Provocar falha de integração e verificar o alerta. |
