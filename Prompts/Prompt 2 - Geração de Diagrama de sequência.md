# Prompt 2 - Geração de Diagrama de Sequência

Você é um arquiteto de software sênior. Gere um diagrama de sequência para a jornada crítica abaixo, seguindo o fluxo REAL da solução.

## Jornada crítica: Disparo de mensagem → Decisão de canal → Entrega → Callback

### Fluxo obrigatório

1. O Outbound Notification Service processa o mailing e dispara um evento para o Digital Assistant Engine.
2. O Digital Assistant Engine recebe o evento e:
   - consulta regras/configurações
   - decide qual canal usar (Canal A ou Canal B)
   - monta o payload específico para o canal escolhido
3. O Digital Assistant Engine chama a API externa do canal escolhido.
4. O canal entrega a mensagem ao Cliente.
5. O canal envia callback de status (entregue, falhou, lido, respondido).
6. O Digital Assistant Engine recebe o callback e:
   - atualiza status
   - processa resposta (se houver)
   - mantém diálogo quando aplicável

## Regras obrigatórias

- O diagrama deve ser comportamental, não estrutural.
- Mostrar ordem temporal das interações.
- Mostrar chamadas síncronas e assíncronas quando relevante.
- Representar claramente:
  - decisão de canal
  - chamada à API externa
  - entrega ao cliente
  - callback retornando ao bot
- Não mostrar detalhes de payloads.
- Não misturar níveis (não mostrar containers internos).
- Marcar sistemas externos como `<<external>>`.

## Participantes obrigatórios

- Time de Campanhas (opcional)
- Outbound Notification Service
- Digital Assistant Engine
- Canal A Provider API <<external>>
- Canal B Provider API <<external>>
- Cliente
- Callback Handler (parte do Bot)

## Entregue

1. 5–10 lacunas relevantes sobre a jornada.
2. 5–8 perguntas de esclarecimento.
3. Roteiro revisado (8–12 linhas) com:
   - Escopo
   - Participantes
   - Caminho principal
   - Caminho alternativo (falha no canal)
   - Decisão de canal
   - Callback
4. Diagrama de sequência completo em Mermaid ou PlantUML.
5. Checklist de revisão (8–12 itens).

## Atualize o README.md

Após gerar o diagrama:

- Adicione o roteiro revisado.
- Adicione o diagrama renderizado.
- Adicione seção “Decisões e Ajustes”.
- Adicione seção “Lacunas e Próximos Passos”.
- Adicione seção “Como este repositório serve de contexto para agentes de desenvolvimento”.

O README deve ser claro, versionável e alinhado com a Unidade III.
