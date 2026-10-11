# Plano de reconstrução integrada — Ateliê de Interiores e Implantação

## Diagnóstico

A versão atual possui dois fluxos tecnicamente separados:

- O editor de interiores original tem mais profundidade: planta 2D, biblioteca de móveis, cotas, paredes, materiais, histórico, exportação, Three.js e modo passeio.
- O módulo de implantação tem a navegação multi-escala correta, mas ainda usa uma representação visual simplificada e não compartilha a mesma linguagem arquitetônica.

A reconstrução não deve substituir o editor original. Deve evoluí-lo para um único produto, com continuidade entre urbanismo, implantação, arquitetura e interiores.

## Visão do produto

> **Do terreno à perspectiva:** uma prancheta visual profissional, editável e local-first para loteamentos, casas, ambientes e interiores.

O usuário deve conseguir:

1. Criar ou importar um projeto.
2. Trabalhar em loteamento, lote, edificação, pavimento ou ambiente.
3. Alterar geometrias, cotas, materiais, mobiliário e propriedades.
4. Alternar entre planta 2D, perspectiva 3D e passeio.
5. Gerar cenas, imagens, relatórios e arquivos de projeto.
6. Voltar a qualquer estado por histórico ou versão salva.

## Princípios não negociáveis

- O editor original permanece funcional durante toda a migração.
- O canvas é o protagonista; painéis devem ser contextuais.
- O mesmo modelo de dados alimenta 2D, 3D, métricas, auditoria e exportação.
- Nenhum retângulo genérico deve substituir uma entidade arquitetônica quando ela puder ser representada com precisão.
- PT-BR é o idioma padrão; idiomas adicionais continuam configuráveis em `i18n.json`.
- Cada etapa pequena deve ser publicada e versionada no GitHub.

## Arquitetura de destino

```text
src/
├── app/
│   ├── bootstrap.js          # inicialização, rotas e contexto atual
│   └── commands.js           # comandos transacionais da interface
├── core/
│   ├── store.js              # estado único do projeto
│   ├── history.js             # undo/redo e snapshots
│   ├── persistence.js         # localStorage/IndexedDB e autosave
│   └── migrations.js          # schema v1/v2 → schema atual
├── domain/
│   ├── project.js             # Project, Site, UrbanPlan
│   ├── architecture.js       # Building, Floor, Room, Wall, Opening
│   ├── furniture.js           # móveis, equipamentos e objetos
│   └── materials.js           # materiais, acabamentos e custos
├── geometry/
│   ├── units.js               # mm, m, px e escala de apresentação
│   ├── polygons.js            # área, perímetro, interseção e offsets
│   ├── snapping.js            # grade, parede, vértice, alinhamento
│   └── measurements.js        # cotas, testada, recuos e distâncias
├── renderers/
│   ├── plan2d/                # SVG 2D do editor original evoluído
│   └── scene3d/               # Three.js, câmeras, luz e materiais
├── features/
│   ├── urban-plan.js          # lotes, vias, quadras e áreas comuns
│   ├── building-editor.js     # paredes, portas, janelas e pavimentos
│   ├── audit.js               # conflitos e regras espaciais
│   ├── scenes.js              # câmeras e apresentações
│   └── reports.js             # pranchas, PDF e resumo técnico
├── ui/
│   ├── shell.js               # toolbar, breadcrumbs e navegação
│   ├── inspector.js           # painel de propriedades
│   ├── library.js             # biblioteca pesquisável
│   └── dialogs.js             # projeto, cenas, ajuda e exportação
└── styles/
    ├── tokens.css             # design system
    └── components.css         # componentes e responsividade
```

A migração será gradual. O `legacy.html`/editor original é a referência funcional durante a extração; o `implantacao.html` é uma área experimental até ser absorvido pelo shell único.

## Fases de execução

### Fase 0 — baseline e proteção

**Objetivo:** manter o que já funciona e tornar a evolução reversível.

- Manter a entrada principal como editor rico 2D/3D.
- Manter `/implantacao.html` acessível sem apresentá-lo como produto independente.
- Garantir `i18n.json`, `manus-routes.json` e importação/exportação.
- Registrar cada alteração em commit pequeno na `main`.
- Criar fixtures de apartamento, lote, casa e loteamento.

**Entrega:** baseline publicado e restaurável.

### Fase 1 — shell único e contexto espacial

**Objetivo:** unificar as duas experiências sem mexer primeiro na geometria.

- Criar uma navegação única: `Loteamento → Lote → Implantação → Pavimento → Ambiente`.
- Incorporar biblioteca, inspector, árvore, cenas e status em um shell comum.
- Adicionar breadcrumbs e botão de retorno entre escalas.
- Preservar as ferramentas existentes do editor de interiores.
- Fazer o módulo de implantação abrir o mesmo projeto local.

**Entrega:** duas escalas no mesmo produto, sem duplicação de projeto.

### Fase 2 — modelo de dados e migração

