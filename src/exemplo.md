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

---

Thresholding
---

Agora que entendemos o desafio de separar objetos em uma imagem, vamos começar com um dos métodos mais simples e intuitivos de segmentação: o **thresholding**.

A ideia é a seguinte: Existe uma imagem inicila colorida, ela é convertida em escala cinza, cada pixel da imagem na escala cinza possui um valor que representa sua intensidade (por exemplo, de 0 a 255). O thresholding define um **limiar** e separa os pixels em dois grupos: os mais claros (acima do limiar) e os mais escuros (abaixo do limiar).

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

O primeiro passo é converte ela para uma escala cinza:

![](imagem_cinza.jpg)

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

Para solucionar esse problema vamos conhecer um novo algoritmo de segmentação.

---

Watershed
---

Para poder entender a ideia do algoritmo precisamos visualizar a imagem de uma forma diferente. Para isso, vamos utilizar um exemplo em baixa resolução da foto das moedas.

![](exemplo_em_baixa_resolucao.png)

O algoritmo também interpreta a imagem em **escala de cinza** como um relevo topográfico, no qual cada pixel possui uma altitude conforme seu nível de brilho (de 0 a 255). Quanto mais branco o pixel, mais alto ele é, e quanto mais escuro, mais baixo ele é.

??? Checkpoint


Com base nessa descrição, pense em como ficaria esse exemplo em baixa resolução no corte representado pela linha vermelha, ou seja, como seria uma "Vista Lateral" da imagem se ela fosse cortada na linha vermelha.

![](Vista_superior.png)

::: Gabarito
![](Vista_superior_e_vista_lateral.png)

Essa seria uma possível analogia de como ficaria uma vista em forma 3D

![](imagem_3D_oposto.png)
:::

???

Ok, agora que sabemos como o algoritmo enxerga as imagens vamos entender a ideia por trás do Watershed (bacias hidrográficas).

Imagine que há uma nascente no centro de cada vale, então a "água" começa a preenchê-los. Quando as "águas" de duas nascentes diferentes se encontram, é construída uma barreira — essa barreira marca a divisão entre diferentes objetos na imagem. Assim, o algoritmo separa a imagem em regiões bem definidas, como se fossem bacias hidrográficas.

:Watershed

??? Checkpoint

Muito simples né? Agora pense em como ficaria a imagem das moedas com base no procedimento descrito

![](imagem_cinza.jpg)

::: Gabarito
![](watershed_SS.png)

:::

???

Não ficou como esperávamos né? Ocorreu o que chamamos de **supersegmentação**.

??? Checkpoint

Olhando para a imagem retornada, por que você acha que isso aconteceu?

::: Gabarito

Vamos voltar ao exemplo em baixa resolução.

Na realidade, a imagem da vista lateral não é sempre como apresentamos anteriormente. Na maioria dos casos ela se parece com isso.

![](Vista_superior_e_vista_lateral_circulos.png)

Possui vários pontos de mínimos locais que o algoritmo interpreta como "nascentes".

: SS

:::

???


Como podemos resolver esse problema?

---

Watershed com Marcadores
---

Os marcadores são uma outra matriz de input do algoritmo que serve para indicar por onde a água deve começar a surgir, ou seja, quantas "nascentes" terão e onde serão posicionadas.

??? Checkpoint

Dada a imagem do exemplo, em que pontos você colocaria os marcadores?

![](Vista_superior_e_vista_lateral_real.png)


::: Gabarito

![](Vista_superior_e_vista_lateral_marcadores.png)

!!!
{red}(Se você não colocou os marcadores exatamente na posição mostrada no gabarito não significa que a sua solução esteja errada, o objetivo é posicionar o marcador onde temos certeza que é um objeto.)
!!!
:::

???

Dessa forma, garantimos que não sejam criadas nascentes desnecessárias e também que o comportamento do algoritmo seja como esperávamos a princípio

: Marcadores

Ok, agora sim podemos simular a imagem original das moedas utilizando a técnica dos marcadores.

??? Checkpoint

Em quais pontos da imagem você colocaria os marcadores?

![](imagem_cinza.jpg)


::: Gabarito

![](imagem_cinza_marcadores.jpg)

:::

::: Resultado

![](Marcadores_moedas_watershed.png)

:::

???

Agora sim alcançamos o resultado que esperávamos. Mas será que esse algoritmo é eficiente em termos de complexidade?

---

Complexidade do algoritmo
---

Para entender a complexidade, é essencial que vocês tenham entendido como esse algoritmo funciona, desde a ideia de visualização do que é feito, por meio da analogia com a altura dos pixels, até o motivo de criação dos marcadores.

![](image_3D.png)

