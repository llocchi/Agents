# 🧪 Cenários de Teste - BMCR v2.0

## Massa de Dados para Testes do Sistema Credit Recovery

---

## 📊 Resumo da Base de Teste

**Total de Clientes:** 15  
**Distribuição por Perfil de Risco:**
- 🟢 BAIXO: 5 clientes (33%)
- 🟡 MEDIO: 6 clientes (40%)
- 🔴 ALTO: 4 clientes (27%)

**Distribuição por Status:**
- NOVO: 6 clientes (primeira tentativa)
- SEM_RESPOSTA: 1 cliente
- FOLLOW_UP: 1 cliente
- BLOQUEADO: 1 cliente
- PRESCRITO_POSSIVEL: 1 cliente
- RESPONDEU_POSITIVO: 1 cliente
- OBJECAO_VALOR_ALTO: 1 cliente
- PEDIU_TEMPO: 1 cliente
- PROCESSO_JUDICIAL: 1 cliente
- SEM_INTERESSE: 1 cliente

---

## 🎯 Cenários de Teste Recomendados

### Cenário 1: Cliente Novo - Baixo Risco ✅ (IDEAL)
**Cliente:** Juliana Martins Alves  
**CPF:** 654.987.321-65  
**Valor:** R$ 950,00  
**Dias Atraso:** 30 dias  
**Score:** 720  
**Expectativa:**
- ✅ Identificação: Sucesso sem 2FA (score < 70)
- ✅ Compliance: LIBERADO
- ✅ Oferta ML: 3 opções (à vista 30%, 3x 20%, 6x 10%)
- ✅ Conversão esperada: >80%
- ✅ Método pagamento preferencial: PIX

---

### Cenário 2: Cliente Médio - Primeira Tentativa 🟡
**Cliente:** João Silva Santos  
**CPF:** 123.456.789-01  
**Valor:** R$ 3.500,00  
**Dias Atraso:** 120 dias  
**Score:** 550  
**Expectativa:**
- ⚠️ Identificação: 2FA obrigatório (score < 600)
- ✅ Compliance: LIBERADO
- ✅ Oferta ML: Desconto até 40%, 12x
- ✅ Conversão esperada: 60-70%
- ⚠️ Possível objeção: "Não tenho dinheiro agora"

---

### Cenário 3: Cliente Alto Risco - Sem Resposta 🔴
**Cliente:** Maria Oliveira Costa  
**CPF:** 987.654.321-09  
**Valor:** R$ 8.750,00  
**Dias Atraso:** 280 dias  
**Score:** 420  
**Tentativas:** 2 anteriores sem resposta  
**Expectativa:**
- 🔴 Identificação: 2FA obrigatório
- ✅ Compliance: LIBERADO
- ⚠️ Oferta ML: Desconto agressivo até 50%, 18x
- ❌ Conversão esperada: 30-40%
- 🚨 Ação: Últi tentativa antes de pausar 60 dias

---

### Cenário 4: Cliente com Objeção "Valor Alto" 💬
**Cliente:** Beatriz Ferreira Souza  
**CPF:** 147.258.369-25  
**Valor:** R$ 11.500,00  
**Dias Atraso:** 450 dias  
**Score:** 410  
**Status Atual:** Levantou objeção "Valor está muito alto"  
**Expectativa:**
- ✅ Educator Agent deve ser acionado
- 📚 Conteúdo educativo: "Juros e correção CDC"
- 🤝 Oferta alternativa: Desconto 45% + 18x
- 🎯 Conversão pós-educação: 40-50%

---

### Cenário 5: Cliente Respondeu Positivo ✅ (FAST TRACK)
**Cliente:** Fernanda Santos Lima  
**CPF:** 741.852.963-85  
**Valor:** R$ 4.100,00  
**Dias Atraso:** 180 dias  
**Status:** Respondeu "Quero negociar, quais as opções?"  
**Expectativa:**
- ⚡ SKIP Steps 1-7 (já autenticado e engajado)
- 🏃 FAST TRACK: Direto para Offer ML → Recovery → Formalização
- ✅ Conversão esperada: >85%
- 💳 Método: PIX ou Card

---

### Cenário 6: Cliente Prescrito - Alerta Compliance ⚠️
**Cliente:** Roberto Carlos Pereira  
**CPF:** 852.369.741-25  
**Valor:** R$ 6.200,00  
**Dias Atraso:** 365 dias (1 ano)  
**Expectativa:**
- 🚨 Debt Agent detecta: Prescrição em 4 anos (CDC Art. 43 §1º)
- ⚠️ Compliance: ALERTA - Abordagem deve ser extra cuidadosa
- 📝 Mensagem: Transparência total sobre prescrição
- 🎯 Oferta: Desconto máximo 50% para incentivar acordo voluntário

