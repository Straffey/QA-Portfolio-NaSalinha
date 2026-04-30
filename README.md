# 🚀 QA Portfolio — Projeto NaSalinha

**Matheus Fellipe Araujo Marques** Ciência da Computação — UFLA  
Desafio Trainee Comp Júnior 2026/1

---

## 📅 Cronograma do Desafio

### 🏗️ Semana 2: Setup e Infraestrutura

Configuração do ambiente e análise técnica do sistema para garantir a base do desenvolvimento.

**Ações Realizadas:**
- [x] Ambiente local isolado e rodando via **Docker Compose**.
- [x] Estruturação do repositório com Backend e Frontend integrados.
- [x] **Bug Crítico Identificado:** Necessidade de mapeamento da porta 5001 no container de backend para comunicação externa.

**Evidência de Ambiente:** ![Ambiente Rodando](./Evidencias/image.png)

---

### 📋 Semana 3: Planejamento e Execução de Testes

Definição de estratégia e cenários detalhados para as funcionalidades core, priorizando segurança e integridade.

#### 🔑 Área Core 1: Autenticação JWT
Validação do fluxo de segurança via tokens para proteção de rotas.

| ID | Caso de Teste | Tipo | Resultado Esperado | Status |
| :--- | :--- | :--- | :--- | :--- |
| **CT-AUTH-01** | Login com credenciais válidas | API | Status 200 OK e Token JWT | ✅ Passou |
| **CT-AUTH-02** | Login com senha incorreta | API | Status 401 Unauthorized | ✅ Passou |

**Validação de API (Token Gerado):** ![Token Gerado](./Evidencias/01-login-sucesso-token.png)

---

#### 📸 Área Core 2: Check-in por Foto
- **CT-CHECK-01:** Envio de imagem válida para validação de presença (Status 201).
- **CT-CHECK-02:** Tentativa de check-in sem anexo para validar regras de obrigatoriedade.

#### 🏆 Área Core 3: Sistema de Pontos
- **CT-PONTOS-01:** Verificação do endpoint `/api/rankings` após novo check-in.
- **CT-PONTOS-02:** Teste de integridade de dados após deleção de registros.

---

## 🛠️ Tecnologias e Infraestrutura

A arquitetura foi desenhada para ser resiliente e de fácil replicação:

- **Docker & Docker Compose:** Orquestração de containers para PostgreSQL e Node.js.
- **Prisma ORM:** Abstração da camada de dados e controle de migrations.
- **Insomnia:** Suite de testes para validação de endpoints e payloads.
- **JWT (JSON Web Token):** Padrão de segurança para autenticação stateless.

---

## ✅ Relatório Técnico de Testes (API)

Cenários de erro e casos de borda testados para garantir a robustez do backend.

| Caso de Teste | Endpoint | Status Esperado | Resultado | Descrição Técnica |
| :--- | :--- | :--- | :--- | :--- |
| **Registro de Usuário** | `/auth/register` | 201 Created | ✅ Sucesso | Persistência correta no PostgreSQL. |
| **Erro de Duplicidade** | `/auth/register` | 409 Conflict | ✅ Sucesso | Bloqueio de e-mails duplicados (Unique Constraint). |
| **Validação de Campos** | `/auth/register` | 400 Bad Request | ✅ Sucesso | Tratamento de payloads incompletos ou malformados. |
| **Segurança (SQLi)** | `/auth/login` | 401/400 | ✅ Sucesso | Proteção contra ataques de SQL Injection no login. |
| **Sanitização** | `/auth/register` | 201 Created | ✅ Sucesso | Limpeza de espaços em branco (trim) nos inputs. |

---

## 🧪 Como Replicar os Testes

Para auditar os resultados, utilize a coleção oficial do Insomnia:

1. **Subir Ambiente:** Execute `docker-compose up -d`.
2. **Importar Collection:** No Insomnia, importe o arquivo: [`./tests/Insomnia_2026-04-25.yaml`](./tests/Insomnia_2026-04-25.yaml).
3. **Execução:** As requisições apontam para `http://localhost:5001`. Para rotas protegidas, atualize o *Bearer Token* com o código gerado no login.

---

## 🐞 Gestão de Falhas

Rastreamento de melhorias e correções:
* **[Visualizar Issues no GitHub](https://github.com/Straffey/QA-Portfolio-NaSalinha/issues)**

---

# 🚀 Relatório de Auditoria e Qualidade - Semana 4

Este repositório contém a auditoria técnica e os testes de QA realizados no sistema **NaSalinha**. O foco desta sprint foi a estabilização do ambiente Docker, integração com serviços externos e validação das regras de negócio.

---

## 🛠️ Status do Ambiente
O ambiente foi totalmente provisionado e validado via Docker:
* **Backend**: Node.js/Prisma operando em containers isolados.
* **Database**: PostgreSQL com integridade referencial validada (testes de exclusão em cascata bem-sucedidos).
* **Storage**: Integração com Cloudinary ativa para persistência de mídia de check-ins.

---

## 🔍 Backlog de Bugs Encontrados (Issues)
Durante a auditoria, foram identificadas falhas críticas que comprometem a segurança e a gamificação do sistema:

| ID | Descrição do Bug | Severidade | Status |
| :--- | :--- | :--- | :--- |
| **#01** | Cadastro e login permitidos sem verificação de e-mail (Violação RF-01/RF-02) | Crítica | Aberto |
| **#27** | Permissão de Check-in em Temporada Encerrada | Alta | Aberto |
| **#28** | Check-ins ignoram moderação (Aprovação automática no Banco) | Crítica | Aberto |
| **#29** | Falha na restrição de frequência (Múltiplos check-ins permitidos por dia) | Crítica | Aberto |
| **#30** | Ranking Geral não atualiza após exclusão manual de foto pelo Admin | Alta | Aberto |

---

## ✅ Testes de Sucesso (Sanidade e Usabilidade)
Apesar dos bugs de regra de negócio, os seguintes fluxos técnicos foram validados com sucesso:
* **Integridade de Dados**: A exclusão de um usuário remove corretamente todos os seus pontos e registros do ranking (sem dados órfãos).
* **Validação de Client-side**: O sistema bloqueia corretamente o upload de arquivos que não sejam imagens (ex: PDFs).
* **Responsividade**: Interface validada para dispositivos móveis via DevTools, garantindo usabilidade para estudantes em campo.

---

## 📁 Evidências de Teste
Para manter o repositório focado exclusivamente no código e infraestrutura, todas as evidências visuais (capturas de tela e logs) foram vinculadas diretamente às suas respectivas **Issues**.

* **Acesso**: Navegue até a aba [Issues](https://github.com/Straffey/QA-Portfolio-NaSalinha/issues) para visualizar os registros detalhados com seus devidos anexos.

---

## 🚀 Próximos Passos (Sugestão para Sprint 5)
1. **Correção Prioritária (#28)**: Implementar o status `PENDING` como padrão para novos check-ins.
2. **Middleware de Data**: Validar no Backend se a temporada está ativa antes de aceitar o upload.

## ⏩ Próximos Passos

- [ ] Automação de scripts de teste (Semana 4 e 5).
- [ ] Integração completa do fluxo de check-in com o Frontend.
- [ ] Relatório final de cobertura de testes (Semana 6).