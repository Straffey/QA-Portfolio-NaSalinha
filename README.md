# 🚀 QA Portfolio — Projeto NaSalinha

### **Matheus Fellipe Araujo Marques**
*Ciência da Computação — Universidade Federal de Lavras (UFLA)*
*Processo de Avaliação Trainee — Comp Júnior 2026/1*

---

## 📌 Sobre o Projeto

O **NaSalinha** é um sistema de gamificação e controle de presença acadêmica baseado em check-ins dinâmicos. Este repositório concentra todo o ciclo de engenharia de qualidade (QA), planejamento de testes, auditoria de segurança de endpoints, debugging de persistência de dados e homologação de regressão para garantir a resiliência e estabilidade da plataforma.

---

## 🛠️ Tecnologias e Infraestrutura Utilizadas

A arquitetura de testes e desenvolvimento foi desenhada para operar de forma isolada, resiliente e facilmente replicável:

* **Docker & Docker Compose**: Orquestração de microsserviços em containers independentes.
* **PostgreSQL 13**: Banco de dados relacional para persistência transacional.
* **Prisma ORM & Prisma Studio**: Abstração da camada de dados, gerenciamento de migrations, data seeding e auditoria visual de registros.
* **Insomnia**: Suite corporativa de testes para validação funcional, automatização de payloads de endpoints e monitoramento de contratos HTTP.
* **JSON Web Tokens (JWT)**: Mecanismo stateless e baseado em claims para autenticação e controle de acesso baseado em regras (RBAC).

## 📅 Cronograma de Execução e Desenvolvimento (Semanas 2 a 6)

### 🏗️ Semana 2: Setup, Infraestrutura e Mapeamento de Portas
Configuração e isolamento do ecossistema local para garantir a integridade e paridade dos ambientes de teste.

* **Ações Realizadas**:
    * Provisionamento do ambiente completo via Docker Compose contendo microsserviços de Frontend, Backend e Banco de Dados.
    * **Resolução de Conectividade (Bug Técnico Fixado)**: Ajuste fino e liberação do mapeamento de portas locais no container do backend (`5001:5001`) e do banco de dados (`5433:5432`) para viabilizar requisições externas originadas de ferramentas de client-side.

---

### 📋 Semana 3: Planejamento Estratégico e Casos de Teste Iniciais
Mapeamento e execução inicial dos cenários de teste das regras de negócio essenciais do núcleo da aplicação.

#### 🔑 Core Area 1: Autenticação Restrita (JWT)
Validação das travas de borda contra payloads maliciosos e garantia de confidencialidade de tokens.

| ID | Caso de Teste | Tipo | Entrada Simulada | Resultado Esperado | Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **CT-AUTH-01** | Login com credenciais válidas | API | Usuário e senha corretos | HTTP 200 OK + `accessToken` gerado | ✅ Passou |
| **CT-AUTH-02** | Login com credenciais inválidas | API | Senha incorreta / E-mail nulo | HTTP 401 Unauthorized / 400 Bad | ✅ Passou |
| **CT-AUTH-03** | Blindagem contra Injeção Lógica | API | Payload com SQLi (`' OR '1'='1`) | HTTP 401/400 Tratado sem vazamento | ✅ Passou |

#### 📸 Core Area 2: Check-in por Evidência Visual
* **CT-CHECK-01**: Envio de imagem válida via multipart/form-data para confirmação de presença (HTTP 201 Created).
* **CT-CHECK-02**: Sanitização de arquivos. Tentativa de upload de extensões proibidas (ex: PDFs ou executáveis) barrada no client-side e tratada no backend.

#### 🏆 Core Area 3: Mecânica de Rankings e Pontuações
* **CT-PONTOS-01**: Validação de concorrência. Monitoramento do endpoint `/api/rankings` para checar o recálculo imediato de posições após um novo check-in.

---

### 🔍 Semana 4: Auditoria Técnica e Abertura de Backlog (Issues)
Fase dedicada à quebra assistida do sistema, gerando relatórios de falhas com base nas regras de negócio e estabilização de integrações externas (como o bucket Cloudinary).

