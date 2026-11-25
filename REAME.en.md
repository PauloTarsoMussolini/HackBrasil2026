# ParSoM Smart Fields — Scrum with AI in Azure DevOps

[Português](README.md) · **English**

**AI-driven development, under management and control.** An Azure DevOps
extension that turns the Epic and the Feature into smart forms and, with
one click, prepares the repository so that an AI coding agent (Claude
Code, OpenAI Codex, Devin or another) works **inside the Scrum process the
team already uses**: reading structured decisions, recording every step on
the board and stopping when it needs a person.

> **More details at https://parsomproject.com.br** (Portuguese, English and
> Spanish): the role of each person on the team, the pages the team fills
> in, the help on every field, the AI files, the board and the control and
> audit guarantees, with the product's real screens.
>
> - **Installation manual, step by step:** https://parsomproject.com.br/manual/
> - **Manual as PDF (Portuguese):** https://parsomproject.com.br/manual/Manual-ParSoM-Smart-Fields.pdf

![An Epic with the ParSoM pages](images/marketplace/epic-form.jpg)

---

## Contents

1. [The problem](#1-the-problem)
2. [The solution](#2-the-solution)
3. [How it works, step by step](#3-how-it-works-step-by-step)
4. [Examples](#4-examples)
5. [Running the project](#5-running-the-project)
6. [Repository structure](#6-repository-structure)
7. [Technologies](#7-technologies)
8. [Security and privacy](#8-security-and-privacy)
9. [Known limitations](#9-known-limitations)
10. [License and credits](#10-license-and-credits)

> **A note on language.** The product's customers are Brazilian teams, so
> the field catalog, the rules served to the AI and the AI files are in
> Portuguese. The extension's screens are in English. In the examples
> below, Portuguese values are kept exactly as they appear in the product,
> with an English translation alongside.

---

## 1. The problem

AI coding agents already write code well. What fails is **what comes
before and around the code**:

- **The AI receives interpretations, not decisions.** A typical Epic
  arrives like this:

  > "Customer registration web system, maybe with AI for search. Use the
  > usual stack. Standard security. PRs to the development branch."

  Which stack? Which branch? What "standard security"? Each AI session
  fills those gaps one way, and each developer corrects them another way.

- **The AI works outside the process.** It writes code in a private
  conversation; the manager doesn't know what was done, by which model, at
  what cost, or who approved it. The Scrum board stops telling the truth.

- **There is no control and no audit trail.** Items are "done" without
  tests, without code review, without defects recorded. Time and tokens
  per phase can't be measured, so the process can't be improved.

- **Azure DevOps doesn't help people fill things in well.** There is no
  multi-select field, no lists that depend on other lists, no rules like
  "if the solution is API-only, the screen fields don't apply". Whoever
  fills in the form has to memorize what each field means.

## 2. The solution

ParSoM Smart Fields tackles the problem in three layers, all inside the
Azure DevOps the company already has.

### 2.1 Smart fields on the Epic and the Feature

- **Ready-made catalog**: 84 fields on the Epic (on 4 pages: vision and
  governance; application, integration and AI; data, platform and FinOps;
  security, quality and operations) and 40 on the Feature (scope and
  experience; build and delivery), tied together by **249 rules**.
- **Cascading lists**: a field offers only the values that match another
  (picked "Web SPA"? the framework field offers React, Angular, Vue,
  Svelte).
- **Multi-select**: checkboxes drawn by the extension, which Azure DevOps
  doesn't have.
- **Applicability rules**: make fields required, hide whole groups or
  recommend values, depending on the answers.
- **Help on every field**: the "?" explains what the field means and,
  generated from the rules themselves, what each answer causes in the rest
  of the form.
- **One-step installation**: the configuration screen reads the catalog,
  shows exactly what it will create, and creates fields, pages, groups and
  rules in the organization's inherited process.

### 2.2 One click: the repository learns how the team works

On the Epic, **"…" → Create AI files in the repository** writes to the
project's repository a set of files that guide the AI through the process.
There are four sets: **Anthropic Claude**, **OpenAI Codex**, **Devin** and
**Outras IAs** (Other AIs, generic).

- A **preview** lists every file before writing.
- **No existing file is ever replaced.**
- Empty repository: the files become the first commit. With content: a new
  branch and a pull request, linked to the Epic.

![Choosing the AI file set](images/marketplace/ai-files.jpg)

### 2.3 The ParSoM rules server

The detailed rules for each stage (interview, slicing, test-first
implementation, code review, defects, closing) **don't live in the
customer's repository**. They are served to the AI, one document at a
time, by an **MCP** (Model Context Protocol) server, only to confirmed
people from licensed organizations. The repository gets a thin set: the
AI's entry file, the project configuration, the hooks and the scripts that
measure, lock and record.

### 2.4 The result: the board tells what the AI is doing

| State | Who acts | What happens |
|---|---|---|
| **Approved** | Architecture and Product Owner | Release the Epic, the Feature and each item. The AI never approves its own items. |
| **Committed** | Developers + AI (`implement`) | The AI implements test-first, on its own branch. |
| **Paused** | Developers | The AI stopped waiting for an answer; it resumes by itself when the answer arrives. |
| **Blocked** | Developers | The item depends on another that isn't finished yet. |
| **Done** | QA | Tests passing, code review in its own task, defects as linked Bugs. |
| **Closed** | Architecture and Product Owner | Closing is always a person's decision. |

Every stage writes on the item itself **what was done, when, with which
model, at what effort and how many tokens**. At the end, the Feature adds
up time and tokens for each phase.

## 3. How it works, step by step

```
 People (decide)                            AI (executes and records)
 ───────────────                            ─────────────────────────
 1. Fill in the Epic and the Feature
    using the smart fields
 2. Click "Create AI files" ──────────────▶ files in the repository
                                            (commit or pull request)
 3. Approve the Feature (Approved)
                                            4. briefing: reads Epic and
                                               Feature, shows the next step
                                            5. bootstrap: generates
                                               CONTEXT.md and the stack
                                               from the Epic
                                            6. interview (when the Feature
                                               asks for it) and slicing
                                               into PBIs
 7. Confirm the slicing (Approved)
                                            8. implement: test-first, own
                                               branch, Committed; pauses
                                               when a decision is needed
 9. Answer questions (Paused → resumes)
                                           10. code review, Bugs, summary;
                                               item to Done
11. Close the Feature and the Epic (Closed)
```

## 4. Examples

### 4.1 The same Epic, written two ways

| Free text | With ParSoM Smart Fields |
|---|---|
| "Customer registration web system, maybe with AI for search. Use the usual stack. Standard security. PRs to the development branch." | **Tipo de Solução** (Solution Type): 01.01.04 - Aplicação Web (Web Application) · **Plataforma de Frontend** (Frontend Platform): 04.01.09 - Web SPA · **Framework**: 04.02.17 - React · **Branch Alvo dos Pull Requests** (Pull Request Target Branch): `develop` · **Uso de IA** (AI Usage): RAG (which makes the knowledge-base fields required) · … |

The AI reads answers, not interpretations.

### 4.2 A catalog rule

Applicability rule (file
`Catalogos/Epic_2.17.0/03-parsom-applicability-rules.json`): an API-only
solution has no screens, so the Frontend group disappears and the backend
fields become required.

```json
{
  "ruleId": "APP-001",
  "name": "API ou Backend sem Frontend",
  "mode": "hard",
  "when": { "all": [
    { "field": "Custom.SolutionType", "operator": "equals",
      "value": "01.01.01 - API / Backend" }
  ]},
  "actions": [
    { "type": "disableGroup", "group": "G04" },
    { "type": "clearGroup",   "group": "G04" },
    { "type": "requireField", "field": "Custom.BackendPlatform" },
    { "type": "requireField", "field": "Custom.ApiStyle" }
  ],
  "reason": "Uma solução definida como API/Backend não necessita dos campos de Frontend."
}
```

(`name`: "API or Backend without Frontend"; `reason`: "A solution defined
as API/Backend doesn't need the Frontend fields.")

Cascading rule (`02-parsom-cascading-rules.json`): the chosen platform
limits the frameworks offered.

```json
{
  "ruleId": "CAS-001",
  "parentField": "Custom.FrontendPlatform",
  "childField": "Custom.FrontendFramework",
  "mappings": {
    "04.01.09 - Web SPA": ["04.02.17 - React", "04.02.03 - Angular", "04.02.23 - Vue", "04.02.20 - Svelte"],
    "04.01.08 - Web .NET": ["04.02.08 - Blazor WebAssembly", "04.02.16 - Razor Pages", "04.02.05 - ASP.NET Core MVC"]
  }
}
```

### 4.3 A work session with the AI

After the button has written the files and the person has cloned the
repository:

```powershell
# Claude Code: opens the session with the right model and effort for the stage
.\scripts\iniciar-sessao.ps1 -Modelo sonnet -Esforco high

# OpenAI Codex
.\scripts\iniciar-sessao.ps1 -Modelo sol -Esforco high
```

(`iniciar-sessao` = "start session"; `-Modelo` = model, `-Esforco` =
effort.)

On opening, a hook runs `scripts/session-start.py` and shows the briefing:
connection to Azure DevOps, target branch, Feature and PBIs, whether the
rules server answered, and a "best model per flow stage" board. From then
on, the analyst calls the stages:

| Stage | Claude Code | OpenAI Codex |
|---|---|---|
| Prepare the project | `/project-bootstrap 655` | `$project-bootstrap 655` |
| Up-front interview | `/grill-with-docs 655` | `$mattpocock-skills:grill-with-docs 655` |
| Slice the Feature into PBIs | `/to-tickets 655` | `$mattpocock-skills:to-tickets 655` |
| Implement a PBI | `/implement 660` | `$mattpocock-skills:implement 660` |

### 4.4 What gets recorded on the item

Each stage writes a standard comment in the work item's discussion. For
example, when an implementation starts (the labels are in Portuguese in
the product; translations on the right):

```
[ IA ] <DD/MM/YYYY> - <HH:MM>
Start Implement
Modelo: <name> (<model id>)          model, e.g. GPT-6.2 Sol (gpt-6.2-sol)
Effort: <value>/100
Ponytail: full
Modelo divergente: ...               model mismatch; only when the session
                                     differs from the recommended one
Sessao: <session-id>                 session
```

And at the end of the Feature, the summary of time and tokens per phase
(slicing, implementation, pauses, code review) goes on the Feature itself.

## 5. Running the project

The project has three parts that run separately: **the extension**
(TypeScript, runs inside Azure DevOps), **the rules server** (ASP.NET Core
+ SQL Server) and **the AI set scripts** (Python, run on the developer's
machine).

### 5.1 Prerequisites

| For | You need |
|---|---|
| Extension | Node.js 18 or newer (tested on 24) and npm |
| Rules server | .NET 10 SDK and SQL Server (tested on 2025) |
| Set scripts | Python 3.11 or newer, Git, Azure CLI (`az`) with the `azure-devops` extension |
| Real use | An Azure DevOps Services (cloud) organization with an inherited Scrum process |

### 5.2 The extension: tests, build and package

```bash
npm ci                 # install dependencies
npm test               # run the tests (Vitest)
npm run lint           # ESLint
npm run build:release  # build with webpack into dist/
npx --yes tfx-cli extension create --manifest-globs Azure-DevOps-Fields.json
                       # produces ParSoMProject.azure-devops-fields-<version>.vsix
```

For development with automatic reload:

```bash
npm run start          # dev server at https://localhost:44300
npm run build-dev      # development build
```

> Every published package needs a new version number in
> `Azure-DevOps-Fields.json` and `package.json`: Azure DevOps caches the
> files of a version that was already published.

### 5.3 Installing in an organization

1. **Process**: in *Organization settings → Process*, create a process
   inherited from **Scrum** and add the states the flow uses: Epic and
   Feature with **Approved** and **Closed**; Product Backlog Item with
   **Paused** and **Blocked**.
2. **Extension**: the extension is **private** and checks the
   organization's license against the ParSoM server. To evaluate it, ask
   for it to be shared by writing to **contato@parsomsolucoes.com.br** with
   the organization's name; ParSoM shares the extension and enables the
   license. (To publish your own copy, change the `publisher` in
   `Azure-DevOps-Fields.json` and upload the `.vsix` at
   https://marketplace.visualstudio.com/manage as a private extension.)
3. **Catalogs**: in *Organization settings*, under *Extensions*, open
   **ParSoM Smart Fields**. Choose the process and the **Epic** type, the
   *Import / Export* tab, *Choose file* →
   `Catalogos/Epic_2.17.0/04-parsom-master-config.json`. Check the preview
   (nothing is created before the next click), click *Create fields and
   load rules* and save. Repeat with the **Feature** type and
   `Catalogos/Feature_1.2.0/06-parsom-feature-config.json` (the preview
   shows 2 pages, 10 groups, 40 fields and 34 rules).
4. **Use**: create an Epic, fill in the ParSoM pages, save, and use
   **"…" → Create AI files in the repository**.

The full walkthrough, with every screen, is in the manual at
**https://parsomproject.com.br/manual/** (Portuguese, English and
Spanish). In the repository:
[`site/manual/index.html`](site/manual/index.html) and the PDF
[`Manual/Manual-ParSoM-Smart-Fields.pdf`](Manual/Manual-ParSoM-Smart-Fields.pdf)
(Portuguese).

### 5.4 The rules server, locally

```bash
# 1. Database
sqlcmd -S "localhost\MSSQLSERVER2025" -E -C -Q "IF DB_ID('ParsomRegras') IS NULL CREATE DATABASE ParsomRegras"
sqlcmd -S "localhost\MSSQLSERVER2025" -E -C -d ParsomRegras -i servidor-mcp/sql/01-esquema.sql
sqlcmd -S "localhost\MSSQLSERVER2025" -E -C -d ParsomRegras -x -f 65001 -i servidor-mcp/sql/02-carga-claude-code.sql

# 2. Service, with the license check turned off (Development environment only)
cd servidor-mcp/ParsomMcp
ASPNETCORE_ENVIRONMENT=Development Parsom__DesligarConferencia=true \
ASPNETCORE_URLS=http://localhost:5181 dotnet run --no-launch-profile
```

To check:

```bash
curl http://localhost:5181/situacao          # status: {"ok":true,"conjunto":"claude-code","versao":"..."}
curl http://localhost:5181/regras-iniciais   # the "inicio" (start) document as text
```

The MCP endpoint is `http://localhost:5181/mcp`, with the tools
`parsom_indice` (index) and `parsom_documento(nome)` (document by name).
Details, hosting installation and how to change a rule (in Portuguese):
[`servidor-mcp/README.md`](servidor-mcp/README.md).

### 5.5 The set scripts, without Azure DevOps

Some scripts have a self-test and run offline:

```bash
cd servidor-mcp/conjunto/codex
python scripts/exige-quadro-etapas.py --autoteste   # prints "ok"
python scripts/atividade.py --autoteste
python scripts/session-start.py --etapas --sem-azure # model-per-stage board
```

## 6. Repository structure

| Folder | Contents |
|---|---|
| `src/observer/` | Watches the work item form: cascades, applicability rules, save messages |
| `src/multiselect/` | The multi-select control |
| `src/fieldhelp/` | The help window behind each field's "?" |
| `src/confighub/` | The configuration screen: catalog import, rule editor, history, export |
| `src/iarepo/` | "Create AI files in the repository" and "Copy AI guidance data" |
| `src/common/` | The pure logic (rules, cascades, applicability, license) and the tests |
| `Catalogos/` | The Epic (2.17.0) and Feature (1.2.0) catalogs |
| `servidor-mcp/ParsomMcp/` | The rules server (ASP.NET Core, MCP) |
| `servidor-mcp/conteudo/<set>/` | The source of the served rules, one `.md` per document |
| `servidor-mcp/conjunto/<set>/` | The thin set the button writes to the repository |
| `servidor-mcp/sql/` | Database schema and the loads generated from the rules |
| `licensing/` | ParSoM website pages: license list and delivery of the sets |
| `marketplace/` | Marketplace texts: description, privacy, license |
| `Manual/`, `site/` | The PDF manual and the website with the manual in three languages |
| `Azure-DevOps-Fields.json` | The extension manifest |

## 7. Technologies

- **Extension**: TypeScript, React 16, `azure-devops-extension-sdk`,
  `azure-devops-ui`, webpack, Vitest.
- **Rules server**: .NET 10, ASP.NET Core,
  `ModelContextProtocol.AspNetCore`, SQL Server 2025.
- **AI sets**: Python (session start and end hooks, token measurement,
  state locks), PowerShell and Bash to open the session.
- **Supported AIs**: Claude Code (Anthropic), OpenAI Codex, Devin and any
  tool that speaks MCP.
- **Flow skills**: [mattpocock/skills](https://github.com/mattpocock/skills)
  (MIT), installed as a plugin in each tool.

## 8. Security and privacy

- **The AI can only do what the person using it can do**: it signs in
  with that person's own personal access token, with access to the board,
  the code and project read access.
- **Data stays in the customer's organization**: rules, configuration and
  history stay in the customer's Azure DevOps. Item content, code and
  identities never go to ParSoM.
- **Rules server**: on every call it checks that the organization is
  licensed and that the token belongs to a person in that organization;
  the token is never stored (only a SHA-256 fingerprint in memory, for 10
  minutes). Every read is logged with the person's Azure DevOps id.
- **Locks**: hooks refuse the command when the AI tries to move a PBI to
  Done without the code review task completed, without the implementation
  summary or with an open defect; they refuse a fix without a recorded
  Bug and a comment with the wrong time; and they require the end-of-stage
  board.

Details: [`marketplace/privacy.md`](marketplace/privacy.md).

## 9. Known limitations

- **Azure DevOps Services** (cloud) only; the on-premises server doesn't
  have the required API.
- The extension's screens are in English; the catalog and the AI files are
  in Portuguese.
- Rules compare fields of the same item: the Feature's rules don't read
  the parent Epic.
- Long-text fields are edited as plain text.
- The AI sets expect an inherited Scrum process with the extra states
  described in 5.3.

## 10. License and credits

The extension's code started as a fork of Microsoft's
[Cascading Picklists](https://github.com/microsoft/azure-devops-extension-cascading-picklist)
extension (MIT license); see [`LICENSE`](LICENSE). The catalogs, the
rules-server rules and the AI sets belong to ParSoM; the extension's terms
of use are in [`marketplace/license.md`](marketplace/license.md).

**More details**: https://parsomproject.com.br ·
**Contact**: contato@parsomsolucoes.com.br
