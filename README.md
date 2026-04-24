# 🚀 QA Audit Portfolio — Projeto NaSalinha

**Auditor:** Matheus Fellipe Araujo Marques
**Instituição:** Ciência da Computação — UFLA
**Desafio:** Trainee Comp Júnior 2026/1

---

## 📅 Cronograma de Auditoria

### 🏗️ Semana 2: Setup, Exploração e Documentação
Nesta etapa, o foco foi a configuração da infraestrutura e o entendimento das regras de negócio do sistema.

**Ações Realizadas:**
- [x] Subida do ambiente local usando **Docker Compose**.
- [x] Criação do repositório no GitHub e estruturação das pastas.
- [x] Navegação pelo sistema para entender o fluxo do usuário (Cadastro, Login e Check-in).
- [x] Configuração de variáveis de ambiente (`.env`) e integração com **Mailtrap** e **Cloudinary**.
- [x] Redação da documentação inicial e estratégia de testes.

**Evidência de Infraestrutura:**
![Ambiente Rodando](./Evidencias/image.png)
> *Status: Backend e Banco de Dados PostgreSQL operacionais.*

---

### 📋 Semana 3: Planejamento e Design de Testes
Criação dos cenários detalhados para as 3 áreas core do sistema, definindo o que testar e os resultados esperados.

#### 🔑 Área Core 1: Autenticação JWT
| ID | Caso de Teste | Tipo | Resultado Esperado |
| :--- | :--- | :--- | :--- |
| **CT-AUTH-01** | Login com credenciais válidas | API (Positivo) | Status 200 OK e recebimento do Token JWT |
| **CT-AUTH-02** | Login com senha incorreta | API (Negativo) | Status 401 Unauthorized e mensagem amigável |
| **CT-AUTH-03** | Validação de Regressão: Acesso após correção | Regressão | Garantir que o token liberado acessa rotas privadas |

**Evidência de Validação (Token JWT):**
![Token Gerado](Evidencias/01-login-sucesso-token.png)

#### 📸 Área Core 2: Check-in por Foto
* **CT-CHECK-01 (API - Positivo):** Enviar imagem `.png` válida e validar status `201 Created` no backend.
* **CT-CHECK-02 (Funcional - Negativo):** Tentar enviar check-in sem arquivo e validar bloqueio do sistema.

**Evidência de Auditoria de API:**
![Exportação Insomnia](./Evidencias/image_6ac798.png)

#### 🏆 Área Core 3: Sistema de Pontos
* **CT-PONTOS-01 (API - Positivo):** Validar se o saldo no endpoint `/api/rankings` reflete o check-in aprovado.
* **CT-PONTOS-02 (Regressão):** Validar integridade dos pontos após simulação de deleção de check-in.

**Evidência de Persistência no Banco:**
![Dados no Prisma](./Evidencias/image_6ac375.png)

---

## 🛠️ Ferramentas de Gestão
Utilizo o **GitHub Projects** para gerenciar o ciclo de vida dos testes (Ready, In Test, Done) e a aba **[Issues](https://github.com/Straffey/QA-Portfolio-NaSalinha/issues)** para documentação detalhada de falhas.

* **Bug Report #1:** Inconsistência de porta mapeada (Docker: 5001).
* **Bug Report #2:** Erro de autorização em integração de mídia.

---

## ⏩ Próximos Passos
* Execução completa dos casos de teste (Semana 4 e 5).
* Testes de regressão e re-teste de bugs corrigidos (Semana 6).
* Gravação do vídeo final de demonstração (Semana 7).