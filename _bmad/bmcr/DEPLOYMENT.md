# 🚀 BMCR v2.0 Deployment Guide

## Guia Completo de Deploy do Módulo Credit Recovery v2.0

**Versão**: 2.0.0  
**Data**: 2025-02-23  
**Autor**: BMAD Agent Builder  
**Evolução**: v1.0 (6 agentes) → v2.0 (12 agentes + ML)

---

## 📋 Pré-Requisitos v2.0

### Infraestrutura Base
- [ ] Node.js 18+ instalado
- [ ] Python 3.10+ instalado (com ML libraries)
- [ ] Docker e Docker Compose configurados
- [ ] Kubernetes (opcional, para escala massiva)
- [ ] Git configurado
- [ ] GPU (opcional, para ML training mais rápido)

### Serviços Externos
- [ ] WhatsApp Business API ou Evolution API
- [ ] CRM configurado (Dynamics 365, Salesforce ou HubSpot)
- [ ] **Auth0** (2FA / Identity Management)
- [ ] **Clicksign** ou **DocuSign** (assinatura digital)
- [ ] Gateway de pagamento PIX/Boleto/Card (Stripe, Iugu, Pagseguro)
- [ ] Redis para filas e webhooks
- [ ] PostgreSQL para acordos e audit logs
- [ ] **BigQuery** ou **Snowflake** (Data Warehouse para ML)
- [ ] **PowerBI** ou **Looker** (BI dashboards)
- [ ] LangSmith ou Datadog para monitoramento

### Credenciais Necessárias v2.0
- [ ] API Keys do LLM Provider (Claude Sonnet 4 ou GPT-4o)
- [ ] Credenciais Evolution API / WhatsApp Cloud API
- [ ] API Keys do CRM
- [ ] **Auth0 Domain + Client ID + Secret**
- [ ] **Clicksign API Key** ou **DocuSign Integration Key**
- [ ] Credenciais Gateway de Pagamento (PIX, boleto, card)
- [ ] **BigQuery Service Account JSON** ou **Snowflake credentials**
- [ ] **PowerBI Workspace ID** ou **Looker API credentials**
- [ ] Strings de conexão PostgreSQL + Redis

---

## 📂 Estrutura do Módulo BMCR v2.0

```
_bmad/bmcr/
├── config.yaml                     # Configuração central
├── README.md                       # Documentação (v2.0)
├── ARCHITECTURE.md                 # Arquitetura técnica (v2.0)
├── DEPLOYMENT.md                   # Este arquivo
├── V2-IMPLEMENTATION-SUMMARY.md    # Executive summary v2.0
├── agents/                         # 12 agentes especializados (v2.0)
│   ├── orchestrator.md             # v2.0 - Estratégico
│   ├── identification.md           # NEW - Autenticação 2FA
│   ├── debt.md                     # NEW - Consulta dívidas
│   ├── compliance.md               # v1.0
│   ├── knowledge.md                # v1.0
│   ├── offer-rules.md              # NEW - Business rules
│   ├── educator.md                 # NEW - Objection handling
│   ├── communication.md            # v1.0
│   ├── offer-ml.md                 # NEW - ML optimizer
│   ├── recovery.md                 # v2.0 - Super Agent
│   ├── formalization.md            # NEW - Digital signature
│   ├── payment.md                  # NEW - Payment processing
│   ├── post-deal.md                # NEW - Post-deal monitoring
│   └── monitoring.md               # NEW - Analytics & ML
├── workflows/                      # Workflows de execução (v2.0)
│   ├── credit-recovery-campaign.yaml       # 14 steps
│   ├── post-deal-monitoring.yaml           # NEW
│   ├── insights-generation.yaml            # NEW
│   ├── client-negotiation.yaml
│   ├── campaign-monitoring.yaml
│   └── compliance-audit.yaml
└── teams/                          # Configuração de times (v2.0)
    ├── team-credit-recovery.yaml           # 12 agents
    └── team-credit-recovery-party.csv      # 14 personas
```

---

## ⚙️ Configuração v2.0

### 1. Configurar `config.yaml`

Edite `_bmad/bmcr/config.yaml` e preencha:

```yaml
# Integrações CRM
crm_integration:
  provider: dynamics365  # ou salesforce, hubspot
  api_endpoint: https://your-crm.api.endpoint
  api_key: ${CRM_API_KEY}

# WhatsApp API
whatsapp_api:
  provider: evolution_api  # ou whatsapp_cloud_api
  api_endpoint: https://your-evolution-api.endpoint
  api_key: ${WHATSAPP_API_KEY}
  webhook_url: https://bmcr.com/webhooks/whatsapp

# Auth0 (NEW v2.0)
auth0:
  domain: YOUR_DOMAIN.auth0.com
  client_id: ${AUTH0_CLIENT_ID}
  client_secret: ${AUTH0_CLIENT_SECRET}
  audience: https://bmcr-api
  2fa_risk_threshold: 70  # Score > 70 requires 2FA

# Digital Signature (NEW v2.0)
digital_signature:
  provider: clicksign  # ou docusign
  api_key: ${CLICKSIGN_API_KEY}
  webhook_url: https://bmcr.com/webhooks/signature
  expiration_hours: 24
  icp_brasil_certified: true

# Payment Gateways (NEW v2.0)
payment_gateways:
  pix:
    provider: stripe  # ou iugu, pagseguro
    api_key: ${STRIPE_API_KEY}
    webhook_url: https://bmcr.com/webhooks/pix
    qr_code_expiry_minutes: 60
  
  boleto:
    provider: itau  # ou bradesco, bb
    api_key: ${BOLETO_API_KEY}
    expiry_days: 3
  
  card:
    provider: stripe
    api_key: ${STRIPE_API_KEY}
    3ds_enabled: true
    pci_dss_compliant: true
  
  auto_debit:
    provider: bacen
    requires_signed_authorization: true

# Data Warehouse (NEW v2.0)
data_warehouse:
  provider: bigquery  # ou snowflake
  project_id: ${GCP_PROJECT_ID}
  dataset: credit_recovery_ml
  credentials_file: ${BIGQUERY_CREDENTIALS_JSON}

# Business Intelligence (NEW v2.0)
bi_platform:
  provider: powerbi  # ou looker, metabase
  workspace_id: ${POWERBI_WORKSPACE_ID}
  api_key: ${POWERBI_API_KEY}

# Orquestração
orchestration_engine: crewai  # ou langgraph

# LLM
llm_providers:
  primary: claude_sonnet_4
  fallback: gpt4o
  claude_api_key: ${CLAUDE_API_KEY}
  openai_api_key: ${OPENAI_API_KEY}

# Machine Learning (NEW v2.0)
ml_config:
  churn_predictor:
    model_type: random_forest
    training_interval_days: 30
    accuracy_threshold: 0.80
  
  objection_classifier:
    model_type: bert_finetuned
    base_model: neuralmind/bert-base-portuguese-cased
    training_interval_days: 30
  
  offer_optimizer:
    model_type: multi_armed_bandit
    exploration_rate: 0.20
    thompson_sampling: true

# Regras de Negócio
max_contact_attempts: 3
contact_interval_hours: 48
max_discount_percentage: 50
max_installments: 24
min_vpn_requirement: 0.00
prescription_years: 5  # CDC Art. 43 §1º

# Compliance
lgpd_retention_years: 7
document_encryption: true
audit_log_enabled: true
```

### 2. Variáveis de Ambiente v2.0

Crie arquivo `.env`:

```bash
# LLM APIs
CLAUDE_API_KEY=sk-ant-xxx
OPENAI_API_KEY=sk-xxx

# WhatsApp
WHATSAPP_API_KEY=xxx
WHATSAPP_WEBHOOK_SECRET=xxx

# CRM
CRM_API_KEY=xxx
CRM_API_ENDPOINT=https://xxx

# Auth0 (NEW v2.0)
AUTH0_DOMAIN=YOUR_DOMAIN.auth0.com
AUTH0_CLIENT_ID=xxx
AUTH0_CLIENT_SECRET=xxx

# Digital Signature (NEW v2.0)
CLICKSIGN_API_KEY=xxx
DOCUSIGN_INTEGRATION_KEY=xxx
DOCUSIGN_USER_ID=xxx

# Payment Gateways (NEW v2.0)
STRIPE_API_KEY=sk_live_xxx
STRIPE_WEBHOOK_SECRET=whsec_xxx
BOLETO_API_KEY=xxx
PIX_API_KEY=xxx

# Databases
DATABASE_URL=postgresql://user:pass@localhost:5432/credit_recovery
REDIS_URL=redis://localhost:6379

# Data Warehouse (NEW v2.0)
GCP_PROJECT_ID=your-project-id
BIGQUERY_CREDENTIALS_JSON=/path/to/service-account.json
# OU
SNOWFLAKE_ACCOUNT=xxx
SNOWFLAKE_USER=xxx
SNOWFLAKE_PASSWORD=xxx

# BI Platform (NEW v2.0)
POWERBI_WORKSPACE_ID=xxx
POWERBI_API_KEY=xxx
# OU
LOOKER_CLIENT_ID=xxx
LOOKER_CLIENT_SECRET=xxx

# Monitoramento
LANGSMITH_API_KEY=xxx
DATADOG_API_KEY=xxx

      - "6379:6379"
    volumes:
      - redis_data:/data

  bmcr-orchestrator:
    build: .
    environment:
      - AGENT_MODE=orchestrator
    env_file:
      - .env
    depends_on:
      - postgres
      - redis
    ports:
      - "8000:8000"

  bmcr-workers:
    build: .
    command: celery -A bmcr.worker worker --loglevel=info
    env_file:
      - .env
    depends_on:
      - postgres
      - redis
    scale: 3

volumes:
  postgres_data:
  redis_data:
```

### 2. Executar Deploy

```bash
# Build das imagens
docker-compose build

# Subir todos os serviços
docker-compose up -d

# Verificar logs
docker-compose logs -f bmcr-orchestrator

# Verificar status
docker-compose ps
```