Para começar, vamos tentar entender como a busca pelo próximo valor que será inundado ocorre. Tente imaginar como acontece.

??? Checkpoint

Com base nas animações, tente imaginar como é feita a busca dos próximos valores a serem “inundados”.

::: Gabarito
O algoritmo inicia-se a partir dos marcadores e, a cada iteração, expande-se para os pixels vizinhos ainda não rotulados. Cada novo pixel adicionado torna-se, por sua vez, ponto de partida para a busca em sua própria vizinhança, até que toda a imagem esteja completamente “inundada”.
:::

???

Já entendemos como os pixels são identificados; a questão agora é decidir qual vizinho deve ser inundado primeiro. Como você acha que é feito?

??? Checkpoint

Qual a ordem de inundação dos pixels vizinhos?

a) Todos juntos.

b) Ordem aleatória.

c) Os que forem encontrados primeiro vão primeiro.

d) Ordem crescente dos valores dos pixels.


::: Gabarito
d) Ordem crescente dos valores dos pixels.

Essa ordem é garantida por uma [fila de prioridade](https://ensino.hashi.pro.br/desprog/aula/17/).
:::

???

Vamos tentar enxergar a complexidade total do algoritmo através do pseudocódigo a seguir. Considere n como o número de pixels que a imagem possui.

```c
Para i em (0, 1, 2, 3, ..., n-1) { // linhas
  Para j em (0, 1, 2, 3, ..., n-1) { // colunas
    ...
    fila de prioridade // Considere a complexidade para a fila como sendo O(f(n))
    ...
  }
}
```

??? Checkpoint

Baseado no pseudocódigo tente obter a complexidade do algoritmo em função de f(n).

::: Gabarito
Como cada pixel é processado exatamente uma vez e inserido na fila de prioridade, a varredura dos n pixels custa *$O(n)$*. 

Além disso, existe um trabalho extra que envolve a manipulação da fila de prioridade, mas, para esse handout, não vamos descer a esse nível de detalhe. Vamos considerar então que a manipulação da fila apresenta custo *$O(f(n))$*.

Portanto, a complexidade total do algoritmo fica *$O(n \cdot f(n))$*.

:::

???

Comparação com os outros algoritmos
---

| Algoritmo                      | Complexidade Geral    |
|--------------------------------|-----------------------|
| *Thresholding*                 | $O(n)$                |
| *Watershed (sem marcadores)*   | $O(n \cdot f(n))$     |
| *Watershed com marcadores*     | $O(n \cdot f(n))$     |

O Thresholding não usa nenhuma estrutura sofisticada de dados e apresenta complexidade linear $O(n)$.

Para o algoritmo de Watershed (com ou sem marcadores), como já falamos antes, a complexidade envolve a busca dos pixels e a manipulação da fila de prioridade.

Para a fila, adotamos como sendo uma caixa preta cujo comportamento de $f(n)$ não foi definido. Porém, para comparar as complexidades dos algoritmos mencionados, considere que $f(n)$ pode ser *log n* ou *1*, dependendo de qual estrutura de fila de prioridade for utilizada.

O Thresholding é extremamente rápido e direto, mas não funciona muito bem para distinguir regiões limítrofes que apresentem valores de intensidade semelhantes.

O Watershed, apesar de possuir uma complexidade igual ou pior que $O(n)$, funciona melhor que o Thresholding para a segmentação de imagens com objetos grudados, apesar de essa não ser a finalidade principal do algoritmo.

Portanto, vale escolher o algoritmo certo para cada necessidade: se a prioridade for *velocidade pura* e o objetivo for uma divisão muito simples, o *thresholding* basta; se for *qualidade de segmentação espacial, especialmente em imagens com ruído ou variações suaves, **Watershed com marcadores* se destaca como a melhor opção.

---

Exercícios complementares
---

??? Para se aprofundar

Abra o arquivo `md watershed_com_marcadores.py` [deste repositório](https://github.com/ferclima05/handout-Watershed.git) para abrir uma simulação em python do algoritmo funcionando. Nela você pode escolher onde ficarão os marcadores e quais marcadores pertencem a cada objeto.

Apertando as teclas de númeos [[a]] a [[z]] você seleciona o marcador.

Após selecionar o marcador desenhe com o mouse onde você quer posiciona-lo.

Depois de selecionar os marcadores, aperte [[2]] para ver o resultado do Watershed.

A tecla [[1]] reseta os marcadores

Aperte [[3]] ou [[esc]] para fechar o programa.

!!!

É necessário instalar a biblioteca OpenCV.

```
pip install opencv-python

```


!!!

???

??? Para se aprofundar

No mesmo repositório estão os códigos do thresholding e K-means para testar!!

???