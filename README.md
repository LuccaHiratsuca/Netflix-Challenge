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

Assim como foi dito na 

## Resultados