#### 🗃️ Painel de Bugs Documentados (Issues Oficiais)

| ID da Issue | Descrição Técnica da Falha | Camada Afetada | Severidade | Status |
| :---: | :--- | :---: | :---: | :---: |
| **#27** | Permissão de Check-in em Temporada com vigência Encerrada | Backend | Alta | Aberto |
| **#28** | Check-ins ignoram estágio de moderação (Auto-approval automático) | Backend | Alta | Aberto |
| **#29** | Falha na restrição de frequência (Permite múltiplos check-ins diários) | Backend | Alta | Aberto |
| **#30** | Ranking Geral não atualiza após a exclusão manual de uma foto pelo Admin | Banco de Dados | Alta | Aberto |
| **#31 / #38**| Autenticação e sessão permitidas sem verificação prévia de e-mail | Backend | Alta | Aberto |

### 🛠️ Semana 5: Debugging Avançado, Data Seeding e Prisma Integration
Fase de superação de bloqueios de dados. Investigação profunda nas tabelas do PostgreSQL e injeção controlada de privilégios para testes avançados.

* **Ações Realizadas**:
    * **Auditoria de Tabelas**: Conexão bem-sucedida do Prisma Studio via string estruturada (`postgresql://nasalinha:nasalinha123@localhost:5433/nasalinha_db?schema=public`) para verificar a persistência real dos dados.
    * **Database Seeding**: Execução assistida do comando `npx prisma db seed` para rodar o script interno `seed.js`, injetando registros mocks e permissões estruturadas diretamente no container.
    * **Resolução de Autenticação Root**: Decodificação da lógica de hashing de senhas contida no script do backend. Descoberta da credencial original (`senha123`), permitindo o login bem-sucedido e a extração do token Bearer de privilégio `ADMIN` através do Insomnia.

---

### 🔄 Semana 6: Roteiro Geral de Testes de Regressão e Re-teste
Garantia de imunidade a efeitos colaterais. Elaboração do plano de teste de regressão para os 10 bugs reportados, visando certificar que as correções aplicadas pela engenharia não quebrem funcionalidades antigas.



#### 🧪 Roteiros de Teste de Regressão Criados

