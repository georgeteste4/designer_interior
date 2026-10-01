# Projeto de planta e design de interiores

Ferramenta de design de interiores totalmente frontend: organize móveis em uma planta 2D, faça medições, marque paredes não estruturais para remoção e alterne para uma cena 3D em Three.js com visão em perspectiva ou modo passeio. O aplicativo continua concentrado no `index.html`, sem etapa de build.

## Funcionalidades

**Planta 2D**

- Exibição nas escalas originais 1:60 e 1:100, com dimensões em mm
- Biblioteca com mais de 60 móveis e eletrodomésticos, organizada por ambientes
- Arrastar, mover, girar, redimensionar e encaixar móveis nas paredes
- Ferramenta de medição com encaixe nas paredes e trava horizontal/vertical
- Marcação de paredes não estruturais para demolição; paredes estruturais são protegidas
- Camadas para cotas, nomes dos ambientes, móveis, grade e paredes estruturais

**Cena 3D**

- Vistas em perspectiva, inclinada e superior; clique em um ambiente para voar até ele
- Modo passeio com WASD + mouse no desktop e joystick virtual em telas sensíveis ao toque
- Alternância entre paredes com altura total e paredes cortadas
- Controle de horário da luz solar e iluminação noturna
- Modelos detalhados de móveis, com portas, puxadores, materiais metálicos, cerâmicos e tecidos
- Seleção e movimentação de móveis em 3D sincronizadas com a planta 2D

**Projeto e estatísticas**

- Cálculo automático das áreas dos ambientes e da área útil interna
- Troca de revestimento por ambiente e estimativa de custo com 5% de perda
- Desfazer / refazer e salvamento automático no `localStorage`
- Interface multilíngue com **Português (Brasil) como padrão**, English e 中文
- Exportação de imagem PNG, relatório imprimível para PDF e importação / exportação da planta em JSON
- Auditoria automática de conflitos entre móveis, circulação de portas e itens fora de ambientes
- Versões salvas localmente, com restauração e exclusão

## Início rápido

```bash
git clone https://github.com/wy51ai/floorplan-3d.git
cd floorplan-3d
python3 -m http.server 8000
```

Abra [http://localhost:8000](http://localhost:8000) no navegador. O servidor local é recomendado porque permite que o aplicativo carregue o arquivo externo `i18n.json`. A abertura direta do `index.html` via `file://` continua funcionando usando o catálogo PT-BR integrado como fallback.

> O Three.js é carregado pelo jsDelivr CDN. É necessária conexão com a internet para abrir a cena 3D.

## Configuração de idiomas

Todas as traduções editáveis ficam em [`i18n.json`](i18n.json). O arquivo define:

- `defaultLocale`: idioma inicial para novos usuários (`pt-BR`)
- `fallbackLocale`: idioma usado quando uma chave não existe no idioma escolhido (`en`)
- `storageKey`: chave usada para lembrar a preferência no navegador
- `locales`: idiomas disponíveis, cada um com `label`, `htmlLang`, `strings` e `names`

Os idiomas disponíveis no catálogo são `pt-BR`, `en` e `zh-CN`. Para adicionar outro idioma, copie um bloco em `locales`, altere o código e preencha as mesmas chaves de `strings` e `names`. Chaves ausentes usam o idioma de fallback.

Exemplo mínimo:

```json
{
  "es": {
    "label": "Español",
    "htmlLang": "es",
    "strings": {
      "language_selector": "Idioma",
      "floor_plan_filename": "plano-interior"
    },
    "names": {
      "客厅": "Sala de estar"
    }
  }
}
```

Depois de editar o JSON, recarregue o servidor local e escolha o idioma no seletor do topo. A preferência fica salva no navegador.

## Atalhos

| Tecla | Ação |
| --- | --- |
| `T` | Alternar 2D / 3D |
| `V` / `M` / `X` | Selecionar / medir / demolir paredes não estruturais |
| `R` / `Shift+R` | Girar o móvel selecionado 90° no sentido horário / anti-horário |
| `Delete` / `Backspace` | Excluir o móvel selecionado |
| `Ctrl/⌘ + D` | Duplicar o móvel selecionado |
| `Ctrl/⌘ + Z`, `Ctrl/⌘ + Shift + Z` | Desfazer / refazer |
| `F` | Ajustar à janela |
| `+` / `-` | Ampliar / reduzir zoom |
| `[` / `]` | Recolher / expandir a biblioteca e o painel direito |
| `Shift + F` | Tela cheia |
| `Esc` | Cancelar a operação atual |
| Passeio | `WASD` / setas, `Shift`, `E` para mover, correr e abrir portas |

## Melhorias de UX e design system

A interface agora usa tokens centralizados de cor, espaçamento, raios, sombras e foco para manter os controles consistentes. Botões, campos e seletores têm estados de hover, foco visível, pressionado e desabilitado, com alvos maiores em telas sensíveis ao toque.

A biblioteca de móveis ganhou busca por nome, ambiente e categoria, incluindo estado vazio orientando a próxima tentativa. O cabeçalho informa quando o projeto foi salvo no dispositivo e inclui uma ajuda rápida com atalhos e controles de toque. Os toggles de 2D/3D, ferramentas, camadas e opções 3D também expõem seus estados para tecnologias assistivas.

## Roadmap recomendado

| Prioridade | Funcionalidade | Benefício principal | Complexidade |
| --- | --- | --- | --- |
| Alta | Presets por ambiente e duplicação de cômodos | Acelera a criação de alternativas | Média |
| Média | Favoritos e conjuntos de móveis salvos | Reduz o tempo de repetição em projetos recorrentes | Baixa |
| Média | Comparação lado a lado entre versões | Facilita decisões e revisões com clientes | Média |
| Média | Biblioteca de materiais com fornecedores configuráveis | Aproxima o protótipo de um fluxo profissional de especificação | Média |
| Futura | Colaboração com comentários e histórico | Permite revisão entre designer e cliente | Alta |
| Futura | Importação de medidas por imagem/DXF e alternativas assistidas | Reduz trabalho manual em plantas existentes | Alta |

## Stack

- HTML, CSS e JavaScript nativos, sem framework e sem build
- SVG para a planta 2D
- [Three.js](https://threejs.org/) r160 para a cena 3D, com OrbitControls, PointerLockControls, RoundedBoxGeometry, RoomEnvironment e CSS2DRenderer
- `localStorage` para persistência local da planta e da preferência de idioma
- `i18n.json` para o catálogo editável de idiomas

## Personalização da planta

Os dados da planta ficam no `index.html`:

- `ROOMS`: polígonos, nomes e revestimentos padrão
- `WALLS` / `WINS`: paredes, janelas e vãos
- `MATS`: materiais e preços
- `LIB`: biblioteca de móveis, dimensões e cores
- `buildFurniture()`: modelos 3D dos móveis

Altere esses dados para adaptar a ferramenta a outra planta. Mantenha os nomes de dados em `i18n.json` para que a interface continue traduzindo os ambientes, materiais e móveis.
