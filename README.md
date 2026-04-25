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

## ⏩ Próximos Passos

- [ ] Automação de scripts de teste (Semana 4 e 5).
- [ ] Integração completa do fluxo de check-in com o Frontend.
- [ ] Relatório final de cobertura de testes (Semana 6).