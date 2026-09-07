# Oferta Especial Início de Mês — HYROX (Upsell / Downsell)

Página de vendas de alta conversão para a oferta de início de mês (Pacote Completo HYROX por **R$ 12,50**).
Segue o design system da página principal (`hyrox-page`): fundo papel `#ECE7DE` com texto tinta `#17160F`, acentos ember/rope,
componentes `.eyebrow` / `.section-title` / `.lede` / `.btn` / `.section--*` / `.offer-bundle`, sprite SVG inline,
e o mesmo pixel com o mesmo desenho de eventos.

## 🎯 Objetivo
Apresentar o pacote fechado com ancoragem de R$ 122,50 por R$ 12,50 à vista no Pix ou cartão.

## 🧱 Gancho obrigatório da linha
Toda página da HYROX Master Class precisa frisar a condição de início de mês, acima da dobra:

- Eyebrow: `Condição Especial · Início de Mês`
- Headline: promessa forte de **adquirir TODOS os produtos** de uma vez, com o preço em destaque.
  Nesta página: `Adquira todos os produtos HYROX de uma vez por R$ 12,50`.

## 🔗 Checkout Oficial Wiapy
- Link: [https://pay.wiapy.com/ogJsn_tWH8XO](https://pay.wiapy.com/ogJsn_tWH8XO)
- SKU: `pacote-completo` · valor `12.50` · nome `Pacote Completo — Início de Mês`
- Todo CTA carrega `data-checkout`, `data-sku` e `data-price` — é isso que alimenta o `InitiateCheckout`.

## 📦 O que está incluso no pacote:
1. **250 Aulas Prontas (Daily WOD)**: Ano inteiro planejado estação a estação com 3 níveis de adaptação. — R$ 27,00
2. **HYROX Master Class (150 páginas)**: Manual técnico de fisiologia e execução. — R$ 10,00
3. **Manual do Coach & Bateria de Testes**: 5 testes em 10 minutos e correções biomecânicas. — R$ 40,50
4. **Programação Oficial de 12 Semanas**: Periodização focada em performance. — R$ 45,00
5. **Suporte & Direcionamento**: Orientação na metodologia. — incluso

Soma avulsa: **R$ 122,50** · Oferta: **R$ 12,50** · Economia: **R$ 110,00** (90%).

## 📊 Rastreamento
- **Meta Pixel** `3602732119884841` — o mesmo da `hyrox-page`, para a audiência ser uma só.
- Eventos: `PageView` · `ViewContent` (1,5s de permanência no box de preço) · `ScrollDepth` 25/50/75/90 (diagnóstico) · `InitiateCheckout` (clique no CTA).
- Cada evento sai com `eventID` próprio, pré-requisito para deduplicar quando a API de Conversões da Wiapy for ligada.
- Trava de um disparo por sessão: a página mede pessoas, não cliques.
- **UTMify** cuida do repasse de UTM ao checkout. Não colar `utm_*` no href à mão — duplica os parâmetros.
- Dentro do navegador embutido do Instagram/Facebook e no celular, o checkout abre na **mesma aba** (`target` removido em runtime) com 200ms de folga para o beacon do pixel sair.

## ♿ Notas de implementação
- Contador de urgência: **5 minutos**, com a expiração gravada em `localStorage` (`hx_promo_expiry_1250`) — recarregar a página não devolve tempo novo.
- Todo acesso a `localStorage`/`sessionStorage` passa por wrapper com `try/catch`: o navegador in-app costuma barrar storage, e uma exceção ali derrubaria contador, som do vídeo, barra fixa e pixel de uma vez.
- A barra fixa usa `visibility` + `inert` quando escondida, para não capturar `Tab` nem leitor de tela.
- `@media (prefers-reduced-motion: reduce)` desliga ticker, pulse e transições.

## 🧭 Estrutura do funil
Cabeçalho de marca (mesmo brand da `hyrox-page`, sem navegação — link de menu é porta de saída
numa página de oferta) → ticker de urgência → hero → **bloco da oferta** → 01 O problema →
02 O pacote → 03 A conta → **faixa de CTA** → 04 Por dentro → 05 Na prática → **faixa de CTA** →
06 Para quem é → garantia → 07 Dúvidas → fechamento → barra fixa.

- **5 pontos de compra** na página: bloco da oferta, duas faixas de CTA no meio, fechamento e barra fixa.
  A trava do `InitiateCheckout` garante um disparo por sessão mesmo com todos eles.
- **01 O problema** vem depois do preço, não antes: quem chega do anúncio pronto pra comprar
  encontra o botão cedo; quem precisa ser convencido tem a agitação logo em seguida.
- **06 Para quem é** diz também para quem *não* é. Reduz reembolso e sustenta a garantia de 7 dias.
- Fundos alternam papel / papel-alt / tinta a cada seção, no ritmo da `hyrox-page`.

## 🎠 Carrossel
Componente único (`.carousel`), usado em **02 O pacote** (5 materiais) e **04 Por dentro** (7 páginas).

- O trilho é `scroll-snap` nativo: continua rolando no dedo e no trackpad. As setas, a barra de
  progresso e o contador são camada extra para quem está no mouse.
- Barra de rolagem nativa escondida (`scrollbar-width: none` + `::-webkit-scrollbar`) — era o que
  aparecia feio embaixo das prévias.
- Se todos os itens couberem na tela, a navegação inteira some (`nav.hidden`) em vez de ficar inerte.
  Precisa do `.carousel__nav[hidden] { display: none }`: `display:flex` tem a mesma especificidade
  que o `[hidden]` do navegador e é declarado depois.
- O passo é medido do DOM (distância entre dois itens), não copiado do CSS — assim o `clamp` da
  largura do item não precisa ser repetido no JS.
- As setas avançam **uma página inteira**, não um item: com três cards visíveis, pular de um em um
  faz a seta parecer quebrada.
- Setas usam `#icon-pace` do sprite, a de voltar espelhada por `scaleX(-1)`.

## 🎨 Ícones
**Nunca emoji na interface.** Todo ícone vem do sprite SVG inline, copiado do `hyrox-page`:
`icon-clock`, `icon-check`, `icon-shield`, `icon-pace`, `icon-play`, `icon-close`, `icon-sound-on/off`.
Uso: `<svg class="icon"><use href="#icon-clock" /></svg>`.
Emoji renderiza na fonte do sistema, traz cor própria e não acompanha o peso tipográfico da página.

## ⚡ Performance
- Imagens medidas e com `width`/`height` fixos + `aspect-ratio` no CSS: zero CLS.
- **O hero é um depoimento em vídeo**, não mais a arte da oferta: `assets/hero-feedback.mp4`
  (H.264 720x1280, 45 s, 7,2 MB) com `poster` no primeiro quadro (`hero-feedback-poster.jpg`, 39 KB) —
  o pôster segura a caixa até o vídeo abrir e é exatamente o quadro em que a reprodução começa, sem salto.
  O master de celular (4K HEVC, 128 MB) fica fora do Git: HEVC não toca em Chrome/Firefox e 128 MB estoura
  o limite de 100 MB por arquivo do GitHub. Regenerar com:
  `ffmpeg -i MASTER.mp4 -vf scale=720:1280 -c:v libx264 -preset slow -crf 29 -maxrate 1200k -bufsize 2400k -pix_fmt yuv420p -r 30 -c:a aac -b:a 64k -ac 1 -movflags +faststart assets/hero-feedback.mp4`
- Os dois vídeos da página (hero e prova) passam pelo mesmo `ligarDepoimento()`: entram mudos, tocam quando
  entram na tela e pausam + voltam a ficar mudos ao sair. Sem isso, os 5,4 MB do `feed.mp4` baixavam no load
  e disputavam banda com o hero.
- Capas são retrato (1200x1500 e 1000x1415); os cards recortam em `3 / 4`, não em paisagem.
- Revelação por scroll (`[data-reveal]`) desligada no celular e sob `prefers-reduced-motion`.

## 🔎 SEO / Social
- JSON-LD `Product` + `Offer` (R$ 12,50, BRL, InStock) apontando para o checkout.
- `twitter:card`, `og:image:width/height`, `theme-color`.
- **Pendente:** `og:url` e `canonical` — dependem da URL final de deploy.

## 🚀 Como publicar:
- **Vercel / Netlify**: Conecte este repositório e faça o deploy automático da raiz (`index.html`).
- **GitHub Pages**: Vá em *Settings > Pages > Branch: main > Save*.
