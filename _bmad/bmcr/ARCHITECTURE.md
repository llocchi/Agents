# 🏗️ BMCR v2.0 Architecture Documentation

## Arquitetura Técnica Detalhada do Credit Recovery Module

**Versão**: 2.0.0  
**Data**: 2025-02-23  
**Autor**: BMAD Agent Builder  
**Evolução**: v1.0 (6 agentes, 7 steps) → v2.0 (12 agentes, 14 steps + ML)

---

## 📐 Visão Geral da Arquitetura v2.0

O módulo BMCR v2.0 implementa uma arquitetura distribuída de **12 agentes especializados** com **Machine Learning integrado** que colaboram para executar recuperação de crédito inteligente, personalizada e em conformidade regulatória.

### Principais Mudanças v2.0:
- ✅ 6 novos agentes especializados (Identificador, Debt, Educador, Formalizador, Payment, Post-Deal, Monitoring)
- ✅ Divisão do Offer em **Rules Engine** + **ML Optimizer** (ROI +10%)
- ✅ Orchestrador: Tático → **Estratégico Macro** (define políticas, não executa cliente-a-cliente)
- ✅ Recovery: Negociador → **Super Agent Conversacional** (orquestra Steps 1-12 via conversa)
- ✅ Pipeline: 7 steps → **14 steps**
- ✅ ML: Churn Prediction, Objection Classifier, Offer Optimizer (Reinforcement Learning)
- ✅ Assinatura digital (Clicksign/DocuSign) + Pagamentos (PIX/boleto/cartão)
- ✅ Monitoramento pós-acordo com prevenção de churn

