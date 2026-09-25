# CLAUDE.md — MARATONASC

Site gerado pelo **SF (Site Factory)** em 15/04/2026. Migrado para o modelo Cloudflare + Supabase em 25/09/2026.

## Contexto do Site

**Nome:** MARATONASC
**Nicho:** Esportes e Fitness
**Keywords:** Uma pessoa do bem com o objetivo de ajudar as pessoas que
**Paleta de cores:** violet | **Fonte:** inter

Uma pessoa do bem com o objetivo de ajudar as pessoas que buscam conhecimento e aprimoramento na prática da corrida ou saúde. Meu nome é Renato Augusto e seja muito bem-vindo ao meu blog. Esse blog foi feito para dividir as minhas experiências, porque quando eu comecei a me interessar por corrida eu tive muitas dúvidas, então decidi ajudar as pessoas que estão passando pelas mesmas dificuldades que eu. Não importa se você treina na academia ou na rua, se você tem 18 ou 90 anos, meu blog com certeza tem algum conteúdo que fará você alcançar seu objetivo. Agora você tem um canal de comunicação forte no ramo da corrida. Você vai poder sempre se manter atualizado assinando a minha newsletter.

## Componentes visuais usados

| Seção | Variante |
|-------|----------|
| Header | Header-E |
| Hero | Hero-C |
| Features | Features-I |
| About Section | About-H |
| Posts | Posts-J |
| Footer | Footer-D |
| Página Sobre | Sobre-H |
| Página Contato | Contato-G |

## Estrutura do projeto

```
src/
  sections/        # Layout escolhido pelo SF — Header, Hero, Features, About, Posts, Footer, Sobre, Contato
  data/            # JSONs com todo o conteúdo editável
  lib/             # supabase.ts (cliente) e posts.ts (getPosts/getPostBySlug)
  components/      # Seo.astro (meta tags + JSON-LD)
  pages/           # Rotas Astro (index, sobre, contato, blog, privacidade, termos, [...slug])
  layouts/         # BaseLayout com fonte e cores dinâmicas
  styles/          # global.css com variáveis CSS de cor
public/
  images/          # hero.jpg, about.jpg, sobre.jpg
```

## O que editar

### Textos e conteúdo
- **`src/data/home.json`** — hero (título, subtítulo, botão), features (título, items), about section (título, desc, stats), posts
- **`src/data/sobre.json`** — conteúdo completo da página Sobre (hero, texto, stats)
- **`src/data/contato.json`** — título, subtítulo, email, tempo de resposta
- **`src/data/siteConfig.json`** — nome, slug, email, redes sociais, menu (título/descrição/OG/JSON-LD derivam daqui)

### Imagens
Imagens já estão em `public/images/` (via Pexels). Para substituir, mantenha os mesmos nomes de arquivo:
- `hero.jpg` — imagem de fundo do Hero (e og:image padrão)
- `about.jpg` — imagem da seção About (home)
- `sobre.jpg` — imagem de fundo da página Sobre

### Posts do blog
Os posts NÃO ficam mais em markdown local. São carregados do Supabase (tabela `network_posts`, filtrados por `domain = maratonasc.com.br`).
- `src/lib/posts.ts` — `getPosts()` e `getPostBySlug()`; `formatContentToHtml()` converte markdown → HTML.
- Sem painel admin. Novos posts/posts editados entram pela plataforma 8links e publicam automaticamente (via Git/CF).

### Cores
Variáveis em `src/styles/global.css`: `--color-primary`, `--color-accent`, `--color-dark`.

## SEO

- `src/components/Seo.astro` injetado pelo `BaseLayout`: title, description, canonical, OG, Twitter, `name="robots"`, JSON-LD (WebSite nas páginas estáticas, BlogPosting nos artigos).
- `src/pages/robots.txt.ts` e `src/pages/sitemap.xml.ts` gerados dinamicamente (sitemap inclui posts com lastmod).

## Deploy

```bash
bun install
bun run build
# Publicar no Cloudflare: a pasta dist/ é servida como Worker (adaptador @astrojs/cloudflare)
# Envs opcionais no CF: SUPABASE_URL e SUPABASE_ANON_KEY (fallbacks embutidos no código)
```