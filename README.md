# Ferrabisk Estúdio — Diagnóstico de Marca

Ferramenta do **Ferrabisk Estúdio** para diagnóstico e criação de marcas. Um único arquivo HTML, autônomo, que abre em qualquer navegador — sem instalação e sem servidor obrigatório.

**Arquivo principal:** `diagnostico-marca-ambiente-unificado.html`

Ele reúne **dois diagnósticos** numa tela inicial de escolha:

1. **Marca consolidada** — para quem já tem uma marca. Diagnóstico completo de identidade, ambiente físico e comunicação visual. Funciona **offline**; IA opcional.
2. **Marca nova** — para quem quer criar uma marca do zero e não sabe por onde começar. A **IA** analisa o nicho e sugere direção, paletas, tipografias e estilo.

---

## Arquivos do pacote

| Arquivo | Para quê |
|--------|----------|
| `diagnostico-marca-ambiente-unificado.html` | A ferramenta. É o único arquivo que os usuários abrem. |
| `proxy-cloudflare-worker.js` | Backend opcional (Cloudflare) para usar IA **sem chave por usuário**. |
| `proxy-node-server.js` | Mesmo backend, alternativa em Node.js/Express. |
| `README.md` | Este guia. |

---

## Como usar

1. Baixe o `diagnostico-marca-ambiente-unificado.html`.
2. Dê dois cliques para abrir no navegador (Chrome, Edge, Firefox, Safari).
3. Na tela inicial, escolha **"Já tenho uma marca"** ou **"Quero criar uma marca"**.

Dá para voltar à tela inicial a qualquer momento pelos botões **Início / Novo diagnóstico**. As respostas ficam salvas no navegador, então fechar e voltar não perde o preenchimento.

---

## Modo 1 — Marca consolidada

Formulário em 6 etapas: Identidade, Cores, Essência, Futuro, Ambiente e Comunicação visual. Campos obrigatórios: **nome**, **segmento** e **cores principais**.

Gera um documento com:

- **Resumo** da marca e do estado atual.
- **Análise aprofundada** (identidade, cultura, ambiente/gargalos, comunicação, futuro).
- **Insights de melhoria** priorizados (alta / média / baixa).
- **3 paletas** calculadas por teoria das cores a partir das cores informadas (refinamento análogo, contraste complementar e neutra arquitetônica).

Funciona **100% offline**. Se houver IA integrada (ver abaixo), o texto da análise é **enriquecido automaticamente**; as paletas continuam calculadas localmente.

---

## Modo 2 — Marca nova (do zero)

Formulário curto em 3 etapas: Negócio/nicho, Público/posicionamento e Personalidade/preferências de cor. Campos obrigatórios: **nicho**, **oferta**, **público** e **personalidade**.

Este modo usa **IA** e gera:

- **Direção de marca** — conceito norteador e posicionamento para o nicho.
- **Paletas de cor** sugeridas para o ramo (com HEX, nome e uso; respeita cores que a pessoa gosta/quer evitar).
- **Tipografias sugeridas** — com **prévia ao vivo**: a ferramenta carrega as fontes do Google Fonts e mostra o nome do negócio em cada uma, com link e justificativa.
- **Estilo visual** — palavras-chave de mood e direção de formas, imagens e materiais.
- **Próximos passos** — de 3 a 5 ações práticas.

> Em qualquer paleta, **clique numa amostra para copiar o HEX**.

---

## Inteligência artificial: 3 formas de usar

> **Importante:** a IA sempre precisa de **uma** chave em algum lugar. O que dá para eliminar é a chave **por usuário**.

A configuração fica no topo do HTML, no objeto `AI_CONFIG`:

```js
var AI_CONFIG = {
  proxyUrl: "",           // Opção A: URL do backend publicado
  embeddedProvider: "",   // Opção B: "anthropic" ou "openai"
  embeddedKey: "",        // Opção B: sua chave de API
  embeddedModel: "",      // opcional: ex. "claude-sonnet-4-6" ou "gpt-4o-mini"
  autoEnrich: true        // consolidada: enriquecer com IA quando houver IA integrada
};
```

Prioridade de uso: **proxy → chave embutida → chave manual do usuário**.

### Sem configurar nada (padrão)
A ferramenta pede a chave manualmente, num painel **"Modo IA"** na última etapa. Cada usuário cola a própria chave. Bom para testar antes de publicar o backend.

### Opção A — Proxy (recomendada, segura)
Você publica o backend uma vez; a chave fica **no servidor** e **ninguém a vê**. Cole a URL do backend em `proxyUrl`. A partir daí, todos usam a IA **sem digitar chave** — a última etapa mostra apenas *"IA integrada ativa"*.

