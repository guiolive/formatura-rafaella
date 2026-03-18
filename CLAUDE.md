# Formatura Dra. Rafaella Chaveiro

## Project Overview
Site estático de celebração da formatura em Medicina da Dra. Rafaella Chaveiro (Medicina 2026, turma UNIEURO). Hospedado via GitHub Pages em **rafaellachaveiro.com.br**. O site é todo em português brasileiro (pt-BR).

## Tech Stack
- **Single-page static site**: tudo em `index.html` (~2135 linhas) — HTML + CSS inline (`<style>`) + JS inline (`<script>`)
- **Fontes**: Google Fonts — Cormorant Garamond (serif, para títulos/corpo) e Montserrat (sans-serif, para labels/detalhes)
- **Backend**: Firebase Realtime Database (SDK compat v10.12.0) para mural de recados e confirmação de presença
- **Analytics**: Google Analytics (gtag.js, ID: `G-9FT1P7L551`) com eventos customizados
- **Hosting**: GitHub Pages com custom domain (CNAME → `rafaellachaveiro.com.br`)
- **CI/CD**: GitHub Actions — deploy automático no push para `master`, auto-merge de branches `claude/*`, validação HTML em PRs

## Arquivos do Projeto
```
index.html                    — Site completo (HTML + CSS + JS all-in-one)
IMG-20260316-WA0134.jpg       — Foto de perfil principal (usada no hero, favicon, OG image)
IMG_9285.jpeg                 — Foto alternativa (não usada atualmente)
rafaella-foto.png             — Foto antiga (não usada atualmente)
CNAME                         — Domínio custom: rafaellachaveiro.com.br
preview-marsala.html          — Preview de tema alternativo (arquivo auxiliar)
.github/workflows/deploy.yml  — Deploy para GitHub Pages (push em master)
.github/workflows/auto-merge.yml — Auto-merge de PRs de branches claude/*
.github/workflows/validate.yml   — Validação HTML em PRs
```

## Paleta de Cores (Marsala/Rosa)
```css
--gold: #70020F              /* Marsala principal — botões, títulos, acentos */
--gold-dark: #4a0008         /* Marsala escuro — hover, itálicos */
--dark-bg: #FFDEE2           /* Rosa claro — fundos de seção */
--dark-secondary: #ffd0d6    /* Rosa médio — bordas de inputs */
--dark-tertiary: #FFE8EB     /* Rosa mais claro */
--dark-quaternary: #FFF0F2   /* Rosa quase branco */
--light-bg: #FFF5F6          /* Fundo principal do body */
--text-primary: #2c2c2c      /* Texto principal */
--text-secondary: #777       /* Texto secundário */
```

## Estrutura do Site (Seções do index.html)

### 1. Navigation (`<nav class="nav">`, id: `navbar`)
- Menu fixo no topo com links: Início, Jornada, Eventos, Recados, Presença, Ingresso
- Efeito blur/sombra ao fazer scroll (classe `.scrolled` adicionada via JS)

### 2. Hero (`<section class="hero">`, id: `inicio`)
- Foto de perfil circular com bordas animadas (spin lento)
- Nome: "Dra. Rafaella Chaveiro" com "Formatura de Medicina"
- **Countdown** até 16/01/2027 às 21h (fuso Brasília -03:00)
  - Mostra dias, horas, minutos e segundos
  - Animação de pulse ao mudar valor
  - Quando chegar a zero: mostra "O grande dia chegou!"
- Data do evento: "16 de Janeiro de 2027"

### 3. Timeline / Jornada (`<section class="timeline-section">`, id: `jornada`)
- 6 itens alternados (esquerda/direita) representando cada ano da faculdade (2021-2026)
- Linha vertical central com dots numerados
- Animação de entrada via IntersectionObserver