---

## 🔧 Integração CrewAI/LangGraph

### Instalar Dependências

```bash
pip install crewai langchain langchain-anthropic langchain-openai
pip install evolution-api-client
pip install redis celery
pip install psycopg2-binary
```

### Exemplo: Orquestração com CrewAI

```python
from crewai import Agent, Task, Crew
from langchain_anthropic import ChatAnthropic

# Configurar LLM
llm = ChatAnthropic(model="claude-sonnet-4-20250514")

# Definir agentes
orchestrator = Agent(
    role="Credit Recovery Orchestrator",
    goal="Coordinate entire recovery pipeline",
    backstory=open("_bmad/bmcr/agents/orchestrator.md").read(),
    llm=llm,
    verbose=True
)

compliance = Agent(
    role="Compliance & Risk Validator",
    goal="Ensure regulatory compliance",
    backstory=open("_bmad/bmcr/agents/compliance.md").read(),
    llm=llm
)

# ... definir outros agentes ...

# Criar tarefas
validate_compliance = Task(
    description="Validate if client can be contacted",
    agent=compliance,
    expected_output="Status: LIBERADO or BLOQUEADO with reason"
)

# Criar crew
credit_recovery_crew = Crew(
    agents=[orchestrator, compliance, knowledge, offer, communication, recovery],
    tasks=[validate_compliance, ...],
    verbose=True
)

# Executar campanha
result = credit_recovery_crew.kickoff(inputs={
    "client_batch": "path/to/clients.csv"
})
```

---

## 📊 Monitoramento

### LangSmith

```python
import os
from langsmith import Client

os.environ["LANGCHAIN_TRACING_V2"] = "true"
os.environ["LANGCHAIN_API_KEY"] = "your-key"

client = Client()

# Criar projeto para campanha
client.create_project(
    project_name="credit-recovery-campaign-001",
    description="Campaign monitoring"
)
```

### Datadog

```bash
# Instalar agent
DD_API_KEY=xxx DD_SITE="datadoghq.com" bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"

# Configurar logs
echo "
logs:
  - type: file
    path: /var/log/bmcr/campaign.log
    service: bmcr
    source: python
" >> /etc/datadog-agent/conf.d/bmcr.yaml
```

---

## ✅ Checklist de Deploy

### Pré-Deploy
- [ ] Todas credenciais configuradas no `.env`
- [ ] Banco de dados PostgreSQL rodando
- [ ] Redis rodando
- [ ] WhatsApp API testada e funcionando
- [ ] CRM API testada e funcionando

### Deploy
- [ ] Docker Compose up sem erros
- [ ] Todos containers rodando (docker ps)
- [ ] Logs sem erros críticos
- [ ] Health checks passando

### Pós-Deploy
- [ ] Executar campanha de teste com 10 clientes
- [ ] Verificar compliance checks funcionando
- [ ] Confirmar envio de mensagens WhatsApp
- [ ] Validar registro de interações no CRM
- [ ] Verificar métricas no dashboard

### Validação de Produção
- [ ] Rodar campanha piloto com 100 clientes
- [ ] Monitorar taxa de erro < 1%
- [ ] Validar taxa de resposta esperada (25-40%)
- [ ] Confirmar auditoria LGPD completa
- [ ] Performance: <30s por cliente processado

---

## 🚨 Troubleshooting

### Problema: Mensagens WhatsApp não enviam
```bash
# Verificar logs do Recovery agent
docker-compose logs bmcr-workers | grep recovery

# Testar API Evolution diretamente
curl -X POST https://your-evolution-api/message/sendText \
  -H "apikey: YOUR_KEY" \
  -d '{"number": "5511999999999", "text": "teste"}'
```

### Problema: Compliance bloqueando todos
```bash
# Verificar logs de compliance
docker-compose logs bmcr-workers | grep compliance

# Validar base LGPD
psql -d credit_recovery -c "SELECT COUNT(*) FROM lgpd_consents WHERE opt_out = false"
```

### Problema: Alta latência no pipeline
```bash
# Verificar uso de recursos
docker stats

# Escalar workers
docker-compose up -d --scale bmcr-workers=5

# Verificar fila Redis
redis-cli LLEN celery
```

---

## 📚 Próximos Passos

1. **Executar Primeira Campanha**: Use workflow `credit-recovery-campaign`
2. **Monitorar KPIs**: Dashboard em tempo real
3. **Iterar Ofertas**: Ajustar regras baseado em performance
4. **A/B Testing**: Testar variações de mensagens
5. **Escalar**: Aumentar workers conforme volume

---

## 🔗 Links Úteis

- [Documentação BMCR](../bmcr/README.md)
- [Agent Manifest](../_config/agent-manifest.csv)
- [CrewAI Docs](https://docs.crewai.com)
- [Evolution API Docs](https://doc.evolution-api.com)
- [LangSmith](https://docs.smith.langchain.com)

---

**Status**: ✅ Ready for Production  
**Última Atualização**: 2025-02-23  
**Mantenedor**: Luciano.locchi
