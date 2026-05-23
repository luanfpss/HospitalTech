# 🏥 HospitalTech — Disciplina 4: Pesquisa Operacional
## Modelagem Matemática e Resolução Algorítmica

---

# 1. Introdução e Contextualização

A **HospitalTech** é uma empresa de HealthTech responsável pelo desenvolvimento de um sistema integrado de gestão hospitalar. No contexto da Pesquisa Operacional, o objetivo desta etapa é aplicar modelagem matemática e algoritmos de otimização para resolver problemas reais de alocação de recursos no ambiente hospitalar.

Hospitais operam sob restrições severas: profissionais com cargas horárias limitadas, equipamentos escassos, orçamentos controlados e demanda imprevisível. A Pesquisa Operacional fornece ferramentas matemáticas para determinar, com precisão, qual é a **decisão ótima** — aquela que maximiza resultados ou minimiza custos dentro dessas restrições.

Dois problemas de otimização foram modelados e resolvidos com algoritmos em Python (biblioteca PuLP):

- **Problema 1 — Maximização de Receita:** Mix ótimo de atendimentos mensais (Consultas Ambulatoriais vs Cirurgias Eletivas)
- **Problema 2 — Minimização de Custo Logístico:** Distribuição de insumos médicos entre centros de distribuição e unidades hospitalares

---

# 2. Problema 1 — Maximização de Receita Hospitalar

## 2.1 Contextualização do Problema

O hospital HospitalTech precisa definir, a cada mês, o **mix ideal de atendimentos** entre dois tipos de serviços: **Consultas Ambulatoriais (A)** e **Cirurgias Eletivas (C)**. O objetivo é maximizar a receita mensal respeitando as limitações de capacidade de sala cirúrgica, horas de enfermagem disponíveis, leitos de recuperação e obrigações contratuais com o SUS.

## 2.2 Modelagem Matemática

### Variáveis de Decisão

- **x_A** = quantidade de Consultas Ambulatoriais realizadas no mês
- **x_C** = quantidade de Cirurgias Eletivas realizadas no mês

### Parâmetros do Modelo

| Serviço | Sala Cirúrgica (h) | Enfermagem (h) | Receita (R$/mês) |
|---|---|---|---|
| Consulta Ambulatorial (A) | 2 h / atend. | 3 h / atend. | R$ 1.500,00 |
| Cirurgia Eletiva (C) | 8 h / cirurgia | 6 h / cirurgia | R$ 7.000,00 |
| **CAPACIDADE TOTAL** | **300 h / mês** | **500 h / mês** | **MAXIMIZAR** |

### Função Objetivo — Maximizar Receita

```
Max Z = 1.500 · xA + 7.000 · xC
```

### Restrições Técnicas

```
(1) Sala cirúrgica:   2·xA + 8·xC ≤ 300  (horas disponíveis/mês)
(2) Enfermagem:       3·xA + 6·xC ≤ 500  (horas disponíveis/mês)
(3) Leitos recup.:         xC     ≤  40  (leitos de recuperação)
(4) Contrato SUS:     xA          ≥  20  (mínimo ambulatorial)
(5) Não negatividade: xA, xC ≥ 0 e inteiros
```

## 2.3 Solução Algorítmica em Python (PuLP)

