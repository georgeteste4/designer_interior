[Português](README.md) · [English](README.en.md)

# Ateliê de Implantação

O **Ateliê de Implantação** é uma prancheta visual local-first para explorar loteamentos, lotes, casas e ambientes. O MVP permite criar e editar elementos espaciais em diferentes escalas, alternar entre planta e perspectiva, salvar cenas, desfazer operações e exportar o projeto em JSON.

## MVP entregue

- **Navegação multi-escala:** Loteamento, Lote, Edificação e Ambiente.
- **Editor 2D:** lote, via, área verde, edificação e ambiente com posição, dimensões, giro, cor, status, visibilidade e nome editáveis.
- **Estrutura do projeto:** árvore lateral com seleção contextual e métricas de lotes, edificações, área de lotes e área construída.
- **Perspectiva:** vista axonométrica gerada a partir do mesmo modelo espacial, com terrenos, vias, áreas verdes, edificações e ambientes.
- **Operações de projeto:** criar, duplicar, remover, renomear, ocultar, importar, exportar, desfazer e refazer.
- **Apresentação:** cenas salvas por contexto e vista, com abertura posterior.
- **Persistência local:** autosave em `localStorage`, sem login e sem envio automático de dados.
- **Compatibilidade:** o editor de interiores anterior permanece disponível em [`legacy.html`](legacy.html).

## Início rápido

```bash
git clone https://github.com/georgeteste4/designer_interior.git
cd designer_interior
python3 -m http.server 8000
```

Abra `http://localhost:8000`. O servidor local é recomendado para manter o comportamento consistente de importação e assets.

## Formato do projeto

O modelo exportado é versionado em `schemaVersion: 2` e possui `metadata`, `site`, `lots`, `roads`, `greens`, `buildings` e `scenes`. Cada entidade usa medidas em metros e pode ser editada no inspector. O arquivo exportado pode ser usado como backup, fixture ou ponto de partida para futuras integrações CAD/BIM.

## Atalhos

| Atalho | Ação |
| --- | --- |
| `Ctrl/Cmd + Z` | Desfazer |
| `Ctrl/Cmd + Y` ou `Ctrl/Cmd + Shift + Z` | Refazer |
| `Ctrl/Cmd + D` | Duplicar item selecionado |
| `Delete` / `Backspace` | Remover item selecionado |
| `Esc` | Limpar seleção |

## Próximos incrementos

O próximo ciclo pode adicionar polígonos livres e snapping, recuos e índices urbanísticos configuráveis, portas/janelas paramétricas, custos e materiais, renderização Three.js conectada ao modelo novo, DXF/imagem como referência, colaboração e sincronização opcional.

## Desenvolvimento

O MVP é deliberadamente sem build e sem dependências de framework para permitir inspeção e execução imediatas. A entrada é `index.html`, os estilos ficam em `src/styles.css`, a lógica em `src/app.js` e a rota declarada em `public/manus-routes.json`. O arquivo `i18n.json` continua sendo o catálogo configurável do editor legado.

## Licença

Este projeto segue a licença [MIT](LICENSE).
