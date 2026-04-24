# 🚀 QA Audit Portfolio — Projeto NaSalinha

 Matheus Fellipe Araujo Marques
 Ciência da Computação — UFLA
 Trainee Comp Júnior 2026/1

---

## 📅 Cronograma de Auditoria

### 🏗️ Semana 2: Setup e Exploração
- [x] Configuração do ambiente local com **Docker Compose**.
- [x] Identificação de portas e infraestrutura. 
- [x] **Bug Encontrado:** Conflito de porta (necessário rodar em 5001).

**Evidência de Ambiente:**
![Ambiente Rodando](./Evidencias/image.png)

---

### 📋 Semana 3: Planejamento de Testes
Design dos casos de teste para as 3 áreas obrigatórias (Core).

#### 🔑 Área Core 1: Autenticação JWT
| ID | Cenário | Resultado Esperado |
| :--- | :--- | :--- |
| **CT-AUTH-01** | Login com credenciais válidas | Status 200 OK e recebimento do Token |
| **CT-AUTH-02** | Login com senha incorreta | Status 401 Unauthorized |

**Validação de API (Login Sucesso):**
![Token JWT Gerado](./Evidencias/01-login-sucesso-token.png)

#### 📸 Área Core 2: Check-in por Foto
* **CT-CHECK-01:** Envio de imagem válida e validação de status 201 no backend.
* **CT-CHECK-02:** Teste de bloqueio ao tentar enviar formulário sem imagem.

#### 🏆 Área Core 3: Sistema de Pontos
* **CT-PONTOS-01:** Validação da atualização do ranking após aprovação do check-in.
* **CT-PONTOS-02:** Teste de regressão para garantir integridade dos cálculos.

---

## 🐞 Gestão de Falhas
Documentação de bugs críticos na aba **[Issues](https://github.com/Straffey/QA-Portfolio-NaSalinha/issues)**.