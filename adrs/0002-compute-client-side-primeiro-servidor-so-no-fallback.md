+++
id = "ADR-0002"
title = "Compute client-side primeiro, servidor só no fallback"
status = "open"
created = "2026-11-06"
priority = "medium"
+++

# ADR-0002: Compute client-side primeiro, servidor só no fallback

## Decision

STT roda no cliente sempre que possível; o servidor só processa
áudio quando a qualidade do lado barato falha. Fontes separadas
dão speaker labels sem diarização:

- `WebSpeechTranscriber` — mic do médico (grátis, nativo, pt-BR)
- `TabAudioTranscriber` — áudio da aba da teleconsulta via
  Whisper WASM (transformers.js) → lado "Paciente"
- `MediaRecorder` — gravação local; sobe ao server APENAS se
  confidence baixa/gaps → Whisper/Deepgram re-processa o trecho

## Rationale

Custo: caminho feliz ~grátis (compute do cliente), server pago só
nos casos ruins — undercuta concorrentes que cobram STT sempre.
LGPD: áudio do paciente nunca sai do dispositivo no caso comum;
sobe só por demanda ("quando necessário"), que segue vendável.

## Constraints

- Web Speech API é mic-only — não aceita MediaStream; tab audio
  exige Whisper WASM (~40-75MB, CPU pesada). Trade-off aceito.
- "Ambos os lados instalarem" foi rejeitado: paciente não instala
  nada — só o médico roda o app.
- Fallback pressupõe gravação local retida até a confiança ser
  avaliada — descartar após aprovação (data minimization).

## Consequences

Port `Transcriber` ganha múltiplas instâncias por fonte; merge por
speaker label na entrada do pipeline. Meeting bot (ADR-0001) fica
como opção p/ calls fora do browser do médico ou sem tab audio.
