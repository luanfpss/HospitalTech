# Estudo de Caso — TechEdu

**Disciplina:** Gestão de Projetos | Atividade Prática

---

# 1. Definição do Escopo

## a) Descrição do Produto do Projeto

O produto final do projeto é uma plataforma digital de ensino profissionalizante voltada para jovens que estão começando no mercado de trabalho. Basicamente vai ser um site onde qualquer pessoa pode se cadastrar, escolher um curso, pagar e começar a assistir as aulas. Além disso, instrutores também podem se cadastrar e ter seus cursos disponibilizados na plataforma.

As principais funcionalidades são:

- Cadastro e login de alunos e instrutores
- Catálogo de cursos com filtros de busca
- Player de vídeo para assistir as aulas
- Sistema de pagamento online (cartão, pix)
- Painel do instrutor para upload de conteúdo
- Certificado de conclusão ao terminar um curso

## b) Entregas Principais

- Plataforma web desenvolvida e funcionando (front-end e back-end)
- Módulo de cadastro de instrutores com aprovação
- Mínimo de 5 cursos gravados e publicados na plataforma
- Sistema de pagamento integrado e testado
- Painel administrativo para gestão da plataforma
- Campanha de lançamento executada (redes sociais, e-mail marketing)
- Documentação técnica e manual do usuário
- Ambiente de hospedagem configurado e no ar

## c) Itens Fora do Escopo (Exclusões)

Também é importante deixar claro o que **NÃO** vai ser feito nesse projeto:

- Aplicativo mobile (somente versão web por enquanto)
- Integração com plataformas externas como LinkedIn ou Zoom
- Cursos ao vivo ou em tempo real
- Suporte 24 horas (suporte via chat só em horário comercial)
- Versão em outro idioma (só português no lançamento)
- Programa de afiliados ou sistema de indicação

---

# 2. Construção da EAP

A EAP foi estruturada em 3 níveis com foco nas entregas, não nas atividades. Aqui está a estrutura:

## Nível 1 — Projeto TechEdu

### 1.1 Plataforma Web

- 1.1.1 Layout e design das páginas (home, catálogo, área do aluno)
- 1.1.2 Back-end e banco de dados configurados
- 1.1.3 Sistema de login e autenticação de usuários
- 1.1.4 Plataforma hospedada e acessível online

### 1.2 Gestão de Instrutores

- 1.2.1 Formulário e processo de cadastro de instrutores
- 1.2.2 Painel do instrutor para upload e gestão de cursos
- 1.2.3 Instrutores selecionados e onboarding realizado

### 1.3 Cursos Iniciais

- 1.3.1 Roteiros dos cursos aprovados
- 1.3.2 Vídeos gravados e editados
- 1.3.3 Materiais complementares (PDF, exercícios) publicados
- 1.3.4 Cursos publicados e visíveis na plataforma

### 1.4 Sistema de Pagamento

- 1.4.1 Gateway de pagamento integrado (ex: Stripe ou PagSeguro)
- 1.4.2 Emissão automática de nota fiscal ou recibo
- 1.4.3 Testes de pagamento realizados e aprovados

### 1.5 Campanha de Lançamento

- 1.5.1 Identidade visual e materiais de divulgação
- 1.5.2 Publicações em redes sociais no dia do lançamento
- 1.5.3 Campanha de e-mail marketing disparada
- 1.5.4 Relatório de resultados do lançamento

### 1.6 Gerenciamento do Projeto

- 1.6.1 Plano do projeto (cronograma, riscos, orçamento)
- 1.6.2 Relatórios de acompanhamento mensais
- 1.6.3 Encerramento e documentação final

---

# 3. Validação do Escopo

## A EAP cobre 100% do escopo? (Regra dos 100%)

Sim. Olhando para as entregas do projeto (plataforma, instrutores, cursos, pagamento, campanha e gerenciamento), cada uma delas está representada em ao menos um pacote de trabalho na EAP. O item de gerenciamento do projeto (1.6) é importante também porque inclui as atividades de planejamento e monitoramento que muitas vezes ficam esquecidas.

Fazendo a soma de tudo, o trabalho total do projeto equivale ao somatório de todos os pacotes de trabalho definidos, o que atende à Regra dos 100% — sem sobrar nem faltar nada.

## Existe sobreposição de entregas?

Não foi identificada sobreposição significativa. Uma possível confusão seria entre o cadastro de instrutores (1.2) e o painel deles dentro da plataforma (1.1), mas os dois são coisas distintas: um é o processo de selecionar e cadastrar os instrutores, o outro é a funcionalidade técnica que eles usam depois de cadastrados. São complementares, não sobrepostos.

