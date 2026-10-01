# Brief de design — melhorias de UX

## Direção aceita

**Estúdio editorial quente**: preservar o caráter técnico e acolhedor do app atual, usando uma base de papel marfim, texto carvão, terracota como ação principal e teal como apoio. A interface deve parecer uma prancheta de projeto bem organizada, não um painel corporativo genérico.

## Princípios

- **Clareza antes da densidade:** revelar controles por contexto e reduzir ruído visual sem esconder ações essenciais.
- **Confiança operacional:** deixar evidente o estado do projeto, o que foi salvo e o que pode ser desfeito.
- **Feedback direto:** cada interação importante deve produzir estado visual, foco, mensagem ou atualização de conteúdo.
- **Multimodalidade:** mouse, toque, teclado e leitores de tela devem encontrar alvos e estados equivalentes.
- **Progressão natural:** biblioteca → posicionamento → ajuste no painel → visualização 3D → exportação.

## Sistema visual

- Tokens de cor, espaço, raio, sombra, tipografia e foco em `:root`.
- Botões com estados consistentes: padrão, hover, focus-visible, ativo, primário, perigo e desabilitado.
- Campos de busca e inputs com altura, borda e foco compatíveis com os botões.
- Status de persistência e toast usando linguagem curta e acionável.
- Responsividade mantida: no toque, alvos maiores; em telas estreitas, painéis continuam sendo drawers.

## Melhorias implementadas nesta rodada

1. Feedback de salvamento automático no cabeçalho.
2. Busca rápida na biblioteca de móveis, com mensagem de estado vazio.
3. Ajuda contextual com atalhos e controles essenciais.
4. Estados `aria-pressed`, foco visível e labels para controles interativos.
5. Design tokens e estados de componentes consolidados.
6. Auditoria automática de conflitos, circulação de portas e itens fora dos ambientes.
7. Relatório imprimível com planta, áreas, materiais, custos e diagnóstico.
8. Versões salvas localmente com restauração, exclusão e suporte a desfazer.

## Próximas oportunidades

- Favoritos e conjuntos de móveis salvos por ambiente.
- Presets de layout por cômodo e duplicação de ambientes.
- Comparação lado a lado de versões do projeto.
- Colaboração com comentários por ambiente e histórico de alterações.
- Biblioteca de materiais com fornecedores e links configuráveis.
- Importação de medidas por imagem/DXF e geração assistida de alternativas.

## Voz da interface

Curta, objetiva e tranquilizadora. Preferir verbos de ação (“Buscar”, “Salvar”, “Ajustar”, “Desfazer”) e mensagens que expliquem o próximo passo (“Nenhum móvel encontrado. Tente outro termo.”).
