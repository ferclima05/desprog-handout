Segmentação por Watershed com Marcadores
======

O que é segmentação?
---------

Explicação sobre o que é o problema da segmentação

??? Atividade

Veja a imagem a seguir e diga pense em quais pixels pertencem a x e quais pertencem a y 

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

---

Tresholding
---

Vamos ver como funciona um algorítmo mais simples para segmentação

1. Converter a imagem para escala de cinza (caso seja uma imagem colorida). Isso simplifica a segmentação, já que se trabalha apenas com uma única camada de intensidade;

2. Escolher um valor de limiar (threshold): Este valor determina a divisão entre os pixels do fundo e os do objeto. Os pixels com intensidade superior ao valor de limiar são classificados como parte do objeto, enquanto os pixels abaixo desse valor são classificados como fundo;

3. Aplicar o limiar: Para cada pixel da imagem, se a sua intensidade for maior que o limiar, ele é classificado como um objeto (por exemplo, valor 1), caso contrário, como fundo (valor 0).

??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

---

K-means
---

Um próximo passo mais complexo para a segmentação de imagens, após a limiarização, seria usar o algoritmo de k-means. O k-means é um algoritmo de agrupamento que pode ser usado para segmentar a imagem em diferentes regiões (clusters), com base nas características dos pixels, como a intensidade de cor ou de brilho. Ele pode lidar melhor com imagens que possuem mais variabilidade de cores e intensidades, algo que a limiarização simples não consegue resolver de forma eficaz.

1. Inicialização: Você escolhe o número de clusters (k) que deseja criar na imagem. Este número pode ser ajustado conforme a necessidade, dependendo de quantas regiões distintas você espera identificar na imagem;

2. Atribuição de pixels aos clusters: O algoritmo calcula a distância (geralmente a distância euclidiana) entre cada pixel e os centróides (médias) dos clusters. Cada pixel é então atribuído ao cluster cujo centróide está mais próximo;

3. Recalcular os centróides: Após todos os pixels terem sido atribuídos a um cluster, os centróides dos clusters são recalculados com base na média das intensidades dos pixels de cada cluster;

4. Iteração: Os passos 2 e 3 são repetidos até que os centróides dos clusters se estabilizem (ou seja, não mudem mais significativamente entre as iterações).

??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

---

Watershed
---

Vamos ver como funciona watershed

??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

---

Watershed com Marcadores
---

Introduzindo a ideia dos marcadores

??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

---

Complexidade do algoritmo
---

Para determinar a complexidade do algoritmo de segmentação por Watershed com marcadores, precisamos pensar em como cada pixel da imagem é “inundado” e rotulado ao longo da execução.

![](image_3D.png)

Como vimos na explicação do algoritmo, o procedimento consiste em extrair o pixel de menor valor de gradiente da fila de prioridade, rotulá-lo com o valor correspondente e inserir os vizinhos ainda não inundados na fila de prioridade.

No exemplo acima, a fila de prioridade foi baseada na estrutura de **buckets**, pois o mapa de gradiente foi normalizado para valores inteiros entre 0 e 255 (mesma faixa dos níveis de cinza dos pixels), tornando essa abordagem tanto mais simples de implementar quanto mais eficiente em desempenho, pois é mais rápida do que se fosse utilizado heap binário.

Nesse processo, duas operações dominam o custo total: a manipulação da fila de prioridade e a verificação dos vizinhos de cada pixel. Tente pensar qual a complexidade de cada uma das operações.

??? Exercício

Pense qual é a complexidade da manipulação da fila de prioridade e qual a complexidade da verificação dos vizinhos de cada pixel.

::: Gabarito
A manipulação da fila de prioridade, baseada na estrutura de buckets, ocorre da seguinte forma: 
* Acessa o bucket número g (array g) - **O(1)**;
* Adiciona ou retira o pixel da fila desse bucket - **O(1)**.

Não há necessidade de comparar chaves nem de reorganizar a estrutura, então cada inserção ou remoção é sempre feita em tempo constante.

Ao verificar os vizinhos de um pixel, você sempre faz um número fixo de checagens:

* **4-conectividade**: testa até 4 vizinhos (cima, baixo, esquerda, direita).

* **8-conectividade**: testa até 8 vizinhos (inclui diagonais).

