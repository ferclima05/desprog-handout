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

Mostrar como chegamos na complexidade do Tresholding, Watershed, Wathershed com Marcadores 

| Tresholding | Watershed | Watershed c/ Marcadores |
|-------------|-----------|-------------------------|
| O()         | O()       | O()                     |

??? Exercício

Este é um exemplo de exercício, entre `md ???`.

::: Gabarito
Este é um exemplo de gabarito, entre `md :::`.
:::

???

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
