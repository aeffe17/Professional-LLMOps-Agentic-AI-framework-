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

## GitHub Integration and Authorization Model

Il framework supporta il dialogo diretto con GitHub tramite connettore ChatGPT, MCP server, GitHub App o Personal Access Token.

L'impostazione ChatGPT **Allow all actions** autorizza l'assistente a invocare i tool disponibili, ma **non concede da sola permessi di scrittura a GitHub**. L'autorizzazione effettiva dipende dai permessi configurati sulla GitHub App, sul fine-grained PAT o sul token OAuth utilizzato dall'integrazione.

```text
CHATGPT ACTION PERMISSION
≠ GITHUB REPOSITORY WRITE AUTHORIZATION
```

### Required GitHub permissions

Per consentire branch, commit, modifiche ai file e Pull Request avviate dal dialogo con ChatGPT, l'integrazione GitHub deve avere almeno i seguenti permessi:

| Permesso GitHub | Livello richiesto | Finalità |
|---|---|---|
| `Metadata` | Read | Accesso di base al repository |
| `Contents` | **Read and write** | Lettura file, creazione branch, commit, modifica e creazione file |
| `Pull requests` | **Read and write** | Apertura, aggiornamento, commento e gestione delle Pull Request |
| `Issues` | Read, oppure Read and write | Lettura issue; scrittura solo se espressamente abilitata |
| `Workflows` | **Read and write** | Solo per modifiche a `.github/workflows/`, previa revisione umana |

`Contents: Read and write` è il permesso indispensabile per creare branch ed effettuare commit; `Pull requests: Read and write` è necessario per aprire o aggiornare una Pull Request. [36][42][43]

### How to unlock write access

#### Case 1 — GitHub App

1. Accedi a GitHub come owner dell'App o come amministratore dell'organizzazione.
2. Apri **Settings → Developer settings → GitHub Apps**.
3. Seleziona l'App utilizzata dall'integrazione ChatGPT.
4. Apri **Permissions & events**.
5. Nella sezione **Repository permissions**, imposta:
   - `Contents`: **Read and write**;
   - `Pull requests`: **Read and write**;
   - `Issues`: **Read** oppure **Read and write**, secondo necessità;
   - `Workflows`: **Read and write**, solo se l'agente deve modificare GitHub Actions.
6. Salva le modifiche.
7. Apri l'installazione dell'App e **approva esplicitamente i nuovi permessi**.
8. Verifica che il repository sia incluso in **Repository access**.

GitHub richiede che ogni account o organizzazione dove l'App è installata approvi le nuove permission; finché questa approvazione non avviene, i permessi aggiornati non diventano effettivi. [32][34]

#### Case 2 — Fine-grained Personal Access Token

1. Apri **GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens**.
2. Crea o modifica il token usato dall'integrazione.
3. In **Repository access**, seleziona esplicitamente il repository target.
4. In **Permissions → Repository permissions**, imposta:
   - `Contents`: **Read and write**;
   - `Pull requests`: **Read and write**;
   - `Workflows`: **Read and write**, solo se necessario.
5. Rigenera o salva il token, se richiesto.
6. Aggiorna il token nella configurazione del connettore ChatGPT/MCP.

I fine-grained PAT consentono di attribuire permessi granulari e di limitare l'accesso a specifici repository. [31][40]

#### Case 3 — OAuth connection

1. Revoca la connessione GitHub esistente, se non consente l'aggiornamento dei permessi.
2. Riconnetti GitHub da ChatGPT o dal client MCP.
3. Durante il consenso OAuth, verifica che siano richiesti gli scope equivalenti a:
   - lettura e scrittura dei contenuti;
   - lettura e scrittura delle Pull Request;
   - eventuale scrittura dei workflow.
4. Se gli scope proposti restano in sola lettura, utilizzare una GitHub App o un fine-grained PAT con i permessi sopra indicati.

### Repository write policy

Il framework adotta il principio del minimo privilegio e vieta di default le operazioni distruttive o ad alto impatto.

```yaml
github_integration:
  mode: "chatgpt_mcp_or_github_app"
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
    modify_repository_settings: "denied"
    modify_github_secrets: "denied"
```

### Authorized GitHub operations

| Operazione | Default | Condizione |
|---|---|---|
| Leggere repository, file, issue e PR | Consentita | Repository autorizzato |
| Ricercare codice e documentazione | Consentita | Repository autorizzato |
| Creare un branch | Consentita | Branch conforme alla policy e permesso `Contents: write` |
| Committare su feature branch | Consentita | Modifica dichiarata e permesso `Contents: write` |
| Aprire Pull Request in draft | Consentita dopo conferma | Permessi `Contents: write` e `Pull requests: write` |
| Modificare `.github/workflows/` | Richiede approvazione umana | Permessi `Contents: write` e `Workflows: write` |
| Fare merge di una Pull Request | Richiede approvazione umana | Permessi e policy di merge |
| Push diretto su `main` | Vietato | Richiede eccezione documentata e approvazione |
| Cancellare branch, release o repository | Vietato | Non autorizzato di default |

### Dialogue protocol for repository changes

Quando l'utente chiede una modifica al repository, ChatGPT deve seguire questo protocollo:

1. **Identificare il repository** e verificare che sia autorizzato.
2. **Verificare i permessi effettivi** dell'integrazione prima di proporre operazioni di scrittura.
3. **Proporre un piano**: branch, file interessati, modifiche previste, motivazione e rischi.
4. **Mostrare un diff o riepilogo preciso** delle modifiche.
5. **Richiedere conferma esplicita** per la creazione del branch, dei commit e della Pull Request.
6. **Eseguire solo le azioni autorizzate**, senza mai superare il perimetro dichiarato.
7. **Restituire evidenze**: URL della Pull Request, commit SHA, branch, stato e rollback point.
8. **Registrare il trace**: richiesta, piano, evidenze, approvazione, esito, errori e azioni di rollback.

### Example approved dialogue

```text
USER
→ "Aggiorna il README per documentare le autorizzazioni GitHub e permetti
   la creazione di branch e Pull Request da ChatGPT."

AGENT
→ Repository: OWNER/llmops-agentic-professional-framework
→ Branch proposto: docs/update-github-write-authorizations
→ File modificato: README.md
→ Operazioni: create_branch, update_file, create_draft_pull_request
→ Non saranno eseguiti: merge, push su main, cancellazioni.

→ Confermi la creazione del branch e della Pull Request in draft?

USER
→ "Sì, approvo branch e Pull Request in draft."

AGENT
→ Crea il branch.
→ Applica le modifiche al README.
→ Apre la Pull Request in draft.
→ Restituisce URL, commit SHA, stato e istruzioni di rollback.
```

### Handling `403 Resource not accessible by integration`

Se la creazione del branch restituisce:

```text
403 Resource not accessible by integration
```

l'agente deve:

1. Non ripetere automaticamente la stessa chiamata.
2. Classificare l'errore come **authorization failure**.
3. Verificare che `Contents` sia impostato su **Read and write**.
4. Verificare che la GitHub App sia installata e autorizzata sul repository target.
5. Verificare che l'utente abbia approvato i nuovi permessi dopo la modifica.
6. Verificare che il repository sia incluso nel perimetro di accesso.
7. Se il permesso `Pull requests: write` manca, proporre la creazione del branch e del commit, ma non della Pull Request.
8. Se necessario, proporre l'intervento manuale tramite interfaccia GitHub o Git locale.

L'errore indica che l'integrazione può leggere la risorsa ma non dispone dell'autorizzazione di scrittura necessaria per l'operazione richiesta. [32][34]

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
