# Prompt 1 — Geração de Diagrama C4 (Containers) 

Você é um arquiteto de software sênior. Antes de gerar qualquer diagrama, identifique lacunas e faça perguntas de esclarecimento.

## Descrição (linguagem natural)

O sistema **Outbound Notification Service** é responsável por disparar mensagens proativas para clientes em diferentes canais digitais.

O fluxo começa quando o time de campanhas faz upload de um arquivo contendo identificadores de usuários e o caso de uso que será disparado.

Esse arquivo é recebido por um **Mailing Ingestion Service**, que registra o mailing em um banco de dados e salva o arquivo em um armazenamento de objetos, gerando uma URL pública.

Um **Mailing Processing Service** baixa o arquivo, lê todas as linhas e coloca cada item em uma fila de processamento.  
Se o disparo for imediato, os itens vão para a fila **SendMessage**.  
Se for agendado, vão para a fila **ProcessLine**, que é consumida por um **Scheduling Service**, responsável por mover os itens para a fila **SendMessage** no horário programado.

Um **Message Dispatch Service** consome cada item da fila e envia para o **Message Orchestration Service**, registrando o status em banco.

O **Message Orchestration Service** consulta um **Configuration Service** para descobrir qual template usar, qual canal está configurado e qual endpoint deve receber a mensagem.  
Com essas informações, ele monta o payload completo (cada canal tem formato próprio) e publica na fila **MessageProducer**.

O **Message Producer** consome o payload e faz uma requisição HTTP para o **Channel Orchestrator**, sem saber qual canal está sendo usado.

O **Channel Orchestrator** recebe a mensagem e aciona o plugin correspondente ao canal (ex.: Canal A, Canal B), que entrega a mensagem ao cliente.

O sistema **Digital Assistant Engine** é responsável por processar respostas dos clientes e conduzir diálogos quando houver interação bidirecional.

---

## Objetivo do diagrama

- **Tipo:** estrutural  
- **Nível:** C4 — Containers  
- **Escopo:** fluxo completo do disparo proativo, desde o upload do arquivo até a entrega no canal e possível interação com o Digital Assistant Engine.

---

## Restrições

- Marcar integrações externas como `<<external>>`.  
- Não listar endpoints, rotas ou payloads.  
- Não misturar níveis do C4.  
- Mostrar apenas dependências relevantes.  
- Não expor detalhes internos dos canais externos.

---

## Entregue

### 1) Liste 5–10 lacunas ou ambiguidades relevantes.

### 2) Liste 5–8 perguntas de esclarecimento (curtas e objetivas).

### 3) Reescreva a descrição como um roteiro revisado (8–12 linhas), incorporando:
- Escopo  
- Nível  
- Limites e responsabilidades  
- Integrações externas  
- Restrições  
- Lacunas  

---

## Regras

- Se faltar contexto, explicite; não assuma fatos.  
- **Não gere o diagrama nesta etapa.**  
- O PlantUML será solicitado em um próximo prompt.

---

## Formato

Markdown estruturado.

---

## Saída esperada

Um roteiro que caiba nos elementos de
**Escopo, Nível, Limites, Integrações, Restrições, Lacunas.**

---