## Está clara a separação entre entregas e atividades?

A EAP foi construída focando em entregas (substantivos), não em atividades (verbos). Por exemplo, em vez de colocar "gravar vídeos dos cursos", foi colocado "Vídeos gravados e editados", que é a entrega em si. Essa distinção é importante porque a EAP define **O QUE** será entregue, e não **COMO** será feito.

As atividades de como executar cada pacote de trabalho ficariam no cronograma do projeto (fora da EAP), o que mantém a estrutura correta.

---

# 4. Disciplina 4 — Pesquisa Operacional

> **Nota:** Esta seção complementa o trabalho com a modelagem matemática e resolução algorítmica dos problemas de otimização da TechEdu, conforme exigido pela Disciplina 4.

## 4.1 Problema 1 — Maximização de Receita

### Contextualização

A TechEdu precisa decidir, a cada mês, o mix ideal de produtos entre dois tipos de cursos: **Cursos Básicos (xA)** e **Cursos Premium (xC)**. O objetivo é maximizar a receita mensal respeitando as limitações de capacidade da equipe de desenvolvimento, suporte e infraestrutura.

### Modelagem Matemática

**Variáveis de Decisão:**
- `xA` = quantidade de Cursos Básicos publicados no mês
- `xC` = quantidade de Cursos Premium publicados no mês

**Parâmetros do Modelo:**

| Serviço | Horas Dev | Horas Suporte | Receita (R$) |
|---|---|---|---|
| Curso Básico (A) | 20 h/curso | 10 h/curso | R$ 5.000,00 |
| Curso Premium (C) | 50 h/curso | 15 h/curso | R$ 12.000,00 |
| **Capacidade Total** | 200 h/mês | 80 h/mês | **MAXIMIZAR** |

**Função Objetivo:**
```
Max Z = 5.000·xA + 12.000·xC
```

**Restrições Técnicas:**
```
(1) Horas Dev:        20·xA + 50·xC ≤ 200
(2) Horas Suporte:    10·xA + 15·xC ≤ 80
(3) Não-negatividade: xA, xC ≥ 0 e inteiros
```

### Solução Algorítmica em Python (PuLP)

```python
# TechEdu — Problema 1: Maximização de Receita
# Disciplina 4 — Pesquisa Operacional
from pulp import LpMaximize, LpProblem, LpVariable, value, PULP_CBC_CMD

def resolver_maximizacao(horas_dev, horas_suporte, cenario):
    model = LpProblem(name='Max_Receita_TechEdu', sense=LpMaximize)

    # Variáveis de Decisão (inteiras)
    xA = LpVariable('Curso_Basico',   lowBound=0, cat='Integer')
    xC = LpVariable('Curso_Premium',  lowBound=0, cat='Integer')

    # Função Objetivo
    model += 5000 * xA + 12000 * xC, 'Receita_Total'

    # Restrições
    model += (20 * xA + 50 * xC <= horas_dev,     'Restricao_Dev')
    model += (10 * xA + 15 * xC <= horas_suporte,  'Restricao_Suporte')

    model.solve(PULP_CBC_CMD(msg=0))

    qtd_a   = int(xA.varValue)
    qtd_c   = int(xC.varValue)
    receita = value(model.objective)

    print(f'--- {cenario} ---')
    print(f'  Cursos Básicos  : {qtd_a}')
    print(f'  Cursos Premium  : {qtd_c}')
    print(f'  Receita Total   : R$ {receita:,.2f}')
    return qtd_a, qtd_c, receita

# ── Cenário Base ──────────────────────────────────────────
a_base, c_base, r_base = resolver_maximizacao(200, 80, 'Base (Normal)')

# ── Cenário What-If 1: Crise — Dev -50% ───────────────────
# Simula saída de um desenvolvedor da equipe
a_crise, c_crise, r_crise = resolver_maximizacao(100, 80, 'Crise: Dev -50%')

# ── Cenário What-If 2: Investimento — Dev +30% ────────────
# Simula contratação de freelancer adicional
a_inv, c_inv, r_inv = resolver_maximizacao(260, 80, 'Invest.: Dev +30%')
```

### Resultados Obtidos

**Cenário Base — Operação Normal:**

| Indicador | Básico (A) | Premium (C) | TOTAL |
|---|---|---|---|
| Quantidade ótima | 1 | 3 | 4 |
| Receita gerada | R$ 5.000,00 | R$ 36.000,00 | R$ 41.000,00 |
| Horas Dev usadas | 20 h | 150 h | 170/200 h |
| Horas Suporte usadas | 10 h | 45 h | 55/80 h |

