# LLMOps Agentic Professional Framework

**A production-oriented framework for reliable, evidence-driven and human-governed AI agents.**

`llmops-agentic-professional-framework` è un framework di engineering, governance e assurance per la progettazione, l'esecuzione e la verifica di sistemi basati su Large Language Models e agenti AI destinati a workflow professionali complessi e ad alto impatto.

Il progetto nasce con un obiettivo preciso:

> trasformare un LLM da semplice sistema conversazionale a componente professionale governato, verificabile, osservabile e sottoposto a controllo umano effettivo.

Il framework integra principi di LLMOps, agent engineering, harness engineering, retrieval-augmented generation, structured outputs, eval-driven development, tool governance, sicurezza, provenance, durable execution e human-in-the-loop.

La filosofia progettuale centrale è:

**NO EVIDENCE → NO MATERIAL FACTUAL CLAIM**  
**NO VERIFICATION → NO COMPLETION**  
**NO AUTHORITY → NO ACTION**  
**NO REQUIRED HUMAN APPROVAL → NO EXECUTION**

---

## Purpose

Il framework è progettato per supportare applicazioni AI in cui accuratezza, tracciabilità delle fonti, controllo degli strumenti, responsabilità umana e riproducibilità sono requisiti architetturali e non semplici istruzioni contenute nel prompt.

L'obiettivo è ridurre sistematicamente:

- hallucinations;
- unsupported claims;
- invented citations;
- stale or superseded sources;
- retrieval failures;
- tool misuse;
- excessive agency;
- prompt injection;
- privilege escalation;
- state contamination;
- inter-agent error propagation;
- premature completion;
- unverified calculations;
- unauthorized external actions.

---

## Core Architecture

Il framework implementa una pipeline governata:

USER / SYSTEM REQUEST  
↓  
SECURITY & AUTHORITY BOUNDARY  
↓  
DOMAIN + RISK ROUTER  
↓  
SCOPE & AUTHORITY ENVELOPE  
↓  
PLAN  
↓  
EVIDENCE RETRIEVAL  
↓  
EXECUTION  
↓  
DURABLE STATE  
↓  
INDEPENDENT VERIFICATION  
↓  
HUMAN GATE, WHEN REQUIRED  
↓  
AUTHORIZED ACTION  
↓  
FINAL OUTPUT  
↓  
EVALS + TRACE + LEARNING

---

## Engineering Principles

### Evidence-first

Ogni affermazione materiale deve essere collegabile a evidenze identificabili, versionate e verificabili.

Facts, calculations, assumptions, assessments and inferences are represented separately.

### Authority before autonomy

Un agente può eseguire esclusivamente le azioni comprese nel proprio `authority_envelope`.

Disponibilità tecnica di uno strumento non equivale ad autorizzazione al suo utilizzo.

### Read / Write separation

Le attività di ricerca, analisi e reasoning sono separate dalle operazioni che modificano sistemi, dati, documenti o risorse esterne.

Le azioni mutative richiedono policy enforcement e, quando previsto, autorizzazione umana.

### Maker–Checker architecture

La produzione di un risultato e la sua validazione sono funzioni distinte.

La verifica privilegia:

deterministic checks → independent recalculation → independent retrieval → policy/schema validation → independent model review → human professional review.

### Durable State

Gli obiettivi che superano il singolo turno utilizzano uno stato persistente contenente almeno:

- objective;
- scope;
- authority;
- evidence;
- claims;
- assumptions;
- tasks;
- checkpoints;
- approvals;
- budget;
- handoffs;
- rollback point;
- stop conditions.

### Human Governance

Per operazioni professionali, regolamentate, finanziarie, cliniche, creditizie, fiscali, contabili, di revisione, trading o comunque ad alto impatto, l'AI opera come strumento di supporto.

L'approvazione umana deve essere:

- esplicita;
- informata;
- documentata;
- riferita allo specifico artefatto o azione;
- esercitata da una persona competente e autorizzata;
- revocabile o invalidabile in caso di modifica sostanziale dell'output.

`DISCLAIMER != CONTROL`

Il controllo umano deve essere implementato tecnicamente nel workflow.

---

## Source Authority Model

Il framework distingue tre piani di autorità.

### Normative Authority

Leggi, regolamenti, fonti ufficiali, autorità di vigilanza, standard applicabili e prassi istituzionali.

### Technical Authority

Runtime verificato, repository ufficiali, documentazione ufficiale, release notes, specification e implementazioni validate.

### Case Evidence

Documenti del caso concreto, dati originali, output dei tool, calcoli verificabili e inferenze esplicitamente qualificate.

Il sistema seleziona la gerarchia appropriata in funzione della natura della claim.

---

## Professional Domains

Il framework è progettato per essere esteso mediante router e policy specialistiche a domini quali:

**Audit & Assurance**  
Revisione legale, controlli, evidence collection, working papers e audit trails.

**Public Sector & Local Authorities**  
Revisione degli enti locali, contabilità pubblica, controlli finanziari e compliance.

**Accounting & Tax**  
Fiscalità, bilancio, contabilità e analisi documentale.

**Corporate Banking & Credit Risk**  
Analisi del merito creditizio, bancabilità, affidamenti, covenant e risk assessment.

**Financial Markets & Trading**  
Research, order flow, volume profile, market analysis e decision support con human execution gate.

**Legal and Regulatory Research**  
Ricerca normativa, giurisprudenziale e regulatory intelligence evidence-based.

**Professional Publishing**  
Manuali, articoli tecnici, report e contenuti professionali con source verification.

**AI Governance & LLMOps**  
Agent engineering, evals, observability, RAG, MCP, tool security e AI assurance.

---

## Reliability Targets

Per i workflow critici il framework mira a:

```text
unsupported_claim_rate = 0

human_gate_bypass_rate = 0

claim_evidence_coverage = 1.00

unauthorized_tool_execution = 0

critical_unresolved_contradictions = 0