### 4. Eventos (`<section class="event-section">`, id: `evento`)
- 3 cards em grid responsivo:
  - **Culto Ecumênico**: 14/01/2027 às 10h (local a confirmar)
  - **Colação de Grau**: 15/01/2027 às 18h (local a confirmar)
  - **Baile de Formatura**: 16/01/2027 às 21h (card destacado, com link para ingresso)

### 5. Mural de Recados (`<section class="messages-section">`, id: `recados`)
- Formulário: nome (max 50 chars) + mensagem (max 500 chars)
- **Sistema de moderação**: mensagens são salvas com `approved: false`, precisam de aprovação
- **Painel Admin**: acessível via `?admin=SENHA` na URL (senha verificada com SHA-256 hash)
  - Hash da senha: `424d5811e96ee00f948be2dda7499efc418b7338170b98d80c45e097171ba45b`
  - Mostra pendentes (com botão aprovar/excluir) e aprovados (com botão excluir)
- Rate limiting: 30s entre mensagens
- Fallback para localStorage se Firebase não estiver disponível
- XSS prevention: `escapeHtml()` em todos os dados renderizados

### 6. Confirmação de Presença (`<section class="presenca-section">`, id: `presenca`)
- Formulário por família: nome da família + adicionar membros individualmente
- Máximo de 20 pessoas por família
- Rate limiting: 60s entre confirmações
- Contador de famílias/pessoas confirmadas exibido abaixo do form
- Dados salvos no Firebase (path: `presencas/`) com fallback localStorage

### 7. Ingresso Simbólico (`<section class="ticket-section">`, id: `ingresso`)
- Visual de ticket com detalhes do baile (data, horário, assento VIP)
- Botão "Garantir Meu Ingresso" → link externo para eformei.com.br (loja de ingressos)
- Nota: "Pagamento via PIX ou cartão"

### 8. Footer
- Citação: "Curar as vezes, aliviar frequentemente, consolar sempre"
- Assinatura: "Com amor, Dra. Rafaella Chaveiro"

## Firebase (Realtime Database)
- **Project**: `rafasite-74c7b`
- **Database URL**: `https://rafasite-74c7b-default-rtdb.firebaseio.com`
- **Paths**:
  - `messages/` — Recados do mural (campos: name, text, timestamp, approved)
  - `presencas/` — Confirmações de presença (campos: family, members[], timestamp)
  - `.info/connected` — Verificação de conexão

## Google Analytics - Eventos Rastreados
- `click_nav` — Cliques no menu de navegação
- `click_ingresso` — Clique no botão de comprar ingresso
- `confirmar_presenca` — Envio de confirmação de presença (com quantidade de pessoas)
- `enviar_recado` — Envio de recado no mural
- `ver_secao` — Visualização de cada seção (via IntersectionObserver, threshold 50%)

## Responsividade (Mobile)
- Breakpoint principal: `max-width: 768px`
- Nav: scroll horizontal sem scrollbar, fonte menor
- Hero: foto menor (200px), countdown compacto sem separadores
- Timeline: layout vertical à esquerda (dots à esquerda, cards à direita)
- Forms: campos empilhados verticalmente
- Mural: grid de 1 coluna

## Segurança
- Meta tags: `X-Content-Type-Options: nosniff`, `X-Frame-Options: DENY`, `referrer: strict-origin-when-cross-origin`
- Admin via hash SHA-256 (nunca expõe senha no código)
- XSS: escape de HTML em todas as renderizações de dados do usuário
- Input sanitization: truncamento de strings (50/500/30 chars)
- Rate limiting client-side em formulários

## Development Notes
- **Sem build step** — editar `index.html` diretamente, deploy automático via GitHub Pages
- Testar abrindo `index.html` no browser
- Branch `master` é a branch de produção
- Branches `claude/*` têm auto-merge para master via GitHub Actions
- A foto usada atualmente é `IMG-20260316-WA0134.jpg` com `?v=2` cache-buster e `object-position: center 20%; transform: scale(1.15)` para foco no rosto
