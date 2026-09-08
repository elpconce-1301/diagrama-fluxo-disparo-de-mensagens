# Saída Prompt 1 — Geração de Diagrama C4 (Containers)

## 1) Lacunas relevantes (10)

- Falta definição do formato do arquivo (CSV/JSON) e esquema dos campos.
- Não está claro como é a autenticação/autorização para upload e para a URL pública do objeto.
- Indeterminado comportamento de erro/retry (falha no download, falha no envio, backoff, DLQ).
- Inexistência de regras de idempotência e garantia de entrega (at-most-once, at-least-once, exactly-once).
- Não há limites/expectativas de throughput e latência (TPS, picos, SLAs).
- Falta política de privacidade/retention para arquivos, mailings e logs (ex.: PII, GDPR).
- Ambiguidade sobre ordenação e prioridade das mensagens nas filas.
- Integrações de canais (plugins) não têm especificação de contrato nem restrições de segurança.
- Não foi definido monitoramento/observability e métricas-chave (erro, latência, filas).
- Não ficou claro como o Digital Assistant Engine se integra (diretamente em fila, via webhook, ou serviço externo).

## 2) Perguntas de esclarecimento (8)

1. Qual o formato e esquema do arquivo de mailing?
2. Qual o tamanho máximo esperado por arquivo e taxa de upload pico?
3. As URLs de objeto são públicas por design ou devem exigir autenticação?
4. Qual garantia de entrega devemos suportar (at-most-once / at-least-once / exactly-once)?
5. Como tratar retries e falhas permanentes — DLQ ou outro fluxo?
6. Quais canais (nomes) e quem provê os plugins (internos vs <<external>>)?
7. O `Digital Assistant Engine` é um componente interno ou serviço <<external>>?
8. Há requisitos de retenção de dados e conformidade (GDPR/opt-out)?

## 3) Roteiro revisado (10 linhas)

1. Escopo: diagrama C4 (Containers) do fluxo completo — desde o upload do arquivo de mailing até a entrega no canal e possível interação com o Digital Assistant Engine.
2. Nível: Container — mostrar serviços, filas, banco(s) e armazenamento de objetos; não mostrar classes, endpoints ou payloads.
3. Limites e responsabilidades: `Mailing Ingestion Service` (recebe upload, persiste metadados, registra arquivo no armazenamento), `Mailing Processing Service` (processa arquivo e enfileira itens), `Scheduling Service` (move itens agendados para `SendMessage`), `Message Dispatch Service` (consome fila e invoca orquestração), `Message Orchestration Service` (resolve template/canal/configuração e publica na fila `MessageProducer`), `Message Producer` (faz requisição HTTP ao `Channel Orchestrator`), `Channel Orchestrator` (seleciona plugin de canal), `Digital Assistant Engine` (processa respostas/diálogos).
4. Filas visíveis: `SendMessage`, `ProcessLine`, `MessageProducer` — indicar direções de dependência entre produtores/consumidores.
5. Integrações externas: marcar armazenamento de objetos externo e plugins de canal como `<<external>>`; se o `Digital Assistant Engine` for externo, marcá-lo também como `<<external>>`.
6. Restrições de diagrama: não listar endpoints/rotas/payloads, não misturar níveis C4, exibir apenas dependências relevantes.
7. Requisitos operacionais a explicitar antes do desenho: formato/volume de arquivos, autenticação das URLs, garantias de entrega, políticas de retry e DLQ, e metas de SLA/throughput.
8. Segurança & privacidade: explicitar onde ocorrerá mascaramento/anonimização, controle de acesso a URLs e retenção de PII.
9. Observability: indicar que cada container deve expor métricas e logs (erros, latência, tamanho de filas) para desenho de dependências operacionais.
10. Lacunas pendentes: arquitetura deve priorizar respostas às lacunas listadas acima antes de gerar o PlantUML final.
