# Resolução do Hands-on: Mini-SRS “TaskMaster”

## 1. Três User Stories com Critérios de Aceitação

| ID    | User Story                                                                 | Critérios de Aceitação                                                                                                                                   |
|-------|-----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| **US-01** | **Como usuário autenticado**, quero **listar minhas tarefas**, para **acompanhar meu backlog**.                    | 1. GET `/api/tasks` retorna apenas tarefas do usuário autenticado.<br>2. Resposta `200` com JSON array de objetos `{ id, título, status, prazo }`.       |
| **US-02** | **Como líder de equipe**, quero **convidar um colega por email a um projeto**, para **colaborar nas tarefas**.    | 1. POST `/api/projects/{projectId}/invite` com body `{ email }`.<br>2. Retorna `200` e `{ sucesso: true }` se email válido e membro não cadastrado ainda. |
| **US-03** | **Como membro de equipe**, quero **atualizar o status de uma tarefa**, para **refletir o progresso do trabalho**. | 1. PATCH `/api/tasks/{taskId}` aceita `{ status }` onde `status ∈ {“pendente”, “em_andamento”, “concluída”}`.<br>2. Retorna `200` e objeto tarefa atualizado. |

---

## 2. Diagrama ER Simplificado


- **Usuário** (id PK, nome, email, senhaHash)  
- **Projeto** (id PK, nome, descrição, donoId FK → Usuário)  
- **Tarefa** (id PK, título, descrição, status, prazo, projetoId FK → Projeto, atribuidoA FK → Usuário)  

---

## 3. Dois Endpoints REST (Spec)

| Método | Rota                               | Request Body                                | Resposta                                                   | Códigos                |
|--------|------------------------------------|---------------------------------------------|------------------------------------------------------------|------------------------|
| POST   | `/api/projects/{projectId}/invite` | `{ "email": "colega@exemplo.com" }`         | `{ "sucesso": true, "mensagem": "Convite enviado." }`      | `200`, `400`, `401`    |
| PATCH  | `/api/tasks/{taskId}`              | `{ "status": "em_andamento" }`              | `{ "id": "...", "título": "...", "status": "em_andamento" }` | `200`, `400`, `401`, `404` |

---

# Critérios de Avaliação

| Critério                       | Excelente (10)                                                                          | Bom (7)                                                        | Precisa Melhorar (4)                          |
|--------------------------------|-----------------------------------------------------------------------------------------|----------------------------------------------------------------|-----------------------------------------------|
| **User Stories & Aceitação**   | Histórias claras, centradas no usuário; critérios completos e testáveis                | Histórias bem escritas; critérios genéricos ou incompletos     | Falta clareza no formato ou sem critérios     |
| **Modelagem de Dados (ER)**    | Relacionamentos corretos; entidades e chaves bem definidas; diagrama visual coerente  | Modelo básico ok; alguns relacionamentos confusos              | Falta diagrama ou relacionamentos incorretos  |
| **Spec de Endpoints**          | Endpoints bem detalhados (rota, payload, respostas, códigos); padronização consistente | Specs funcionais, mas sem padronização ou com poucos detalhes | Ausência de informação crítica (status, formatos) |
| **Aderência ao Tempo**         | Completa dentro do tempo estipulado (20 min)                                            | Quase completa, faltam pequenos ajustes                        | Incompleta ou excedeu muito o tempo           |
| **Documentação & Organização** | Usa Markdown de forma limpa; seções bem estruturadas; fácil de ler e navegar           | Organização razoável; algumas seções desalinhadas              | Documento confuso ou mal formatado            |

> **Pontuação final:**  
> - Some cada critério (máximo 50 pontos), depois normalize para 10 ou 100.  
> - Forneça feedback qualitativo: destaque pontos fortes e sugestões de melhoria.  