```
┌─────────────────────────────────────────────────────────────────────┐
│                     CAMADA DE ENTRADA v2.0                          │
│  - CSV Batch Upload                                                 │
│  - CRM Event Trigger (webhook)                                      │
│  - Scheduled Campaign (cron)                                        │
│  - API REST (single client)                                         │
└────────────────────┬────────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────────┐
│            ORCHESTRATOR ESTRATÉGICO 🎯 (v2.0)                       │
│  - Define POLÍTICAS de campanha (não executa cliente-a-cliente)     │
│  - Objetivos: ROI target, recovery rate, duração máxima             │
│  - Segmentação: score, valor dívida, idade, perfil comportamental   │
│  - Estratégia: intensidade contato, canais, horários preferenciais  │
│  - Delega EXECUÇÃO para Recovery Super Agent                        │
└────────────────────┬────────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────────┐
│                  PIPELINE SEQUENCIAL v2.0 (14 STEPS)                │
│                                                                      │
│  Step 0: [ORCHESTRATOR 🎯] → Define políticas macro-campanha       │
│          Output: Segmentação + objetivos + estratégias              │
│                     │                                                │
│                     ▼                                                │
│  Step 1: [IDENTIFICATION 🔐] → Autentica CPF + birthdate + 2FA    │
│          Output: AUTENTICADO / FALHOU (max 3 tentativas)            │
│                     │ [SE FALHOU → LOG + SKIP]                      │
│                     ▼                                                │
│  Step 2: [DEBT 📑] → Consulta portfólio + prescrição + consolida  │
│          Output: Lista dívidas válidas + consolidação               │
│                     │                                                │
│                     ▼                                                │
│  Step 3: [COMPLIANCE 🛡️] → Valida LGPD/CDC/BACEN                 │
│          Output: LIBERADO / BLOQUEADO                               │
│                     │ [SE BLOQUEADO → LOG + SKIP]                   │
│                     ▼                                                │
│  Step 4: [KNOWLEDGE 🧠] → Prepara contexto + estratégia           │
│          Output: Perfil completo + abordagem recomendada            │
│                     │                                                │
│                     ▼                                                │
│  Step 5: [OFFER-RULES 💰] → Calcula baseline (VPL + matriz)       │
│          Output: 3 ofertas baseline (100% VPL+)                     │
│                     │                                                │
│                     ▼                                                │
│  Step 6: [EDUCATOR 💬] → Trata objeções [CONDICIONAL]             │
│          Trigger: SE cliente levantou objeção                       │
│          Output: Objeção resolvida / Max 3 tentativas atingido      │
│                     │                                                │
│                     ▼                                                │
│  Step 7: [COMMUNICATION ✍️] → Engajamento inicial WhatsApp        │
│          Output: Mensagem enviada + status entrega + resposta       │
│                     │                                                │
│                     ▼                                                │
│  Step 8: [OFFER-ML 🤖] → Otimiza com Reinforcement Learning       │
│          Model: Multi-Armed Bandit (Thompson Sampling)              │
│          Output: Ofertas otimizadas (+10% ROI vs baseline)          │
│                     │                                                │
│                     ▼                                                │
│  Step 9: [RECOVERY 🦸] → Super Agent orquestra negociação         │
│          Modo: Conversacional (chama agentes 1-8 nos bastidores)    │
│          Output: ACORDO FECHADO / SEM INTERESSE / CALLBACK          │
│                     │                                                │
│              [SE SEM ACORDO → Follow-up agendado]                   │
│                     │                                                │
│                     ▼                                                │
│  Step 10: [OFFER-RULES 💰] → Finaliza termos acordados            │
│          Output: Acordo draft com todas as condições                │
│                     │                                                │
│                     ▼                                                │
│  Step 11: [FORMALIZATION 📝] → Assinatura digital (Clicksign)     │
│          Output: ASSINADO / PENDENTE / EXPIRADO (24h)               │
│                     │ [SE EXPIRADO → Reenviar ou callback]          │
│                     ▼                                                │
│  Step 12: [PAYMENT 💳] → Processa 1º pagamento (PIX/boleto/card)  │
│          Output: PAGO / PENDENTE / FALHOU                           │
│                     │ [SE FALHOU → Retry 3x ou novo método]         │
│                     ▼                                                │
│  Step 13: [POST-DEAL 🔁] → Ativa monitoramento + lembretes        │
│          Schedule: D-3, D-1, D+1, D+3                               │
│          Output: Monitoramento ATIVO + score churn                  │
│                     │                                                │
│                     ▼                                                │
│  Step 14: [MONITORING 📊] → Analytics + Dashboards + ML retrain   │
│          Frequency: Métricas 5min / Dashboards real-time            │
│          Output: KPIs + anomalies + ML performance                  │
└─────────────────────────────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────────┐
│                CAMADA DE PERSISTÊNCIA v2.0                           │
│  - PostgreSQL (acordos, histórico, audit logs - 7 anos)             │
│  - BigQuery/Snowflake (Data Warehouse para ML training)             │
│  - Redis (cache, sessões, filas, webhooks)                          │
│  - CRM (sincronização bidirecional Dynamics 365/Salesforce)         │
│  - Document Storage (S3/Azure Blob - contratos assinados)           │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🧩 Componentes Detalhados

### 1. Orchestrator Agent 🎯

**Responsabilidades:**
- Coordenação global do fluxo
- Gerenciamento de filas e priorização
- Retry logic e error handling
- Rate limiting (50 clientes/batch)
- Escalation para humanos

**Tecnologias:**
- **Queue**: Redis/RabbitMQ
- **Scheduler**: Celery/Cron
- **State Management**: Redis

**Fluxo de Execução:**
```python
def execute_campaign(client_batch):
    for client in prioritize(client_batch):
        try:
            # Step 1: Compliance
            compliance_status = compliance_agent.validate(client)
            if compliance_status == "BLOQUEADO":
                log_and_skip(client, compliance_status.reason)
                continue
            
            # Step 2: Knowledge
            profile = knowledge_agent.enrich(client)
            
            # Step 3: Offer
            offers = offer_agent.calculate(profile)
            
            # Step 4: Communication
            message = communication_agent.generate(profile, offers)
            
            # Step 5: Recovery
            result = recovery_agent.negotiate(client, message, offers)
            
            # Step 6: CRM Sync
            crm.update(client.id, result)
            
        except Exception as e:
            retry_or_escalate(client, e)
