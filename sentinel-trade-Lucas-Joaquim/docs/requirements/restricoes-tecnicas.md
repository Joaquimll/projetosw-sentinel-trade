# Restrições Técnicas: SentinelTrade

Restrições que limitam as decisões de projeto e implementação. As da seção 1 vêm do enunciado do Exercício Integrador. As da seção 2 são decisões do grupo e podem ser revistas.

## 1. Restrições do enunciado

| ID | Restrição |
|----|-----------|
| RT-01 | O sistema opera com **ativos, contas e cotações simulados**. Não há integração com bolsa real nem uso de dinheiro real. |
| RT-02 | O provedor de cotações e a Bolsa/Corretora são **serviços simulados**, acessados por API. |
| RT-03 | O sistema deve ser projetado como plataforma **distribuída, segura, escalável e tolerante a falhas**. |
| RT-04 | O projeto é desenvolvido em equipe de até 3 integrantes. |
| RT-05 | O código e a documentação ficam em **repositório Git privado**. |
| RT-06 | O repositório **não pode conter** senhas, tokens, chaves de API ou dados sensíveis. Configurações locais ficam em `.env`, e apenas o `.env.example` é versionado. |
| RT-07 | A documentação de requisitos, os diagramas e as evidências ficam na pasta `docs/`. |

## 2. Decisões do grupo

| ID | Restrição |
|----|-----------|
| RT-08 | A linguagem de implementação é **Python**. |
| RT-09 | Dependências são gerenciadas com `pip` e listadas em `requirements.txt`, dentro de ambiente virtual (`venv`). |
| RT-10 | O armazenamento das transações financeiras deve oferecer **transações ACID** (RNF-06), e a trilha de auditoria deve ser **append-only** (RNF-05). |
| RT-11 | As comunicações usam **TLS 1.3**, e os dados sensíveis em repouso usam **AES-256** (RNF-01). |
| RT-12 | As APIs de negociação exigem **Idempotency-Key** (RNF-04). |
| RT-13 | Os testes automatizados são escritos com **pytest**, na pasta `tests/`. |
| RT-14 | Os diagramas UML são feitos no **draw.io**, com fonte `.drawio` em `docs/diagrams/src/` e exportação em `docs/diagrams/exports/`. |
| RT-15 | A documentação é escrita em **Markdown**. |
| RT-16 | Os commits seguem a evolução do projeto, com mensagens descritivas (por exemplo, `docs:`, `feat:`, `test:`). |
