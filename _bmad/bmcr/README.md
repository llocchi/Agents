# 🤝 BMCR v2.0 - BMAD Credit Recovery Module

**Sistema Multi-Agente Orquestrado de 12 Agentes para Recuperação de Crédito Inteligente com ML**

---

## 📋 Visão Geral

O módulo **BMCR v2.0** implementa uma arquitetura avançada de **12 agentes especializados** que trabalham de forma coordenada para maximizar a recuperação de crédito através de abordagem personalizada, compliance rigoroso, automação inteligente e **otimização via Machine Learning**.

## 🎯 Arquitetura de Agentes (v2.0)

### 1. **Orquestrador Estratégico** 🎯
Define políticas, objetivos e segmentação de campanhas no nível **macro-estratégico**. Não executa pipeline cliente-a-cliente, mas define diretrizes.

### 2. **Identificador** 🔐
Autentica cliente com CPF + data de nascimento + **2FA** (quando risco > 70). Max 3 tentativas. Detecta fraudes.

### 3. **Consultor de Dívidas** 📑
Consulta portfólio completo de dívidas, verifica **prescrição** (5 anos CDC), consolida múltiplas dívidas.

### 4. **Compliance & Risco** 🛡️
Valida conformidade com **LGPD** (Art. 8, 9, 37, 46), **CDC** (Art. 43 §1º, Art. 52), **BACEN** Res. 4.658/2018.

### 5. **Conhecimento do Cliente** 🧠
Prepara contexto completo: histórico de pagamentos, comunicações prévias, perfil psicográfico, estratégia de abordagem personalizada.

### 6. **Motor de Regras de Oferta** 💰
Calcula ofertas baseline usando **regras de negócio**: VPL, matriz de desconto por score (0-100) e idade da dívida. Mandato: 100% VPL positivo.

### 7. **Educador Financeiro** 💬
Trata **objeções** do cliente (5 tipos: "não tenho", "dívida prescrita", "não reconheço", "valor alto", "vou processar"). Max 3 tentativas. Converte 40-50%.

### 8. **Comunicação** ✍️
Gerencia engajamento inicial via WhatsApp com mensagens **personalizadas** e tom adequado. Taxa de resposta: 40%+.

### 9. **Otimizador de Oferta ML** 🤖
Otimiza ofertas usando **Reinforcement Learning** (Multi-Armed Bandit, Thompson Sampling). Garante +10% ROI vs regras puras.

### 10. **Super Agente Conversacional** 🦸
Orquestra **jornada completa** do cliente por conversa no WhatsApp (Steps 1-12). Chama agentes especializados nos bastidores. Mantém memória conversacional.

### 11. **Formalizador** 📝
Gerencia **assinatura digital** de acordos via Clicksign/DocuSign. CDC Art. 52 compliance. Retenção 7 anos. Taxa de assinatura: 85%+.

### 12. **Gestor de Pagamento** 💳
Processa pagamentos **PIX**/boleto/cartão/débito automático. Monitora webhooks. PCI-DSS. Conversão 1º pagamento: 90%+.

### 13. **Monitor Pós-Acordo** 🔁
Monitora acordos ativos, envia lembretes (D-3, D-1, D+1, D+3), **prevê churn** com ML, oferece renegociação proativa. Churn <15%.

### 14. **Analista de Insights** 📊
Gera **dashboards** (BigQuery, PowerBI), detecta anomalias, **retrina modelos ML** mensalmente. Acurácia: 80%+.

---

## 📊 Pipeline de Execução (14 Steps)

```
0. Orquestrador → Define políticas de campanha (objetivos, segmentação, estratégias)
1. Identificador → Autentica cliente (CPF + birthdate + 2FA se risco >70)
2. Consultor Dívidas → Consulta portfólio completo + prescrição + consolidação
3. Compliance → Valida LGPD/CDC/BACEN
4. Conhecimento → Prepara contexto e estratégia personalizada
5. Oferta Regras → Calcula baseline com VPL + matriz desconto
6. Educador → Trata objeções [CONDICIONAL - se cliente objetou]
7. Comunicação → Envia mensagem inicial WhatsApp
8. Oferta ML → Otimiza com Reinforcement Learning (+10% ROI)
9. Super Agent → Orquestra negociação conversacional completa
10. Oferta Regras → Finaliza termos acordados
11. Formalizador → Envia para assinatura digital (Clicksign)
12. Pagamento → Processa 1º pagamento (PIX/boleto/cartão)
13. Pós-Acordo → Ativa monitoramento + lembretes + prevenção churn
14. Insights → Coleta métricas + gera dashboards + retreina ML
```

