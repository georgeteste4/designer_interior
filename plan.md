# Plano de evolução — plataforma de projeto de interiores, implantação e loteamentos

## 1. Visão refinada do produto

Transformar o protótipo atual em um **estúdio visual local-first para planejamento espacial**, capaz de trabalhar em múltiplas escalas sem perder a simplicidade do editor atual:

- **Loteamento:** quadras, lotes, vias, calçadas, áreas verdes e equipamentos comuns.
- **Lote:** perímetro, recuos, orientação solar, acessos, topografia simplificada e implantação.
- **Casa/edificação:** pavimentos, paredes, portas, janelas, escadas e cobertura.
- **Ambientes:** móveis, materiais, circulação, iluminação e perspectivas 3D.
- **Apresentação:** cenas 3D, pranchas, imagens, relatório e exportação de dados.

O produto deve funcionar como uma ferramenta de projeto, não apenas como um visualizador: cada elemento precisa ser editável, duplicável, ocultável, agrupável, nomeável e recuperável por histórico.

## 2. Direção de produto

### Usuários principais

1. Designer de interiores e arquiteto que precisa testar alternativas rapidamente.
2. Incorporador ou urbanista que deseja explorar implantação de lotes e casas.
3. Cliente que precisa revisar versões visualmente sem dominar software CAD.
4. Estudante ou profissional autônomo que precisa de uma ferramenta leve e acessível.

### Promessa

> Do lote à perspectiva: uma prancheta visual, editável e compreensível para transformar medidas em decisões espaciais.

### Princípios funcionais

- **Contexto antes da complexidade:** mostrar apenas as ferramentas relevantes ao nível atual: loteamento, lote, edificação, pavimento ou ambiente.
- **Tudo pode ser editado:** geometria, nomes, dimensões, materiais, mobiliário, camadas e metadados.
- **Feedback espacial imediato:** alterações 2D refletem no 3D, nas métricas e nas validações.
- **Local-first e reversível:** autosave, versões, desfazer/refazer, importação/exportação e nenhum bloqueio por conta.
- **Apresentável por padrão:** o mesmo modelo usado para editar deve gerar uma perspectiva e uma prancha legíveis.

## 3. Escopo por fases

### Fase 0 — baseline e controle de versão

- Preservar o app funcional atual.
- Registrar o estado atual em commit.
- Manter `i18n.json` como fonte editável de idiomas, incluindo PT-BR, English, 中文 e 繁體中文.
- Adicionar este plano e uma rotina de commits incrementais.

### Fase 1 — fundação do editor profissional

- Migrar gradualmente o monólito `index.html` para módulos sem reescrever o motor de uma vez.
- Definir um modelo versionado de `Project` com:
  - `site`: lote, orientação, topografia e restrições.
  - `urbanPlan`: lotes, vias, quadras e áreas comuns.
  - `buildings`: edificações e pavimentos.
  - `rooms`: ambientes e polígonos.
  - `elements`: móveis, esquadrias, portas, equipamentos e objetos 3D.
  - `materials`, `layers`, `scenes`, `measurements` e `metadata`.
- Separar estado de domínio, histórico, geometria, renderização e interface.
- Criar esquema de migração para projetos antigos do formato atual.
- Adotar validação de dados na entrada e IDs estáveis.

### Fase 2 — navegação multi-escala

- Criar seletor de contexto: **Loteamento → Lote → Implantação → Pavimento → Ambiente**.
- Permitir alternar entre visão geral e foco local sem perder seleção.
- Incluir breadcrumbs, árvore de projeto e filtros de camadas.
- Adicionar ferramentas de desenho para retângulo, polígono, linha, eixo, guia e área.
- Implementar snapping configurável: grade, vértices, paredes, alinhamento, esquadro e distância.
- Permitir duplicar, agrupar, bloquear, ocultar, renomear e excluir elementos.

### Fase 3 — loteamentos, terrenos e implantação

