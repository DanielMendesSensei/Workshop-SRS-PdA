# Template - Software Requirements Specification (SRS)

> Template com base no Workshop:
> SRS na Prática com Sensei! 🔎

---

## 1. Introdução

### 1.1 Propósito do Documento

Descreva brevemente o objetivo deste SRS e seu público-alvo.

### 1.2 Escopo do Projeto

* Nome do sistema: TaskMaster (ou seu projeto)
* Visão geral: sistema de gerenciamento colaborativo de tarefas via API RESTful
* Principais funcionalidades: autenticação, gestão de projetos, tarefas, filtros, etc.

### 1.3 Definições, Acrônimos e Abreviações

| Termo | Definição                         |
| ----- | --------------------------------- |
| API   | Application Programming Interface |
| JWT   | JSON Web Token                    |
| …     | …                                 |

## 2. Descrição Geral

### 2.1 Visão Geral do Produto

Contextualize o sistema, principais benefícios e diferenciais.

### 2.2 Ambiente Operacional

* Node.js ≥ 18
* MongoDB / PostgreSQL
* Docker / Kubernetes (opcional)

### 2.3 Restrições, Suposições e Dependências

* Comunicação via HTTPS
* Front-end hospedado em domínio X
* Dependência de serviço de email para notificações (v2)

## 3. Requisitos Funcionais

Para cada requisito, usar o formato de User Story + Critérios de Aceitação.

| ID    | User Story                                                                    | Critérios de Aceitação                                                                        |
| ----- | ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| RF-01 | *Como usuário, quero me cadastrar com email e senha, para acessar o sistema.* | 1. Email é único e válido<br>2. Senha com ≥ 8 caracteres<br>3. Retorna código 201 e token JWT |
| RF-02 | *Como usuário, quero fazer login, para obter meu token JWT.*                  | 1. Valida credenciais<br>2. Retorna 200 e { token }                                           |
| RF-03 | *Como líder de equipe, quero criar projetos, para organizar minhas tarefas.*  | 1. Projeto tem nome e descrição obrigatórios<br>2. Retorna 201 com objeto projeto             |
| …     | …                                                                             | …                                                                                             |

## 4. Requisitos Não-Funcionais

| Categoria       | Descrição / Métrica                                          |
| --------------- | ------------------------------------------------------------ |
| Desempenho      | Tempo de resposta < 200 ms (pico 500 simultâneos)            |
| Disponibilidade | SLA: 99,9 % uptime                                           |
| Segurança       | Senha com bcrypt (cost 12), HTTPS, CORS restrito             |
| Escalabilidade  | Arquitetura sem estado, deploy em cluster PM2                |
| Monitoramento   | Logs JSON no Winston + Elastic Stack; métricas no Prometheus |

## 5. Modelo de Dados

### 5.1 Diagramas: Conceitual e ER

*Inserir diagrama conceitual e diagrama ER aqui (draw\.io, link ou imagem).*

### 5.2 Dicionário de Dados

| Entidade | Atributo | Tipo   | Descrição                          |
| -------- | -------- | ------ | ---------------------------------- |
| Usuário  | id       | UUID   | Identificador único do usuário     |
| Usuário  | email    | string | Email de login                     |
| Projeto  | nome     | string | Nome do projeto                    |
| Tarefa   | status   | enum   | pendente, em\_andamento, concluída |
| …        | …        | …      | …                                  |

## 6. Especificação de API

Descreva cada endpoint em tabela ou formato OpenAPI.

### 6.1 Exemplo de Endpoint

| Método | Rota                 | Request Body                               | Response                           | Códigos       |
| ------ | -------------------- | ------------------------------------------ | ---------------------------------- | ------------- |
| POST   | `/api/auth/register` | `{ "email": "string", "senha": "string" }` | `{ "id": "uuid", "token": "jwt" }` | 201, 400, 409 |

### 6.2 Template OpenAPI (YAML)

```yaml
openapi: 3.0.0
info:
  title: TaskMaster API
  version: 1.0.0
paths:
  /api/projects:
    get:
      summary: Listar projetos
      responses:
        '200':
          description: OK
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Project'
components:
  schemas:
    Project:
      type: object
      properties:
        id:
          type: string
        nome:
          type: string
```

## 7. Priorização (MVP vs v2+)

| Funcionalidade                   | MVP | v2+ |
| -------------------------------- | :-: | :-: |
| Autenticação (email/senha + JWT) |  ✔  |     |
| CRUD de Projetos                 |  ✔  |     |
| CRUD de Tarefas                  |  ✔  |     |
| Filtros e Ordenação              |  ✔  |     |
| Convites por email               |     |  ✔  |
| Notificações por email           |     |  ✔  |

## 8. Glossário

* **API**: Application Programming Interface
* **JWT**: JSON Web Token
* **ER**: Entity-Relationship

## 9. Referências e Apêndices

* Links de ferramentas: draw\.io, Swagger Editor
* Template base: `srs-template`
* [IEEE Recommended Practice for Software Requirements Specifications
](http://www.math.uaa.alaska.edu/~afkjm/cs401/IEEE830.pdf)

## 10. Aprovações

| Stakeholder           | Função  | Data       | Assinatura |
| --------------------- | ------- | ---------- | ---------- |
| Product Owner         | \[Nome] | YYYY-MM-DD |            |
| Arquiteto de Software | \[Nome] | YYYY-MM-DD |            |