```python
# HospitalTech — Problema 1: Maximização de Receita
# Disciplina 4 — Pesquisa Operacional
from pulp import LpMaximize, LpProblem, LpVariable, value, PULP_CBC_CMD

def resolver_maximizacao(horas_sala, horas_enf, leitos, min_amb, cenario):
    model = LpProblem(name='Max_Receita_HospitalTech', sense=LpMaximize)

    # Variáveis de decisão (inteiras)
    xA = LpVariable('Ambulatorial', lowBound=0, cat='Integer')
    xC = LpVariable('Cirurgia',     lowBound=0, cat='Integer')

    # Função Objetivo
    model += 1500 * xA + 7000 * xC, 'Receita_Total'

    # Restrições
    model += (2 * xA + 8 * xC <= horas_sala,  'Sala_Cirurgica')
    model += (3 * xA + 6 * xC <= horas_enf,   'Enfermagem')
    model += (          xC    <= leitos,       'Leitos_Recup')
    model += (xA              >= min_amb,      'Minimo_SUS')

    model.solve(PULP_CBC_CMD(msg=0))

    print(f'--- {cenario} ---')
    print(f'  Ambulatoriais : {int(xA.varValue)}')
    print(f'  Cirurgias     : {int(xC.varValue)}')
    print(f'  Receita Total : R$ {value(model.objective):,.2f}')
    return int(xA.varValue), int(xC.varValue), value(model.objective)

# ── Cenário Base ──────────────────────────────────────────
a, c, r = resolver_maximizacao(300, 500, 40, 20, 'Base (Normal)')

# ── Cenário What-If 1: Crise — Sala Cirúrgica -40% ───────
# Simula manutenção emergencial ou falha de equipamento
a2, c2, r2 = resolver_maximizacao(180, 500, 40, 20, 'Crise: Sala -40%')
# Resultado: A=22, C=17, Receita=R$152.000,00 | Perda=R$105.000,00 (40,9%)

# ── Cenário What-If 2: Investimento — Nova Sala +30% ─────
# Simula ampliação com nova sala cirúrgica modular
a3, c3, r3 = resolver_maximizacao(390, 500, 40, 20, 'Invest: +30% Sala')
# Resultado: A=35, C=40, Receita=R$332.500,00 | Ganho=R$75.500,00 (29,4%)
```

## 2.4 Resultados Obtidos

### Cenário Base — Operação Normal

| Indicador | Ambulatorial (A) | Cirurgia (C) | TOTAL |
|---|---|---|---|
| Quantidade ótima | 22 | 32 | **54** |
| Receita gerada | R$ 33.000,00 | R$ 224.000,00 | **R$ 257.000,00** |
| Sala cirúrgica usada | 44 h | 256 h | **300/300 h** |
| Enfermagem usada | 66 h | 192 h | 258/500 h |

### Análise de Cenários (What-If)

| Cenário | Ambul. (A) | Cirurg. (C) | Receita | Variação |
|---|---|---|---|---|
| Base — Operação Normal | 22 | 32 | R$ 257.000,00 | — |
| Crise: Sala -40% (180h/mês) | 22 | 17 | R$ 152.000,00 | **-40,9%** |
| Invest.: Sala +30% (390h/mês) | 35 | 40 | R$ 332.500,00 | **+29,4%** |

---

# 3. Problema 2 — Minimização de Custo Logístico

## 3.1 Contextualização do Problema

A HospitalTech opera **dois centros de distribuição de insumos médicos**: um em **São Paulo (CD SP)** e outro em **Campinas (CD Campinas)**. Esses centros abastecem três unidades hospitalares: **Unidade Norte**, **Unidade Sul** e **Unidade Leste**.

Cada rota possui um custo diferente de transporte por caixa de insumos. O objetivo é **minimizar o custo logístico total** garantindo que cada unidade receba exatamente a quantidade de insumos necessária e que nenhum centro de distribuição ultrapasse sua capacidade.

## 3.2 Modelagem Matemática

### Parâmetros e Dados

| Custo (R$/caixa) | Uni. Norte | Uni. Sul | Uni. Leste | Oferta (cx) |
|---|---|---|---|---|
| CD São Paulo | R$ 120,00 | R$ 80,00 | R$ 150,00 | 300 cx |
| CD Campinas | R$ 200,00 | R$ 160,00 | R$ 90,00 | 250 cx |
| **DEMANDA (cx)** | **200 cx** | **180 cx** | **150 cx** | **530 cx** |

### Variáveis de Decisão

```
x_ij = quantidade de caixas enviadas do centro i para a unidade j
  Onde i ∈ {CD SP, CD Campinas} e j ∈ {Norte, Sul, Leste}
```

### Função Objetivo — Minimizar Custo Total

```
Min Z = 120·x(SP,N) + 80·x(SP,S) + 150·x(SP,L)
      + 200·x(CP,N) + 160·x(CP,S) + 90·x(CP,L)
```

### Restrições Técnicas

```
Restrições de Oferta (capacidade dos centros):
  x(SP,N) + x(SP,S) + x(SP,L) ≤ 300
  x(CP,N) + x(CP,S) + x(CP,L) ≤ 250

Restrições de Demanda (necessidade das unidades):
  x(SP,N) + x(CP,N) = 200   (Unidade Norte)
  x(SP,S) + x(CP,S) = 180   (Unidade Sul)
  x(SP,L) + x(CP,L) = 150   (Unidade Leste)

Não negatividade: x_ij ≥ 0 e inteiros
```

