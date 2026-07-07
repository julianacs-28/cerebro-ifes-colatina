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
| 3 | Criar o JSON da edição | `edicoes/ed-XX/dados.json` (+ `imagens/` se houver) |
| 4 | Importar no editor | `radar_sistema_dppge.html` → **Importar JSON** ou **Nova Edição** |
| 5 | Editar e salvar | `radar_sistema_dppge.html` → **Exportar JSON** sobrescreve o `dados.json` da edição |
| 6 | Enviar o e-mail | **Gerar para Outlook** → nova aba → `Ctrl+A` → `Ctrl+C` → colar no Outlook |
| 7 | Atualizar o site | `python3 scripts/merge_edicao.py edicoes/ed-XX` |
| 8 | Publicar | `git commit` + `git push` → GitHub Pages atualiza em ~1 min |

### Detalhes de cada passo

**1. Pesquisar** — levantar editais, chamadas, bolsas e eventos abertos *na data de hoje*. Checar sempre o prazo real de inscrição, não a data de publicação do edital.

**2. Curadoria** — filtrar pelo que é relevante para o campus e ainda está com prazo aberto. Categorias disponíveis: Pesquisa & Fomento, Internacionalização, Bolsas & Auxílios, Eventos & Capacitações, Premiações, Extensão & Comunidade.

**3. Criar o JSON** — uma pasta nova por edição (`edicoes/ed-XX/`). Imagens exclusivas daquela notícia ficam em `edicoes/ed-XX/imagens/`, referenciadas com caminho relativo à raiz de `radar-oportunidades/` (ex: `edicoes/ed-03/imagens/destaque.jpg`) para funcionar igual no editor e no site.

**4. Importar no editor** — botão **📥 Importar JSON** carrega o rascunho da edição, ou **🆕 Nova Edição** começa do zero já com o próximo número sugerido. A prévia do boletim atualiza em tempo real.

**5. Editar e salvar** — ajustar Destaque, Categorias e Informes nas abas do editor. **💾 Salvar Edição** guarda um rascunho no histórico do navegador; **⬇ Exportar JSON** baixa o arquivo final.

**6. Enviar o e-mail** — **🚀 Gerar para Outlook** abre o boletim pronto em nova aba. Nessa aba: `Ctrl+A`, `Ctrl+C`, colar no corpo de um e-mail novo no Outlook (nunca colar o código-fonte) e enviar.

**7. Atualizar o site** — funde a edição no acumulado público, preservando tudo que já foi divulgado antes (sem duplicar, sem perder a data de cadastro original de cada item):
```bash
cd radar-oportunidades
python3 scripts/merge_edicao.py edicoes/ed-02
```

**8. Publicar** — commit + push para o GitHub. O GitHub Pages redeploya sozinho.

---

## Referência rápida

**Status automático do site** (dois eixos independentes):
- **Novo** = inserido no `banco.json` há ≤ 20 dias
- Urgência do prazo: **Prazo próximo** (>20 dias) · **Últimos dias** (≤20 dias) · **Fluxo contínuo** (sem prazo fixo)

**Categorias:** `pesquisa` · `internac` · `bolsas` · `evento` · `premiacoes` · `extensao`