```

---

### 2. Compliance Agent 🛡️

**Responsabilidades:**
- Validação LGPD (opt-out, consentimento)
- Verificação de horários permitidos (8h-20h)
- Consulta de processos judiciais
- Validação de frequência de contatos
- Verificação de óbito e fraude

**Integra com:**
- **LGPD Consent API**: Gestão de consentimentos
- **Judicial Database**: Base de processos
- **BACEN/CDC Rules Engine**: Regras regulatórias
- **Fraud System**: Marcadores de fraude

**Checklist de Validação:**
```json
{
  "lgpd_check": "PASS|FAIL",
  "horario_check": "PASS|FAIL",
  "frequencia_check": "PASS|FAIL",
  "judicial_check": "PASS|FAIL",
  "obito_check": "PASS|FAIL",
  "fraude_check": "PASS|FAIL"
}
```

**Decisão:** Se **qualquer** check = FAIL → status = BLOQUEADO

---

### 3. Knowledge Agent 🧠

**Responsabilidades:**
- Agregação de dados de múltiplas fontes
- Cálculo de score de recuperabilidade (0-100)
- Segmentação comportamental
- Recomendação de tom e canal
- Análise de contexto (desemprego, esquecimento, má-fé)

**Integra com:**
- **CRM API**: Dynamics 365, Salesforce, HubSpot
- **Data Warehouse**: Histórico completo
- **Score Engine**: Modelo ML de propensão
- **External APIs**: Serasa, Boa Vista

**Algoritmo de Score:**
```python
score = (
    payment_history_score * 0.40 +
    debt_aging_score * 0.25 +
    previous_attempts_score * 0.20 +
    behavioral_score * 0.15
)

# Classificação
if score >= 80: classification = "alta_recuperabilidade"
elif score >= 50: classification = "media_recuperabilidade"
elif score >= 20: classification = "baixa_recuperabilidade"
else: classification = "muito_baixa" # Considerar write-off
```

---

### 4. Offer Agent 💰

**Responsabilidades:**
- Cálculo de 3 opções de acordo por cliente
- Aplicação de regras por aging e políticas
- Ajuste conforme score de recuperabilidade
- Cálculo de VPL (Valor Presente Líquido)
- Recomendação baseada em ML

**Regras de Cálculo:**

| Aging      | Max Desconto | Max Parcelas |
|------------|--------------|--------------|
| 30-90d     | 15%          | 6x           |
| 91-180d    | 25%          | 12x          |
| 181-360d   | 35%          | 18x          |
| 360+       | 50%          | 24x          |

**Ajuste por Score:**
- Score 80-100: desconto -5pp (limite inferior)
- Score 50-79: desconto padrão
- Score 20-49: desconto +5pp (limite superior)
- Score 0-19: desconto máximo + considerar write-off

**Cálculo de VPL:**
```python
def calculate_vpl(valor_parcela, num_parcelas, taxa_mensal=0.02):
    vpl = 0
    for t in range(1, num_parcelas + 1):
        vpl += valor_parcela / (1 + taxa_mensal) ** t
    
    # Haircut se score baixo
    if score < 50:
        vpl *= 0.80  # 20% de desconto no VPL
    
    return vpl

# Recomendação: opção com maior VPL esperado
vpl_esperado = vpl * probabilidade_conversao
```

---

### 5. Communication Agent ✍️

**Responsabilidades:**
- Geração de mensagens personalizadas
- Aplicação de tom adequado ao perfil
- Validação LGPD de conteúdo
- Criação de variações A/B
- Programação de follow-ups

**Templates por Estágio:**

```yaml
primeiro_contato_informal:
  max_chars: 300
  tom: leve, amigável
  exemplo: "Oi {nome}! Tudo bem? Aqui é {operador} da {empresa}. Temos uma condição especial pra você. Posso te passar os detalhes?"