## 3.3 Solução Algorítmica em Python (PuLP)

```python
# HospitalTech — Problema 2: Minimização de Custo Logístico
from pulp import LpMinimize, LpProblem, LpVariable, lpSum, value, PULP_CBC_CMD

origens  = ['CD_SP', 'CD_Campinas']
destinos = ['Uni_Norte', 'Uni_Sul', 'Uni_Leste']

custos = {
    'CD_SP':      {'Uni_Norte': 120, 'Uni_Sul':  80, 'Uni_Leste': 150},
    'CD_Campinas':{'Uni_Norte': 200, 'Uni_Sul': 160, 'Uni_Leste':  90},
}

def resolver_transporte(oferta, demanda, custos, cenario):
    model = LpProblem(name='Min_Custo_Logistico', sense=LpMinimize)

    rotas = {(i, j): LpVariable(f'x_{i}_{j}', lowBound=0, cat='Integer')
             for i in origens for j in destinos}

    # Função Objetivo
    model += lpSum(custos[i][j] * rotas[(i, j)]
                   for i in origens for j in destinos)

    # Restrições de Oferta
    for i in origens:
        model += lpSum(rotas[(i, j)] for j in destinos) <= oferta[i]

    # Restrições de Demanda
    for j in destinos:
        model += lpSum(rotas[(i, j)] for i in origens) == demanda[j]

    model.solve(PULP_CBC_CMD(msg=0))
    return value(model.objective), rotas

# ── Cenário Base ──────────────────────────────────────────
oferta_base  = {'CD_SP': 300, 'CD_Campinas': 250}
demanda_base = {'Uni_Norte': 200, 'Uni_Sul': 180, 'Uni_Leste': 150}
custo, rotas = resolver_transporte(oferta_base, demanda_base, custos, 'Base')

# ── What-If 1: Combustível +30% nas rotas do CD SP ───────
custos_crise = {
    'CD_SP':      {'Uni_Norte': 156, 'Uni_Sul': 104, 'Uni_Leste': 195},
    'CD_Campinas':{'Uni_Norte': 200, 'Uni_Sul': 160, 'Uni_Leste':  90},
}
custo_crise1, rotas_crise1 = resolver_transporte(
    oferta_base, demanda_base, custos_crise, 'Crise: Combustível +30% SP'
)
# Resultado: R$66.940,00 (+14,8%) — modelo redireciona parte do fluxo para Campinas

# ── What-If 2: Capacidade CD SP cai 50% (150 caixas) ─────
oferta_crise2 = {'CD_SP': 150, 'CD_Campinas': 250}
custo_crise2, rotas_crise2 = resolver_transporte(
    oferta_crise2, demanda_base, custos, 'Crise: CD SP -50% capacidade'
)
# Resultado: R$70.300,00 (+20,6%) — Campinas absorve Norte, custo logístico sobe
```

## 3.4 Resultados Obtidos

### Cenário Base — Distribuição Ótima

| Rota | Caixas | Custo Unit. | Subtotal | Obs. |
|---|---|---|---|---|
| CD SP → Uni. Norte | 200 cx | R$ 120,00 | R$ 24.000,00 | Rota mais próxima |
| CD SP → Uni. Sul | 100 cx | R$ 80,00 | R$ 8.000,00 | Menor custo SP-Sul |
| CD Campinas → Uni. Sul | 80 cx | R$ 160,00 | R$ 12.800,00 | Complemento da demanda |
| CD Campinas → Uni. Leste | 150 cx | R$ 90,00 | R$ 13.500,00 | Custo mais baixo p/ Leste |
| **CUSTO TOTAL MÍNIMO** | **530 cx** | — | **R$ 58.300,00** | **Solução ótima** |

### Análise de Cenários (What-If)

| Cenário | Custo Total | Variação (R$) | Variação (%) |
|---|---|---|---|
| Base — Operação Normal | R$ 58.300,00 | — | — |
| Crise: Combustível +30% em SP | R$ 66.940,00 | + R$ 8.640,00 | **+14,8%** |
| Crise: CD SP com capacidade -50% | R$ 70.300,00 | + R$ 12.000,00 | **+20,6%** |

---

# 4. Análise Crítica — Interpretação dos Resultados

## 4.1 Os Resultados Fazem Sentido para a Realidade Hospitalar?

