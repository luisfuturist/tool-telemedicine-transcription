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

## Veredito (2026-11-06)

Endgame, não wedge. Risco: video call é problema impiedoso — se
cai, a culpa é nossa. Mitigação: **audio-only first** (voz sem
vídeo — muita anamnese não precisa de vídeo, 10x mais confiável/
barata de operar). Gate: piloto confirma que médico manda nosso
link; se não, scribe-alongside é o teto.

## Fases

1. validar via piloto (pergunta do link)
2. call audio-only 1:1 (LiveKit/vonage voice) + scribe por perna
3. vídeo opcional + scheduling + waiting room + prontuário

## Posicionamento (2026-11-06)

Não inovar na call — call é commodity por design, genérica-e-
confiável é o objetivo. A inovação é o que a call PRODUZ: a call
alheia termina em silêncio documental; a nossa termina com a
anamnese revisada. Diferenciais: pré-consulta assíncrona (triagem
por chat → anamnese pré-preenchida), assist durante a call,
contexto longitudinal por paciente (moat que compõe), e
transcrição+evidência como registro jurídico. Framing:
"telemedicina genérica por fora, documentação inteligente por
dentro" — a call é o gancho, o registro é o negócio.
