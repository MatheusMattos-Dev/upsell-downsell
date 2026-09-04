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

## ⚡ Performance
- Imagens medidas e com `width`/`height` fixos + `aspect-ratio` no CSS: zero CLS.
- A arte do hero (941x1672, 507 KB) é o LCP — leva `fetchpriority="high"` e tem `preconnect` para o domínio dela.
- `feed.mp4` tem **5,4 MB** e por isso **não** usa `autoplay`: o play só acontece quando a seção entra na tela,
  e o vídeo pausa e volta a ficar mudo ao sair. Sem isso, o arquivo baixava no load e disputava banda com o hero.
- Capas são retrato (1200x1500 e 1000x1415); os cards recortam em `3 / 4`, não em paisagem.
- Revelação por scroll (`[data-reveal]`) desligada no celular e sob `prefers-reduced-motion`.

## 🔎 SEO / Social
- JSON-LD `Product` + `Offer` (R$ 12,50, BRL, InStock) apontando para o checkout.
- `twitter:card`, `og:image:width/height`, `theme-color`.
- **Pendente:** `og:url` e `canonical` — dependem da URL final de deploy.

## 🚀 Como publicar:
- **Vercel / Netlify**: Conecte este repositório e faça o deploy automático da raiz (`index.html`).
- **GitHub Pages**: Vá em *Settings > Pages > Branch: main > Save*.