### Opção B — Chave embutida (rápida, menos segura)
Preencha `embeddedProvider` e `embeddedKey` no próprio arquivo. Funciona na hora, mas **quem tiver o arquivo consegue ver a chave** — use só para distribuição interna/confiável.

---

## Publicando o proxy (Opção A)

### Cloudflare Worker (grátis, ~5 min) — `proxy-cloudflare-worker.js`
1. Acesse o painel da Cloudflare → **Workers & Pages → Create → Worker**.
2. Cole o conteúdo do arquivo e faça o **Deploy**.
3. Em **Settings → Variables and Secrets**, adicione:
   - `AI_PROVIDER` = `anthropic` (ou `openai`)
   - `AI_API_KEY` = sua chave secreta
   - `AI_MODEL` = `claude-sonnet-4-6` (ou `gpt-4o-mini`) — opcional
   - `ALLOW_ORIGIN` = `*` (ou o domínio onde o HTML será aberto)
4. Copie a URL do Worker e cole em `AI_CONFIG.proxyUrl` no HTML.

### Node.js / Express (alternativa) — `proxy-node-server.js`
Instale (`npm install express cors`), defina as variáveis de ambiente (`AI_PROVIDER`, `AI_API_KEY`, `AI_MODEL`, `ALLOW_ORIGIN`, `PORT`) e rode com `node server.js`. Publique num host (Render, Railway, Fly.io, VPS…) e use a URL pública em `proxyUrl`.

**Contrato do backend:** o HTML envia `POST { prompt, max_tokens }` e recebe `{ text }`. Para trocar de provedor, basta mudar as variáveis no servidor — não é preciso mexer no HTML.

### Segurança da chave
- **Proxy:** a chave nunca chega ao navegador. É a forma correta para uso aberto/escala.
- **Chave embutida ou manual no navegador:** prática para uso interno, mas a chave fica exposta a quem tiver o arquivo. Não use em computadores compartilhados nem distribua o arquivo com a chave salva.

---

## Personalização

Tudo que costuma mudar fica no topo do arquivo (abra com um editor de texto/código).

### Cores do estúdio — objeto `THEME`
```js
var THEME = {
  bg:        "#4c5a3e", // FUNDO da página — verde musgo
  panel:     "#5e6f4d", // caixas de seção
  panelDark: "#3d4832", // barra de etapas
  textLight: "#f4eee3", // marfim — escritos claros
  textWarm:  "#e4d2b4", // areia — textos de apoio
  inputBg:   "#f4eee3", // marfim — caixas de resposta
  inputText: "#332420", // marrom café — texto digitado
  accent:    "#b5562f", // terracota — botões e destaques
  accent2:   "#6e2a33", // bordô — cor secundária
  cafe:      "#332420"  // marrom café
};
```
Para trocar o fundo, mude `bg` (ex.: `"#6e2a33"` bordô ou `"#b5562f"` terracota). Os tons de apoio são recalculados automaticamente.

### Logo
A logo está embutida em base64 (sem dependência externa). Para trocá-la, substitua o valor de `LOGO_SRC` por um novo data URI `data:image/png;base64,...`.

### Textos e perguntas
As perguntas ficam nos arrays `STEPS` (marca consolidada) e `STEPS_NOVA` (marca nova). O título do cabeçalho está na função `header`.

---

## Privacidade e dados

- As respostas são salvas **apenas no navegador** (armazenamento local), separadas por modo.
- No modo offline, nada é enviado para fora.
- Quando a IA é usada, apenas o conteúdo das respostas é enviado ao provedor/proxy, e só no momento de gerar.
- Os botões **Início / Novo diagnóstico** limpam o resultado atual.

---

## Impressão / PDF

O botão **Imprimir / Salvar PDF** abre a caixa de impressão do navegador (escolha "Salvar como PDF"). O documento é formatado automaticamente em **fundo claro com texto escuro** — o tema escuro é só da tela —, ficando limpo para entregar ao cliente. Botões de interface não aparecem na impressão.

---

## Notas técnicas

- Arquivo único, sem build e sem dependências instaláveis.
- Fontes (Archivo, Inter, IBM Plex Mono) vêm do Google Fonts; **offline, o navegador usa fontes do sistema** sem quebrar o layout.
- A prévia de tipografia da marca nova carrega fontes do Google Fonts em tempo real (requer internet).
- Chamadas de IA usam `fetch`. Sem proxy, algumas redes podem bloquear a chamada direta ao provedor por CORS — nesse caso, a marca consolidada cai para o modo local e a marca nova exibe o erro para você corrigir. O proxy resolve isso, pois trata CORS no servidor.
