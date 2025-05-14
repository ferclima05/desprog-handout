Segmentação por Watershed com Marcadores
======

## O que é segmentação de imagens?

A segmentação de imagens é uma das tarefas mais fundamentais em visão computacional. Ela consiste em **dividir uma imagem em regiões que façam sentido** — por exemplo, identificar onde estão os objetos, separá-los do fundo, e entender onde um objeto termina e outro começa.

Esse processo é essencial em diversas áreas:

- Na medicina, para separar tumores de tecidos saudáveis em exames de imagem;
- Na indústria, para identificar defeitos em peças;
- No agronegócio, para contar frutas em uma árvore ou grãos em uma esteira;
- Em sistemas autônomos, para distinguir faixas, placas, obstáculos.

Vamos começar com um exemplo simples:

![](moedas_simples.png)

Temos duas moedas sobre um fundo branco. A da esquerda é de bronze. A da direita é de prata.

???

Se você tivesse que ensinar um algoritmo a "separar" essas duas moedas, por onde começaria?

- a) Pela posição delas
- b) Pela cor ou intensidade
- c) Pelo tamanho
- d) Pelo formato

::: Gabarito
**b)**. O primeiro indício para separá-las seria a **diferença de cor ou intensidade** — a moeda de bronze é mais escura que a de prata, o que já pode ser útil para um algoritmo simples.
:::

???

Observe novamente a imagem anterior. Agora reflita:

- E se o fundo não fosse branco?
- E se as duas moedas fossem do mesmo material?
- E se uma moeda estivesse parcialmente em cima da outra?
- E se houvesse sombra?

Esses fatores dificultam a segmentação porque: A segmentação depende de **contraste entre objeto e fundo**, **separação física entre objetos** e **condições de iluminação** que não distorçam as intensidades.

## Algoritmos de segmentação

Para que um computador consiga **analisar imagens**, a primeira tarefa é entender o que é objeto e o que é fundo. Isso é especialmente importante em aplicações que envolvem contar, medir ou identificar regiões específicas.

??? Explorando o problema

Olhe para a imagem abaixo.

![](moedas1.png)

* O que o algoritmo precisa fazer para identificar os objetos na imagem?

* Por que identificar os limites entre os objetos é importante para contar e medir?

::: Gabarito
O algoritmo precisa detectar **diferenças de intensidade** ou **textura** que marcam as bordas dos objetos. Sem identificar os limites, o computador não consegue separar objetos que estão na mesma imagem.

![](moeda_segmentada.png)

:::

???

Segmentar imagens economiza tempo humano e evita erros. Antes, esse processo era feito manualmente ou com muito treinamento de IA. Mas existem algoritmos que oferecem uma forma automática e precisa de dividir imagens em regiões distintas. Isso abre caminho para muitas aplicações reais, com menos custo e mais eficiência.

## Thresholding

Agora que entendemos o desafio de separar objetos em uma imagem, vamos começar com um dos métodos mais simples e intuitivos de segmentação: o **thresholding**.

A ideia é a seguinte: Cada pixel da imagem possui um valor que representa sua intensidade (por exemplo, de 0 a 255). O thresholding define um **limiar** e separa os pixels em dois grupos: os mais claros (acima do limiar) e os mais escuros (abaixo do limiar).

???

Dado esse funcionamento, qual tipo de imagem tende a funcionar melhor com thresholding?

- a) Imagens com muitos detalhes e texturas finas
- b) Imagens com contraste forte entre objeto e fundo
- c) Imagens com objetos da mesma cor do fundo
- d) Imagens com ruído e variação de sombra

::: Gabarito
**b)**. O thresholding funciona melhor quando há um contraste claro entre objeto e fundo.
:::

???

Vamos aplicar isso a uma imagem com várias moedas:

![](moedas1.png)

A ideia é separar as moedas (em tons escuros e médios) do fundo (branco), usando um limiar de intensidade.

O código faz algo como:

```c
if (pixel > limiar) {
    resultado = 255;
} else {
    resultado = 0;
}
```

???

Com base na imagem acima, o que você espera do resultado do thresholding?

- a) Todas as moedas serão separadas corretamente
- b) As moedas grudadas serão tratadas como uma única região
- c) O fundo será incluído como parte dos objetos
- d) O resultado será uma imagem em tons de cinza

::: Gabarito
**b)**. Como o thresholding não entende a forma ou a separação entre objetos, ele tende a **juntar moedas que estão próximas ou encostadas**.
:::

???

## Resultado do thresholding

Veja o que acontece ao aplicar o threshold com limiar fixo de 200:

![resultado](moedas_thresh.png)

Embora as moedas sejam bem destacadas do fundo, **o algoritmo não consegue separar moedas grudadas**. Todas aparecem como uma única grande região branca.


!!! Aviso
Thresholding é ótimo para **destacar objetos do fundo quando há contraste**, mas **não entende a estrutura ou a forma dos objetos**. Por isso, ele falha quando os objetos estão sobrepostos ou conectados.
!!!
