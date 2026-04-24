# Auditoria de QA - Projeto NaSalinha (Matheus Marques)
Auditoria realizada por: Matheus Fellipe Araujo Marques
Curso: Ciência da Computação - UFLA

Este repositório contém a documentação da auditoria de qualidade realizada durante o Desafio 2026/1.

# Progresso - Semana 2
- [x] Configuração do ambiente local (Docker)
- [x] Integração com Mailtrap para testes de e-mail
- [x] Integração com Cloudinary para armazenamento de mídia
- [x] Setup do banco de dados (Prisma/PostgreSQL)

# Tecnologias
- Docker
- Mailtrap
- Cloudinary
- Insomnia
- Vs Code

#  Evidências de Setup
![Sistema Funcionando](Evidencias/image.png)

---

## 🎯 Planejamento de Testes (Semana 3)

Para a próxima etapa, mapeei os seguintes cenários de teste para garantir a integridade das funções principais do sistema:

### 🔑 Autenticação e Cadastro
| Cenário | Método | Endpoint | Resultado Esperado |
| :--- | :--- | :--- | :--- |
| Login com credenciais válidas | POST | `/api/auth/login` | Status 200 e recebimento do Token JWT |
| Login com senha incorreta | POST | `/api/auth/login` | Status 401 e mensagem de erro amigável |
| Cadastro de e-mail já existente | POST | `/api/auth/register` | Status 400 e bloqueio de duplicidade |

### 📩 Recuperação de Senha (Esqueci a Senha)
| Cenário | Método | Endpoint | Resultado Esperado |
| :--- | :--- | :--- | :--- |
| Solicitar para e-mail cadastrado | POST | `/api/auth/forgot-password` | Recebimento do link no Mailtrap |
| Solicitar para e-mail inexistente | POST | `/api/auth/forgot-password` | Erro informando que usuário não existe |

---

## 🐞 Gestão de Falhas (Bug Reports)

Utilizo a aba **[Issues](https://github.com/Straffey/QA-Portfolio-NaSalinha/issues)** deste repositório para documentar falhas técnicas encontradas durante a auditoria. Atualmente, o seguinte bug está mapeado:

* **ID #1:** Erro 401 (Unauthorized) na integração com Cloudinary.

## 🚀 Semana 3: Auditoria Técnica
Nesta etapa, realizei a validação do ambiente de backend do projeto NaSalinha.

Ações realizadas:

Setup: Clonagem do repositório base e configuração de variáveis de ambiente (.env).

Containerização: Inicialização dos serviços de Banco de Dados (PostgreSQL) e API via Docker Compose.

Persistência: Sincronização do schema do banco de dados utilizando Prisma ORM (npx prisma migrate dev).

Validação de API: Testes de integração das rotas de autenticação via Insomnia, validando a geração de JWT (JSON Web Token).

## ⏩ Próximos Passos (Semana 4 & 5)
Com o ambiente estável e autenticação funcional, o foco agora será:

Validação de Fluxos Complexos: Testar o ciclo de vida dos Check-ins e Seasons.

Automação: Início da escrita de scripts para automação de testes de regressão.