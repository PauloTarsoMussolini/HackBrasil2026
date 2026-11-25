# ParSoM Smart Fields — Scrum com IA no Azure DevOps

**Português** · [English](README.en.md)

**Desenvolvimento com IA, sob gestão e controle.** Uma extensão do Azure
DevOps que transforma o Epic e a Feature em formulários inteligentes e, com
um clique, prepara o repositório para que uma IA de programação (Claude
Code, OpenAI Codex, Devin ou outra) trabalhe **dentro do Scrum que a equipe
já usa**: lendo decisões estruturadas, registrando cada passo no quadro e
parando quando precisa de uma pessoa.

> **Mais detalhes em https://parsomproject.com.br** (português, inglês e
> espanhol): o papel de cada pessoa do time, as páginas que a equipe
> preenche, a ajuda de cada campo, os arquivos de IA, o quadro e as
> garantias de controle e auditoria, com as telas reais do produto.
>
> - **Manual de instalação, passo a passo:** https://parsomproject.com.br/manual/
> - **Manual em PDF:** https://parsomproject.com.br/manual/Manual-ParSoM-Smart-Fields.pdf

![Um Epic com as páginas ParSoM](images/marketplace/epic-form.jpg)

---

## Sumário

1. [O problema](#1-o-problema)
2. [A solução](#2-a-solução)
3. [Como funciona, passo a passo](#3-como-funciona-passo-a-passo)
4. [Exemplos](#4-exemplos)
5. [Como executar o projeto](#5-como-executar-o-projeto)
6. [Estrutura do repositório](#6-estrutura-do-repositório)
7. [Tecnologias](#7-tecnologias)
8. [Segurança e privacidade](#8-segurança-e-privacidade)
9. [Limitações conhecidas](#9-limitações-conhecidas)
10. [Licença e créditos](#10-licença-e-créditos)

---

## 1. O problema

As IAs de programação já escrevem código bem. O que falha é **o que vem
antes e em volta do código**:

- **A IA recebe interpretações, não decisões.** Um Epic típico chega assim:

  > "Sistema web de cadastro de clientes, talvez com IA para busca. Usar a
  > stack de sempre. Segurança padrão. PRs para a branch de desenvolvimento."

  Qual stack? Qual branch? Que "segurança padrão"? Cada sessão de IA
  preenche essas lacunas de um jeito, e cada desenvolvedor corrige de outro.

- **A IA trabalha fora do processo.** Ela cria código numa conversa
  privada; o gestor não sabe o que foi feito, por qual modelo, com quanto
  custo, nem quem aprovou. O quadro do Scrum deixa de contar a verdade.

- **Não há controle nem auditoria.** Itens são "concluídos" sem testes,
  sem revisão de código, sem registro de defeitos. Não dá para medir tempo
  e tokens por fase, nem para melhorar o processo.

- **O Azure DevOps não ajuda a preencher bem.** Não há campo de múltipla
  escolha, não há listas que dependem de outras, não há regras do tipo
  "se a solução é só API, os campos de tela não se aplicam". Quem preenche
  precisa decorar o que cada campo significa.

## 2. A solução

O ParSoM Smart Fields ataca o problema em três camadas, todas dentro do
Azure DevOps que a empresa já tem.

### 2.1 Campos inteligentes no Epic e na Feature

- **Catálogo pronto**: 84 campos no Epic (em 4 páginas: visão e
  governança; aplicação, integração e IA; dados, plataforma e FinOps;
  segurança, qualidade e operação) e 40 na Feature (escopo e experiência;
  construção e entrega), ligados por **249 regras**.
- **Listas em cascata**: um campo oferece só os valores que combinam com
  outro (escolheu "Web SPA"? o framework oferece React, Angular, Vue,
  Svelte).
- **Múltipla escolha**: caixas de seleção desenhadas pela extensão, que o
  Azure DevOps não tem.
- **Regras de aplicabilidade**: tornam campos obrigatórios, escondem grupos
  inteiros ou recomendam valores, conforme as respostas.
- **Ajuda em cada campo**: o "?" explica o que o campo significa e, gerado
  das próprias regras, o que cada resposta provoca no resto do formulário.
- **Instalação em um passo**: a tela de configuração lê o catálogo, mostra
  exatamente o que vai criar e cria campos, páginas, grupos e regras no
  processo herdado da organização.

### 2.2 Um clique: o repositório aprende como a equipe trabalha

No Epic, **"…" → Create AI files in the repository** grava no repositório
do projeto um conjunto de arquivos que conduzem a IA pelo processo. Há
quatro conjuntos: **Anthropic Claude**, **OpenAI Codex**, **Devin** e
**Outras IAs** (genérico).

- Uma **prévia** lista cada arquivo antes de gravar.
- **Nenhum arquivo existente é substituído.**
- Repositório vazio: os arquivos viram o primeiro commit. Com conteúdo:
  branch nova e pull request, ligados ao Epic.

![Escolha do conjunto de arquivos de IA](images/marketplace/ai-files.jpg)

### 2.3 O servidor de regras ParSoM

As regras detalhadas de cada etapa (entrevista, fatiamento, implementação
com testes primeiro, revisão de código, defeitos, fechamento) **não ficam
no repositório do cliente**. Elas são servidas à IA, um documento por vez,
por um servidor **MCP** (Model Context Protocol), só para pessoas
confirmadas de organizações licenciadas. O repositório recebe um conjunto
enxuto: a entrada da IA, a configuração do projeto, os ganchos (hooks) e os
scripts que medem, travam e registram.

### 2.4 O resultado: o quadro conta o que a IA está fazendo

| Estado | Quem age | O que acontece |
|---|---|---|
| **Approved** | Arquitetura e Product Owner | Liberam Epic, Feature e cada item. A IA nunca aprova os próprios itens. |
| **Committed** | Desenvolvedores + IA (`implement`) | A IA implementa com testes primeiro, numa branch própria. |
| **Paused** | Desenvolvedores | A IA parou esperando uma resposta; volta sozinha quando ela chega. |
| **Blocked** | Desenvolvedores | O item depende de outro que ainda não terminou. |
| **Done** | QA | Testes passando, revisão de código numa tarefa própria, defeitos como Bugs ligados. |
| **Closed** | Arquitetura e Product Owner | Fechar é sempre decisão de uma pessoa. |

Cada etapa grava no próprio item **o que foi feito, quando, com qual
modelo, com qual esforço e quantos tokens**. Ao fim, a Feature soma tempo e
tokens de cada fase.

## 3. Como funciona, passo a passo

```
 Pessoas (decidem)                          IA (executa e registra)
 ─────────────────                          ───────────────────────
 1. Preenchem o Epic e a Feature
    nos campos inteligentes
 2. Clicam "Create AI files" ─────────────▶ arquivos no repositório
                                            (commit ou pull request)
 3. Aprovam a Feature (Approved)
                                            4. briefing: lê Epic e Feature,
                                               mostra o próximo passo
                                            5. bootstrap: gera CONTEXT.md
                                               e a stack a partir do Epic
                                            6. entrevista (quando a Feature
                                               pede) e fatiamento em PBIs
 7. Confirmam o fatiamento (Approved)
                                            8. implement: testes primeiro,
                                               branch própria, Committed;
                                               pausa quando precisa decidir
 9. Respondem perguntas (Paused → volta)
                                           10. revisão de código, Bugs,
                                               resumo; item em Done
11. Fecham a Feature e o Epic (Closed)
```

## 4. Exemplos

### 4.1 O mesmo Epic, escrito de duas maneiras

| Texto livre | Com ParSoM Smart Fields |
|---|---|
| "Sistema web de cadastro de clientes, talvez com IA para busca. Usar a stack de sempre. Segurança padrão. PRs para a branch de desenvolvimento." | **Tipo de Solução**: 01.01.04 - Aplicação Web · **Plataforma de Frontend**: 04.01.09 - Web SPA · **Framework**: 04.02.17 - React · **Branch Alvo dos Pull Requests**: `develop` · **Uso de IA**: RAG (o que torna obrigatórios os campos de base de conhecimento) · … |

A IA lê respostas, e não interpretações.

### 4.2 Uma regra do catálogo

Regra de aplicabilidade (arquivo
`Catalogos/Epic_2.17.0/03-parsom-applicability-rules.json`): uma solução que
é só API não tem tela, então o grupo de Frontend some e os campos de
backend passam a ser obrigatórios.

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

Regra em cascata (`02-parsom-cascading-rules.json`): a plataforma escolhida
limita os frameworks oferecidos.

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

### 4.3 Uma sessão de trabalho com a IA

Depois que o botão gravou os arquivos e a pessoa clonou o repositório:

```powershell
# Claude Code: abre a sessão com o modelo e o esforço certos para a etapa
.\scripts\iniciar-sessao.ps1 -Modelo sonnet -Esforco high

# OpenAI Codex
.\scripts\iniciar-sessao.ps1 -Modelo sol -Esforco high
```

Ao abrir, um gancho (hook) roda `scripts/session-start.py` e mostra o
briefing: conexão com o Azure DevOps, branch alvo, Feature e PBIs, se o
servidor de regras respondeu, e um quadro "Modelo ideal por etapa do
fluxo". Daí em diante, o analista chama as etapas:

| Etapa | Claude Code | OpenAI Codex |
|---|---|---|
| Preparar o projeto | `/project-bootstrap 655` | `$project-bootstrap 655` |
| Entrevista prévia | `/grill-with-docs 655` | `$mattpocock-skills:grill-with-docs 655` |
| Fatiar a Feature em PBIs | `/to-tickets 655` | `$mattpocock-skills:to-tickets 655` |
| Implementar uma PBI | `/implement 660` | `$mattpocock-skills:implement 660` |

### 4.4 O que fica registrado no item

Cada etapa escreve um comentário padronizado na discussão do work item,
por exemplo, ao começar uma implementação:

```
[ IA ] <DD/MM/AAAA> - <HH:MM>
Start Implement
Modelo: <nome> (<model id>)          ex.: GPT-6.2 Sol (gpt-6.2-sol)
Effort: <valor>/100
Ponytail: full
Modelo divergente: ...               só quando a sessão foge do recomendado
Sessao: <session-id>
```

E, ao fim da Feature, o resumo de tempo e tokens por fase (fatiamento,
implementação, pausas, revisão de código) vai para a própria Feature.

## 5. Como executar o projeto

O projeto tem três partes que rodam separadas: **a extensão** (TypeScript,
roda dentro do Azure DevOps), **o servidor de regras** (ASP.NET Core +
SQL Server) e **os scripts dos conjuntos de IA** (Python, rodam na máquina
de quem programa).

### 5.1 Pré-requisitos

| Para | Precisa de |
|---|---|
| Extensão | Node.js 18 ou mais novo (testado no 24) e npm |
| Servidor de regras | .NET 10 SDK e SQL Server (testado no 2025) |
| Scripts dos conjuntos | Python 3.11 ou mais novo, Git, Azure CLI (`az`) com a extensão `azure-devops` |
| Usar de verdade | Uma organização Azure DevOps Services (nuvem) com um processo herdado de Scrum |

### 5.2 A extensão: testes, compilação e pacote

```bash
npm ci                 # instala as dependências
npm test               # roda os testes (Vitest)
npm run lint           # ESLint
npm run build:release  # compila com webpack para dist/
npx --yes tfx-cli extension create --manifest-globs Azure-DevOps-Fields.json
                       # gera ParSoMProject.azure-devops-fields-<versão>.vsix
```

Para desenvolver com recarga automática:

```bash
npm run start          # servidor de desenvolvimento em https://localhost:44300
npm run build-dev      # compilação de desenvolvimento
```

> Cada pacote publicado precisa de um número de versão novo em
> `Azure-DevOps-Fields.json` e `package.json`: o Azure DevOps guarda em
> cache os arquivos de uma versão já publicada.

### 5.3 Instalar numa organização

1. **Processo**: em *Organization settings → Process*, crie um processo
   herdado de **Scrum** e acrescente os estados que o fluxo usa: Epic e
   Feature com **Approved** e **Closed**; Product Backlog Item com
   **Paused** e **Blocked**.
2. **Extensão**: a extensão é **privada** e verifica a licença da
   organização no servidor da ParSoM. Para avaliar, peça o
   compartilhamento em **contato@parsomsolucoes.com.br** informando o nome
   da organização; a ParSoM compartilha a extensão e libera a licença. (Para
   publicar a sua própria cópia, troque o `publisher` em
   `Azure-DevOps-Fields.json` e envie o `.vsix` em
   https://marketplace.visualstudio.com/manage como extensão privada.)
3. **Catálogos**: em *Organization settings*, no grupo *Extensions*, abra
   **ParSoM Smart Fields**. Escolha o processo e o tipo **Epic**, aba
   *Import / Export*, *Choose file* →
   `Catalogos/Epic_2.17.0/04-parsom-master-config.json`. Confira a prévia
   (nada é criado antes do próximo clique), clique em *Create fields and
   load rules* e salve. Repita com o tipo **Feature** e
   `Catalogos/Feature_1.2.0/06-parsom-feature-config.json` (a prévia
   mostra 2 páginas, 10 grupos, 40 campos e 34 regras).
4. **Usar**: crie um Epic, preencha as páginas ParSoM, salve, e use
   **"…" → Create AI files in the repository**.

O passo a passo completo, com cada tela, está no manual, em
**https://parsomproject.com.br/manual/** (português, inglês e espanhol).
No repositório: [`site/manual/index.html`](site/manual/index.html) e o PDF
[`Manual/Manual-ParSoM-Smart-Fields.pdf`](Manual/Manual-ParSoM-Smart-Fields.pdf)
(português).

### 5.4 O servidor de regras, localmente

```bash
# 1. Banco
sqlcmd -S "localhost\MSSQLSERVER2025" -E -C -Q "IF DB_ID('ParsomRegras') IS NULL CREATE DATABASE ParsomRegras"
sqlcmd -S "localhost\MSSQLSERVER2025" -E -C -d ParsomRegras -i servidor-mcp/sql/01-esquema.sql
sqlcmd -S "localhost\MSSQLSERVER2025" -E -C -d ParsomRegras -x -f 65001 -i servidor-mcp/sql/02-carga-claude-code.sql

# 2. Serviço, com a conferência de licença desligada (só no ambiente Development)
cd servidor-mcp/ParsomMcp
ASPNETCORE_ENVIRONMENT=Development Parsom__DesligarConferencia=true \
ASPNETCORE_URLS=http://localhost:5181 dotnet run --no-launch-profile
```

Para conferir:

```bash
curl http://localhost:5181/situacao          # {"ok":true,"conjunto":"claude-code","versao":"..."}
curl http://localhost:5181/regras-iniciais   # o documento "inicio" em texto
```

O endereço MCP é `http://localhost:5181/mcp`, com as ferramentas
`parsom_indice` e `parsom_documento(nome)`. Detalhes, instalação na
hospedagem e como mudar uma regra: [`servidor-mcp/README.md`](servidor-mcp/README.md).

### 5.5 Os scripts dos conjuntos, sem Azure DevOps

Alguns scripts têm autoteste e rodam sem conexão:

```bash
cd servidor-mcp/conjunto/codex
python scripts/exige-quadro-etapas.py --autoteste   # imprime "ok"
python scripts/atividade.py --autoteste
python scripts/session-start.py --etapas --sem-azure # quadro de modelos por etapa
```

## 6. Estrutura do repositório

| Pasta | O que tem |
|---|---|
| `src/observer/` | Observa o formulário do work item: cascatas, regras de aplicabilidade, mensagens de salvamento |
| `src/multiselect/` | O controle de múltipla escolha |
| `src/fieldhelp/` | A janela de ajuda do "?" de cada campo |
| `src/confighub/` | A tela de configuração: importação de catálogos, editor de regras, histórico, exportação |
| `src/iarepo/` | "Create AI files in the repository" e "Copy AI guidance data" |
| `src/common/` | A lógica pura (regras, cascatas, aplicabilidade, licença) e os testes |
| `Catalogos/` | Os catálogos do Epic (2.17.0) e da Feature (1.2.0) |
| `servidor-mcp/ParsomMcp/` | O servidor de regras (ASP.NET Core, MCP) |
| `servidor-mcp/conteudo/<conjunto>/` | A fonte das regras servidas, um `.md` por documento |
| `servidor-mcp/conjunto/<conjunto>/` | O conjunto enxuto que o botão grava no repositório |
| `servidor-mcp/sql/` | Esquema do banco e as cargas geradas das regras |
| `licensing/` | Páginas do site da ParSoM: lista de licenças e entrega dos conjuntos |
| `marketplace/` | Textos da Marketplace: descrição, privacidade, licença |
| `Manual/`, `site/` | Manual em PDF e o site com o manual em três idiomas |
| `Azure-DevOps-Fields.json` | O manifesto da extensão |

## 7. Tecnologias

- **Extensão**: TypeScript, React 16, `azure-devops-extension-sdk`,
  `azure-devops-ui`, webpack, Vitest.
- **Servidor de regras**: .NET 10, ASP.NET Core,
  `ModelContextProtocol.AspNetCore`, SQL Server 2025.
- **Conjuntos de IA**: Python (ganchos de início e fim de sessão, medição de
  tokens, travas de estado), PowerShell e Bash para abrir a sessão.
- **IAs suportadas**: Claude Code (Anthropic), OpenAI Codex, Devin e
  qualquer ferramenta que fale MCP.
- **Habilidades de fluxo**: [mattpocock/skills](https://github.com/mattpocock/skills)
  (MIT), instaladas como plugin em cada ferramenta.

## 8. Segurança e privacidade

- **A IA só faz o que a pessoa que a usa pode fazer**: ela entra com o
  token pessoal da própria pessoa, com acesso ao quadro, ao código e à
  leitura do projeto.
- **Os dados ficam na organização do cliente**: regras, configuração e
  histórico ficam no Azure DevOps do cliente. Conteúdo dos itens, código e
  identidades nunca vão para a ParSoM.
- **Servidor de regras**: confere em cada chamada que a organização está
  licenciada e que o token pertence a uma pessoa daquela organização; o
  token nunca é guardado (só uma impressão SHA-256 em memória, por 10
  minutos). Cada leitura fica registrada com o id da pessoa no Azure DevOps.
- **Travas**: ganchos recusam o comando quando a IA tenta mover uma PBI
  para Done sem a tarefa de revisão de código concluída, sem o resumo da
  implementação ou com defeito em aberto; recusam correção sem Bug
  registrado e comentário com a hora errada; e cobram o quadro de fim de
  etapa.

Detalhes: [`marketplace/privacy.md`](marketplace/privacy.md).

## 9. Limitações conhecidas

- Só **Azure DevOps Services** (nuvem); o servidor local (on-premises) não
  tem a API necessária.
- As telas da extensão estão em inglês; o catálogo e os arquivos de IA, em
  português.
- As regras comparam campos do mesmo item: as regras da Feature não leem o
  Epic pai.
- Campos de texto longo são editados como texto simples.
- Os conjuntos de IA esperam um processo herdado de Scrum com os estados
  extras descritos em 5.3.

## 10. Licença e créditos

O código da extensão nasceu de um fork da extensão
[Cascading Picklists](https://github.com/microsoft/azure-devops-extension-cascading-picklist)
da Microsoft (licença MIT); veja [`LICENSE`](LICENSE). Os catálogos, as
regras do servidor e os conjuntos de IA são da ParSoM; os termos de uso da
extensão estão em [`marketplace/license.md`](marketplace/license.md).

**Mais detalhes**: https://parsomproject.com.br ·
**Contato**: contato@parsomsolucoes.com.br