apresentacao_ofertas:
  formato: enumerated_list
  exemplo: |
    {nome}, separei algumas opções:
    1⃣ À vista: R$ 7.800 (22% desconto)
    2⃣ Parcelado: 6x de R$ 1.417
    3⃣ Facilitado: 12x de R$ 792
    Qual faz mais sentido pra você?

fechamento:
  objetivo: confirmar_acordo
  exemplo: "Perfeito, {nome}! Vou gerar {tipo_pagamento}: R$ {valor}. Te envio já já. Confirma quando fizer o pagamento?"
```

**Validações:**
- ❌ Proibido mencionar "dívida", "cobrança", "débito" na 1ª mensagem
- ✅ Sempre identificar empresa e operador
- ✅ CTA claro em toda mensagem
- ✅ Tom empático, nunca ameaçador

---

### 6. Recovery Agent 🤝

**Responsabilidades:**
- Envio de mensagens via WhatsApp
- Interpretação de respostas (NLP)
- Condução de negociação seguindo árvore de decisão
- Fechamento de acordos
- Registro completo no CRM
- Agendamento de follow-ups

**Árvore de Decisão:**

```
📤 1ª MENSAGEM
    ├─ 💬 RESPONDEU
    │   ├─ "Quais opções?" → APRESENTAR_OFERTAS
    │   ├─ Positivo → APRESENTAR_OFERTAS
    │   ├─ "Sem interesse" → ENCERRAR_RESPEITOSAMENTE
    │   └─ Dúvida → ESCLARECER_DUVIDA
    ├─ ⏰ SEM RESPOSTA (48h)
    │   └─ FOLLOW_UP → (incrementa tentativas)
    ├─ 🚷 BLOQUEOU
    │   └─ REGISTRAR_BLOQUEIO → NÃO RECONTATAR
    └─ ❌ NÚMERO INVÁLIDO
        └─ ATUALIZAR_NUMERO

📋 APRESENTAR_OFERTAS
    ├─ ✅ ESCOLHEU → GERAR_PAGAMENTO
    ├─ 🔄 QUER NEGOCIAR → CONTRA_PROPOR
    ├─ ⏳ PEDIU TEMPO → AGENDAR_CALLBACK (24-48h)
    └─ ❌ RECUSOU → REGISTRAR_RECUSA (+30d retry)

💳 GERAR_PAGAMENTO
    ├─ 🎉 CONFIRMADO → ACORDO_FORMALIZADO ✅
    ├─ ⏳ NÃO PAGO → LEMBRETE_PAGAMENTO (24h)
    └─ ❌ DESISTIU → ACORDO_QUEBRADO
```

**Interpretação de Respostas (NLP):**
```python
intent_patterns = {
    "interesse_positivo": ["sim", "quero", "pode", "me manda", "aceito", "quais são"],
    "negociacao": ["muito caro", "não consigo", "desconto", "mais barato"],
    "sem_interesse": ["não quero", "não tenho interesse", "agora não"],
    "duvida": ["é da empresa?", "quem é?", "por que?", "como assim?"]
}

def classify_intent(response_text):
    # Usar NLP ou regex para classificar
    # Claude/GPT-4o também pode ser usado para interpretação
    pass
```

---

## 🔌 Integrações Externas

### WhatsApp Business API

**Opção 1: Evolution API (Open Source)**
```bash
# Endpoint
POST https://your-evolution-api.com/message/sendText
Headers:
  apikey: YOUR_API_KEY

Body:
{
  "number": "5511999999999",
  "text": "Mensagem aqui"
}

