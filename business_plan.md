# Business Plan — Anamnese AI

*Status: draft — a validar nas decisões abertas no fim.*

## Problema

Médico brasileiro gasta ~40% da consulta documentando. Anamnese mal
registrada = erro clínico, exposição jurídica, e consulta de 15min
vira 25min de prontuário. EMRs brasileiros são ruins de UX — o médico
digita olhando pra tela, não pro paciente.

## Solução

Ambient scribe: app grava a consulta → transcreve (PT-BR) → estrutura
anamnese clínica (QP, HDA, antecedentes, exame físico, hipóteses,
conduta) → médico revisa e aprova → exporta pra prontuário/EMR.

## Quem paga

- **MVP**: médico individual (B2C) — self-serve, modelo Freed
  (~R$99–149/mês, product-led, CAC baixo)
- **Alvo**: clínicas pequenas/médias (B2B) — assinatura por cadeira

## Concorrência

- **Global**: Abridge (~$5B val), Nuance DAX (Microsoft), Nabla,
  Heidi, Ambience, Freed, Suki — guerra com centenas de milhões em
  funding.
- **Brasil**: menos maduro, mas EMRs (Feegow/Doctoralia, iClinic,
  Amplimed, MV, Tasy) correm pra embutir scribe — risco de virar
  feature do prontuário.
- **Commoditização**: Whisper + LLM fazem 80% disso num fim de
  semana — o produto não pode ser só "transcreve".

## Diferenciação (escolher 1–2, não todas)

1. **PT-BR clínico profundo** — taxonomia de anamnese brasileira,
   CID-10, semântica local; corpus proprietário vira moat.
2. **Especialidade-first** — uma especialidade com template rico
   (psiquiatria, dermato, cardio…), onde a estrutura da anamnese é
   o valor, não a transcrição.
3. **LGPD-native** — consentimento do paciente, anonimização/edge,
   trilha de auditoria — argumento de venda institucional.
4. **Integração EMR** — escrever nos prontuários existentes
   (Feegow, iClinic…) em vez de competir com eles.

## Modelo

- Freemium: N consultas/mês grátis → R$99–299/mês por médico.
- B2B: por cadeira/mês + onboarding; per-consult em volume alto.
- Margem: transcrição (~$0.006/min) + LLM ≈ centavos/consulta →
  margem SaaS normal.

## Riscos principais

- **Regulatório**: LGPD — dados sensíveis de saúde (consentimento,
  DPO, minimização). CFM não exige certificação pra documentação
  auxiliar hoje, mas erro do modelo = liability do médico que
  respinga em nós.
- **Competição de plataforma**: EMR embute scribe de graça →
  precisamos ser melhores ou integrar.
- **Precisão clínica**: alucinação em anamnese não é bug, é
  incidente — todo campo precisa apontar pro trecho do áudio que
  o fundamenta (source-linking).

## MVP (hipótese)

Upload/gravação de áudio → transcrição PT-BR → anamnese estruturada
editável (1 especialidade) → exportar/copiar → métricas: % campos
aceitos sem edição, tempo economizado declarado, D7 retention de
médicos pagantes.

## Kill / validate

10 médicos usando por 2 semanas: se ninguém paga, ou se tempo de
edição > tempo de digitação, morre.

## Decisões (2026-11-06)

- **Modelo**: Freed — self-serve, médico individual, product-led.
- **Posicionamento**: ferramenta de documentação, NUNCA suporte à
  decisão — toda anamnese é rascunho revisado/aprovado pelo médico.
  Fora de SaMD/ANVISA; human-in-the-loop é o escudo de liability.
- **Telemedicina-first**: teleconsulta é o modo fácil — canais de
  áudio por participante = speaker labels grátis (diarização
  resolvida) + bot visível = consentimento embutido.
- **Presencial = modo ditado**: médico narra sozinho pós-consulta
  (mono-falante — Web Speech basta). Diarização em mic único é
  problema conhecido e NÃO prioridade.
- **Moat**: não é tech (tudo é commodity) — é canal + contexto
  acumulado do paciente entre consultas + embedding no workflow.

## Sinal de validação (2026-11-06)

Entrevistas informais: médicos dizem "relevante", JÁ TESTARAM
concorrentes (provável gringo, USD) e acharam caro. Leitura: dor
real + consciência de mercado + sensibilidade a preço = abertura
pra undercut local (R$99–149 vs. ~R$500+ dos gringos). Ainda não é
prova de compra — validar via piloto: 2 semanas grátis com os
mesmos médicos, perguntando (1) qual concorrente testaram,
(2) preço+idioma ou só preço foi o problema, (3) quanto pagariam.

## Pesquisa de canal (2026-11-06)

Realidade BR: WhatsApp domina o atendimento remoto (~69% dos
médicos — APM) e é INCAPTURÁVEL (sem bot, sem aba). Browser-based
calls (Meet/Zoom/Doctoralia-vídeo) são minoria capturável por
tab audio/bot. Consequência: ditado pós-consulta + upload de
gravação viram o caminho primário; tab audio/bot cobrem o
segmento browser. A integração de rotina decisiva é "copiar pro
EMR" (clipboard), não API. Pergunta-chave do piloto: "em que app
roda a consulta?"

## Decisões abertas

1. **Especialidade do MVP** — a que tivermos acesso a médicos reais.
2. **LGPD como feature** — auditável/on-prem desde o dia 1, ou
   compliance mínimo no MVP?
3. **Stack de transcrição** — bot teleconsulta (Recall.ai) vs.
   upload+Whisper vs. Deepgram com diarização.
