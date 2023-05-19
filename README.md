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

### Valores de K:
Na decomposição matricial, o valor de k representa a quantidade de componentes que serão utilizados para a reconstrução da matriz original. Ou seja, a partir dele, definimos quantos dos valores singulares desejamos manter na decomposição. 
## Implementação
Com o dataframe recebido, a primeira coisa que fizemos foi utilizar a função ``pivot`` para criar uma talela pivô a partir do Dataframe, onde as colunas são os filmes e as linhas são os usuários, e os valores são as notas dadas pelos usuários aos filmes.

Em seguida, para os valores ausentes nessa tabela, acabamos por preenchê-los com o valor **2.5**, que é a nota média dos filmes (de 0 a 5). Uma vez que tentamos afetar o mínimo possível os resultados, já que não temos informações sobre esses valores.

### Loop Principal:
``` python
dfResult = pd.DataFrame(columns=['Iteration', 'K', 'Matrix Original', 'Matrix Decomposition', 'Error'])

k_list= [50,100,150,200,500,650]

for k in k_list:
    for i in range(0,101):
        matrix_b = deepcopy(matrix)
        index, column = get_User_Movie(df)
        changeValue(matrix_b, index, column)
        matrix_b = decomposition(matrix_b, k)
        diff = abs(matrix[index][column] - matrix_b[index][column])
        print('Iteration:', i, 'K:', k, 'Matrix Original:', matrix[index][column], 'Matrix Decomposition:', matrix_b[index][column], 'Error:', diff)
        dfResult['Iteration'] = [i]
        dfResult['K'] = [k]
        dfResult['Matrix Original'] = [matrix[index][column]]
        dfResult['Matrix Decomposition'] = [matrix_b[index][column]]
        dfResult['Error'] = [diff]
        dfResult.to_csv('data/result.csv', mode='a', header=False)

dfResult.head()
```

Esse código realiza um loop para diferentes valores de "k" na lista k_list e iterações de 0 a 100. Para cada combinação de "k" e iteração, ele executa o seguinte:

1. Cria uma cópia da matriz original chamada matrix_b usando a função deepcopy.
2. Obtém aleatoriamente um índice e uma coluna utilizando a função ``get_User_Movie``.
3. Modifica o valor da matriz matrix_b na posição especificada pelo índice e coluna utilizando a função ``changeValue``.
4. Realiza a decomposição da matriz matrix_b utilizando a função ``decomposition`` e o valor de "k" atual.
5. Calcula a diferença absoluta entre o valor original da matriz e o valor obtido após a decomposição.
6. Atualiza o DataFrame dfResult com os valores da iteração atual, "k", matriz original, matriz de decomposição e erro.
7. Salva o conteúdo do DataFrame dfResult no arquivo CSV "data/result.csv" usando o modo "append" para adicionar os dados sem substituir o conteúdo existente.`

### Funções:
- **get_User_Movie**: Essa função é responsável por retornar um índice e uma coluna aleatórios da matriz. Esses valores são utilizados para modificar um valor da matriz original e, posteriormente, comparar com o valor obtido após a decomposição.

- **changeValue**: Essa função é responsável por modificar um valor da matriz na posição especificada pelo índice e coluna recebidos como parâmetro. Essa função é utilizada para modificar um valor da matriz original e, posteriormente, comparar com o valor obtido após a decomposição.

- **decomposition**: Essa função é responsável por realizar a decomposição da matriz recebida como parâmetro utilizando o valor de "k" recebido como parâmetro. Essa função retorna a matriz obtida após a decomposição.

- **comprimir**: Essa função é responsável por realizar a compressão da matriz recebida como parâmetro utilizando o valor de "k" recebido como parâmetro. Essa função retorna a matriz obtida após a compressão.


#### Observações:
Para a realização do cálculo da decomposição matricial, com o algoritmo SVD, foi utilizado a função ``svd`` da biblioteca ``scipy.linalg`` e, para o cálculo da diagonal da matriz Σ, foi utilizado a função ``diag`` da biblioteca ``scipy.linalg``.

## Resultados
