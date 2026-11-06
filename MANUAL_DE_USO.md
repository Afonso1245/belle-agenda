# Manual de uso - Belle Agenda AI

## Primeiros passos
1. Abra o app e permita as **notificações** quando solicitado.
2. Em **Início → ⋯ → Serviços e preços**, cadastre seus serviços (nome, preço, duração).
3. Em **Clientes → +**, cadastre as clientes. Para lembretes, preencha o **WhatsApp com DDI e DDD** (ex.: 5511999998888) e/ou o **e-mail**.
4. Toque em **Agendar** para criar o primeiro atendimento.

## Início (dashboard)
Mostra os ganhos do mês, de hoje e da semana, valores a receber, lucro estimado, gráfico dos últimos 7 dias e a agenda de hoje.

## Agenda
Alterne entre **Dia / Semana / Mês** e navegue pelas setas. No modo mês, toque em um dia para abri-lo. Toque em um atendimento para: concluir e marcar como pago, editar, cancelar ou excluir. Ao salvar, o app avisa se houver **conflito de horário**.

## Clientes
Busca por nome, ficha com observações e **histórico de atendimentos** com total gasto.

## Financeiro
- Faturamento do mês e do ano, despesas, lucro estimado, pagamentos pendentes e ticket médio.
- Gráfico anual e ganhos por serviço.
- **Despesas:** botão "+ Despesa".
- **Relatórios:** ícone de compartilhar → mês ou ano (CSV), enviado pelo WhatsApp, e-mail, Drive etc.

Regras de cálculo: *faturamento* = atendimentos marcados como pagos; *pendente* = concluídos e não pagos; *lucro estimado* = faturamento - despesas; *ticket médio* = faturamento ÷ atendimentos pagos.

## Belle AI
Em **Ajustes**, informe o formato da API, a URL base, o modelo e a sua chave. Depois pergunte, por exemplo: "Quais os compromissos de amanhã?", "Resumo financeiro do mês", "Quais serviços são mais procurados?", "Sugira horários livres", "Redija o lembrete para as clientes de amanhã". A IA responde apenas com os dados do seu salão e **não envia mensagens** - o envio é feito pelo app (abaixo). Os dados da agenda e do financeiro são enviados ao provedor de IA que você configurou.

## Lembretes automáticos
- **Para você:** uma notificação é criada 24h antes de cada atendimento.
- **Para a cliente:** em **Ajustes**, ative WhatsApp Business (Phone Number ID, token e nome do template aprovado, com 4 variáveis: nome, serviço, data, hora) e/ou e-mail (endpoint, chave e remetente). O app envia automaticamente quando faltam até 24h.
- Acompanhe em **Início → ⋯ → Lembretes enviados**: Enviado, Falhou (com o erro), Enviando. Use **Processar agora** para forçar uma rodada.
- Cada atendimento recebe no máximo um lembrete por canal; falhas são retentadas até 3 vezes.

## Dicas
- Se os lembretes atrasarem, desative a otimização de bateria para o app nas configurações do Android.
- Faça cópia dos relatórios CSV periodicamente: os dados ficam somente no aparelho.
