# Formatura Dra. Rafaella Chaveiro

## Project Overview
Static graduation celebration website for Dra. Rafaella Chaveiro's medical school graduation (Medicina 2026). Hosted via GitHub Pages at rafaellachaveiro.com.br.

## Tech Stack
- Single-page static site: `index.html`
- Vanilla HTML, CSS (inline `<style>`), and JavaScript (inline `<script>`)
- Google Fonts: Cormorant Garamond, Montserrat
- Firebase Realtime Database for the message board ("mural de recados")
- GitHub Pages hosting with custom domain (CNAME)

## Project Structure
- `index.html` — Full site (HTML + CSS + JS all-in-one)
- `rafaella-foto.png` — Profile photo
- `CNAME` — Custom domain configuration

## Development Notes
- All styles and scripts are inline within `index.html`
- The site is in Brazilian Portuguese (pt-BR)
- Firebase config is embedded in the HTML for the message board feature
- No build step required — changes to `index.html` are deployed directly via GitHub Pages
- Test changes by opening `index.html` in a browser

## Validation
- Ensure valid HTML5 structure
- Check that all sections render correctly on mobile and desktop
- Verify Firebase integration for the message board is functional
