# MEMORIA.md — Simulado CAP 2026

## Status Atual
Site estático hospedado no GitHub Pages (branch `gh-pages`, repo `adilsonee/simuladocap`), servido em `https://adilsonee.github.io/simuladocap/`. Aplicação single-page em `index.html` (HTML/CSS/JS puro, sem build) que lê as questões de `questoes.json` via `fetch`. Fluxo: login (nome + chave de acesso fixa) → menu (Simulado Geral / por Matéria) → seleção de quantidade → quiz com navegador de questões e cronômetro → tela de resultados com revisão e justificativa. Histórico por nome fica salvo em `localStorage`. App funcionando corretamente após o fix descrito abaixo.

## O que foi feito
- **2026-09-22** — Corrigido bug que deixava a aplicação inutilizável após o login: `#screen-shell` nascia com `class="hidden"` (`display:none !important` no CSS), e `showOnly()` só alterava `style.display` inline daquele elemento — que o `!important` sempre sobrepunha. Resultado: tela ficava vazia/preta para sempre após "Entrar", mesmo com `questoes.json` carregado corretamente. Corrigido fazendo `showOnly()` usar `classList` (show/hide) para `screen-shell` igual às demais telas. Commit `59a0724`. Validado com Chrome headless simulando login completo (tela do menu aparece com cards e histórico).

## Decisões de Arquitetura
- App sem framework/build step, propositalmente simples para deploy direto via GitHub Pages (`.nojekyll` presente para não filtrar arquivos).
- Chave de acesso (`ACCESS_KEY = "CAP2026"`) hardcoded no JS — não é autenticação real, apenas controle de acesso informal ao material.
- Dados das questões centralizados em `questoes.json` (schema: `{ disciplinas: [{ id, nome, questoes: [{ id, enunciado, alternativas, respostaCorreta, justificativa }] }] }`), carregado em runtime — permite atualizar o banco de questões sem tocar no HTML/JS.

## Pendências / Próximos Passos
- Nenhuma pendência conhecida no momento. Repositório limpo, `gh-pages` sincronizado com `origin`.

## Comandos Úteis
- Servir localmente para testar: `python3 -m http.server 8791` dentro de `simuladocap-pages/` (não abrir `index.html` direto via `file://`, pois `fetch('questoes.json')` falha por CORS).
- Deploy: qualquer commit/push na branch `gh-pages` do repo `adilsonee/simuladocap` já reflete em produção (GitHub Pages).
- Validar `questoes.json`: `python3 -c "import json; json.load(open('questoes.json', encoding='utf-8'))"`.
