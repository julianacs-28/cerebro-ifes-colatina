# Radar de Oportunidades — DPPGE · Ifes Campus Colatina

Guia de referência do fluxo de trabalho: da pesquisa de editais na web até a publicação no site. Para detalhes técnicos (design system, estrutura de JSON, regras de compatibilidade com Outlook), ver [CLAUDE.md](CLAUDE.md).

**Site público:** https://julianacs-28.github.io/cerebro-ifes-colatina/radar-oportunidades/
**Sistema interno (editor):** https://julianacs-28.github.io/cerebro-ifes-colatina/radar-oportunidades/radar_sistema_dppge.html

---

## Mapa de pastas

```
radar-oportunidades/
├── index.html                  # site público — lê banco.json
├── banco.json                  # TODAS as oportunidades já publicadas (nunca editar à mão)
├── radar_sistema_dppge.html    # editor do boletim
├── edicoes/
│   ├── ed-01/
│   │   ├── dados.json          # conteúdo desta edição
│   │   └── imagens/            # imagens exclusivas da edição
│   └── ed-02/  ...
└── scripts/
    └── merge_edicao.py         # funde a edição no banco.json
```

`banco.json` ≠ `edicoes/ed-XX/dados.json`. O primeiro é o acumulado público e cumulativo do site; o segundo é o conteúdo isolado de uma edição do boletim (o que foi enviado por e-mail naquela ocasião).

---

## Fluxo de trabalho

| # | Passo | Onde |
|---|---|---|
| 1 | Pesquisar oportunidades na web | Fapes · CNPq · CAPES · Ifes/PRPPG · INPE · SBPC |
| 2 | Fazer a curadoria — o que entra, em qual categoria | Decisão editorial |
| 3 | Criar a pasta da edição e o JSON com o resultado da pesquisa | `edicoes/ed-XX/dados.json` (+ `imagens/` se houver) |
| 4 | Carregar no editor | `radar_sistema_dppge.html` → **Importar JSON** (ou **Nova Edição**, se for preencher na mão) |
| 5 | Revisar e exportar | `radar_sistema_dppge.html` → **Exportar JSON** baixa o arquivo final |
| 6 | Enviar o e-mail | **Gerar para Outlook** → nova aba → `Ctrl+A` → `Ctrl+C` → colar no Outlook |
| 7 | Atualizar o site | `python3 scripts/merge_edicao.py edicoes/ed-XX` |
| 8 | Publicar | `git commit` + `git push` → GitHub Pages atualiza em ~1 min |

### Detalhes de cada passo

**1. Pesquisar** — levantar editais, chamadas, bolsas e eventos abertos *na data de hoje*. Checar sempre o prazo real de inscrição, não a data de publicação do edital.

**2. Curadoria** — filtrar pelo que é relevante para o campus e ainda está com prazo aberto. Categorias disponíveis: Pesquisa & Fomento, Internacionalização, Bolsas & Auxílios, Eventos & Capacitações, Premiações, Extensão & Comunidade.

**3. Criar a pasta e o JSON** — a pasta `edicoes/ed-XX/` **não é criada automaticamente por nenhuma ferramenta**: ela é criada uma vez, manualmente (ou pedindo apoio técnico), a cada nova edição. Dentro dela entra o `dados.json` já com o conteúdo pesquisado e curado (pode ser montado direto no formato do JSON a partir da pesquisa — ver "📄 Modelo + Prompt IA" no editor). Imagens exclusivas daquela notícia ficam em `edicoes/ed-XX/imagens/`, referenciadas com caminho relativo à raiz de `radar-oportunidades/` (ex: `edicoes/ed-03/imagens/destaque.jpg`).

**4. Carregar no editor** — duas portas de entrada, conforme o caso:
- **📥 Importar JSON** — quando já existe um `dados.json` pronto (fruto da pesquisa) para a edição *nova*. Carrega tudo de uma vez nos campos do formulário.
- **🆕 Nova Edição** — quando não há JSON pronto e o conteúdo será digitado direto no formulário. Limpa a tela e sugere o próximo número de edição.

⚠️ **Nunca importe o `banco.json` nem o `dados.json` de uma edição *anterior* para começar uma edição nova** — isso traria conteúdo antigo (e o número errado) para dentro da edição atual. Importar serve para carregar o conteúdo *daquela* edição específica, seja para criá-la pela primeira vez ou para retomar um rascunho já iniciado.

**5. Revisar e exportar** — ajustar Destaque, Categorias e Informes nas abas do editor (a prévia atualiza em tempo real). **💾 Salvar Edição** guarda um rascunho no histórico *local do seu navegador* (útil só como rascunho pessoal — ver nota abaixo). **⬇ Exportar JSON** baixa o arquivo final para a pasta Downloads do computador; **isso não sobrescreve nada automaticamente** — o arquivo baixado precisa ser movido/salvo manualmente para `edicoes/ed-XX/dados.json`.

**6. Enviar o e-mail** — **🚀 Gerar para Outlook** abre o boletim pronto em nova aba. Nessa aba: `Ctrl+A`, `Ctrl+C`, colar no corpo de um e-mail novo no Outlook (nunca colar o código-fonte) e enviar.

**7. Atualizar o site** — funde a edição no acumulado público. Esta etapa **sempre soma, nunca substitui**: o conteúdo de edições anteriores permanece intacto no `banco.json`, sem duplicar e sem perder a data de cadastro original de cada item:
```bash
cd radar-oportunidades
python3 scripts/merge_edicao.py edicoes/ed-02
```

**8. Publicar** — commit + push para o GitHub. O GitHub Pages redeploya sozinho.

---

## Dois "históricos" diferentes — não confundir

| | Histórico do editor | Site público |
|---|---|---|
| Onde vive | `localStorage` do navegador (via **💾 Salvar Edição**) | `banco.json`, versionado no git |
| O que guarda | Rascunhos pessoais de edições | Todas as oportunidades já publicadas |
| Persiste entre computadores/pessoas? | Não — só naquele navegador | Sim — o mesmo arquivo para todo mundo |
| É o registro oficial para consulta/dashboard? | Não, é conveniência pessoal | **Sim** |

O dashboard e o histórico que valem como registro institucional são sempre os do **site** (alimentados pelo `banco.json`), não os do editor.

## Perguntas frequentes

**Exportar o JSON traz os dados de todas as edições anteriores?**
Não. Ele exporta só o que está preenchido no formulário no momento — uma edição por vez.

**Importar um JSON antigo por engano apaga alguma coisa?**
Não. "Importar JSON" só preenche os campos na tela; nenhum arquivo é alterado ou apagado. Quem de fato atualiza o `banco.json` é sempre o `scripts/merge_edicao.py`, e ele só soma dados — nunca remove o que já está lá.

**Preciso criar a pasta `edicoes/ed-XX/` toda vez?**
Sim, uma vez por edição — não existe automação para isso ainda. É rápido: basta pedir apoio técnico para criar a pasta e posicionar o `dados.json` nela.

---

## Referência rápida

**Status automático do site** (dois eixos independentes):
- **Novo** = inserido no `banco.json` há ≤ 20 dias
- Urgência do prazo: **Prazo próximo** (>20 dias) · **Últimos dias** (≤20 dias) · **Fluxo contínuo** (sem prazo fixo)

**Categorias:** `pesquisa` · `internac` · `bolsas` · `evento` · `premiacoes` · `extensao`
