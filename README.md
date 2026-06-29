# Diagnóstico de Marca & Ambiente — Versão Híbrida

Ferramenta do **Ferrabisk Estúdio** para diagnóstico de marca, espaço e comunicação visual.
Arquivo: `diagnostico-marca-ambiente-hibrido.html`

É um **formulário guiado** que uma marca preenche e que, ao final, gera automaticamente um documento com resumo, análise aprofundada, insights de melhoria e três paletas de cor. Funciona **100% offline por padrão** e, opcionalmente, pode usar uma IA externa para enriquecer o texto.

---

## Como usar

1. Baixe o arquivo `diagnostico-marca-ambiente-hibrido.html`.
2. Dê dois cliques para abrir em qualquer navegador (Chrome, Edge, Firefox, Safari).
3. Não precisa instalar nada, não precisa de servidor e, no modo padrão, não precisa de internet.

O preenchimento acontece em 6 etapas, navegáveis pela barra de progresso no topo:

| Etapa | Conteúdo |
|------|----------|
| 1. Identidade | Nome, segmento, anos de mercado, público-alvo |
| 2. Cores | Cores principais e secundárias, estado da identidade visual |
| 3. Essência | Missão, valores, personalidade, sensação desejada |
| 4. Futuro | Objetivos, expansão, percepção futura |
| 5. Ambiente | Espaço atual, gargalos, se o ambiente comunica a marca, limitações |
| 6. Comunicação | Sinalização/wayfinding, materiais, desafios visuais |

Campos obrigatórios (marcados com `*`): **nome da marca**, **segmento** e **cores principais**. Os demais são opcionais — quanto mais preenchido, mais rica a análise.

Ao clicar em **Gerar diagnóstico**, a ferramenta produz o documento. Botões disponíveis no resultado: **Imprimir / Salvar PDF**, **Editar respostas** e **Nova marca**.

---

## O que é gerado

- **Resumo** — síntese da marca e do estado atual.
- **Análise aprofundada** — blocos sobre identidade, essência/cultura, ambiente físico e gargalos, comunicação visual e prospecção futura.
- **Insights de melhoria** — recomendações priorizadas (alta / média / baixa), focadas em arquitetura de ambientes e comunicação visual.
- **3 paletas de cor**, calculadas por teoria das cores a partir das cores informadas:
  1. **Refinamento análogo** — harmonia em torno da cor principal.
  2. **Contraste complementar** — opostos calibrados para destaques e sinalização.
  3. **Neutra arquitetônica** — tons de baixa saturação para acabamentos, paredes e mobiliário.

Cada cor traz HEX, nome e uso sugerido no ambiente. **Clique em qualquer amostra para copiar o HEX.**

> As paletas reconhecem tanto códigos HEX (`#b5562f`) quanto nomes em português (ex.: "terracota", "verde musgo", "areia", "azul marinho").

---

## Modo IA (opcional)

Por padrão, todo o diagnóstico é gerado **localmente, sem IA e sem internet** — o resultado é consistente e determinístico.

Na última etapa do formulário há um painel recolhível **"Modo IA · opcional"**. Ao ativá-lo:

1. Escolha o **provedor**: Anthropic (Claude) ou OpenAI.
2. Cole a **sua própria chave de API**.
3. Opcionalmente, ajuste o **modelo** (padrão: `claude-sonnet-4-6` para Anthropic, `gpt-4o-mini` para OpenAI).

Quando ativo, a IA escreve o **resumo, a análise e os insights** (texto mais fluido e interpretativo). As **paletas continuam sempre calculadas localmente**, por serem mais confiáveis.

**Comportamentos importantes:**

- **Queda automática para o modo local.** Se a chave for inválida, faltar internet ou o navegador bloquear a chamada, a ferramenta não trava: gera o diagnóstico offline e exibe um aviso discreto.
- **Selo de origem.** No topo do resultado aparece se a análise foi *"gerada localmente · offline"* ou *"enriquecida por IA · [provedor]"*.
- **Chave sob seu controle.** Por padrão a chave **não é salva** (some ao recarregar). A caixa *"Lembrar a chave neste navegador"* a guarda localmente apenas se marcada.

### ⚠️ Segurança da chave de API

Colar a chave direto no navegador é prático para **uso interno do estúdio**, mas a chave fica armazenada/visível no seu computador e é enviada direto ao provedor escolhido.

- Não use em computadores compartilhados ou públicos.
- Não distribua o arquivo com a chave "lembrada" salva dentro dele.
- Para uso aberto a clientes ou em escala, o correto é um pequeno backend que guarde a chave — não exponha a chave no navegador.

---

## Personalização

Tudo que costuma mudar fica logo no topo do arquivo (abra com um editor de texto/código).

### Cores do estúdio

No objeto `THEME` (início do arquivo):

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

Para trocar o fundo, mude `bg` (ex.: `"#6e2a33"` bordô ou `"#b5562f"` terracota). Os tons de apoio que dependem do destaque são recalculados automaticamente.

### Logo

A logo está embutida no próprio arquivo (em base64), então não há dependência de imagem externa. Para trocá-la, substitua o valor da variável `LOGO_SRC` por um novo data URI `data:image/png;base64,...`.

### Textos

O título do cabeçalho e as perguntas do formulário estão em variáveis de fácil edição (`STEPS` para as etapas/perguntas; o texto "Ferrabisk Estúdio · …" na função `header`).

---

## Privacidade e dados

- As respostas são salvas **apenas no navegador** (armazenamento local), para não perder o preenchimento ao fechar a aba. Nada é enviado para fora no modo offline.
- O botão **Nova marca** limpa as respostas salvas.
- No Modo IA, somente o conteúdo das respostas é enviado ao provedor escolhido, e apenas no momento de gerar o diagnóstico.

---

## Impressão / PDF

O botão **Imprimir / Salvar PDF** abre a caixa de impressão do navegador (escolha "Salvar como PDF"). O documento é formatado automaticamente em **fundo claro com texto escuro** — o tema escuro é só da tela —, ficando limpo para entregar ao cliente. Os botões de interface não aparecem na versão impressa.

---

## Notas técnicas

- Arquivo único, sem build e sem dependências instaláveis.
- As fontes (Archivo, Inter, IBM Plex Mono) vêm do Google Fonts via internet; **offline, o navegador usa fontes do sistema** automaticamente, sem quebrar o layout.
- O Modo IA usa `fetch` direto para a API do provedor. Alguns ambientes podem bloquear a chamada por CORS — nesse caso, a ferramenta cai para o modo local.
- Testado como página estática local (`file://`) e também funciona servido por qualquer servidor de arquivos estáticos.

---

## Duas versões disponíveis

| Arquivo | Descrição |
|--------|-----------|
| `diagnostico-marca-ambiente.html` | Versão **somente offline**, sem qualquer IA. Base sólida e determinística. |
| `diagnostico-marca-ambiente-hibrido.html` | Esta versão: **offline por padrão**, com Modo IA opcional para enriquecer o texto. |

Ambas compartilham o mesmo motor de cálculo de paletas e a mesma identidade visual do estúdio.