1. **Validação de Vigência (Issue #27)**:
   * *Re-teste*: Verificar se requisições para temporadas desativadas retornam erro de validação.
   * *Regressão*: Realizar um check-in em uma temporada aberta comum. O sistema deve continuar aceitando dados normalmente nas datas vigentes.

2. **Ciclo de Moderação (Issue #28)**:
   * *Re-teste*: Novos check-ins de usuários devem cair no banco com a flag `status: "PENDING"`.
   * *Regressão*: Certificar que o painel visual do aluno consiga renderizar a string "Pendente" sem gerar quebras na interface gráfica.

3. **Trava de Frequência Diária (Issue #29)**:
   * *Re-teste*: O segundo check-in na mesma sala no mesmo dia civil deve retornar HTTP 429 ou 400.
   * *Regressão*: Simular a virada de data no banco e realizar um novo check-in. O sistema deve liberar o acesso, confirmando que a trava expira corretamente a cada 24 horas.

4. **Autenticação e Registro Seguro (Issues #31 / #38)**:
   * *Re-teste*: Tentar fazer login em contas recém-criadas sem verificação. A API deve rejeitar o acesso.
   * *Regressão*: Logar com usuários consolidados e verificados anteriormente. Os tokens JWT válidos devem continuar sendo emitidos normalmente.

5. **Integridade Referencial de Mídias (Issue #34)**:
   * *Re-teste*: Deletar uma foto de check-in deve recalcular ou invalidar o saldo de pontos associado.
   * *Regressão*: Executar upload e deleção de mídias em check-ins pendentes. O fluxo de pontos do usuário não deve ficar negativo ou corrompido na fase pendente.

6. **Sincronização de Camadas de Saldo (Issue #35)**:
   * *Re-teste*: Penalidades manuais dadas pelo painel administrativo devem diminuir o valor em tempo real na tabela de usuários via Prisma Studio.
   * *Regressão*: Disparar rotinas paralelas de adição de pontos por check-ins válidos para comprovar que correções no decremento não causaram concorrência de escrita.

7. **Isolamento de privilégios de Endpoint (Issue #36)**:
   * *Re-teste*: Endpoint `PATCH /api/checkins/:id` deve processar alterações de status quando acionado por um administrador.
   * *Regressão*: Tentar bater na mesma rota com a role de um usuário comum (`MEMBER`). O middleware de segurança deve retornar HTTP `403 Forbidden`, provando que o endpoint não foi exposto ao público por engano.

8. **Controle Estrito de Acesso - RBAC (Issue #37)**:
   * *Re-teste*: Endpoints de gestão de temporadas (`GET/POST /api/seasons`) devem barrar perfis de nível `TRAINEE`.
   * *Regressão*: Enviar as mesmas requisições usando as credenciais do usuário administrador de testes. O acesso deve retornar HTTP 200/201 sem restrições.

9. **Filtro Visual de Unicidade (Issue #39)**:
   * *Re-teste*: Forçar duas temporadas ativas no banco. O Frontend deve filtrar e destacar apenas uma como operacional.
   * *Regressão*: Entrar na aba de histórico de temporadas passadas ou encerradas. A correção do filtro visual de temporadas ativas não deve ocultar nem misturar os registros das temporadas já finalizadas.

10. **Sanitização Extrema de Inputs Categóricos (Issue #40)**:
    * *Re-teste*: Tentar salvar uma temporada com o ano extrapolado (ex: `999999-01-01`). O backend deve retornar HTTP `400 Bad Request`.
    * *Regressão*: Criar uma temporada com uma data ISO padrão válida dentro do ano letivo vigente. A validação do schema não deve ser restritiva a ponto de impedir o uso normal do sistema.

    ## 📊 4. Relatório de Testes (Sumário Final Consolidador)

Ao encerrar o ciclo completo de auditoria e automação na suíte do Insomnia, a cobertura de qualidade do ecossistema **NaSalinha** gerou a seguinte matriz de entrega estável:

| Métrica | Valor |
| :--- | :--- |
| **Testes Totais Executados** | 30 |
| **Passou (Test Done)** | 17 |
| **Falhou (Bugs Encontrados)** | 13 |
| **Taxa de Defeitos (Defect Density)**| 43.3% |
| **Status Final da Sprint** | 🔴 **Reprovado (Retido para Correções)** |

> ⚠️ **Parecer Técnico do QA**:(Alguns dos Testes estao reportados dentro de outros ex: testes de login, considerei todos como um [1]) O sistema foi formalmente *Reprovado nesta Sprint* devido à alta densidade de bugs críticos encontrados em fluxos vitais. Embora 17 cenários fundamentais (como validações sintáticas de cadastro e barramento de mídias ausentes) tenham passado com sucesso, os 12 bugs mapeados expõem falhas severas de segurança e integridade (vazamento de tokens no registro, estouro de exceções 500 e quebra do fluxo de moderação da diretoria). O deploy em produção está vetado até a mitigação do backlog.

   ## 🧪 Como Replicar os Testes Locais

Para auditar e reproduzir as validações mapeadas neste portfólio, execute o passo a passo a seguir:

1. Visitar os Containers: Na raiz do projeto, suba os serviços em segundo plano:
   docker-compose up -d

2. Popular a Base de Dados: Execute o reset e a inserção das sementes de administração:
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
   npx prisma db seed

3. Executar o Prisma Studio (Opcional): Caso queira auditar as atualizações das tabelas visualmente:
   npx prisma studio --url="postgresql://nasalinha:nasalinha123@localhost:5433/nasalinha_db?schema=public"

4. Importar Massa de Testes no Insomnia: Importe a collection oficial localizada na pasta de testes.

5. Configurar Credenciais: Utilize o e-mail admin@nasalinha.com e a senha senha123 no endpoint POST /api/auth/login para capturar e atualizar seu token global de autorização.
'@ | Out-File -FilePath .\README.md -Append -Encoding utf8
   

