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

```text
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
```

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

```text
deterministic checks
→ independent recalculation
→ independent retrieval
→ policy/schema validation
→ independent model review
→ human professional review
```

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

```text
DISCLAIMER != CONTROL
```

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

## GitHub Integration

Il framework supporta l'interazione con GitHub tramite connettore MCP, plugin o GitHub App, ma ogni operazione di scrittura è vincolata a un `authority_envelope` esplicito.

La disponibilità di un tool MCP non autorizza di per sé la sua esecuzione: l'agente deve verificare repository, branch, permessi, rischio e approvazione richiesta prima di agire.

### Supported GitHub operations

| Operazione | Classificazione | Autorizzazione richiesta |
|---|---|---|
| Lettura repository, file, issue e PR | Read | Consenso connessione + accesso repository |
| Ricerca codice, documentazione e cronologia | Read | Consenso connessione + accesso repository |
| Creazione branch | Write | `contents: write` + policy repository |
| Commit su branch non protetto | Write | `contents: write` + approvazione secondo policy |
| Apertura Pull Request | Write | `contents: write` + `pull_requests: write` |
| Commento su issue o Pull Request | Write | `issues: write` oppure `pull_requests: write` |
| Modifica di `.github/workflows/` | High-risk write | `contents: write` + `workflows: write` + human approval |
| Merge di Pull Request | High-risk write | `pull_requests: write` + human approval |
| Cancellazione branch, release o repository | Destructive | Esplicitamente vietata di default |

### Minimum GitHub permissions

Per consentire la creazione di branch e Pull Request dall'assistente, configurare l'integrazione con i seguenti permessi minimi:

| Permesso GitHub | Livello | Finalità |
|---|---:|---|
| `Metadata` | Read | Accesso di base al repository |
| `Contents` | Read and write | Lettura file, creazione branch, commit e modifica contenuti |
| `Pull requests` | Read and write | Apertura, aggiornamento e gestione delle Pull Request |
| `Issues` | Read, oppure Read and write | Lettura issue; scrittura solo se abilitata dalla policy |
| `Workflows` | Read and write | Solo se l'agente è autorizzato a modificare GitHub Actions |

Il permesso `Contents: Read and write` è necessario per creare branch e pubblicare modifiche ai file; `Pull requests: Read and write` è necessario per aprire o gestire Pull Request. [16][25][30]

### Recommended least-privilege profile

Per questo framework si raccomanda la configurazione iniziale:

```yaml
github_integration:
  mode: "mcp"
  default_access: "read_only"

  allowed_repositories:
    - "OWNER/llmops-agentic-professional-framework"

  branch_policy:
    protected_branches:
      - "main"
    allowed_branch_prefixes:
      - "feature/"
      - "fix/"
      - "docs/"
      - "chore/"
      - "release/"
    direct_push_to_default_branch: false
    require_pull_request: true

  permissions:
    metadata: "read"
    contents: "read_and_write"
    pull_requests: "read_and_write"
    issues: "read"
    workflows: "read_and_write"

  write_gates:
    create_branch: "auto_if_policy_compliant"
    commit_to_feature_branch: "auto_if_policy_compliant"
    create_pull_request: "human_confirmation"
    modify_workflow: "human_approval"
    merge_pull_request: "human_approval"
    delete_branch: "denied"
    delete_repository: "denied"
```

### Authority envelope example

Prima di una modifica al repository, l'agente deve materializzare un envelope simile al seguente:

```json
{
  "action": "create_branch_and_pull_request",
  "repository": "OWNER/llmops-agentic-professional-framework",
  "source_branch": "main",
  "target_branch": "docs/update-readme-github-integration",
  "permitted_operations": [
    "create_branch",
    "create_or_update_file",
    "create_pull_request"
  ],
  "forbidden_operations": [
    "merge_pull_request",
    "delete_branch",
    "modify_repository_settings",
    "modify_github_secrets",
    "push_to_main"
  ],
  "approval_required": true,
  "approval_scope": "branch_creation_and_draft_pull_request",
  "rollback_strategy": "close_pull_request_and_delete_feature_branch",
  "evidence": [
    "README.md",
    "docs/github-integration-policy.md"
  ]
}
```

### ChatGPT / MCP operating rule

Quando l'utente chiede una pubblicazione o modifica su GitHub, l'agente deve:

1. Verificare che il repository sia incluso in `allowed_repositories`.
2. Verificare che l'operazione rientri nel proprio `authority_envelope`.
3. Proporre il nome del branch, i file interessati e un riepilogo delle modifiche.
4. Richiedere conferma esplicita prima di creare branch, commit o Pull Request.
5. Non effettuare mai push diretti su `main`.
6. Aprire, di norma, una Pull Request in `draft`.
7. Registrare nel trace: richiesta, piano, evidenze, diff, approvazione, esito e rollback point.

Esempio di flusso approvato:

```text
USER
→ "Aggiorna il README con la sezione GitHub Integration."

AGENT
→ Analizza il repository.
→ Propone branch: docs/update-readme-github-integration.
→ Mostra il diff previsto.
→ Richiede conferma.

USER
→ "Approvo creazione branch e Pull Request in draft."

AGENT
→ Crea il branch.
→ Applica le modifiche.
→ Apre la Pull Request in draft.
→ Restituisce URL, commit SHA, stato e rollback point.
```

### Failure handling

In caso di errore `403 Resource not accessible by integration`, l'agente deve:

1. Non ripetere automaticamente la stessa chiamata.
2. Classificare l'errore come **authorization failure**, non come errore tecnico generico.
3. Verificare se il token/App è installato sul repository target.
4. Verificare che `Contents`, `Pull requests` e, se necessario, `Workflows` siano abilitati in scrittura.
5. Proporre all'utente l'intervento manuale via interfaccia GitHub o Git locale.
6. Registrare l'evento nel trace di governance.

L'errore indica in genere che l'integrazione dispone di lettura ma non del permesso di scrittura necessario per l'operazione richiesta. [3][16]

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

unauthorized_repository_write = 0

direct_push_to_protected_branch = 0
```