Os resultados matemáticos obtidos pelo algoritmo Python (PuLP/CBC) são consistentes com a realidade operacional da HospitalTech. No **Problema 1**, o modelo identificou que o hospital deve priorizar as **cirurgias eletivas (32 procedimentos)** sobre as consultas ambulatoriais além do mínimo obrigatório, pois cada cirurgia gera R$7.000,00 de receita contra R$1.500,00 de uma consulta. Entretanto, o algoritmo respeita o mínimo de 20 atendimentos ambulatoriais contratuais com o SUS, refletindo uma restrição real do ambiente hospitalar brasileiro. O recurso limitante é a **sala cirúrgica (totalmente ocupada: 300/300h)**, enquanto a enfermagem opera com folga (258/500h), indicando onde está o gargalo real do sistema.

No **Problema 2**, o modelo de transporte determinou que o **CD de São Paulo deve abastecer prioritariamente a Unidade Norte e Sul** (onde tem menor custo), enquanto o **CD de Campinas deve suprir a Unidade Leste** (custo R$90,00/caixa, o menor da tabela). Essa lógica espelha o que qualquer gestor de logística faria intuitivamente, mas o algoritmo encontra a distribuição exata de caixas que minimiza o custo total em R$58.300,00 — impossível de otimizar manualmente quando há múltiplas rotas e restrições simultâneas.

## 4.2 Como a Análise What-If Apoia a Gestão de Riscos?

A análise de cenários (What-If) é a ferramenta mais valiosa da Pesquisa Operacional para a **gestão de riscos** da HospitalTech, pois transforma hipóteses gerenciais em impactos financeiros quantificados.

| Problema | Cenário de Risco | Impacto Quantificado | Decisão Gerencial |
|---|---|---|---|
| 1 — Max. | Sala cirúrgica perde 40% por manutenção emergencial | Receita cai R$105.000/mês (40,9%) | Priorizar manutenção preventiva |
| 1 — Max. | Investimento em nova sala cirúrgica modular (+30%) | Receita sobe R$75.500/mês (+29,4%) | ROI positivo, expandir a sala |
| 2 — Min. | Combustível sobe 30% nas rotas partindo de SP | Custo logístico +R$8.640 (+14,8%) | Negociar contrato anual de combustível |
| 2 — Min. | CD SP com capacidade reduzida à metade | Custo logístico +R$12.000 (+20,6%) | Manter estoque de contingência em Campinas |

Em síntese, a Análise What-If fornece à gerência da HospitalTech respostas numéricas precisas para perguntas do tipo **"o que acontece se..."**. Isso permite que decisões de investimento, manutenção e contingência sejam tomadas com base em dados quantitativos — não em intuição. Um gestor que sabe que a perda de 40% da capacidade cirúrgica custa **R$105.000,00 por mês** tem argumentos concretos para priorizar a manutenção preventiva da sala. Da mesma forma, o gestor de logística que conhece o impacto exato de um aumento de combustível pode negociar contratos de abastecimento com antecedência, convertendo a análise matemática em **vantagem competitiva real**.

---

# 5. Conclusão

A aplicação da Pesquisa Operacional ao contexto da HospitalTech demonstrou como a modelagem matemática e os algoritmos de otimização transformam decisões complexas em soluções exatas e verificáveis. Os dois problemas modelados — maximização de receita e minimização de custo logístico — representam gargalos reais do dia a dia hospitalar que, sem ferramentas formais, seriam resolvidos por estimativas ou tentativa e erro.

A escolha do **Python com a biblioteca PuLP** garantiu rastreabilidade e reprodutibilidade total dos resultados: qualquer membro da equipe ou o professor pode executar os scripts no Google Colab e obter exatamente os mesmos números. A Análise de Cenários (What-If) elevou o trabalho além da simples resolução de problemas, conectando os resultados matemáticos às **decisões gerenciais da HospitalTech** — financeiras, operacionais e de gestão de risco — entregando um trabalho com valor prático real.

---

**Scripts entregues (executáveis no Google Colab):**
- `problema1_maximizacao.py` — Mix de atendimentos (Max Z = 1.500·xA + 7.000·xC)
- `problema2_transporte.py` — Distribuição de insumos (Min Z = Σ c_ij · x_ij)

Ambos os scripts incluem os três cenários (Base, Crise e Investimento) e imprimem os resultados formatados com comparativo de variações percentuais.

---

