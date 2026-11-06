+++
id = "P-001"
title = "Própria call platform — ser o canal, não capturar"
status = "open"
created = "2026-11-06"
priority = "high"
labels = ["estrategia"]
+++

# P-001: Própria call platform — ser o canal, não capturar

## Ideia

Em vez de capturar calls alheias (matriz frágil: tab audio, WASM,
bot, loopback), o produto vira a sala de teleconsulta:

- médico manda link por WhatsApp (a rotina já é essa)
- paciente entra na nossa call via browser (zero install)
- médico no nosso app
- pernas de áudio separadas nativamente = labels perfeitos,
  zero diarização, consent vira banner, LGPD limpa

## Trade-offs

**Pró**: elimina TODA a matriz de captura; qualidade controlada;
cobre WhatsApp/telefone/browsers sem exceção; moat — a call E a
documentação vivem no mesmo lugar.

**Contra**: deixa de ser scribe-alongside → vira plataforma de
teleconsulta (scheduling, waiting room, vídeo confiável); compete
com Doctoralia-vídeo/Conexa no canal.

## Caminho

LiveKit/Daily/Twilio pra infra (weeks, não months). Estratégia:
scribe-alongside como wedge → se piloto mostrar que médico topa
mandar NOSSO link em vez do link do Meet, a plataforma vence.
Pergunta de validação nas entrevistas: "mandaria nosso link?"

## Fases

1. validar via piloto (pergunta do link)
2. sala WebRTC 1:1 (LiveKit) + scribe nativo por perna
3. scheduling + waiting room + prontuário embutido