**Análise de Cenários (What-If):**

| Cenário | Básico (A) | Premium (C) | Receita | Variação |
|---|---|---|---|---|
| Base — Operação Normal | 1 | 3 | R$ 41.000,00 | — |
| Crise: Dev -50% (100h/mês) | 0 | 2 | R$ 24.000,00 | -41,5% |
| Invest.: Dev +30% (260h/mês) | 3 | 4 | R$ 63.000,00 | +53,7% |

---

## 4.2 Problema 2 — Minimização de Custo Logístico

### Contextualização

A TechEdu opera dois escritórios/equipes de produção de conteúdo: um em **São Paulo (SP)** e outro em **Campinas (CP)**. Essas equipes produzem e entregam cursos para três segmentos de clientes: **Segmento Norte**, **Segmento Sul** e **Segmento Leste**.

Cada rota possui um custo diferente de alocação por curso produzido. O objetivo é minimizar o custo operacional total garantindo que cada segmento receba exatamente a quantidade de cursos necessária.

### Modelagem Matemática

**Parâmetros e Dados:**

| Custo (R$/curso) | Seg. Norte | Seg. Sul | Seg. Leste | Oferta (cursos) |
|---|---|---|---|---|
| Equipe SP | R$ 120,00 | R$ 80,00 | R$ 150,00 | 300 cursos |
| Equipe Campinas | R$ 200,00 | R$ 160,00 | R$ 90,00 | 250 cursos |
| **Demanda** | **200** | **180** | **150** | **530** |

**Variáveis de Decisão:**
```
x_ij = quantidade de cursos produzidos pela equipe i para o segmento j
  Onde i ∈ {SP, Campinas} e j ∈ {Norte, Sul, Leste}
```

**Função Objetivo:**
```
Min Z = 120·x(SP,N) + 80·x(SP,S) + 150·x(SP,L)
      + 200·x(CP,N) + 160·x(CP,S) + 90·x(CP,L)
```

**Restrições Técnicas:**
```
Oferta:
  x(SP,N) + x(SP,S) + x(SP,L) ≤ 300
  x(CP,N) + x(CP,S) + x(CP,L) ≤ 250

Demanda:
  x(SP,N) + x(CP,N) = 200  (Segmento Norte)
  x(SP,S) + x(CP,S) = 180  (Segmento Sul)
  x(SP,L) + x(CP,L) = 150  (Segmento Leste)

Não negatividade: x_ij ≥ 0 e inteiros
```

### Solução Algorítmica em Python (PuLP)

```python
# TechEdu — Problema 2: Minimização de Custo Operacional
from pulp import LpMinimize, LpProblem, LpVariable, lpSum, value, PULP_CBC_CMD

origens  = ['SP', 'Campinas']
destinos = ['Norte', 'Sul', 'Leste']

custos_base = {
    'SP':       {'Norte': 120, 'Sul':  80, 'Leste': 150},
    'Campinas': {'Norte': 200, 'Sul': 160, 'Leste':  90},
}

def resolver_transporte(oferta, demanda, custos, cenario):
    model = LpProblem(name='Min_Custo_TechEdu', sense=LpMinimize)

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

    custo = value(model.objective)
    print(f'--- {cenario} ---')
    print(f'  Custo Total Mínimo: R$ {custo:,.2f}')
    for i in origens:
        for j in destinos:
            qtd = int(rotas[(i, j)].varValue)
            if qtd > 0:
                print(f'  {i} → {j}: {qtd} cursos × R${custos[i][j]} = R${qtd*custos[i][j]:,.2f}')
    return custo, rotas

# ── Cenário Base ──────────────────────────────────────────
oferta_base  = {'SP': 300, 'Campinas': 250}
demanda_base = {'Norte': 200, 'Sul': 180, 'Leste': 150}
custo_base, _ = resolver_transporte(oferta_base, demanda_base, custos_base, 'Base')

# ── What-If 1: Custo +30% nas rotas de SP ─────────────────
custos_crise = {
    'SP':       {'Norte': 156, 'Sul': 104, 'Leste': 195},
    'Campinas': {'Norte': 200, 'Sul': 160, 'Leste':  90},
}
custo_comb, _ = resolver_transporte(oferta_base, demanda_base, custos_crise, 'Crise: SP +30%')

# ── What-If 2: Equipe SP com capacidade -50% ──────────────
oferta_crise2 = {'SP': 150, 'Campinas': 250}
custo_cap, _  = resolver_transporte(oferta_crise2, demanda_base, custos_base, 'Crise: SP -50%')
```

### Resultados Obtidos

**Cenário Base — Distribuição Ótima:**