---

## 🛠️ Stack Tecnológico v2.0

| Camada | Tecnologias |
|--------|-------------|
| **Orquestração** | CrewAI / LangGraph |
| **LLM** | Claude Sonnet 4 / GPT-4o |
| **WhatsApp** | Evolution API / WhatsApp Cloud API |
| **CRM** | Dynamics 365 / Salesforce / HubSpot |
| **Autenticação** | Auth0 (2FA) / OAuth 2.0 |
| **Assinatura Digital** | Clicksign / DocuSign / ICP-Brasil |
| **Pagamento** | Stripe / Iugu / Pagseguro / PIX SPI |
| **Machine Learning** | TensorFlow / PyTorch / Prophet / Scikit-learn |
| **Data Warehouse** | BigQuery / Snowflake |
| **BI** | PowerBI / Looker / Metabase |
| **Dados** | PostgreSQL + Redis Cache |
| **Deploy** | Docker / Kubernetes / Azure |
| **Monitoramento** | LangSmith / Datadog / Grafana |

---

## 📈 KPIs Esperados v2.0

| Métrica | v1.0 | **v2.0 (com ML)** |
|---------|------|-------------------|
| Taxa de Identificação | - | **85-95%** |
| Taxa de Resposta WhatsApp | 25-40% | **40-50%** |
| Taxa de Acordo | 12-20% | **20-30%** |
| Taxa de Assinatura | - | **85%+** |
| Conversão 1º Pagamento | - | **90%+** |
| Churn Rate | - | **<15%** |
| ROI vs Baseline | - | **+10%** (com ML) |
| Tempo Médio (contato→acordo) | 3-5 dias | **2-4 dias** |
| NPS | - | **≥4.3/5** |

---

## 🚀 Como Usar

### 1. Ativar o Team
```bash
Carregar team "Credit Recovery Team v2.0" via party mode
```

### 2. Executar Campanha
```yaml
Workflow: credit-recovery-campaign (v2.0)
Input: CSV com CPFs/contratos inadimplentes + objetivos de campanha
Output: 
  - Dashboards em tempo real
  - Acordos formalizados
  - Relatório executivo
```

### 3. Monitorar Acordos Ativos
```yaml
Workflow: post-deal-monitoring
Schedule: Diário às 9h
Actions:
  - Envia lembretes D-3, D-1, D0, D+1, D+3
  - Detecta risco de churn
  - Oferece renegociação proativa
```

### 4. Analytics & ML Retraining
```yaml
Workflow: insights-generation
Schedule: Semanal (domingos 2h)
Actions:
  - Gera dashboards pipeline + financeiro
  - Detecta anomalias e bottlenecks
  - Retrina modelos ML (churn, objeções, ofertas)
  - Envia relatório executivo por email
```

---

## ⚖️ Compliance & Governança

✅ **LGPD**: Artigos 8º, 9º, 37, 46 (consentimento, anonimização, registro auditável)  
✅ **CDC**: Art. 43 §1º (prescrição 5 anos), Art. 52 (formalização clara acordos)  
✅ **BACEN**: Resolução 4.658/2018 (débito automático)  
✅ **ICP-Brasil**: Assinaturas digitais válidas com Clicksign/DocuSign  
✅ **Horários**: Contato apenas 8h-20h (horário local do cliente)  
✅ **Frequência**: Máximo 1 contato a cada 48h  
✅ **Auditoria**: Registro completo de todas interações (7 anos retenção)  
✅ **PCI-DSS**: Pagamentos seguros com tokenização  

---

## 📞 Agentes Disponíveis v2.0