Como esse número de vizinhos não cresce com o tamanho da imagem **𝑁**, cada pixel gera **O(1)** operações de vizinhança. No total, para **𝑁** pixels, isso dá **O(N)**, mas por pixel é sempre **O(1)**.
:::

???

??? Exercício

Mas e se ao invés de utilizar a estrutura de buckets, fosse utilizada a estrutura de heap binário? Qual seria a complexidade nesse caso?

::: Gabarito
Nesse caso, ao usar um **heap binário** em vez de buckets, cada inserção (push) e extração (pop) custa **O(log N)**, pois envolve operações de heapify (up ou down) que, no pior caso, percorrem toda a altura da árvore binária completa, o que corresponde a aproximadamente **log N** comparações e trocas.
:::

???

Comparação com os outros algoritmos
---

| Algoritmo                      | Complexidade Geral    |
|--------------------------------|-----------------------|
| **Thresholding**               | O(N)                  |
| **Watershed (sem marcadores)** | O(N)                  |
| **Watershed com marcadores**   | O(N)                  |
| **k-means**                    | O(N·k·I)              |

- **N** = número de pixels da imagem  
- **k** = número de clusters
- **I** = número de iterações até convergência  

**Observação:**  
- Nesta tabela consideramos a versão de Watershed que usa **buckets**, reduzindo seu custo para **O(N)**.  
- Se, em vez de buckets, fosse utilizado um **heap binário**, a complexidade de Watershed aumentaria para **O(N log N)**.  
- Em contextos práticos, k-means também é tratado como **O(N)** quando o número de clusters (k) e de iterações (I) são fixos e pequenos.


Em termos de **complexidade**, todas as técnicas apresentadas — **Thresholding**, **Watershed** (com ou sem marcadores, usando buckets) e **k-means** (com *k* e *I* constantes) — podem ser implementadas em tempo **O(N)**. Essa semelhança significa que, do ponto de vista de escalabilidade, nenhuma delas se torna intratável apenas pelo crescimento do número de pixels.

No entanto, cada método traz **vantagens e limitações** específicas:

- **Thresholding** é extremamente rápido e direto (uma única passagem em O(N)), mas não distingue regiões limítrofes que apresentem valores de intensidade semelhantes.
- **k-means**, embora também linear quando *k* e *I* são fixos, dependendo da escolha de *k*, pode gerar clusters espacialmente desconectados e normalmente requer pós-processamento para refinar fronteiras.  
- **Watershed com buckets** combina segmentação guiada pelas bordas (cristas de gradiente) com complexidade linear, produzindo **fronteiras alinhadas** a contornos reais e garantindo regiões coerentes com os objetos da imagem — ideal para cenários em que a precisão de delimitação é tão importante quanto a eficiência.

Portanto, mesmo partindo de **O(N)** para todos, vale escolher o algoritmo certo para cada necessidade: se a prioridade for **velocidade pura** e o objetivo for uma divisão muito simples, o **thresholding** basta; se for **qualidade de segmentação espacial**, especialmente em imagens com ruído ou variações suaves, **Watershed com buckets** se destaca como a melhor opção.

---
Fim do handout - Abaixo são exemplos de componentes que podemos usar para fazer o Handout 

Você também pode criar

1. listas;

2. ordenadas,

assim como

* listas;

* não-ordenadas

e imagens. Lembre que todas as imagens devem estar em uma subpasta *img*.

![](logo.png)

Para tabelas, usa-se a [notação do
MultiMarkdown](https://fletcher.github.io/MultiMarkdown-6/syntax/tables.html),
que é muito flexível. Vale a pena abrir esse link para saber todas as
possibilidades.

| coluna a | coluna b |
|----------|----------|
| 1        | 2        |

Ao longo de um texto, você pode usar *itálico*, **negrito**, {red}(vermelho) e
[[tecla]]. Também pode usar uma equação LaTeX: $f(n) \leq g(n)$. Se for muito
grande, você pode isolá-la em um parágrafo.

$$\lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} \leq 1$$

Para inserir uma animação, use `md :` seguido do nome de uma pasta onde as
imagens estão. Essa pasta também deve estar em *img*.

:bubble

Você também pode inserir código, inclusive especificando a linguagem.

``` py
def f():
    print('hello world')
```

``` c
void f() {
    printf("hello world\n");
}
```

Se não especificar nenhuma, o código fica com colorização de terminal.

```
hello world
```


!!! Aviso
Este é um exemplo de aviso, entre `md !!!`.
!!!


??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???
