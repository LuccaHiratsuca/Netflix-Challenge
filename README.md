# Netflix Challenge

## Integrante(s):
* [Lucca Hiratsuca Costa](https://github.com/LuccaHiratsuca)

## Introdução
Esse projeto foi desenvolvido para ser um sistema preditor de nota de filmes x usuário, sem a utilzação de bibliotecas que fazem recomendações.

Para isso, com base no banco de dados e, a apartir do uso do algoritmo de decomposição matricial, SVD com remoção de componentes, buscamos predizer avaliações as quais sejam próximas as verdadeiras.

## Dados utilizados
Para a realização desse desafio, foi utilizado o [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset), um dataset que possui, dentre diversas informações, a avaliação de usuários em relação a filmes.

Por conta de ser uma tabela muito grande (``ratings.csv``), foi utilizado um subconjunto desse ``ratings_small.csv``, que pode ser encontrado [aqui](https://www.kaggle.com/rounakbanik/the-movies-dataset?select=ratings_small.csv).

## Pré-requisitos e dependências

Clonar o repositório:
```bash
https://github.com/LuccaHiratsuca/Netflix-Challenge.git
```

Após clonar o repositório, instalar as dependências:
```bash
pip install -r requirements.txt
```

Por fim, para rodar o código, basta executar o arquivo `main.py`:
```bash
python main.py
```
## Modelo Matemático
### Algoritmo de decomposição matricial: SVD
Assim como foi dito na introdução, foi utilizado o algoritmo de decomposição matricial, SVD: Single Value Decomposition (Decomposição em Valores Singulares).

Essa técnica consiste em decompor uma matriz em três **componentes**, dessa maneira permitindo uma representação mais simples e compacta dos dados.

### Fórmula do SVD
A fórmula do SVD é dada por:

Dada uma matriz **A** de dimensão **m x n**, podemos decompor essa matriz em três componentes:

$$A = U \Sigma V^T$$

Onde:
- **A** é a matriz original de dimensão **m x n**
- **U** é uma matriz unitária, também de dimensão **m x n**, cujas colunas são os autovetores da esquerda de **A**, encontrados pela multiplicação: **AA<sup>T</sup>**
- **Σ** é uma matriz diagonal de tamanho **m x n**, cujas entradas são os valores singulares de **A**.
- **V<sup>T</sup>** é a matriz transposta de **V**, também unitária, de dimensão **n x n**, cujas colunas são chamadas de vetores singulares da direitos de **A**, encontrados pela multiplicação: **A<sup>T</sup>A**

### Interpretação do SVD
Após a decomposição da matriz **A**, podemos interpretar os componentes da seguinte maneira:

- **U**: representa a relação entre as amostras originais e as amostras transformadas.
- **Σ**: contém os valores singulares, os quais representam a importância de cada componente. 
- **V<sup>T</sup>**: descreve a relação entre as características originais e as características transformadas.

Após a decomposição, os valores presentes na matriz diagonal Σ acabam nos proporcionando informações sobre a importância relativa das amostras transformadas. Esses valores singulares são ordenados em ordem decrescente, de forma que os primeiros valores singulares são os mais importantes/ significativos da matriz original.

Sabendo disso, uma estratégia que podemos aplicar é selecionar apenas os primeiros k valores singulares e seus correspondentes vetores singulares esquerdos e direitos, nos permitindo, quando formos reconstuir a matriz, obter uma representação dessa de maneira mais compacta e com menos ruídos, sem a perda de muita informação.

Em suma, ao retirar os componentes menos significantes do SVD, estamos selecionando apenas os primeiros k valores singulares e seus vetores singulares correspondente, nos resultando em uma aproximação da matriz original com uma dimensionalidade reduzida. Por fim, nos permitindo simplificar a representação dos dados e, ao mesmo tempo, pegarmos apenas os aspectos mais relevantes das informações.

## Implementação



## Resultados