# Vidraçaria Transparência — manual do site

**Nome trocado de "WG Vidros" para "Vidraçaria Transparência"**, mesma identidade
visual, nome completo em todo lugar — inclusive cabeçalho, rodapé e trilho vertical
(a versão curta "TRANSPARÊNCIA" só nesses três lugares foi uma economia de espaço
minha; o cliente pediu o nome completo em tudo). Isso quebrava o cabeçalho no celular
— "VIDRAÇARIA TRANSPARÊNCIA" não cabia numa linha só no tamanho de fonte do desktop
— corrigido com um tamanho de fonte menor abaixo de 480px e `min-height` no lugar de
`height` fixa no `.cabecalho` (ver seção 4). A paleta também foi aliviada — todas as
cores escuras (`--fundo`, `--superficie`, `--superficie-funda`, `--borda`) ficaram um
pouco mais claras, mesma família quente, sem perder contraste (reconferido depois da
troca). O restante deste documento ainda cita "WG Vidros" nalguns pontos — a essência
não mudou, só o nome.

Site estático de uma página só (scroll contínuo, sem sub-páginas). Não precisa de
servidor, banco de dados nem plugin — são arquivos soltos, qualquer hospedagem que
sirva HTML funciona.

A ação principal do site é **chamar no WhatsApp**. Não existe formulário: todo botão
abre a conversa já com uma mensagem escrita, diferente em cada seção.

**Passe final de revisão (limpeza + acessibilidade + mobile-first):** conferi o
arquivo inteiro à procura de sobreposição, código morto e duplicação. Achei e
corrigi 4 problemas reais, documentados onde acontecem:
- O zoom de entrada da foto (`.foto-zoom`) nunca rodou — o seletor CSS esperava
  `.foto` como filho de `.foto-zoom`, mas no HTML são a mesma tag. Corrigido em
  `assets/estilo.css`, seção 18.
- O menu do celular aberto (`z-index:90`) ficava por baixo da barra fixa de
  WhatsApp (`z-index:110`) — a barra desenhava por cima do próprio menu. Corrigido
  (menu agora é 115) e adicionei trava de rolagem do fundo enquanto o menu está
  aberto (`body:has(.menu-movel.aberto)`).
- O enquadramento da foto do herói (`--posicao`) estava "desktop-first" — o valor
  sem media query era o do desktop, sobrescrito só abaixo de 700px. Invertido para
  mobile-first, como o resto do arquivo.
- CSS morto removido: variável `--largura-media` e classe `.limite-media` (nunca
  usadas), estilos de formulário `.campo` (não existe formulário no site),
  utilitário `.cursor-ponteiro` (não usado em lugar nenhum).

Também suavizei os cantos do site inteiro — `--raio-sm` (botões, campos) foi de
2px para 8px, `--raio` (cards) de 4px para 16px — e redesenhei a grade de
serviços: era um grid com frestas de 1px imitando planilha, virou cards com
espaço real entre eles e cantos arredondados.

---

## 1. O que você precisa trocar antes de publicar

Esses dados estão como exemplo. Abra `index.html` e use "substituir tudo" em cada linha.

| Procure por | Troque por | Onde aparece |
|---|---|---|
| `5500900000000` | 55 + DDD + número, sem espaço nem traço | Todos os botões de WhatsApp |
| `(00) 90000-0000` | o mesmo número, escrito para leitura | Rodapé, barra fixa do celular |
| `contato@exemplo.com.br` | e-mail real | Rodapé |
| `Rua Exemplo, 000` | endereço completo | Rodapé, dados estruturados (JSON-LD) |
| `Cidade Exemplo` / `Cidade Exemplo – UF` | cidade e estado reais | Rodapé, seção final, JSON-LD |
| `00000-000` | CEP real | JSON-LD |
| `00.000.000/0001-00` | CNPJ real | Rodapé |
| `Seg a sex, 8h–18h · Sáb, 8h–12h` | horário real, se for diferente | Rodapé |
| `00` / `+000` / `00h` | números reais da seção "WG Vidros em números", ou apague a métrica se não tiver o dado | Seção de prova social |