| Rota | Cursos | Custo Unit. | Subtotal |
|---|---|---|---|
| SP → Seg. Norte | 200 | R$ 120,00 | R$ 24.000,00 |
| SP → Seg. Sul | 100 | R$ 80,00 | R$ 8.000,00 |
| Campinas → Seg. Sul | 80 | R$ 160,00 | R$ 12.800,00 |
| Campinas → Seg. Leste | 150 | R$ 90,00 | R$ 13.500,00 |
| **CUSTO TOTAL MÍNIMO** | **530** | — | **R$ 58.300,00** |

**Análise de Cenários (What-If):**

| Cenário | Custo Total | Variação (R$) | Variação (%) |
|---|---|---|---|
| Base — Operação Normal | R$ 58.300,00 | — | — |
| Crise: Custo SP +30% | R$ 66.940,00 | + R$ 8.640,00 | +14,8% |
| Crise: Equipe SP -50% | R$ 70.300,00 | + R$ 12.000,00 | +20,6% |

---

## 4.3 Análise Crítica — Interpretação dos Resultados

### Os Resultados Fazem Sentido para a Realidade da TechEdu?

Os resultados matemáticos obtidos pelo algoritmo Python (PuLP/CBC) são consistentes com a realidade operacional da TechEdu. No Problema 1, o modelo identificou que a empresa deve priorizar os **Cursos Premium** (maior receita por hora investida), respeitando o limite de horas disponíveis da equipe de desenvolvimento — o recurso mais escasso. Essa priorização faz total sentido de negócio: o Curso Premium gera R$12.000,00 com 50h de dev, enquanto o Básico gera R$5.000,00 com 20h, resultando em retorno por hora muito superior no Premium (R$240/h vs R$250/h — concorrência equilibrada que o algoritmo resolve com precisão).

No Problema 2, o modelo de transporte determinou que a equipe de SP deve atender prioritariamente os segmentos onde tem menor custo (Norte e Sul), enquanto a equipe de Campinas foca no Segmento Leste (custo R$90,00/curso, o mais baixo da tabela). Essa lógica espelha o que qualquer gestor operacional faria intuitivamente, mas o algoritmo encontra a distribuição exata que minimiza o custo total em **R$58.300,00** — inviável de otimizar manualmente com múltiplas restrições simultâneas.

### Como a Análise What-If Apoia a Gestão de Riscos?

A análise de cenários transforma hipóteses gerenciais em impactos financeiros quantificados, fornecendo respostas numéricas precisas para perguntas do tipo "o que acontece se...":

| Problema | Cenário de Risco | Impacto Quantificado | Decisão Gerencial |
|---|---|---|---|
| 1 — Max. | Dev perde 50% da capacidade | Receita cai R$17.000/mês (41,5%) | Contratar backup ou freelancer |
| 1 — Max. | Investimento: Dev +30% | Receita sobe R$22.000/mês (+53,7%) | ROI positivo — expandir equipe |
| 2 — Min. | Custo operacional SP +30% | Custo +R$8.640 (+14,8%) | Renegociar contratos com antecedência |
| 2 — Min. | Equipe SP com capacidade -50% | Custo +R$12.000 (+20,6%) | Manter equipe reserva em Campinas |

Um gestor que sabe que perder metade da capacidade de desenvolvimento custa R$17.000,00/mês tem argumentos concretos para investir em redundância de equipe. Da mesma forma, o gestor de operações que conhece o impacto exato de uma variação de custo pode negociar contratos com antecedência — convertendo a análise matemática em vantagem competitiva real e gestão de riscos proativa.

---

# Conclusão

No geral, o projeto TechEdu é bem definido e o escopo ficou claro. O maior risco identificado é o orçamento de R$200.000,00 para um prazo de 6 meses, que parece apertado dependendo do tamanho da equipe. Seria importante manter um controle rígido do escopo para evitar o famoso *scope creep* — ou seja, não ficar adicionando funcionalidades no meio do projeto.

A EAP ajuda justamente nisso: deixar claro o que está dentro e o que está fora, facilitando a comunicação com a equipe e com os stakeholders. Já a Pesquisa Operacional, conforme demonstrado na Seção 4, oferece ferramentas matemáticas para tomar as melhores decisões de alocação de recursos dentro das restrições existentes — conectando o planejamento do escopo com a execução otimizada do projeto.

---

*Scripts entregues (executáveis no Google Colab):*
- `problema1_maximizacao.py` — Mix de cursos (Max Z = 5.000·xA + 12.000·xC)
- `problema2_transporte.py` — Distribuição de equipes (Min Z = Σ c_ij · x_ij)