- Cadastro e edição de lotes por polígono ou dimensões.
- Subdivisão de terreno e geração assistida de lotes por testada/profundidade.
- Vias, calçadas, faixas de recuo, áreas verdes e áreas institucionais.
- Orientação norte, rosa dos ventos e estudos básicos de insolação.
- Recuos configuráveis, taxa de ocupação e índice de aproveitamento.
- Implantação de casas com arrastar, girar, cotar e validar limites.
- Medições de área, testada, profundidade e frente para via.

### Fase 4 — edificações e ambientes

- Paredes por desenho, espessura, altura e tipo.
- Portas e janelas parametrizadas com largura, altura, peitoril e sentido de abertura.
- Ambientes por fechamento de contorno, com área, perímetro e nome.
- Pavimentos, escadas e ligação vertical.
- Biblioteca pesquisável por categoria, favoritos e conjuntos.
- Materiais com cor, textura, fabricante, referência, custo e unidade.
- Regras de circulação, acessibilidade e conflitos com aberturas.

### Fase 5 — perspectivas e apresentação

- Cenas 3D salvas por câmera, ambiente, horário e estilo.
- Câmera orbit, passeio e vistas ortogonais.
- Controle de luz solar, iluminação artificial e materiais.
- Anotações e cotas visíveis por cena.
- Pranchas com título, escala, legenda, norte, carimbo e imagens.
- Exportação PNG, SVG, PDF, JSON e pacote de projeto.
- Relatório com áreas, materiais, custos, alertas e histórico de versões.

### Fase 6 — colaboração e produto conectado (opcional)

- Compartilhamento por arquivo e link.
- Comentários por elemento, ambiente ou lote.
- Papéis de visualização e edição.
- Sincronização remota e histórico de alterações.
- Login e backend somente depois de validar o fluxo local-first.

## 4. Arquitetura técnica proposta

### Estratégia de migração

Usar uma migração incremental, preservando o funcionamento atual a cada etapa:

1. Extrair dados e utilitários do `index.html` para `src/core`.
2. Extrair o catálogo de móveis, materiais e geometrias para `src/domain`.
3. Extrair o renderizador SVG 2D e os adaptadores Three.js para `src/renderers`.
4. Extrair painéis, toolbar, biblioteca e diálogos para `src/ui`.
5. Introduzir TypeScript e Vite quando a separação de módulos estiver estável.
6. Migrar componentes para React apenas se a complexidade de interface justificar; não reescrever o motor geométrico sem necessidade.

### Estrutura alvo

```text
floorplan-3d/
├── index.html                    # entrada da aplicação
├── i18n.json                     # catálogo editável de idiomas
├── public/
│   ├── manus-routes.json         # rotas declaradas do web app
│   └── assets/                   # ícones e assets estáticos
├── src/
│   ├── app/                      # bootstrap, comandos e composição
│   ├── core/                     # estado, histórico, persistência e migrações
│   ├── domain/                   # Project, Site, Lot, Building, Room, Element
│   ├── geometry/                 # polígonos, interseção, snapping, cotas e unidades
│   ├── renderers/
│   │   ├── plan2d/               # SVG/canvas 2D
│   │   └── scene3d/              # Three.js, câmeras e materiais
│   ├── features/                 # auditoria, loteamento, relatório, versões, cenas
│   ├── ui/                       # shell, toolbar, inspector, árvore e diálogos
│   └── styles/                   # tokens, componentes e responsividade
├── tests/                        # testes de domínio e regressão de projetos
├── README.md
├── ideas.md
└── plan.md
```

### Decisões técnicas