Depois de publicar, troque também `og:image` e `og:url` para o endereço completo,
começando com `https://`. WhatsApp e Facebook só mostram a miniatura com URL completa
— e crie `assets/compartilhamento.jpg` (1200×630), que ainda não existe.

---

## 2. As fotos — 2 por seção, retrato e paisagem

Cada uma das 3 seções com foto tem **duas** imagens agora:

| Seção | Retrato (celular, abaixo de 700px) | Paisagem (desktop, 700px+) |
|---|---|---|
| Herói | `foto-heroi-mobile.jpg` (4:5) | `foto-heroi.jpg` (16:9) |
| Diferenciais | `foto-diferenciais-mobile.jpg` (4:5) | `foto-diferenciais.jpg` (16:9) |
| Clímax | `foto-climax-mobile.jpg` (4:5) | `foto-climax.jpg` (16:9) |

Isso existe porque uma foto paisagem, cortada por `background-size:cover` num
celular alto e estreito, mostra só uma fatia vertical fina do enquadramento original
— a composição inteira (a luz, a profundidade, o clima) se perde. A versão retrato é
pensada pra essa fatia alta desde o início. Todas vieram como PNG em alta
(~1,6-2,2 MB cada); convertidas pra JPEG otimizado (`assets/*.jpg`, 146-260 KB cada)
e originais arquivados em `../fotos-originais/wg-vidros-*-original.png`. Também
recortei `assets/compartilhamento.jpg` (1200×630) a partir do herói desktop, pra
miniatura de link.

**Como o enquadramento funciona:** cada `.foto` tem duas variáveis CSS,
`--posicao` (que parte da foto fica visível no recorte) e `--veu` (o degradê escuro
por trás do texto, pra garantir contraste). A regra de `--imagem`/`--posicao` de cada
seção aparece duas vezes em `assets/estilo.css` (seção 8): uma vez fora de qualquer
media query (mobile, usa a foto retrato) e de novo dentro de `@media (min-width:
700px)` (desktop, usa a paisagem). Se trocar alguma foto por uma nova:

1. Salve com o **mesmo nome exato** dentro de `assets/` — nenhum outro código muda.
   Se só tiver uma foto nova (não duas), pode apontar as duas variáveis pro mesmo
   arquivo — vai funcionar, só não vai ficar tão bem enquadrado no celular quanto
   uma retrato dedicada.
2. Abra a página e confira o enquadramento no celular *e* no desktop — a paisagem
   ainda precisa de `--posicao` ajustado (o corte nela é mais agressivo); a retrato
   geralmente funciona bem só com `center`, mas confira.
3. Se a foto nova tiver a parte escura em outro canto, ajuste `--veu` também: herói e
   diferenciais usam um degradê vertical (texto embaixo), o clímax usa horizontal
   no desktop (texto à esquerda) — no celular ele usa o `--veu` padrão (vertical),
   porque a foto retrato do clímax tem a luz mais concentrada em cima, não à esquerda.

**Cache do CSS:** o link do `estilo.css` tem `?v=6` no `index.html` e no `404.html`.
Aumente esse número toda vez que editar o CSS e quiser ter certeza que quem já visitou
o site (ou uma ferramenta de preview) não fica com uma cópia antiga em cache — o
`_headers`/`vercel.json` guardam `/assets/*` por um dia inteiro.

**Selos sobre foto — cuidado com colisão.** No herói e no capítulo "Diferenciais", os
selos (`.pilula`) são posicionados por `top/left/right` em `style` inline, direto no
`index.html`. Adicionando uma foto de verdade eu conferi as posições por script (não só
visualmente) e achei 3 colisões — texto por baixo de selo — em breakpoints que não
apareceram no primeiro teste visual. Regra que segui pra corrigir: todo selo fica
acima de ~24-26% de altura, porque o texto é ancorado embaixo e pode passar de
450-500px de altura sozinho. Se adicionar um selo novo ou reescrever o texto de algum
existente (ficando mais longo), confira de novo — é fácil um selo formal recolocado
"por olho" cair sobre o parágrafo numa largura que você não testou.

