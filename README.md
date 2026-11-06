# Belle Agenda AI

App Android (Flutter/Dart) para gestão profissional de manicures: agenda, clientes, serviços, financeiro, gráficos, relatórios CSV, lembretes 24h e assistente de IA (Belle AI) com API real configurável.

> **Status do build:** o código-fonte está completo, mas o **APK ainda não foi compilado nem testado em aparelho**. O ambiente onde o projeto foi escrito não tinha Flutter/Android SDK nem acesso de rede para instalá-los. Siga uma das duas rotas abaixo para gerar o APK. Pode haver pequenos ajustes de compilação na primeira execução (ver "Se o build falhar").

## Gerar o APK

### Rota A - no seu computador
Requisitos: Flutter estável (3.27 ou superior), JDK 17, Android SDK (via Android Studio).

```bash
cd belle_agenda_ai
python3 tool/setup_android.py     # cria android/, aplica manifesto, minSdk 23 e desugaring
flutter pub get
flutter analyze
flutter build apk --release
# saída: build/app/outputs/flutter-apk/app-release.apk
```
Instalar no celular: `flutter install` (USB com depuração ativada) ou copie o `.apk` e abra no aparelho (permita "instalar apps desconhecidos").

### Rota B - sem instalar nada (GitHub Actions)
1. Crie um repositório no GitHub e envie esta pasta.
2. Aba **Actions → Build APK → Run workflow**.
3. Baixe o artefato `belle-agenda-ai-apk` (contém o `app-release.apk`).

O APK de release usa a chave de debug por padrão (serve para uso pessoal). Para publicar na Play Store, configure uma keystore própria: https://docs.flutter.dev/deployment/android#signing-the-app

## Arquitetura

```
lib/
  main.dart                 inicialização (locale, settings, notificações)
  state.dart                AppState: contador de versão que recarrega as telas
  core/                     tema (rosa queimado/pastel/bege) e formatadores
  data/
    models.dart             Client, Service, Appointment, Expense, ReminderLog
    database.dart           SQLite (sqflite) + consultas + reserva atômica de envios
  services/
    stats.dart              faturamento, lucro, ticket médio, horários livres
    ai_service.dart         Belle AI (Anthropic Messages ou OpenAI-compatível)
    messaging.dart          WhatsApp Cloud API (oficial) e e-mail via API HTTP
    reminder_engine.dart    envio real com deduplicação, tentativas e log
    notifications.dart      notificações locais 24h antes + tarefa em segundo plano
    report_service.dart     relatórios CSV compartilháveis
    settings.dart           chaves em armazenamento seguro (Android Keystore)
  ui/                       dashboard, agenda, clientes, serviços, financeiro, IA, ajustes
tool/setup_android.py       gera android/ e aplica os ajustes
android_overrides/          AndroidManifest com permissões e receivers
```

### Banco de dados
SQLite local (`belle_agenda.db`). Todas as entidades têm `updated_ms` e `deleted` (exclusão lógica), preparando a sincronização em nuvem por "última escrita vence" por registro. A sincronização em si **não está implementada**: falta escolher o backend.

### Lembretes e automação
- **Notificação local** é agendada no aparelho para 24h antes de cada atendimento (funciona offline) e reagendada ao abrir o app.
- **Envio ao cliente (WhatsApp/e-mail):** uma tarefa em segundo plano (a cada ~15 min, quando há internet) e a abertura do app chamam o `ReminderEngine`, que envia para atendimentos que começam em até 24h.
- **Nada é simulado:** o status `Enviado` só é gravado após resposta HTTP 2xx com id do provedor; caso contrário grava `Falhou` com o erro devolvido.
- **Duplicidade:** `UNIQUE(agendamento, canal, tipo)` + reserva em transação. Falhas são retentadas até 3 vezes; envios travados em "Enviando" por mais de 10 min voltam a ser elegíveis.

### Limites conhecidos
- **Push remoto (FCM)** não está incluído: exige um projeto Firebase seu. As notificações são locais.
- O Android pode atrasar tarefas em segundo plano (economia de bateria). Em alguns fabricantes, desative a otimização de bateria para o app.
- O WhatsApp exige conta Business verificada e **template aprovado** pela Meta para mensagens iniciadas pela empresa.
- Relatórios saem em CSV (abre no Excel/Planilhas); PDF não foi implementado.

## Se o build falhar
Rode `flutter analyze` e envie a saída: por não ter havido compilação, erros de API entre versões do Flutter/pacotes são possíveis e costumam ser de uma linha.
