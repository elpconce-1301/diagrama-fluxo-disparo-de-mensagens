# Prompt 1.1 - Atualização de Diagrama C4

Você é um arquiteto de software sênior. Avalie o diagrama atual e gere um novo diagrama C4 (Containers) corrigido, seguindo o fluxo REAL da solução:

## Fluxo correto (obrigatório)

1. O Outbound Notification Service (Push) dispara campanhas.
2. O Push envia eventos para o Digital Assistant Engine (Bot).
3. O Bot decide qual canal usar (ex.: Canal A, Canal B).
4. O Bot monta o payload específico para cada canal.
5. O Bot chama APIs externas diferentes para cada canal.
6. O canal entrega a mensagem ao cliente.
7. Callbacks e respostas retornam para o Bot.

## Regras obrigatórias do diagrama

- Nível: C4 — Containers.
- Não misture níveis (não mostrar componentes, classes, endpoints).
- Marcar integrações externas como `<<external>>`.
- O Push não decide canal.
- O Push não chama APIs externas.
- O Bot é quem decide o canal.
- O Bot é quem chama APIs externas.
- O Bot processa callbacks e respostas.
- Mostrar apenas dependências relevantes.
- Manter nomes genéricos:
  - Outbound Notification Service
  - Mailing Ingestion Service
  - Mailing Processing Service
  - Scheduling Service
  - Message Dispatch Service
  - Digital Assistant Engine
  - Channel A Provider API <<external>>
  - Channel B Provider API <<external>>

## Entregue

1. 5–10 lacunas encontradas no diagrama atual.
2. 5–8 perguntas de esclarecimento.
3. Roteiro revisado (8–12 linhas) com:
   - Escopo
   - Nível
   - Limites e responsabilidades
   - Integrações externas
   - Restrições
   - Lacunas
4. Novo diagrama C4 Containers em Mermaid ou PlantUML (você escolhe).
5. Checklist de revisão (8–12 itens).

## Atualize o README.md

Após gerar o novo diagrama:

- Adicione o roteiro revisado.
- Adicione o diagrama renderizado.
- Adicione seção “Decisões e Ajustes”.
- Adicione seção “Lacunas e Próximos Passos”.
- Adicione seção “Como este repositório serve de contexto para agentes de desenvolvimento”.

O README deve ser claro, versionável e alinhado com a Unidade III.