---

### Cenário 7: Cliente Bloqueado - Judicial 🚫
**Cliente:** Marcelo Santos Pereira  
**CPF:** 951.753.824-68  
**Valor:** R$ 18.000,00  
**Dias Atraso:** 600 dias  
**Status:** Processo judicial em andamento  
**Expectativa:**
- 🛑 Compliance Agent: BLOQUEADO
- ❌ Pipeline NÃO deve prosseguir
- 📋 Log: "Cliente em processo judicial - contato proibido"
- 🔄 Ação: Aguardar decisão judicial

---

### Cenário 8: Cliente "Pediu Tempo" - Follow-up ⏰
**Cliente:** Patricia Silva Rodrigues  
**CPF:** 852.741.963-85  
**Valor:** R$ 5.300,00  
**Status:** Pediu 15 dias para organizar finanças  
**Última Interação:** 2025-12-08  
**Expectativa:**
- ⏰ Post-Deal Agent: Agendar callback para 2025-12-23
- 💬 Mensagem follow-up: "Olá Patricia, passaram os 15 dias..."
- ✅ Oferta mantida (mesmas condições)
- 🎯 Conversão esperada: 65-75%

---

### Cenário 9: Cliente Novo - Mass Market 🟢
**Cliente:** Ricardo Alves Costa  
**CPF:** 963.258.741-25  
**Valor:** R$ 780,00  
**Dias Atraso:** 60 dias  
**Score:** 650  
**Expectativa:**
- ✅ Identificação: Sucesso rápido
- ✅ Compliance: LIBERADO
- 💰 Oferta: Desconto até 30%, max 6x (mass market)
- 📱 Canal: WhatsApp automático
- ✅ Conversão esperada: >70%
- ⚡ ROI: Alto (custo baixo, conversão alta)

---

### Cenário 10: Cliente High-Value - Atenção Especial 💎
**Cliente:** Ana Paula Rodrigues  
**CPF:** 789.123.456-78  
**Valor:** R$ 15.000,00  
**Dias Atraso:** 540 dias  
**Score:** 380  
**Status:** Bloqueado (3 tentativas falhadas)  
**Expectativa:**
- 🚫 Automático: BLOQUEADO (max tentativas)
- 👤 Escalação: Para operador humano (high-value)
- 📞 Canal: Ligação telefônica + WhatsApp
- 💰 Oferta especial: Desconto 45% + condições VIP
- 🎯 Meta: Recuperar pelo menos 50% (R$ 7.500)

---

## 🎬 Como Usar os Cenários

### Teste Unitário (1 cliente):
```
CN (Client Negotiation)
CPF: 654.987.321-65
```

### Teste Batch (múltiplos clientes):
```
RC (Run Campaign)
Input: _bmad/bmcr/data/test-clients.csv
```

### Teste Party Mode (todos os 12 agentes):
```
PM (Party Mode)
Carregar: Credit Recovery Team v2.0
```

---

## 📊 KPIs Esperados na Base de Teste

| Métrica | Meta v2.0 | Esperado Base Teste |
|---------|-----------|---------------------|
| Taxa Identificação | 85-95% | ~90% (13/15) |
| Taxa Compliance OK | 100% | ~93% (14/15) |
| Taxa Resposta | 40-50% | ~47% (7/15) |
| Taxa Acordo | 20-30% | ~27% (4/15) |
| Taxa Assinatura | 85% | ~85% (acordo=4, assinados=3) |
| Conversão 1º Pag | 90% | ~90% (assinados=3, pagos=3) |
| Churn Rate | <15% | ~10% (1/10 quebrou) |

---

## 🧪 Comandos de Teste Rápido

```bash
# Teste 1: Cliente Ideal (Juliana - baixo risco)
CN → CPF: 65498732165

# Teste 2: Cliente com Objeção (Beatriz)
CN → CPF: 14725836925

# Teste 3: Cliente Prescrito (Roberto)
CN → CPF: 85236974125

# Teste 4: Cliente Bloqueado (Ana Paula)
CN → CPF: 78912345678

# Teste 5: Campanha Completa
RC → CSV: test-clients.csv
```

---

**📝 Nota:** Todos os CPFs, nomes e dados são **fictícios** e gerados para fins de teste apenas.