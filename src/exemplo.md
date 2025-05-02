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
## 2. O que é Thresholding?

Imagine que você está olhando uma imagem como esta:

![](tomate.png)

Você consegue identificar facilmente os tomates vermelhos? E os amarelos? Para nós, humanos, é relativamente simples. Mas... e para um computador?

Vamos imaginar que você precisa ensinar um computador a **separar objetos do fundo em uma imagem**. Uma das das formas mais simples de fazer isso é com um algoritmo chamado **thresholding** (ou "limiarização").

O algoritmo de thresholding tenta **transformar uma imagem complexa em uma simples**: ele define um valor de corte e separa os pixels em dois grupos:

* Os que estão **acima do valor**, que chamaremos de objeto.
* Os que estão **abaixo do valor**, que chamaremos de fundo.

Por exemplo: “se a intensidade do pixel for maior que 128, ele faz parte do objeto”.

Essa ideia transforma uma imagem em **preto e branco**, facilitando o processamento posterior.

??? Exercício

Imagine uma imagem com os seguintes valores de pixels (em escala de cinza):

```
100 120 135 150 160
```

Se o limiar for `130`, quais desses valores você acha que deveriam virar **branco (255)** e quais deveriam virar **preto (0)**?

::: Gabarito
Valores maiores que 130 viram 255 (branco): 135, 150, 160  
Valores menores ou iguais viram 0 (preto): 100, 120
:::
???

Vantagens:

* Simples e rápido
* Funciona bem com imagens em preto e branco, ou com objetos escuros sobre fundo claro (ou vice-versa)

Desvantagens:

* Falha quando há **variações de iluminação**
* Não consegue distinguir **cores semelhantes**
* Não separa objetos colados


??? Exercício

Considere a imagem acima e imagine que queremos segmentar apenas os **tomates vermelhos**.

Use um valor de threshold igual a 128. O que acontece?

- a) Os tomates vermelhos são corretamente segmentados?
- b) Os tomates amarelos aparecem como fundo ou objeto?
- c) Os tomates colados são separados ou agrupados?

::: Gabarito
- a) Apenas parte dos tomates vermelhos é segmentada. Alguns podem ser ignorados se estiverem mal iluminados.
- b) Os tomates amarelos podem acabar sendo confundidos com o fundo ou com os vermelhos, dependendo da intensidade.
- c) Tomates colados são agrupados em uma única massa, pois o algoritmo não considera forma.

:::

???

Agora que você entendeu a ideia, tente imaginar como seria o código em C para fazer isso com uma imagem inteira.

Você precisa de:
- Um **laço** que percorra todos os pixels
- Uma **condição** que compara a intensidade com o limiar
- Uma **decisão**: escrever 0 ou 255 no pixel de saída

??? Exercício

Esse código percorre toda a imagem, pixel por pixel. A operação é rápida, com complexidade linear $O(n)$.

::: Gabarito

```c
for (int i = 0; i < largura * altura; i++) {
    if (imagem[i] > limiar) {
        resultado[i] = 255; // objeto (tomate)
    } else {
        resultado[i] = 0;   // fundo
    }
}
```
:::

???

Não se preocupe se não tiver certeza do código final ainda. A seguir, você poderá ver como isso foi implementado.
Clone o repositório a seguir para ver o código completo em C que aplica thresholding a uma imagem:

```bash
git clone https://github.com/ferclima05/handout-Watershed
```

Você pode compilar com `gcc` e testar com imagens `.pgm` para ver o funcionamento.

!!! Aviso
Apesar de simples, thresholding **não funciona bem em imagens complexas** com objetos sobrepostos ou cores próximas. Ele deve ser usado com cautela.
!!!

Após aplicar o threshold o resultado é esta imagem binária:

![](saida_tresh.png)

Note como os tomates mais escuros foram preservados e o fundo foi descartado. Porém, alguns objetos colados ainda estão unidos. Resolveremos isso nos próximos algoritmos.

---

## 3. O que é K-means?

Agora vamos dar um passo além. E se, ao invés de comparar cada pixel com um único valor, o computador conseguisse **agrupar os pixels com base em semelhanças**?

É exatamente isso que o algoritmo **K-means** faz.

Imagine novamente a imagem dos tomates. Só que agora, ao invés de pensar em claro e escuro, você quer que o computador **identifique grupos diferentes de cor**:

- Tomates vermelhos
- Tomates amarelos
- Fundo branco

O K-means é um algoritmo de **agrupamento**. Ele funciona assim:

1. Escolhe `k` grupos (você decide quantos quer).
2. Atribui cada pixel ao grupo mais parecido com ele.
3. Atualiza os grupos com base nos pixels atribuídos.
4. Repete até os grupos pararem de mudar.

No nosso caso, podemos pedir para o K-means encontrar `k=3` grupos. A ideia é que cada grupo represente uma cor dominante da imagem.

??? Exercicio

Imagine os seguintes pixels representados por valores de intensidade (simples, para fins didáticos):

```
20, 25, 210, 220, 240, 100, 110
```

Suponha que `k = 3`. Você poderia agrupar manualmente esses valores em 3 grupos? Em que intervalos faria isso?

::: Gabarito
Uma possibilidade:
- Grupo 1: 20, 25 (valores baixos)
- Grupo 2: 100, 110 (valores médios)
- Grupo 3: 210, 220, 240 (valores altos)
:::

???

Diferente do thresholding, aqui você não compara com um único valor. Você precisa:

- Inicializar `k` centros (valores médios aleatórios ou distribuídos)
- Para cada pixel, descobrir a qual centro ele está mais próximo
- Atualizar os centros com a média dos pixels atribuídos

Antes de ver o código, pense:
> Como você faria isso em C, onde não há biblioteca pronta para agrupar?

Clone o repositório a seguir para ver a implementação completa do K-means em C:


```bash
git clone https://github.com/ferclima05/handout-Watershed
```

Compile com `gcc` e execute com uma imagem `.pgm`. O programa vai agrupar os pixels em `k` grupos diferentes.

Veja abaixo a imagem após aplicar o K-means com `k = 3`:

![](saida_means.png)

Note como agora conseguimos distinguir diferentes grupos de cor. Porém, os objetos ainda estão colados. Resolveremos isso na próxima técnica!

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