# Webhook para respostas
POST https://your-app.com/webhook/whatsapp
{
  "event": "messages.upsert",
  "data": {
    "key": {"remoteJid": "5511999999999@s.whatsapp.net"},
    "message": {"conversation": "Resposta do cliente"}
  }
}
```

**Opção 2: WhatsApp Cloud API (Oficial)**
```bash
POST https://graph.facebook.com/v18.0/{phone_number_id}/messages
Headers:
  Authorization: Bearer YOUR_TOKEN

Body:
{
  "messaging_product": "whatsapp",
  "to": "5511999999999",
  "text": {"body": "Mensagem aqui"}
}
```

### CRM Integration

**Dynamics 365:**
```python
import requests

crm_endpoint = "https://org.crm.dynamics.com/api/data/v9.2"
headers = {"Authorization": f"Bearer {access_token}"}

# Atualizar lead
response = requests.patch(
    f"{crm_endpoint}/leads(lead_id)",
    json={
        "statuscode": 3,  # Em Negociação
        "description": "Cliente respondeu com interesse",
        "bmcr_recovery_score": 75,
        "bmcr_last_interaction": "2025-02-23T14:30:00Z"
    },
    headers=headers
)
```

**Salesforce:**
```python
from simple_salesforce import Salesforce

sf = Salesforce(
    username='user@example.com',
    password='password',
    security_token='token'
)

# Atualizar oportunidade
sf.Opportunity.update(
    'opp_id',
    {'Stage': 'Negotiation', 'BMCR_Score__c': 75}
)
```

---

## 📊 Monitoramento e Observabilidade

### Métricas-Chave (KPIs)

```json
{
  "campaign_metrics": {
    "total_clients_processed": 1000,
    "blocked_by_compliance": 150,
    "successfully_contacted": 680,
    "response_rate": 0.35,
    "conversion_rate": 0.18,
    "avg_negotiation_time_hours": 72,
    "total_recovered_value": 1250000.00,
    "campaign_roi": 3.2
  },
  "agent_metrics": {
    "compliance": {
      "blocked_by_lgpd": 80,
      "blocked_by_schedule": 30,
      "blocked_by_judicial": 25,
      "validation_time_avg_ms": 150
    },
    "knowledge": {
      "profiles_enriched": 850,
      "avg_score": 62,
      "data_completeness": 0.92
    },
    "offer": {
      "offers_calculated": 850,
      "avg_discount": 0.22,
      "avg_vpl": 8200.00
    },
    "communication": {
      "messages_generated": 1500,
      "response_rate_by_tone": {
        "informal": 0.42,
        "formal": 0.31,
        "urgente": 0.28
      }
    },
    "recovery": {
      "messages_sent": 1500,
      "agreements_closed": 180,
      "avg_messages_per_deal": 5.2,
      "blocked_rate": 0.05
    }
  }
}
```

### Dashboard em Tempo Real

Tecnologias sugeridas:
- **Grafana**: Dashboards customizáveis
- **Metabase**: BI self-service
- **LangSmith**: Tracing de agentes
- **Datadog**: APM completo

---

## 🔒 Segurança e Compliance

### LGPD (Lei Geral de Proteção de Dados)

**Princípios Implementados:**
1. **Consentimento**: Validação obrigatória antes de contato
2. **Finalidade**: Uso exclusivo para recuperação de crédito
3. **Transparência**: Cliente sabe quem está contatando e por quê
4. **Segurança**: Dados sensíveis criptografados em repouso e trânsito
5. **Retenção**: Logs mantidos por período regulatório (5 anos)

**Direitos do Titular:**
- Opt-out: Sistema honra imediatamente
- Acesso: Cliente pode solicitar histórico completo
- Retificação: Dados podem ser corrigidos
- Exclusão: Right to be forgotten (após período legal)

### BACEN e CDC (Código de Defesa do Consumidor)

**Restrições Implementadas:**
- Contato apenas 8h-20h horário local
- Máximo 1 contato a cada 48h
- Linguagem respeitosa, sem ameaças
- Identificação clara da empresa
- Não mencionar dívida publicamente

### Auditoria

**Registro Completo:**
```json
{
  "audit_entry": {
    "timestamp": "2025-02-23T14:30:00Z",
    "client_id": "123456789",
    "agent": "recovery",
    "action": "message_sent",
    "channel": "whatsapp",
    "message_id": "msg_xyz",
    "compliance_validated": true,
    "operator": "system_auto",
    "outcome": "response_received"
  }
}
```

---

## 🚀 Escalabilidade

### Horizontal Scaling

```yaml
# docker-compose.scale.yml
services:
  bmcr-workers:
    image: bmcr:latest
    deploy:
      replicas: 10  # Escalar para 10 workers
    environment:
      - WORKER_CONCURRENCY=4