**Objetivo:** fazer 2D e 3D consumirem o mesmo domínio.

- Definir `schemaVersion: 3`.
- Modelar `site`, `urbanPlan`, `lots`, `roads`, `greens`, `buildings`, `floors`, `rooms`, `walls`, `openings`, `furniture`, `materials` e `scenes`.
- Criar migração determinística dos projetos atuais.
- Validar IDs, dimensões, limites, unidades e referências entre entidades.
- Preservar projetos salvos no formato anterior.

**Entrega:** projeto único, versionado e compatível com arquivos existentes.

### Fase 3 — implantação arquitetônica real

**Objetivo:** substituir a prancheta de retângulos por uma ferramenta de implantação útil.

- Terreno com perímetro editável.
- Lotes por polígono, testada e profundidade.
- Divisas, muros, recuos e faixas de proteção.
- Vias, calçadas, acessos e portões.
- Áreas verdes e equipamentos comuns.
- Rosa dos ventos e orientação solar.
- Cotas de frente, profundidade e distância.
- Snapping de vértice, linha, grade, alinhamento e distância.
- Validação de edificação fora do lote ou dentro do recuo.

**Entrega:** implantação com leitura arquitetônica e regras básicas de projeto.

### Fase 4 — edificação conectada ao editor original

**Objetivo:** ligar a casa implantada ao detalhamento de interiores.

- Edificação com footprint, pavimentos e altura.
- Entrada da edificação abre sua planta interna no mesmo projeto.
- Paredes desenháveis com espessura e altura.
- Portas e janelas parametrizadas.
- Ambientes fechados por contorno com área e perímetro.
- Escadas e ligação entre pavimentos.
- Móveis e equipamentos da biblioteca original.
- Regras de circulação e conflitos com aberturas.

**Entrega:** a casa deixa de ser um retângulo e passa a ser um modelo arquitetônico navegável.

### Fase 5 — 3D e apresentação profissional

**Objetivo:** transformar o modelo em apresentação, não apenas edição.

- Render 3D da implantação e da edificação com Three.js.
- Câmera orbit, superior, inclinada e passeio.
- Materiais coerentes entre 2D e 3D.
- Luz solar, noite e iluminação interna.
- Cenas com câmera, contexto, camadas e horário.
- Pranchas com título, escala, norte, legenda e carimbo.
- Exportação PNG, SVG, PDF, JSON e pacote de projeto.
- Relatório com áreas, materiais, custos e alertas.

**Entrega:** projeto apresentável para cliente, estudo ou documentação.

### Fase 6 — colaboração opcional

Somente depois de validar o fluxo local-first:

- Compartilhamento por arquivo ou link.
- Comentários por lote, ambiente ou elemento.
- Papéis de visualização e edição.
- Sincronização remota e histórico colaborativo.

## Design system da reconstrução

### Direção

**Estúdio editorial técnico:** prancheta arquitetônica, papel de especificação e ferramenta digital de precisão.

### Regras visuais

- Fundo papel quente, carvão para estrutura e terracota para ações.
- Teal para cotas, guias e dados de apoio.
- Canvas sempre maior que o painel.
- Ferramentas agrupadas por contexto e não por implementação.
- Estados selecionado, bloqueado, oculto e inválido claramente distintos.
- Rótulos em escala real do desenho; nenhuma tipografia pode dominar o canvas.
- Painéis com densidade profissional, mas sem excesso de controles simultâneos.
- Animações curtas de 180–280 ms; respeitar `prefers-reduced-motion`.

### Componentes centrais

- Topbar com projeto, idioma, autosave e ações globais.
- Context rail com níveis espaciais.
- Toolbar contextual por modo.
- Canvas com minimap, norte, escala e coordenadas.
- Inspector dividido em propriedades, geometria, materiais e documentação.
- Biblioteca com busca, categorias, favoritos e drag-and-drop.
- Árvore hierárquica com visibilidade e bloqueio.
- Dialog de cenas, versões, auditoria e exportação.

## Critérios para cada entrega

Uma fase só será considerada concluída quando:

- O editor original continuar abrindo.
- Um projeto existente continuar carregando.
- Importação e exportação permanecerem compatíveis.
- O novo recurso funcionar em PT-BR.
- A interface não criar uma segunda linguagem visual incoerente.
- O deploy do GitHub Pages terminar com sucesso.
- Houver commit pequeno, descritivo e sincronizado na `main`.

## Ordem recomendada quando houver capacidade de implementação

1. Shell único e navegação de contexto.
2. Modelo `schemaVersion: 3` e migração.
3. Implantação com terreno, lotes, vias, recuos e snapping.
4. Conexão da edificação ao editor interno original.
5. Polígonos, paredes, portas e janelas.
6. Integração 3D e cenas.
7. Pranchas, relatório e colaboração opcional.

A próxima implementação deve começar pela **Fase 1**, não por uma reescrita total. Assim cada ciclo produz uma melhoria visível, preserva o editor original e mantém o site publicável.