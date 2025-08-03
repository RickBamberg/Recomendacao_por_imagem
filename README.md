# Sistema de Recomendação de Produtos por Similaridade Visual

## Resumo do Projeto

Este projeto implementa um Sistema de Recomendação que sugere produtos com base em sua **aparência visual**. Diferente de sistemas tradicionais que se baseiam em texto (marca, modelo, preço), este motor utiliza técnicas de Deep Learning para "ver" as imagens e encontrar itens com formato, cor e textura similares.

O objetivo principal era cumprir o desafio do curso de Machine Learning, desenvolvendo um sistema capaz de receber a imagem de um produto e retornar uma lista de outros produtos visualmente parecidos, extraídos de um banco de dados com 4 categorias: `smartphone`, `smartwatch`, `notebook` e `tablet`.

---

## Como Funciona: A Pipeline Técnica

O sistema funciona em três etapas principais:

1.  **Vetorização de Imagens (Extração de Características):**
    *   Cada imagem do nosso banco de dados é processada por uma rede neural convolucional pré-treinada (**Google MobileNetV2**), carregada através do TensorFlow Hub.
    *   Em vez de classificar a imagem, nós extraímos a última camada de características da rede. O resultado é um **vetor** (uma lista de 1280 números) que serve como uma "assinatura visual" da imagem. Imagens parecidas terão vetores matematicamente próximos.

2.  **Indexação para Busca Rápida:**
    *   Comparar um vetor de busca com milhares de outros um a um seria muito lento. Para resolver isso, utilizamos a biblioteca **Annoy (Approximate Nearest Neighbors Oh Yeah)**, desenvolvida pelo Spotify.
    *   Todos os vetores de imagem são inseridos em um índice Annoy. Essa estrutura de dados organiza os vetores de forma otimizada, permitindo buscas por "vizinhos mais próximos" de forma quase instantânea.

3.  **Busca por Similaridade:**
    *   Quando uma nova imagem é fornecida (a imagem de busca), ela passa pelo mesmo processo de vetorização.
    *   O vetor resultante é então usado para consultar o índice Annoy, que retorna os vetores mais próximos (mais similares) que ele contém.
    *   As imagens correspondentes a esses vetores são exibidas como recomendação.

---

## Estrutura de Arquivos e Pastas

Para que o sistema funcione, as imagens devem ser organizadas em uma estrutura de pastas específica, onde cada subpasta representa uma categoria de produto.

```
/projeto-recomendacao/
|
├── meus_produtos/
│   ├── smartphone/
│   │   ├── iphone_preto_1.jpg
│   │   ├── galaxy_azul_1.jpg
│   │   └── ...
│   ├── smartwatch/
│   │   ├── applewatch_branco_1.jpg
│   │   └── ...
│   ├── notebook/
│   │   ├── macbook_cinza_1.jpg
│   │   └── ...
│   └── tablet/
│       ├── ipad_preto_1.jpg
│       └── ...
|
├── product_recommender.ipynb  # (O nosso código Python)
└── README.md                  # (Este arquivo)
```

---

## Exemplo de Resultado

Abaixo, um exemplo de saída do sistema. Uma imagem de um smartphone é usada como busca, e o sistema recomenda outros produtos com características visuais similares (como cor, formato e brilho da tela).

**Imagem de Busca:**

![Imagem de um smartphone usado para a busca](URL_DA_SUA_IMAGEM_DE_BUSCA_AQUI)  <!-- Substitua por um print real -->

**Resultados Recomendados:**

![Resultados da busca por similaridade](URL_DO_SEU_PRINT_COM_OS_RESULTADOS_AQUI) <!-- Substitua por um print real -->

---

## Tecnologias Utilizadas

*   **Linguagem:** Python 3
*   **Deep Learning:** TensorFlow 2 e TensorFlow Hub
*   **Indexação e Busca:** Annoy
*   **Processamento de Imagem:** Pillow (PIL)
*   **Manipulação de Dados:** NumPy
*   **Visualização:** Matplotlib