```

**Capacidade Estimada:**
- 1 worker: ~500 clientes/hora
- 10 workers: ~5000 clientes/hora
- 100 workers: ~50.000 clientes/hora

### Rate Limiting

```python
# Rate limiting por cliente
RATE_LIMITS = {
    "whatsapp_messages_per_client_24h": 3,
    "api_requests_per_second": 100,
    "concurrent_negotiations": 50
}
```

---

## 📈 Roadmap Futuro v2.0

### V2.1 (Q3 2025)
- [ ] Voice AI para ligações telefônicas (Vapi, ElevenLabs)
- [ ] Multi-canal expandido (SMS, Email, RCS, Telegram)
- [ ] Deep Learning para personalização extrema
- [ ] Blockchain para smart contracts imutáveis

### V2.2 (Q4 2025)
- [ ] Gamificação (recompensas pagamentos antecipados)
- [ ] Chatbot web para portal do cliente
- [ ] AutoML para otimização automática de modelos
- [ ] Multi-tenancy para diferentes empresas

### V3.0 (Q1 2026)
- [ ] Plataforma SaaS completa
- [ ] Marketplace de estratégias de recuperação
- [ ] API pública para integrações
- [ ] Mobile app para gestores

---

## 🤖 Machine Learning Components v2.0

### 1. Churn Predictor (Post-Deal Monitor)

**Tipo**: Supervised Learning - Classification  
**Objetivo**: Prever probabilidade de quebra de acordo antes que aconteça

```python
# Features Engineering
features = {
    "payment_history_score": float,  # 0-100 histórico pagamentos
    "days_since_first_contact": int,
    "installments_paid_pct": float,  # % parcelas pagas
    "avg_response_time_hours": float,
    "communication_engagement": float,  # 0-1 taxa resposta
    "credit_score": int,
    "debt_age_days": int,
    "payment_method": str,  # PIX, boleto, card
    "objections_raised": int,
    "socioeconomic_cluster": str  # A, B, C, D
}

# Model Architecture
model = RandomForestClassifier(
    n_estimators=200,
    max_depth=15,
    min_samples_split=50,
    class_weight='balanced'
)

# Training
X_train, y_train = load_historical_data(days=180)
model.fit(X_train, y_train)

# Prediction
churn_probability = model.predict_proba(client_features)
if churn_probability > 0.70:
    trigger_proactive_renegotiation(client)
```

**Performance KPIs:**
- Accuracy: ≥80%
- Precision: ≥75% (evita falsos positivos)
- Recall: ≥85% (detecta maioria dos churns reais)
- F1-Score: ≥0.80
- AUC-ROC: ≥0.85

**Retraining Schedule**: Monthly (1st Sunday 2AM)

---

### 2. Objection Classifier (Educator Agent)

**Tipo**: NLP - Multi-Class Classification  
**Objetivo**: Identificar tipo de objeção e sentimento do cliente

```python
# Classes
objection_types = [
    "financial_hardship",      # "Não tenho dinheiro agora"
    "debt_prescription",        # "Dívida prescrita"
    "debt_not_recognized",      # "Não reconheço essa dívida"
    "amount_dispute",           # "Valor está muito alto"
    "legal_threat"              # "Vou processar vocês"
]

