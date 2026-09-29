# SentinelTrade

Plataforma distribuída de negociação financeira da corretora fictícia **Orion Capital**, desenvolvida como Exercício Integrador da disciplina de Projeto de Software (Universidade Presbiteriana Mackenzie, FCI).

> Projeto acadêmico. Todos os ativos, contas e cotações são **simulados**. Não há integração com bolsa real nem uso de dinheiro real.

## Visão do projeto

O SentinelTrade permite que investidores autorizados acompanhem cotações, mantenham sua carteira de ativos, enviem ordens de compra e venda e consultem o histórico de operações, com controle de risco e trilha de auditoria imutável em todas as etapas. A plataforma é pensada para ser segura, escalável e tolerante a falhas.

Principais funcionalidades:

- Autenticação com MFA
- Gestão de investidores, contas, carteiras e limites
- Recebimento de cotações em tempo quase real (provedor simulado)
- Envio, cancelamento e consulta de ordens
- Validação de saldo, custódia, limite de risco e pregão antes de transmitir a ordem
- Integração com Bolsa/Corretora simulada e acompanhamento do ciclo de vida das ordens
- Logs de auditoria imutáveis
- Notificações ao investidor
- Recuperação de falhas e prevenção de ordens duplicadas (idempotência)

Mais detalhes em [`docs/requirements/vision.md`](docs/requirements/vision.md).

## Integrantes

- Lucas dos Santos Bartolomeu
- Joaquim Lange Lima Amaral

## Tecnologias

- Python 3.11 ou superior
- pip e venv para dependências e ambiente virtual
- pytest para testes

> Acrescentar aqui o framework web, o banco de dados e a mensageria assim que definirmos.

## Como executar

**1. Clonar o repositório**

```bash
git clone https://github.com/lucasbartolomeu1/sentinel-trade.git
cd sentinel-trade
```

**2. Criar e ativar o ambiente virtual**

```bash
python -m venv .venv
source .venv/bin/activate      # Linux/macOS
# .venv\Scripts\activate       # Windows
```

**3. Instalar as dependências**

```bash
pip install -r requirements.txt
```

**4. Configurar as variáveis de ambiente**

```bash
cp .env.example .env
```

Edite o `.env` com os valores do seu ambiente. Esse arquivo **não** é versionado.

**5. Executar os testes**

```bash
pytest
```

**6. Executar a aplicação**

> O comando de execução será documentado aqui quando o ponto de entrada da aplicação estiver pronto.

## Estrutura do repositório

```
sentinel-trade/
├── docs/
│   ├── architecture/      # decisões e desenho de arquitetura
│   ├── diagrams/
│   │   ├── src/           # fontes dos diagramas (.drawio)
│   │   └── exports/       # diagramas exportados (PNG/PDF)
│   └── requirements/      # visão, requisitos, regras, critérios e matriz
├── src/
│   └── sentinel_trade/    # código da aplicação
├── tests/                 # testes automatizados
├── .env.example           # modelo de variáveis de ambiente
├── .gitignore
├── requirements.txt
└── README.md
```

## Documentação

| Documento | Descrição |
|-----------|-----------|
| [`vision.md`](docs/requirements/vision.md) | Visão, objetivos, escopo e atores |
| [`requirements.md`](docs/requirements/requirements.md) | Requisitos funcionais e não funcionais |
| [`regras-de-negocio.md`](docs/requirements/regras-de-negocio.md) | Regras de negócio |
| [`criterios-de-aceitacao.md`](docs/requirements/criterios-de-aceitacao.md) | Critérios de aceitação |
| [`matriz-rastreabilidade.md`](docs/requirements/matriz-rastreabilidade.md) | Requisito, caso de uso, implementação e teste |
| [`docs/diagrams/exports/`](docs/diagrams/exports/) | Diagrama de casos de uso (UML) |

## Segurança

- Nunca versione senhas, tokens, chaves de API ou dados sensíveis.
- Use o arquivo `.env` (ignorado pelo Git) para configurações locais e mantenha o `.env.example` apenas com valores de exemplo.
