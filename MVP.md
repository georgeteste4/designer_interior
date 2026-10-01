# MVP — Ateliê de Implantação

## Objetivo

Entregar uma primeira experiência utilizável para criar um estudo de implantação sem depender de CAD: o usuário abre um projeto, navega por escalas, cria elementos, ajusta propriedades, visualiza uma perspectiva e salva uma cena.

## Fluxo principal entregue

1. Abrir o Ateliê e receber um projeto exemplo editável.
2. Navegar entre loteamento, lote, edificação e ambiente.
3. Criar lote, via, área verde, edificação ou ambiente pela Biblioteca.
4. Selecionar qualquer entidade na planta ou na árvore.
5. Ajustar nome, posição, dimensões, giro, cor, status e visibilidade no Inspector.
6. Duplicar, remover, desfazer e refazer alterações.
7. Alternar para Perspectiva e salvar a vista como cena.
8. Exportar o projeto JSON ou importar um backup.
9. Abrir o editor legado para detalhar interiores quando necessário.

## Limites conscientes do MVP

A geometria inicial usa retângulos paramétricos e unidades em metros. O MVP ainda não pretende substituir um CAD: polígonos livres, curvas, snapping avançado, validação legal de recuos, topografia real, materiais físicos, renderização fotorealista e colaboração remota ficam para ciclos seguintes. O modelo já reserva espaço para esses recursos sem quebrar o formato exportado.