# Model Architecture (BERT Fine-tuned)
from transformers import BertForSequenceClassification

model = BertForSequenceClassification.from_pretrained(
    "neuralmind/bert-base-portuguese-cased",
    num_labels=5
)

# Training Data
train_data = [
    {"text": "Não tenho como pagar agora, perdi meu emprego", "label": "financial_hardship"},
    {"text": "Essa dívida já prescreveu, tenho direito", "label": "debt_prescription"},
    # ... 10.000+ exemplos rotulados
]

# Fine-tuning
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_dataset,
    eval_dataset=eval_dataset
)
trainer.train()

# Inference
objection_text = "O valor está muito alto, não consigo pagar isso"
predicted_type = model.predict(objection_text)
# Output: "amount_dispute" (confidence: 0.92)
```

**Performance KPIs:**
- F1-Score: ≥0.85 (weighted average)
- Inference Time: <500ms
- Sentiment Accuracy: ≥90%

**Retraining**: Monthly + Active Learning (confidence <0.70 → human review)

---

### 3. Offer Optimizer (Reinforcement Learning)

**Tipo**: Multi-Armed Bandit - Thompson Sampling  
**Objetivo**: Maximizar ROI mantendo conversão via experimentação contínua

```python
# Arms (ofertas possíveis)
arms = {
    "arm_1": {"discount": 0.30, "installments": 1},   # À vista 30% off
    "arm_2": {"discount": 0.20, "installments": 3},   # 3x 20% off
    "arm_3": {"discount": 0.10, "installments": 6},   # 6x 10% off
    "arm_4": {"discount": 0.05, "installments": 12},  # 12x 5% off
    "arm_5": {"discount": 0.40, "installments": 1}    # À vista 40% off (agressivo)
}

# Thompson Sampling Implementation
class ThompsonSampling:
    def __init__(self, n_arms):
        self.successes = [1] * n_arms  # Prior: Beta(1,1)
        self.failures = [1] * n_arms
    
    def select_arm(self):
        samples = [
            np.random.beta(self.successes[i], self.failures[i])
            for i in range(len(self.successes))
        ]
        return np.argmax(samples)
    
    def update(self, arm, reward):
        if reward > 0:  # Deal closed
            self.successes[arm] += 1
        else:  # Deal rejected
            self.failures[arm] += 1

# Exploration vs Exploitation
exploration_rate = 0.20  # 20% explora novas ofertas
if random.random() < exploration_rate:
    selected_arm = random.choice(arms)
else:
    selected_arm = bandit.select_arm()

# Feedback Loop
client_accepted = recovery_agent.offer(selected_arm)
bandit.update(selected_arm, reward=calculate_roi(selected_arm, client_accepted))
```

**Performance KPIs:**
- ROI Lift vs Baseline: ≥+10%
- Exploration Rate: 15-20%
- Convergence Time: <1000 trials
- Regret: Minimize cumulative regret

**Retraining**: Continuous online learning + Monthly full retraining

---

### 4. Anomaly Detection (Monitoring Agent)

**Tipo**: Time Series Analysis + Unsupervised Learning  
**Objetivo**: Detectar comportamentos anômalos em métricas críticas

```python
# Time Series Forecasting (Prophet)
from fbprophet import Prophet

metrics = ["identification_rate", "signature_rate", "payment_conversion", "churn_rate"]

for metric in metrics:
    # Historical data
    df = load_metric_history(metric, days=90)
    
    # Treinar Prophet
    model = Prophet(
        changepoint_prior_scale=0.05,
        seasonality_mode='multiplicative'
    )
    model.fit(df)
    
    # Forecast próximos 7 dias
    future = model.make_future_dataframe(periods=7)
    forecast = model.predict(future)
    
    # Detectar anomalias (>2σ fora da previsão)
    current_value = get_current_value(metric)
    expected_value = forecast['yhat'].iloc[-1]
    std_dev = forecast['yhat_upper'].iloc[-1] - expected_value
    
    if abs(current_value - expected_value) > 2 * std_dev:
        alert = {
            "metric": metric,
            "current": current_value,
            "expected": expected_value,
            "severity": "HIGH" if abs(z_score) > 3 else "MEDIUM",
            "recommended_action": get_recommendation(metric)
        }
        send_alert_to_stakeholders(alert)

