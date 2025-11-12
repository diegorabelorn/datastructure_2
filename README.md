# Análise da Árvore Geradora Mínima (MST) de Pontos de Interesse (Delegacias, bases policiais) no Nordeste

## Visão Geral do Projeto

Este projeto utiliza a Teoria dos Grafos e dados de mapas de ruas reais (OpenStreetMap, via OSMnx) para determinar a infraestrutura viária mínima necessária para conectar um conjunto de Pontos de Interesse (POIs) específicos em grandes cidades.

## Objetivo

Estimar o comprimento mínimo de vias reais (km) necessário para interligar as delegacias e bases policiais de nove capitais do Nordeste brasileiro, utilizando o conceito de Árvore Geradora Mínima (MST) sobre as rotas de menor distância (A*/Dijkstra).
O resultado é uma métrica robusta em quilômetros que estima a eficiência logística da rede urbana para a interconexão mínima.

## Metodologia

A análise foi conduzida seguindo cinco etapas principais:

**1- Modelagem do Grafo Viário**: O grafo de ruas de cada cidade foi baixado via OSMnx (network_type='drive') e projetado para um sistema de coordenadas métricas (UTM) para garantir a precisão dos cálculos de distância em metros.

**2- Identificação de POIs**: Delegacias de Polícia foram localizadas (amenity=police) e mapeadas para os nós viários mais próximos de cada grafo.

**3- Cálculo da Rota Mínima (A_star / Dijkstra)**: Para cada par de POIs, a rota mais curta e seu custo total (distância em km) foram calculados usando o algoritmo Dijkstra (que atua como A* no grafo de distâncias).

**4- Construção do Grafo Completo e MST**: Um grafo completo foi construído onde os vértices são os POIs e os pesos das arestas são as distâncias de rota calculadas no Passo 3. A MST foi então calculada sobre este grafo para encontrar a soma mínima de distâncias de vias (MST Total em km) que interliga todos os pontos.

**5- Visualização e Comparação**: Para cada cidade, o subgrafo da MST foi plotado sobre a malha viária para visualização. As métricas-chave foram consolidadas para análise comparativa.

## Resultados e métricas chave
O resultado da análise em 9 capitais do Nordeste está consolidado no arquivo *mst_comparison_table.csv*.

#### Métricas Calculadas:

* **MST Total (km)**: O comprimento total de vias que compõe a rede de interconexão mínima.

* **Média KM/POI (km/POI)**: Métrica de Densidade Logística (MST Total / POIs Conectados). Valores menores indicam POIs mais compactados e maior eficiência de interconexão.

* **Desvio Padrão (km)**: Métrica de Uniformidade da Rede, calculada sobre os comprimentos individuais das arestas da MST em cada cidade. Um DP baixo sugere que a rede é composta por segmentos de comprimento semelhante.


### Tabela Comparativa de Métricas (Capitais do Nordeste)
| Cidade | POIs (Buscados) | POIs Conectados (n) | MST Total (km) | Média KM/POI (km/POI) | Média KM/Aresta MST | Desvio Padrão (km) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Salvador-BA** | 120 | 115 | 190,00 | 1,65 | 1,67 |1,165 |
| **Recife-PE** | 88 | 85 | 110,00 | 1,29 | 1,31 | 0,935 |
| **Fortaleza-CE** | 79 | 74 | 119,34 | 1,61 | 1,63 | 0,878 |
| **Natal-RN** | 100 | 69 | 67,30 | 0,98 | 0,99 | 0,557 |
| **Maceió-AL** | 55 | 50 | 75,00 | 1,50 | 1,53 | 0,589 |
| **São Luis-MA** | 41 | 39 | 100,69 | 2,58 | 2,65 | 1,037 |
| **Aracaju-SE** | 40 | 38 | 50,00 | 1,32 | 1,35 | 0,593 |
| **Teresina-PI** | 38 | 35 | 97,13 | 2,78 | 2,86 | 0,892 |
| **João Pessoa-PB** | 25 | 23 | 46,76 | 2,03 | 2,13 | 0,604 |

> **Legenda:** $V$ = Número de Vértices (Nós); $E$ = Número de Arestas.

## Análise Crítica Consolidada

A análise da MST revelou insights importantes sobre a topologia e a logística urbana das capitais:

* **Variação de Densidade**: Cidades como **Natal (0.9754 km/POI)** e **Recife (1.2941 km/POI)** apresentaram a maior eficiência logística. Isso significa que suas delegacias e bases policiais estão mais próximas umas das outras na rede viária do que, por exemplo, em **Teresina (2.7751 km/POI)** ou **São Luís (2.5818 km/POI)**, onde as distâncias médias entre as unidades são significativamente maiores, provavelmente devido a extensas áreas periurbanas ou alta segmentação geográfica (como os rios que circundam Teresina e as áreas costeiras de São Luís).

* **Uniformidade da Resposta**: O **Desvio Padrão** mede a previsibilidade da rede mínima. **Natal (0,557 km)** e **Aracaju (0,593 km)**, com o DP mais baixo, possuem a rede de interconexão mais uniforme. Cidades com DP alto como **Salvador (1,165 km)** e **São Luís (1,037 km)** indicam que a interconexão depende de poucos segmentos de rota muito longos que atuam como gargalos, elevando a variabilidade da logística.

* **Limitações do Método**: A escolha de "delegacia" tende a agrupar os POIs em áreas de maior densidade populacional. No entanto, o método está limitado pela qualidade dos dados OSMnx e pela própria topologia urbana. A diferença entre POIs Buscados e Conectados (ex: 31 POIs perdidos em Natal) é uma limitação direta do grafo viário, refletindo imperfeições na conectividade da malha urbana registrada.

## Arquivos de Saída

* **mst_comparison_table.csv**: Contém a tabela de resultados consolidada.

* **MST_<Cidade>_<Estado>_Brazil.jpeg**: Nove arquivos JPEG, cada um mostrando a plotagem do subgrafo da MST (rotas em vermelho) sobre a malha viária da cidade, com os POIs marcados em azul.

## Links
* **Youtube**: https://youtu.be/sVHl4S-f98M
* **Substack**: https://open.substack.com/pub/diegorabelos/p/o-esqueleto-logistico-das-cidades?r=1lh0a7&utm_campaign=post&utm_medium=web&showWelcomeOnShare=true