---

## 3. Uma coisa ainda em aberto

**O depoimento.** A seção está pronta e comentada dentro do `index.html`, logo depois
dos números. Ela só entra no ar com uma avaliação real, com nome de quem escreveu.
Nunca escreva depoimento inventado: além de enganar quem visita o site, avaliação
falsa é prática enganosa perante o Código de Defesa do Consumidor.

---

## 4. O herói tem uma estrutura diferente do resto — de propósito

O cliente trouxe um componente de referência (React + Tailwind + shadcn, de outro
projeto) pedindo pra instalar no herói. O stack não bate com este site (aqui é
HTML/CSS/JS puro, sem Node — igual a todos os outros da oficina, de propósito: CSP
fechado, zero build, qualquer hospedagem serve). Em vez de trazer o stack, portei só
o efeito visual pro CSS/JS que já existe:

- **`.heroi` virou dois elementos.** `.heroi` só reserva o respiro ao redor (10px no
  celular, até 20px no desktop); `.heroi-moldura` é quem tem `.foto`, o
  `border-radius` (20px a 36px conforme a tela) e `overflow:hidden`. Pílulas, texto e
  indício de rolagem são todos relativos à moldura agora, não mais ao `.heroi`.
- **Título palavra por palavra.** Cada `.palavra` no `h1` nasce com uma variável
  `--i` (0, 1, 2...) que vira atraso em cascata no CSS (`assets/estilo.css`, perto de
  "Herói"). O `h1` carrega o texto real via `aria-label`; o `span` com as palavras é
  `aria-hidden`, então leitor de tela nunca lida com a fragmentação.
- **Botão-cápsula: única exceção ao raio reto do site.** `.botao-capsula` (só o CTA
  principal do herói) é a única coisa arredondada em formato de pílula fora dos
  selos de diferencial — decisão deliberada, documentada em comentário no CSS, pra
  não virar um "esqueci de padronizar" nas próximas alterações.

## 5. A marca foi refeita — a versão original tinha um bug de fundo

O símbolo `#i-panes` original "furava" o segundo retângulo com `fill="var(--fundo)"`
para simular transparência. Funcionava por acidente só quando o ícone estava exatamente
sobre `--fundo` — no cabeçalho transparente (sobre a foto do herói) e no rodapé (sobre
`--superficie-funda`, uma cor diferente) sobrava um remendo sólido visível, sem gradiente,
sem nada. Agora são dois símbolos com papéis separados:

- **`#i-marca`** (cabeçalho, rodapé): fill translúcido de `currentColor` em vez de cor
  sólida — funciona sobre qualquer fundo porque é transparência de verdade, não uma cor
  copiada. Cantos com `rx=.6`, acompanhando o resto do site (ver seção de botões). Ganhou
  um traço diagonal em `var(--destaque)` — o único acento de cor da marca, um brilho no
  canto do vidro.
- **`#i-panes`** (ícone do card "Vidros especiais sob medida"): virou traço puro, sem
  fill, pra combinar com os outros 5 ícones da grade de serviços — antes ele destoava por
  ser o único preenchido/colorido no meio de ícones de linha.

`assets/favicon.svg` já usava fill translúcido (nunca teve o bug), só ganhou os mesmos
cantos arredondados pra bater com o resto. `404.html` tem sua própria cópia do símbolo
(cada página carrega só os ícones que usa) — também estava com a versão antiga e foi
corrigida junto.

Não trouxe: o vídeo de fundo (não existe um da WG Vidros — ficou a foto, que já tem
o leve zoom de entrada de `.foto-zoom`) nem a navegação em pílula flutuante do
componente original (o cabeçalho fixo atual já funciona bem em todas as seções, e
duas navegações concorrentes seria pior, não melhor).

---

## 6. Como o site foi construído

### A referência de marca