| Agente | Comando | Arquivo | Status |
|--------|---------|---------|--------|
| Orquestrador Estratégico | `orchestrator` | [orchestrator.md](agents/orchestrator.md) | v2.0 ✅ |
| Identificador | `identification` | [identification.md](agents/identification.md) | v2.0 ✅ |
| Consultor de Dívidas | `debt` | [debt.md](agents/debt.md) | v2.0 ✅ |
| Compliance & Risco | `compliance` | [compliance.md](agents/compliance.md) | v1.0 ✅ |
| Conhecimento | `knowledge` | [knowledge.md](agents/knowledge.md) | v1.0 ✅ |
| Motor Regras Oferta | `offer-rules` | [offer-rules.md](agents/offer-rules.md) | v2.0 ✅ |
| Educador Financeiro | `educator` | [educator.md](agents/educator.md) | v2.0 ✅ |
| Comunicação | `communication` | [communication.md](agents/communication.md) | v1.0 ✅ |
| Otimizador ML | `offer-ml` | [offer-ml.md](agents/offer-ml.md) | v2.0 ✅ |
| Super Agent | `recovery` | [recovery.md](agents/recovery.md) | v2.0 ✅ |
| Formalizador | `formalization` | [formalization.md](agents/formalization.md) | v2.0 ✅ |
| Gestor Pagamento | `payment` | [payment.md](agents/payment.md) | v2.0 ✅ |
| Monitor Pós-Acordo | `post-deal` | [post-deal.md](agents/post-deal.md) | v2.0 ✅ |
| Analista Insights | `monitoring` | [monitoring.md](agents/monitoring.md) | v2.0 ✅ |

---

## 🎯 Workflows Principais v2.0

| Workflow | Descrição | Schedule |
|----------|-----------|----------|
| **credit-recovery-campaign** | Pipeline completo 14 passos com 12 agentes | Manual/API trigger |
| **post-deal-monitoring** | Monitoramento de acordos ativos + prevenção churn | Diário 9h |
| **insights-generation** | Analytics, dashboards, ML retraining | Semanal domingo 2h |
| **client-negotiation** | Negociação individual cliente específico | Manual |
| **campaign-monitoring** | Monitora métricas campanha em tempo real | Contínuo |
| **compliance-audit** | Auditoria conformidade regulatória | Mensal |

---

## 🔬 Componentes de Machine Learning

### 1. **Churn Predictor** (Post-Deal Monitor)
- **Modelo**: Random Forest / XGBoost
- **Features**: Histórico pagamento, tempo resposta, interações, score crédito
- **Acurácia**: ≥80%
- **Retreinamento**: Mensal

### 2. **Objection Classifier** (Educador)
- **Modelo**: BERT fine-tuned (Portuguese)
- **Classes**: 5 tipos objeções + sentimento
- **F1-Score**: ≥0.85
- **Retreinamento**: Mensal

### 3. **Offer Optimizer** (Oferta ML)
- **Modelo**: Multi-Armed Bandit (Thompson Sampling)
- **Objetivo**: Maximizar ROI mantendo conversão
- **ROI Lift**: +10% vs baseline
- **Exploração**: 15-20%

### 4. **Anomaly Detection** (Insights Analyst)
- **Modelo**: Prophet + Isolation Forest
- **Métricas**: Taxa identificação, assinatura, conversão, churn
- **Alertas**: Automáticos quando desvio >2σ

---

## 📝 Notas de Implementação v2.0

✅ Sistema projetado para **escala massiva** (milhares clientes/dia)  
✅ Arquitetura **resiliente** com retry automático e fallback  
✅ Personalização baseada em **ML + regras de negócio**  
✅ **Compliance by design** (LGPD/CDC/BACEN desde arquitetura)  
✅ **Observabilidade full**: logs, traces, métricas, dashboards  
✅ **A/B testing** integrado para experimentação contínua  
✅ **API-first**: todos agentes expõem APIs REST  
✅ **Event-driven**: webhooks para pagamentos, assinaturas, respostas WhatsApp  

---

## 🏗️ Próximos Passos (Roadmap)

- [ ] **Voice AI**: Integrar chamadas de voz automáticas (Vapi, ElevenLabs)
- [ ] **Multi-canal**: Adicionar SMS, Email, RCS
- [ ] **Deep Learning**: Modelos generativos para personalização extrema
- [ ] **Blockchain**: Smart contracts para acordos imutáveis
- [ ] **Gamificação**: Recompensas para pagamentos antecipados

---

**🚀 Desenvolvido com BMAD Framework v6.0.0-alpha.23**
- Integração nativa com CRMs e APIs de WhatsApp
- Observabilidade completa com LangSmith/Datadog

---

**Versão**: 1.0.0  
**Status**: Production Ready  
**Última Atualização**: 2025-02-23  
**Autor**: Luciano.locchi (via Agent Builder)
