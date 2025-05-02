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

Agora que já vimos como funcionam os algoritmos mais simples de segmentação, vamos entender a ideia por trás do Watershed (bacias hidrográficas).

Vamos utilizar a imagem dos comprimidos como exemplo, e faremos uma simplificação dela para compreender melhor o funcionamento do algoritmo.

![](comprimidos_matrizes.png)

O algoritmo interpreta a imagem em escala de cinza como um relevo topográfico, no qual cada pixel possui uma altitude conforme seu nível de brilho. Quanto mais branco o pixel, mais alto ele é, e quanto mais escuro, mais baixo ele é.

![](comprimidos_matrizes_cinza_.png)

No caso dessa imagem, como o fundo é branco, é como se houvesse buracos de diferentes profundidades.

O algoritmo simula a água subindo por esses vales: imagine que há uma nascente no centro de cada vale, então a "água" começa a preenchê-los. Quando as "águas" de duas nascentes diferentes se encontram, é construída uma barreira — essa barreira marca a divisão entre diferentes objetos na imagem. Assim, o algoritmo separa a imagem em regiões bem definidas, como se fossem bacias hidrográficas.

Mas como definimos onde essas nascentes devem ser posicionadas?

---

Watershed com Marcadores
---

Os marcadores são uma outra matriz de input do algoritmo que serve para indicar por onde a agua deve começar a surgir.

![](marcadores.png)


??? Exercício

Abra o arquivo `md watershed_com_marcadores.ipynb` [deste repositorio](https://github.com/ferclima05/handout-Watershed.git) para abrir uma simulação em python do algoritmo funcionando. Nela você pode escolher onde ficarão os marcadores e quais marcadores pertencem a cada objeto.

Apertando as teclas de númeos 1 a 9 você seleciona o marcador.

Depois de selecionar os marcadores, aperte [[w]] para aplicar Watershed.

::: Gabarito
O resultado deve ser parecido com isso

![](saida_watershed.png)

{red}(*O algoritmo tem dificuldade em identificar os comprimidos brancos por conta do fundo.)

:::

???

Apesar de ser um algoritmo poderoso, o Watershed também apresenta algumas limitações importantes que precisam ser consideradas:

**Supersegmentação:** quando a imagem possui muito ruído ou pequenas variações de intensidade, o algoritmo pode interpretar cada pequeno vale como uma região separada. Isso gera um número excessivo de divisões, dificultando a segmentação correta.

**Baixo contraste:** se os objetos da imagem tiverem um brilho muito próximo ao do fundo, o relevo gerado terá poucas diferenças de altitude. Isso dificulta que o algoritmo identifique onde estão os vales, resultando em regiões pouco definidas ou incorretas.

Esses problemas podem ser amenizados com técnicas de pré-processamento, como suavização (blur), remoção de ruído ou definição manual de marcadores.



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