O cliente anexou um case de branding para uma marca de cozinhas ("KANTO", de Anna
Malofeeva) como direção de arte — não como conteúdo. As regras extraídas dele estão
em `design-system/wg-vidros/MASTER.md` (a versão gerada automaticamente pelo skill de
UI/UX foi corrigida à mão nesse arquivo: o padrão genérico sugeria fundo claro e uma
paleta "Liquid Glass" que não bate com a referência real, que é escura e fotográfica).

As três assinaturas visuais que vieram da referência, adaptadas para vidro:

- **Trilho vertical** — "WG VIDROS" girado 90°, correndo na borda esquerda da tela em
  telas largas (1280px+). Em telas menores essa marca aparece no cabeçalho normal.
- **Selos em pílula sobre foto** — os diferenciais ("Vidro Temperado", "Garantia"...)
  flutuam sobre a fotografia em vidro fosco (`backdrop-filter: blur`), o único momento
  glassmorphism do sistema — vidro de verdade sobre uma foto de vidro.
- **A cor vem da foto, não da paleta.** A interface é quase monocromática e escura; o
  dourado/latão (`--destaque`, `#C89456`) aparece só em botão principal e estado ativo.

### A marca (logotipo)

Não existia identidade visual — liberdade total, confirmada com o cliente. A marca
é duas "placas de vidro" retangulares sobrepostas, com preenchimento translúcido: a
mesma lógica de transparência e camada que aparece em qualquer trabalho real de
vidraçaria (box, guarda-corpo, fachada — tudo é vidro sobre vidro). Detalhes de
implementação (símbolo `#i-marca`, o bug do fill sólido que existiu antes e como foi
corrigido) estão na seção 5.

### O conteúdo não depende de JavaScript

Os blocos aparecem com uma animação suave ao rolar a tela, mas o texto é visível por
padrão. Se o JavaScript falhar ou estiver desligado, o site continua legível — existe
um prazo de segurança de 4 segundos que revela tudo caso o observador de rolagem não
rode. `prefers-reduced-motion` desliga a animação inteira, sem esconder nada.

### Sem GSAP nem CDN de script

Outros sites da oficina usam essa mesma regra: o `Content-Security-Policy` só libera
`script-src 'self'`, então toda animação é `IntersectionObserver` + CSS puro, sem
biblioteca externa. Mantém o CSP idêntico ao dos outros sites e o site leve.

---

## 7. Rodar no seu computador

```bash
python -m http.server 5177 --directory wg-vidros
```

Depois abra `http://localhost:5177`. (Já existe uma entrada "wg-vidros" tanto no
`.claude/launch.json` da raiz do workspace quanto em `wg-vidros/.claude/launch.json`,
para pré-visualizar direto pela ferramenta de preview.)

---

## 8. Mapa dos arquivos

| Arquivo | O que é |
|---|---|
| `index.html` | A página inteira — herói, serviços, diferenciais, processo, números, CTA final |
| `404.html` | Página de erro |
| `assets/estilo.css` | Toda a aparência do site, comentada em português |
| `assets/favicon.svg` | Ícone da aba do navegador e marca reaproveitada no site |
| `assets/foto-*.jpg` (6 arquivos: heroi/diferenciais/climax × normal/mobile) | As fotos do site, retrato + paisagem, já otimizadas — ver seção 2 |
| `assets/compartilhamento.jpg` | Miniatura ao compartilhar o link (1200×630), recortada do herói |
| `../fotos-originais/wg-vidros-*-original.png` | Os PNGs em alta, sem otimizar — fora da pasta do site, não sobem pro Vercel |
| `design-system/wg-vidros/MASTER.md` | Ficha de design (cores, tipografia, motion) gerada + corrigida à mão |
| `_headers` | Segurança e cache. Netlify e Cloudflare leem sozinhos |
| `vercel.json` | Os mesmos cabeçalhos, no formato que a Vercel lê |
| `robots.txt`, `sitemap.xml` | Bloqueiam indexação por enquanto — ver seção 1 |
| `.vercelignore` | Impede que `LEIA-ME.md`, `_headers` e a pasta `design-system/` subam para a Vercel |
