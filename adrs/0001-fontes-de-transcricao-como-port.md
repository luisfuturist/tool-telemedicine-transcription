+++
id = "ADR-0001"
title = "Fontes de transcrição como port"
status = "open"
created = "2026-11-06"
priority = "medium"
+++

# ADR-0001: Fontes de transcrição como port

## Decision

Toda fonte de áudio converte para o mesmo stream de
`TranscriptEvent`s e entra na mesma pipeline
(`AnamnesisExtractor` → revisão médica). Adapters:

- `WebSpeechTranscriber` (MVP): browser, presencial, client-side.
- `MeetingBotTranscriber` (futuro): bot entra na chamada
  Meet/Zoom/Teams via meeting-bot-as-a-service (Recall.ai et al.),
  transcript chega por webhook server-side.
- `UploadTranscriber` (futuro): upload de gravação → Whisper.

## Rationale

Teleconsulta é o caso de uso natural (a consulta já é digital),
mas construir bot próprio (Puppeteer + captura de áudio) é
semana(s) de trabalho frágil. Bot-as-a-service custa ~$0.70–1/h
por chamada — vira linha de pricing (presencial grátis, bot por
consulta). Bônus LGPD: o bot visível na chamada É o consentimento.

## Consequences

`Transcriber` client e server convergem conceitualmente — o que
importa é o stream de eventos entrando no pipeline, não o lado
onde o adapter roda. `consulta/nova` (live browser) é um caso
especial de "fonte". O consentimento é por-design: fonte bot =
visível; fonte browser/upload = médico declara consentimento.