# Isolation Forest (outliers multivariados)
from sklearn.ensemble import IsolationForest

iso_forest = IsolationForest(contamination=0.05)
iso_forest.fit(historical_metrics)
anomalies = iso_forest.predict(current_metrics)
```

**Performance KPIs:**
- Detection Rate: ≥95% (true anomalies detected)
- False Positive Rate: <10%
- Time to Alert: <5 minutes
- Historical Lookback: 90 days

---

## 🔗 Integrações Adicionais v2.0

### Auth0 (2FA / Identity Management)
- **Endpoint**: `https://YOUR_DOMAIN.auth0.com/oauth/token`
- **Purpose**: Autenticação 2FA para identificação de alto risco (score >70)
- **Flow**: SMS OTP / Authenticator App
- **Fallback**: Email OTP se SMS indisponível

### Clicksign / DocuSign (Digital Signature)
- **Clicksign API**: `https://api.clicksign.com/v1/documents`
- **DocuSign API**: `https://demo.docusign.net/restapi`
- **Purpose**: Assinatura digital de acordos (CDC Art. 52 compliance)
- **Validity**: ICP-Brasil certified
- **Storage**: S3/Azure Blob (encrypted, 7-year retention)

### Payment Gateways
```python
# PIX (SPI - Sistema de Pagamentos Instantâneos)
pix_config = {
    "provider": "Bacen SPI",
    "qr_code_expiry": "60 minutes",
    "webhook_url": "https://bmcr.com/webhooks/pix",
    "instant_confirmation": True
}

# Boleto (Registered Boleto)
boleto_config = {
    "provider": "Itaú/Bradesco/BB",
    "expiry_days": 3,
    "registration_required": True,
    "bar_code_generation": True
}

# Credit/Debit Card
card_config = {
    "provider": "Stripe/Iugu",
    "3ds_enabled": True,
    "tokenization": True,
    "pci_dss_compliant": True
}

# Auto Debit (BACEN Res. 4.658/2018)
auto_debit_config = {
    "requires_signed_authorization": True,
    "cancellation_deadline_days": 30,
    "bacen_compliant": True
}
```

### BigQuery / Snowflake (Data Warehouse)
- **Purpose**: Armazenar dados históricos para ML training
- **Data Retention**: Ilimitado (com particionamento)
- **Tables**:
  - `agreements_history`
  - `negotiation_transcripts`
  - `payment_events`
  - `ml_training_features`

### PowerBI / Looker / Metabase (BI)
- **Dashboards**:
  - Pipeline Performance (conversion per step)
  - Financial ROI (recovered vs cost)
  - ML Model Performance (accuracy, drift)
  - Campaign Comparison (A/B testing results)

---

## 📚 Referências Técnicas v2.0

- [CrewAI Documentation](https://docs.crewai.com)
- [LangGraph Documentation](https://python.langchain.com/docs/langgraph)
- [Thompson Sampling](https://en.wikipedia.org/wiki/Thompson_sampling)
- [Prophet Time Series](https://facebook.github.io/prophet/)
- [BERT Portuguese](https://huggingface.co/neuralmind/bert-base-portuguese-cased)
- [Clicksign API](https://developers.clicksign.com)
- [PIX Bacen](https://www.bcb.gov.br/estabilidadefinanceira/pix)
- [LGPD Official Text](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
- [BACEN Resolutions](https://www.bcb.gov.br)

---

**Última Atualização**: 2025-02-23  
**Versão da Arquitetura**: 2.0.0  
**Status**: ✅ Production Ready (12 agents + ML)
