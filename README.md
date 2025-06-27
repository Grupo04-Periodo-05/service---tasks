# Microserviço de Tarefas

Este é um microserviço para gerenciamento de tarefas, desenvolvido em Node.js e TypeScript. Ele oferece uma API RESTful para criar, atualizar, excluir e consultar tarefas.

## Funcionalidades

* **Criação de Tarefas:** Permite a criação de novas tarefas.
* **Atualização de Tarefas:** Permite a atualização de tarefas existentes.
* **Exclusão de Tarefas:** Permite a exclusão de tarefas.
* **Consulta de Tarefas:**
    * Buscar tarefas por grupo.
    * Buscar tarefas por usuário.
    * Buscar tarefas por data de vencimento.

## Tecnologias Utilizadas

* **Node.js:** Ambiente de execução JavaScript.
* **TypeScript:** Superset do JavaScript que adiciona tipagem estática.
* **Express:** Framework web para Node.js.
* **Prisma:** ORM para Node.js e TypeScript.
* **Axios:** Cliente HTTP para realizar requisições.
* **Swagger UI Express:** Para documentação da API.
* **Docker:** Para containerização da aplicação.
  
## Arquitetura

O projeto adota uma abordagem de Arquitetura Limpa, dividindo as responsabilidades em quatro camadas principais:

* `src/domain`: Contém as entidades de negócio (`Task`, `User`) e regras de negócio puras, sem dependências externas.
* `src/application`: Orquestra os fluxos de trabalho (casos de uso), como `CreateTaskUseCase` ou `SendTaskRemindersUseCase`. Define as interfaces dos repositórios.
* `src/infrastructure`: Implementa as interfaces da camada de aplicação. É aqui que se encontram os detalhes de infraestrutura, como o cliente Prisma (`PrismaTaskRepository`), os gateways para outros serviços (`UserGateway`, `NotificationGateway`) e a configuração do banco de dados.
* `src/presentation`: A camada mais externa, responsável por expor a aplicação ao mundo. Inclui os controladores Express (`TaskController`) e a definição das rotas da API (`task.routes.ts`).

## Modelo de Dados (Schema do Banco)

O banco de dados é gerenciado pelo Prisma. A principal tabela é a `Task`, cuja estrutura pode ser inferida a partir das migrações:

* `id`: `TEXT` (Chave Primária, UUID)
* `title`: `TEXT`
* `description`: `TEXT`
* `status`: `TEXT` (e.g., "PENDING", "IN_PROGRESS", "DONE")
* `userId`: `TEXT`
* `groupId`: `TEXT`
* `createdAt`: `TIMESTAMP(3)`
* `updatedAt`: `TIMESTAMP(3)`
* `dueDate`: `TIMESTAMP(3)` (Data de vencimento da tarefa)
* `lastReminderSentAt`: `TIMESTAMP(3)` (Registra o último envio de lembrete)

## Configuração

A aplicação é configurada através de variáveis de ambiente. Crie um arquivo `.env` na raiz do projeto com base no arquivo `src/config/index.ts`:

```env
# Configurações do Servidor
PORT=3001

# URLs dos Gateways (outros microserviços)
USER_SERVICE_URL=http://localhost:3000/api/users
NOTIFICATION_SERVICE_URL=http://localhost:3004/api/notifications

# String de Conexão do Banco de Dados (Prisma)
DATABASE_URL="postgresql://user:password@localhost:5432/mydatabase?schema=public"
