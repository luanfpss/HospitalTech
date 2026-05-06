# HOSPITALTECH — Sistema de Gestão Hospitalar
## SEGURANÇA DA INFORMAÇÃO
### Módulo 03 — Documento Complementar

| | |
|---|---|
| **Data** | Março de 2026 |
| **Elaborado por** | Equipe HospitalTech |
| **Versão** | 1.0 — Complemento |
| **Base** | OWASP Top 10, CVE, LGPD, NIST |

---

## Sumário

- [3.1 — Mapeamento de Ameaças e Matriz GUT](#31--mapeamento-de-ameaças-e-matriz-gut)
- [3.2 — Políticas de Acesso e Identidade (IAM)](#32--políticas-de-acesso-e-identidade-iam)
- [3.3 — Adequação à LGPD — RoPA Expandido](#33--adequação-à-lgpd--ropa-registro-de-operações-de-tratamento)
- [3.4 — Análise Crítica — A Vulnerabilidade Mais Perigosa](#34--análise-crítica-a-vulnerabilidade-mais-perigosa)

---

## 3.1 — Mapeamento de Ameaças e Matriz GUT

### O que é a Matriz GUT?

Imagina que o hospital tem 20 problemas de segurança para resolver, mas o orçamento só cobre 5 deles agora. Como decidir quais atacar primeiro? É aí que entra a **Matriz GUT**. Ela dá uma nota para cada ameaça com base em três perguntas simples:

- **Gravidade** — se essa falha for explorada, qual o estrago?
- **Urgência** — precisamos corrigir agora ou pode esperar?
- **Tendência** — se ignorarmos, isso tende a piorar com o tempo?

Multiplicamos os três valores e chegamos a uma pontuação. Quanto maior, mais urgente.

#### Escala de Referência da Matriz GUT

| Valor | Gravidade (G) | Urgência (U) | Tendência (T) |
|---|---|---|---|
| 5 | Catastrófico — pode fechar o hospital | Corrija agora, hoje | Espalha rápido, tipo vírus |
| 4 | Grave — vaza dados de pacientes | Corrija esta semana | Piora bastante em pouco tempo |
| 3 | Moderado — afeta parte do sistema | Corrija este mês | Piora aos poucos |
| 2 | Leve — impacto pequeno | Pode esperar 3 meses | Fica mais ou menos igual |
| 1 | Insignificante — quase não afeta | Pode esperar o ano todo | Não piora nada |

---

### As 20 Ameaças Identificadas no HospitalTech

> As ameaças abaixo foram pesquisadas com base no **OWASP Top 10** e no banco de dados **CVE**, adaptadas ao contexto específico do HospitalTech.
> **Score = G × U × T** | 🔴 Crítico ≥ 100 | 🟠 Alto ≥ 64 | 🟡 Médio ≥ 27 | 🟢 Baixo < 27

| ID | Ameaça | Fonte | G | U | T | Score | Nível |
|---|---|---|---|---|---|---|---|
| A01 | SQL Injection | OWASP A03:2021 | 5 | 5 | 5 | 125 | 🔴 CRÍTICO |
| A02 | Quebra de Autenticação (Sessão roubada) | OWASP A07:2021 | 5 | 5 | 4 | 100 | 🔴 CRÍTICO |
| A03 | Exposição de Dados Sensíveis (prontuários) | OWASP A02:2021 | 5 | 5 | 4 | 100 | 🔴 CRÍTICO |
| A04 | Controle de Acesso Quebrado (IDOR) | OWASP A01:2021 | 5 | 4 | 4 | 80 | 🟠 ALTO |
| A05 | Ransomware no servidor | CVE-2023-20269 | 5 | 5 | 5 | 125 | 🔴 CRÍTICO |
| A06 | Ataque DDoS (Negação de Serviço) | OWASP / CVE | 5 | 4 | 3 | 60 | 🟡 MÉDIO |
| A07 | Cross-Site Scripting (XSS) | OWASP A03:2021 | 4 | 4 | 4 | 64 | 🟠 ALTO |
| A08 | Injeção de Comandos no Servidor (RCE) | CVE-2021-44228 (Log4Shell) | 5 | 5 | 4 | 100 | 🔴 CRÍTICO |
| A09 | Phishing direcionado a funcionários | OWASP / CVE | 4 | 4 | 5 | 80 | 🟠 ALTO |
| A10 | Elevação de Privilégio | OWASP A01:2021 | 4 | 5 | 3 | 60 | 🟡 MÉDIO |
| A11 | Falta de Criptografia em Trânsito (HTTP puro) | OWASP A02:2021 | 4 | 5 | 3 | 60 | 🟡 MÉDIO |
| A12 | Credenciais Padrão de Fábrica | CVE-2023-1389 | 5 | 5 | 3 | 75 | 🟠 ALTO |
| A13 | Falha em Componentes de Terceiros (libs desatualizadas) | OWASP A06:2021 | 4 | 4 | 4 | 64 | 🟠 ALTO |
| A14 | Falta de Logs e Auditoria | OWASP A09:2021 | 3 | 4 | 4 | 48 | 🟡 MÉDIO |
| A15 | Configuração Insegura do Servidor | OWASP A05:2021 | 4 | 3 | 3 | 36 | 🟡 MÉDIO |
| A16 | Força Bruta no Login | CVE / OWASP | 4 | 4 | 3 | 48 | 🟡 MÉDIO |
| A17 | Deserialização Insegura | OWASP A08:2021 | 4 | 3 | 3 | 36 | 🟡 MÉDIO |
| A18 | Enumeração de Usuários na API | OWASP A01:2021 | 3 | 3 | 4 | 36 | 🟡 MÉDIO |
| A19 | Upload de Arquivo Malicioso | OWASP A04:2021 | 4 | 4 | 4 | 64 | 🟠 ALTO |
| A20 | Falha na Gestão de Sessão (sem expiração) | OWASP A07:2021 | 3 | 3 | 3 | 27 | 🟡 MÉDIO |

---

### Justificativas Detalhadas de Cada Ameaça

#### A01 — SQL Injection | OWASP A03:2021 | Score: 5×5×5 = 125 🔴 CRÍTICO
**Como ocorreria:** Um invasor digita comandos SQL no campo de busca de pacientes. O banco de dados obedece e entrega todos os prontuários — ou pior, apaga tudo.

#### A02 — Quebra de Autenticação (Sessão roubada) | OWASP A07:2021 | Score: 5×5×4 = 100 🔴 CRÍTICO
**Como ocorreria:** Ao usar Wi-Fi público, o token de sessão de um médico é capturado. O invasor assume a identidade dele e acessa prontuários como se fosse o próprio médico.

#### A03 — Exposição de Dados Sensíveis (prontuários) | OWASP A02:2021 | Score: 5×5×4 = 100 🔴 CRÍTICO
**Como ocorreria:** Prontuários armazenados sem criptografia. Se o servidor for comprometido, todos os dados clínicos ficam expostos em texto legível.

#### A04 — Controle de Acesso Quebrado (IDOR) | OWASP A01:2021 | Score: 5×4×4 = 80 🟠 ALTO
**Como ocorreria:** Um paciente troca o número do ID na URL do portal e consegue ver o prontuário de outro paciente. Simples assim.

#### A05 — Ransomware no servidor | CVE-2023-20269 | Score: 5×5×5 = 125 🔴 CRÍTICO
**Como ocorreria:** Um funcionário abre um e-mail malicioso. O vírus criptografa todos os arquivos do hospital. Sem backups, não há como atender pacientes.

#### A06 — Ataque DDoS (Negação de Serviço) | OWASP / CVE | Score: 5×4×3 = 60 🟡 MÉDIO
**Como ocorreria:** Milhares de requisições falsas sobrecarregam o servidor. O sistema cai no meio de uma emergência médica — ninguém consegue acessar nada.

#### A07 — Cross-Site Scripting (XSS) | OWASP A03:2021 | Score: 4×4×4 = 64 🟠 ALTO
**Como ocorreria:** Um hacker insere um script malicioso no campo de 'observações do paciente'. Quando outro médico abre o prontuário, o script roda e rouba a sessão dele.

#### A08 — Injeção de Comandos no Servidor (RCE) | CVE-2021-44228 (Log4Shell) | Score: 5×5×4 = 100 🔴 CRÍTICO
**Como ocorreria:** Exploração de bibliotecas desatualizadas do backend. O invasor executa comandos diretamente no servidor — como se tivesse o teclado nas mãos.

#### A09 — Phishing direcionado a funcionários | OWASP / CVE | Score: 4×4×5 = 80 🟠 ALTO
**Como ocorreria:** Um e-mail falso, imitando o RH do hospital, pede que a recepcionista 'atualize sua senha'. Ela digita no site falso e entrega as credenciais ao atacante.

#### A10 — Elevação de Privilégio | OWASP A01:2021 | Score: 4×5×3 = 60 🟡 MÉDIO
**Como ocorreria:** Uma enfermeira descobre que, mudando um parâmetro na requisição HTTP, consegue acessar a aba de administração — área que só o gestor deveria ver.

#### A11 — Falta de Criptografia em Trânsito (HTTP puro) | OWASP A02:2021 | Score: 4×5×3 = 60 🟡 MÉDIO
**Como ocorreria:** O sistema usa HTTP em vez de HTTPS. Qualquer pessoa na mesma rede do hospital consegue interceptar os dados enviados entre o navegador e o servidor.

#### A12 — Credenciais Padrão de Fábrica | CVE-2023-1389 | Score: 5×5×3 = 75 🟠 ALTO
**Como ocorreria:** O banco de dados foi instalado com o usuário padrão 'admin / admin123'. Ninguém trocou. Um scan automático na internet encontra e invade em minutos.

#### A13 — Falha em Componentes de Terceiros (libs desatualizadas) | OWASP A06:2021 | Score: 4×4×4 = 64 🟠 ALTO
**Como ocorreria:** Uma biblioteca de geração de PDF usada no sistema tem uma vulnerabilidade conhecida há 8 meses. Nunca foi atualizada. O invasor usa a falha documentada no CVE.

#### A14 — Falta de Logs e Auditoria | OWASP A09:2021 | Score: 3×4×4 = 48 🟡 MÉDIO
**Como ocorreria:** Sem logs, ninguém sabe que um funcionário demitido ainda acessa o sistema toda noite. A ameaça fica invisível por meses.

#### A15 — Configuração Insegura do Servidor | OWASP A05:2021 | Score: 4×3×3 = 36 🟡 MÉDIO
**Como ocorreria:** O servidor de homologação (testes) está aberto na internet, com dados reais de pacientes. Ninguém percebeu porque 'era só ambiente de testes'.

#### A16 — Força Bruta no Login | CVE / OWASP | Score: 4×4×3 = 48 🟡 MÉDIO
**Como ocorreria:** Um robô testa milhares de combinações de usuário e senha no login do sistema. Sem bloqueio automático, eventualmente ele acerta.

#### A17 — Deserialização Insegura | OWASP A08:2021 | Score: 4×3×3 = 36 🟡 MÉDIO
**Como ocorreria:** Dados enviados pelo cliente são desserializados sem validação. O invasor manipula o objeto enviado para executar código no servidor.

#### A18 — Enumeração de Usuários na API | OWASP A01:2021 | Score: 3×3×4 = 36 🟡 MÉDIO
**Como ocorreria:** A API retorna mensagens diferentes para 'usuário não existe' e 'senha errada'. Um atacante usa isso para descobrir quais CPFs estão cadastrados no sistema.

#### A19 — Upload de Arquivo Malicioso | OWASP A04:2021 | Score: 4×4×4 = 64 🟠 ALTO
**Como ocorreria:** O sistema permite anexar arquivos ao prontuário. Um atacante sobe um arquivo PHP disfarçado de PDF. O servidor executa o arquivo e fica comprometido.

#### A20 — Falha na Gestão de Sessão (sem expiração) | OWASP A07:2021 | Score: 3×3×3 = 27 🟡 MÉDIO
**Como ocorreria:** Um médico esquece o sistema aberto no computador da recepção ao ir embora. A sessão nunca expira. Qualquer pessoa pode usar a conta dele horas depois.

---

## 3.2 — Políticas de Acesso e Identidade (IAM)

### O que é IAM e Zero Trust?

**IAM (Identity and Access Management)** cuida de duas coisas essenciais:

- **Autenticação** — "quem é você?" (como um porteiro que pede seu crachá)
- **Autorização** — "o que você pode fazer aqui?" (o crachá de médico abre salas diferentes do crachá de recepcionista)

O conceito moderno de **Zero Trust** (Confiança Zero) diz que ninguém é confiável por padrão, nem mesmo quem já está dentro da rede. Cada acesso precisa ser verificado.

> No HospitalTech, um vazamento de dados de pacientes pode custar multas de até **R$ 50 milhões** e destruir a reputação do hospital.

---

### As 20 Políticas IAM do HospitalTech

#### 🔵 Políticas de Autenticação

**P01 — Hash de senhas com PBKDF2 + Salt**
Senhas nunca são guardadas em texto puro. Se o banco de dados for roubado, o invasor vê só uma sequência de letras e números sem sentido — não a senha real.

**P02 — Autenticação em Dois Fatores (2FA) obrigatório para médicos e admins**
Mesmo que a senha seja roubada por phishing, o invasor ainda precisaria do celular do médico para entrar. É uma segunda camada de proteção vital em ambiente hospitalar.

**P03 — Bloqueio automático após 3 tentativas de login erradas**
Impede ataques de força bruta, onde robôs tentam milhares de senhas. Depois de 3 erros, a conta trava e avisa o usuário por e-mail.

**P04 — Expiração de sessão após 10 minutos de inatividade**
Médicos frequentemente atendem emergências e deixam computadores abertos. Com expiração automática, o sistema fecha a sessão antes que alguém não autorizado a use.

**P05 — Tokens JWT com validade curta (15 minutos) + Refresh Token**
O token que prova o login tem prazo de validade curto. Se for interceptado numa rede insegura, fica inútil rapidamente. O Refresh Token renova o acesso de forma segura.

**P06 — Comunicação exclusiva via HTTPS (TLS 1.3)**
Todos os dados trafegam criptografados. É como enviar uma carta dentro de um cofre, não num envelope transparente. Ninguém no meio do caminho consegue ler.

**P07 — Validação de e-mail institucional no cadastro de profissionais**
Apenas e-mails do domínio do hospital (`@hospitaltech.com.br`) podem criar contas de profissional. Isso evita que pessoas externas criem contas se passando por funcionários.

**P08 — Política de senha forte: mínimo 12 caracteres, letras, números e símbolos**
Senhas curtas e simples são quebradas em segundos por ferramentas automatizadas. Uma senha de 12 caracteres mistos levaria anos para ser descoberta por força bruta.

**P09 — Registro de log a cada login: IP, data, hora e dispositivo**
Toda vez que alguém entra no sistema, o HospitalTech guarda de onde veio o acesso. Se uma conta for usada da China às 3h da manhã, o sistema detecta a anomalia.

**P10 — Autenticação por certificado digital para integrações externas (API)**
Sistemas externos que se conectam ao HospitalTech precisam apresentar um certificado digital, como um passaporte digital. Sem ele, a conexão é recusada automaticamente.

#### 🟢 Políticas de Autorização

**P11 — RBAC — Controle de Acesso Baseado em Papéis (médico, enfermeiro, admin, paciente)**
Cada perfil só enxerga o que precisa para o seu trabalho. O paciente vê só os próprios exames. O enfermeiro vê prontuários, mas não acessa o módulo financeiro. Simples e eficaz.

**P12 — Princípio do Menor Privilégio (Least Privilege)**
Ninguém tem mais permissão do que o estritamente necessário. Mesmo o diretor do hospital não precisa de acesso ao banco de dados direto — isso reduz o impacto de qualquer conta comprometida.

**P13 — Verificação de permissão em cada requisição (não só no login)**
O sistema não confia só no login. A cada clique, ele verifica se o usuário ainda tem permissão. Impede que alguém que tenha trocado de cargo continue com acesso antigo.

**P14 — Acesso ao prontuário apenas pelo médico responsável pelo paciente**
Um médico ortopedista não consegue acessar prontuários de pacientes de cardiologia sem motivo. Cada prontuário é vinculado ao profissional de referência. Isso protege a privacidade do paciente.

**P15 — Auditoria imutável de todos os acessos a dados sensíveis**
Toda vez que um prontuário é aberto, o sistema registra quem foi, quando foi e o que foi acessado. Esse log não pode ser apagado nem pelo administrador. Prova legal em caso de vazamento.

**P16 — Revogação imediata de acesso ao desligamento de funcionários**
Quando um funcionário é demitido, a conta é bloqueada em até 1 hora. Funcionários desligados são uma das principais causas de vazamentos internos. Velocidade aqui é fundamental.

**P17 — Separação de ambientes: produção, homologação e desenvolvimento**
O banco de dados real (com pacientes reais) nunca fica disponível para o ambiente de testes. Erros de configuração em testes não colocam dados reais em risco.

**P18 — Sanitização e validação de todas as entradas do usuário**
Tudo que o usuário digita é tratado como suspeito antes de ser processado. Isso neutraliza ataques de SQL Injection e XSS diretamente na raiz — sem deixar brechas.

**P19 — Rate Limiting na API: máximo de 100 requisições por minuto por IP**
Se um IP fizer mais de 100 requisições em 60 segundos, o sistema bloqueia temporariamente. Isso impede ataques automatizados de força bruta e DDoS direcionados à API.

**P20 — Mascaramento de dados sensíveis em interfaces e logs**
O número do cartão aparece como `4111 **** **** 1111` em qualquer tela ou log. CPF aparece como `***.***.***-01`. Mesmo que um funcionário veja a tela, não consegue anotar os dados completos.

---

## 3.3 — Adequação à LGPD — RoPA (Registro de Operações de Tratamento)

### O que é o RoPA?

A LGPD exige que toda empresa saiba exatamente:
- quais dados coleta,
- para que usa cada um,
- qual é a justificativa legal para essa coleta, e
- o que o dono dos dados pode pedir.

O **RoPA (Record of Processing Activities)** é esse documento. No HospitalTech, é especialmente crítico porque lidamos com **dados de saúde** — que a LGPD classifica como "dados sensíveis", merecendo proteção redobrada. Multas por não ter o RoPA atualizado chegam a **R$ 50 milhões**.

> 🔴 **Dado Sensível** (saúde) — proteção redobrada exigida pela LGPD
> 🔵 **Dado Pessoal** comum — proteção padrão da LGPD

---

### Os 20 Dados e Processos Mapeados

#### D01 — Nome completo do paciente | 🔵 Pessoal
- **Finalidade:** Identificação do paciente em prontuários, receitas e laudos.
- **Base Legal:** Art. 7.º, II — Obrigação legal (CFM exige identificação nos prontuários).
- **Direitos do Titular:** O paciente pode solicitar correção a qualquer momento pelo portal. A exclusão só é possível após o prazo legal de guarda de prontuários (20 anos, conforme Resolução CFM n.º 1.821/2007).

#### D02 — CPF do paciente | 🔵 Pessoal
- **Finalidade:** Emissão de nota fiscal, identificação única no sistema e obrigações fiscais.
- **Base Legal:** Art. 7.º, II — Obrigação legal (Receita Federal exige o CPF em notas fiscais).
- **Direitos do Titular:** Não pode ser excluído imediatamente. O sistema fará Hard Delete (exclusão definitiva) após 5 anos, respeitando o prazo fiscal.

#### D03 — Data de nascimento | 🔵 Pessoal
- **Finalidade:** Cálculo de idade para dosagem de medicamentos e triagem de risco.
- **Base Legal:** Art. 7.º, II — Obrigação legal (prontuário médico obrigatório).
- **Direitos do Titular:** Pode ser corrigida pelo portal do paciente a qualquer momento, com validação por documento oficial.

#### D04 — Endereço residencial | 🔵 Pessoal
- **Finalidade:** Envio de laudos físicos, agendamento de visita domiciliar e contato de emergência.
- **Base Legal:** Art. 7.º, V — Execução de contrato de prestação de serviços de saúde.
- **Direitos do Titular:** Pode ser atualizado ou excluído pelo portal, exceto durante internações ativas onde é obrigatório para emergências.

#### D05 — Telefone e e-mail de contato | 🔵 Pessoal
- **Finalidade:** Confirmação de consultas, envio de resultados de exames e alertas de saúde.
- **Base Legal:** Art. 7.º, I — Consentimento do titular (obtido no cadastro).
- **Direitos do Titular:** O paciente pode revogar o consentimento pelo portal a qualquer momento, interrompendo o uso desses dados para comunicação.

#### D06 — Diagnósticos e CID | 🔴 Sensível
- **Finalidade:** Registro do histórico clínico, prescrição e acompanhamento do tratamento.
- **Base Legal:** Art. 11, II, b — Tutela da saúde (tratamento indispensável para o atendimento médico).
- **Direitos do Titular:** Não pode ser excluído durante o período de guarda obrigatória (20 anos). Após esse prazo, exclusão mediante solicitação formal ao DPO.

#### D07 — Resultados de exames laboratoriais | 🔴 Sensível
- **Finalidade:** Apoio ao diagnóstico, monitoramento de doenças crônicas e histórico clínico.
- **Base Legal:** Art. 11, II, b — Tutela da saúde.
- **Direitos do Titular:** O paciente pode acessar e baixar todos os resultados pelo portal. A exclusão segue o mesmo prazo do prontuário (20 anos).

#### D08 — Histórico de medicamentos prescritos | 🔴 Sensível
- **Finalidade:** Controle de interações medicamentosas e continuidade do tratamento.
- **Base Legal:** Art. 11, II, b — Tutela da saúde.
- **Direitos do Titular:** Acesso garantido ao paciente pelo portal. Exclusão somente após cumprimento do prazo legal de guarda.

#### D09 — Imagens médicas (raio-X, tomografia, ultrassom) | 🔴 Sensível
- **Finalidade:** Diagnóstico médico, laudos radiológicos e acompanhamento evolutivo.
- **Base Legal:** Art. 11, II, b — Tutela da saúde.
- **Direitos do Titular:** Download disponível no portal em formato DICOM. Exclusão após 20 anos conforme normas do CFM.

#### D10 — Plano de saúde / convênio | 🔵 Pessoal
- **Finalidade:** Faturamento dos procedimentos junto à operadora de saúde e cobrança.
- **Base Legal:** Art. 7.º, V — Execução de contrato de prestação de serviços.
- **Direitos do Titular:** Pode ser atualizado a qualquer momento. Exclusão após quitação completa de débitos e encerramento do contrato com o convênio.

#### D11 — Número do cartão de crédito (PAN) | 🔵 Pessoal
- **Finalidade:** Processamento de pagamentos de consultas e procedimentos particulares.
- **Base Legal:** Art. 7.º, V — Execução de contrato.
- **Direitos do Titular:** Dado mascarado em todas as telas (`4111 **** **** 1111`). Número completo não é armazenado — processado diretamente pela operadora de pagamento (tokenização).

#### D12 — Histórico de pagamentos | 🔵 Pessoal
- **Finalidade:** Comprovante de quitação, relatórios contábeis e auditoria financeira.
- **Base Legal:** Art. 7.º, II — Obrigação legal (legislação fiscal exige guarda por 5 anos).
- **Direitos do Titular:** Acesso pelo portal mediante autenticação. Exclusão após 5 anos conforme prazo fiscal.

#### D13 — IP e logs de acesso ao sistema | 🔵 Pessoal
- **Finalidade:** Auditoria de segurança, detecção de acessos suspeitos e investigação de incidentes.
- **Base Legal:** Art. 7.º, II — Obrigação legal (segurança da informação e rastreabilidade).
- **Direitos do Titular:** Não disponível para alteração pelo paciente — dado de segurança. Mantido por 1 ano para fins de investigação.

#### D14 — CRM e dados do médico responsável | 🔵 Pessoal
- **Finalidade:** Identificação do profissional responsável pelo atendimento em prontuários e receitas.
- **Base Legal:** Art. 7.º, II — Obrigação legal (CFM exige identificação do médico no prontuário).
- **Direitos do Titular:** Médico pode solicitar correção ao RH. Não pode ser excluído enquanto houver prontuários vinculados.

#### D15 — Agenda e horários de consultas | 🔵 Pessoal
- **Finalidade:** Gestão de capacidade dos consultórios, confirmação de presença e relatórios de produtividade.
- **Base Legal:** Art. 7.º, V — Execução de contrato de prestação de serviços.
- **Direitos do Titular:** Paciente pode cancelar agendamentos pelo portal. Registros históricos mantidos por 2 anos para auditoria administrativa.

#### D16 — Dados de internação (data, enfermaria, leito) | 🔵 Pessoal
- **Finalidade:** Gestão hospitalar, faturamento junto ao SUS/planos e controle de ocupação.
- **Base Legal:** Art. 7.º, II — Obrigação legal (notificação compulsória ao SUS).
- **Direitos do Titular:** Acesso pelo portal do paciente. Exclusão após 20 anos junto ao prontuário.

#### D17 — Cookies e dados de navegação no portal | 🔵 Pessoal
- **Finalidade:** Manter sessão autenticada, preferências de interface e análise de usabilidade.
- **Base Legal:** Art. 7.º, I — Consentimento (banner de cookies na primeira visita).
- **Direitos do Titular:** O paciente pode recusar cookies não essenciais no banner. Cookies de sessão são apagados ao fechar o navegador.

#### D18 — Senha (hash) de acesso ao portal | 🔵 Pessoal
- **Finalidade:** Autenticação do paciente e dos profissionais no sistema.
- **Base Legal:** Art. 7.º, II — Obrigação legal (segurança do sistema).
- **Direitos do Titular:** Paciente pode redefinir a senha a qualquer momento pelo portal. Ao excluir a conta, o hash é apagado permanentemente.

#### D19 — Fotos e documentos de identidade enviados | 🔵 Pessoal
- **Finalidade:** Validação do cadastro do paciente e confirmação de identidade em procedimentos.
- **Base Legal:** Art. 7.º, I — Consentimento (obtido durante o cadastro).
- **Direitos do Titular:** Paciente pode solicitar exclusão após confirmação do cadastro. Documentos são apagados em até 30 dias após validação.

#### D20 — Notificações compulsórias de doenças (ex: dengue, COVID) | 🔴 Sensível
- **Finalidade:** Cumprimento da obrigação legal de notificação às autoridades de saúde pública.
- **Base Legal:** Art. 11, II, a — Cumprimento de obrigação legal pelo controlador (Lei Federal n.º 6.259/1975).
- **Direitos do Titular:** Dado compartilhado com órgãos de saúde pública. Paciente é informado sobre o compartilhamento. Não pode ser excluído — obrigação legal.

---

## 3.4 — Análise Crítica: A Vulnerabilidade Mais Perigosa

### SQL Injection | Score GUT: 125 (5 × 5 × 5) 🔴 CRÍTICO

> **Fonte:** OWASP A03:2021 — Injection | **Score máximo possível:** 125 | **Prioridade:** Imediata

---

### Como um invasor exploraria essa falha no HospitalTech?

| Etapa | Descrição |
|---|---|
| **Passo 1 — O invasor encontra o alvo** | O HospitalTech tem um campo de busca de pacientes na tela de internação. O médico digita o nome do paciente e o sistema consulta o banco de dados. Parece simples e inofensivo. |
| **Passo 2 — O ataque começa** | Em vez de digitar um nome, o invasor digita: `' OR '1'='1` — Esse não é um nome. É um comando SQL disfarçado de texto. |
| **Passo 3 — O banco de dados obedece sem questionar** | Se o sistema não tratar a entrada do usuário, o banco de dados interpreta como: "Me dê todos os registros onde o nome for vazio OU onde 1 é igual a 1". Como 1 sempre é igual a 1, o banco retorna todos os pacientes cadastrados de uma vez. |
| **Passo 4 — O estrago pode ser ainda maior** | Com variações mais avançadas, o invasor consegue: extrair senhas de todos os usuários, apagar completamente o banco de dados de prontuários, ou abrir uma porta de entrada remota no servidor. |
| **Passo 5 — O impacto no HospitalTech** | Vazamento de dados de saúde de todos os pacientes, multa de até R$ 50 milhões da ANPD, processos civis dos pacientes prejudicados, e possível paralisação total do sistema hospitalar. |

---

### Como o HospitalTech se protege?

#### ✅ PRIMÁRIA — Elimina o risco na raiz

**Técnica 1 — Queries Parametrizadas (Prepared Statements)**

Em vez de montar a consulta SQL misturando o texto do usuário com o comando, o sistema separa os dois completamente. O banco de dados recebe primeiro a instrução ("busque o paciente pelo nome") e depois o valor ("João Silva") — em separado. Assim, mesmo que o usuário digite `' OR 1=1`, o banco trata isso como um nome de paciente e não encontra nada, sem executar nenhum comando. **O ataque não funciona porque o banco nunca mistura dados com instruções.**

#### 🛡️ COMPLEMENTAR — Detecta e bloqueia antes de chegar ao banco

**Técnica 2 — Sanitização e Validação de Entradas**

Antes de qualquer dado chegar ao banco, o sistema verifica se o conteúdo é válido para aquele campo. Um campo de nome de paciente só aceita letras, espaços e acentos. Se alguém digitar um apóstrofe (`'`) ou ponto e vírgula (`;`) — caracteres típicos de SQL Injection — o sistema recusa imediatamente e registra a tentativa no log de segurança. É uma camada extra que funciona mesmo se houver algum ponto cego nas queries parametrizadas.

---

### Conclusão — Por que isso é suficiente?

A combinação de **queries parametrizadas + sanitização de entradas** cria duas barreiras independentes contra o SQL Injection. Para que o ataque funcione, o invasor precisaria burlar as duas ao mesmo tempo — algo que, com a implementação correta, é virtualmente impossível.

Além disso, toda tentativa de injeção fica registrada no **log de auditoria imutável** do HospitalTech, permitindo identificar a origem do ataque mesmo que ele não tenha tido sucesso.

> Essa abordagem está alinhada com as recomendações do **OWASP**, do **NIST** e com os requisitos da **LGPD** de proteção técnica dos dados pessoais (Art. 46).

---

## Referências

- OWASP FOUNDATION. *OWASP Top Ten 2021*. Disponível em: https://owasp.org/Top10. Acesso em: mar. 2026.
- MITRE CORPORATION. *Common Vulnerabilities and Exposures (CVE)*. Disponível em: https://cve.mitre.org. Acesso em: mar. 2026.
- BRASIL. *Lei n.º 13.709, de 14 de agosto de 2018. Lei Geral de Proteção de Dados Pessoais (LGPD)*. Brasília, DF: Presidência da República, 2018.
- NIST. *SP 800-63B: Digital Identity Guidelines — Authentication and Lifecycle Management*. NIST, 2020.
- CONSELHO FEDERAL DE MEDICINA. *Resolução CFM n.º 1.821/2007 — Prontuário Eletrônico do Paciente*. Brasília: CFM, 2007.
- MICROSOFT. *STRIDE Threat Modeling*. Microsoft Security Documentation, 2024.
- PAYMENT CARD INDUSTRY SECURITY STANDARDS COUNCIL. *PCI DSS v4.0*. PCI SSC, 2022.
- BRASIL. *Lei n.º 6.259, de 30 de outubro de 1975. Notificação Compulsória de Doenças*. Brasília: Presidência da República, 1975.

---

*Documento Confidencial — Elaborado pela Equipe HospitalTech — Versão 1.0 — Março de 2026*
