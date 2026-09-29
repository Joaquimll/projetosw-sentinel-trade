# Regras de Negócio: SentinelTrade

Regras que o sistema deve respeitar, independentemente da tecnologia usada. Valores numéricos (tempos, limites de tentativas) são configuráveis e podem ser ajustados pelo grupo.

| ID | Regra | Requisitos relacionados |
|----|-------|-------------------------|
| RN-01 | Nenhuma operação pode ser realizada sem autenticação concluída com MFA. | RF-01 |
| RN-02 | Uma ordem de compra só é aceita se o saldo disponível for maior ou igual a quantidade × preço (preço limite, em ordem limitada; última cotação, em ordem a mercado). | RF-05 |
| RN-03 | Uma ordem de venda só é aceita se a posição em custódia disponível for maior ou igual à quantidade. Venda a descoberto não é permitida. | RF-05 |
| RN-04 | Ordens só são aceitas com o pregão simulado aberto. | RF-05 |
| RN-05 | Ordem limitada exige preço limite maior que zero. Ordem a mercado não possui preço limite. | RF-04 |
| RN-06 | A quantidade da ordem deve ser um número inteiro maior que zero. | RF-04 |
| RN-07 | A exposição da conta não pode ultrapassar o limite operacional definido para ela. | RF-02, RF-05 |
| RN-08 | Ao aceitar uma ordem, o saldo (compra) ou a custódia (venda) correspondente fica reservado até a execução ou o encerramento da ordem. Se a ordem for cancelada ou rejeitada, a reserva é liberada. | RF-05, RF-06, RNF-06 |
| RN-09 | Só podem ser canceladas ordens nos estados Validada, Enviada ou Parcialmente Executada (nesse caso, cancela-se apenas o restante). Ordens Executadas, Rejeitadas ou Canceladas não podem ser canceladas. | RF-06 |
| RN-10 | A ordem só pode seguir estas transições: Criada → Validada ou Rejeitada; Validada → Enviada, Rejeitada ou Cancelada; Enviada → Parcialmente Executada, Executada, Rejeitada ou Cancelada; Parcialmente Executada → Executada ou Cancelada. Executada, Rejeitada e Cancelada são estados finais. | RF-08 |
| RN-11 | Toda requisição de ordem carrega uma Idempotency-Key única. Se a mesma chave for reenviada, o sistema devolve o resultado original e não cria nova ordem. | RF-04, RNF-04 |
| RN-12 | Ordens a mercado são bloqueadas se a cotação do ativo estiver desatualizada (sem atualização há mais de 5 segundos). | RF-03, RF-05 |
| RN-13 | Se o provedor de cotações ou a Bolsa simulada estiver indisponível, o sistema rejeita novas ordens (circuit breaker) e mantém inalterado o estado das ordens já existentes. | RF-07, RNF-08 |
| RN-14 | Registros de auditoria nunca são alterados ou excluídos. Uma correção é feita com um novo registro que referencia o anterior. | RF-10, RNF-05 |
| RN-15 | O investidor acessa apenas os próprios dados. O operador de compliance tem acesso somente leitura à trilha de auditoria. Alterações de contas e limites só podem ser feitas por operador autorizado. | RF-02, RF-11 |
| RN-16 | Toda mudança relevante de estado da ordem (enviada, executada, rejeitada, cancelada) e toda falha geram notificação ao investidor. | RF-09 |
| RN-17 | Após 5 tentativas consecutivas de MFA incorretas, a conta é bloqueada temporariamente e o evento é registrado na auditoria. | RF-01, RF-10 |
| RN-18 | Só podem ser negociados ativos cadastrados no sistema, dos tipos ação, ETF e FII. | RF-02, RF-04 |