- **Local-first:** `localStorage` inicialmente, com exportação JSON como formato de interoperabilidade; IndexedDB quando o tamanho dos projetos crescer.
- **Estado:** store único de domínio com comandos transacionais e histórico de patches/snapshots.
- **Geometria:** unidades internas em milímetros; conversões explícitas para metros, pixels e unidades de apresentação.
- **Renderização:** manter SVG para edição 2D precisa e Three.js para perspectivas; ambos consomem o mesmo modelo.
- **Contratos:** versão do schema, migrações determinísticas e rejeição segura de arquivos inválidos.
- **Acessibilidade:** teclado, foco visível, `aria-pressed`, atalhos documentados, contraste e suporte a toque.
- **Performance:** renderização por camadas, debounce de autosave, memoização de geometria e descarte de objetos Three.js.
- **Privacidade:** nenhum projeto sai do dispositivo sem uma ação explícita de exportação ou futura sincronização.

## 5. Design system

### Movimento

**Estúdio editorial técnico:** uma mistura de prancheta arquitetônica, papel de especificação e ferramenta digital de precisão. A interface deve ser acolhedora para o cliente e confiável para o profissional.

### Princípios visuais

- Clareza editorial: hierarquia forte, textos curtos e espaços de respiro.
- Precisão tátil: cotas, guias, snapping e estados selecionados sempre visíveis.
- Contexto em camadas: o canvas é protagonista; painéis aparecem quando ajudam.
- Calma operacional: animações curtas, feedback direto e nenhuma mudança silenciosa.

### Filosofia de cor

- Marfim e papel quente para reduzir a sensação de software técnico frio.
- Carvão para leitura e contraste estrutural.
- Terracota como ação própria do produto: selecionar, criar e confirmar.
- Teal para medidas, informações e estados de suporte.
- Vermelho e âmbar somente para riscos reais, conflitos e avisos.

### Layout e assinatura

- Shell em três zonas: navegação/contexto, canvas imersivo e inspector contextual.
- Barra de contexto no topo com breadcrumbs e modo atual.
- Minimap/overview em loteamento e planta.
- Assinaturas: cotas terracota, guias teal e cartões de ambiente com borda de papel.

### Interação e movimento

- Desenho começa com um comando explícito e termina com confirmação visual.
- Seleção revela ações próximas, não uma barra cheia permanentemente.
- Transições entre escalas usam 180–280 ms; mudanças de cena usam 350–500 ms.
- Respeitar `prefers-reduced-motion` e manter ações essenciais instantâneas no teclado.

### Tipografia e voz

- Interface: system sans com alta legibilidade.
- Números e cotas: fonte monoespaçada ou tabular para alinhamento.
- Títulos curtos, verbos de ação e linguagem tranquila: “Criar lote”, “Ajustar recuo”, “Salvar versão”.

### Essência de marca

**Posicionamento:** uma prancheta visual que conecta urbanismo, arquitetura e interiores em uma única experiência compreensível.

**Personalidade:** precisa, acolhedora, exploratória.

**Cor proprietária:** terracota `#B5653A`, usada com parcimônia para tornar ações e seleção reconhecíveis.

## 6. Estratégia de qualidade

- Cada etapa deve preservar a abertura do app e a importação de projetos existentes.
- Validar JSON, sintaxe dos módulos, migrações e `git diff --check` antes de cada commit.
- Criar testes de geometria para interseção, áreas, recuos, portas e snapping.
- Criar fixtures de projetos: apartamento atual, lote retangular, loteamento simples e casa com dois pavimentos.
- Testar fluxos principais em 2D, 3D, toque, teclado e telas estreitas.
- Não publicar nem alterar integrações externas sem uma instrução explícita; o repositório GitHub será atualizado com commits pequenos e rastreáveis.

## 7. Política de versionamento do trabalho

- `master` permanece como linha estável do repositório atual.
- Cada entrega funcional terá um commit com escopo claro, por exemplo:
  - `chore: establish project baseline`
  - `feat: add project context model`
  - `feat: add lot and subdivision editor`
  - `refactor: extract 2d renderer`
  - `fix: preserve legacy project migration`
- Nunca fazer force-push nem reescrever histórico.
- Após cada conjunto validado: commit, `git fetch`, integração segura se necessário e push para `origin/master`.
- O estado do Git será reportado junto com cada atualização para manter rastreabilidade.
