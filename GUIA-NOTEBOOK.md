# Como funciona o notebook Digit Recognizer

O [notebook](solucao-digit-recognizer.ipynb) ensina o computador a reconhecer dígitos manuscritos de 0 a 9. Ele aprende com imagens cujo dígito já é conhecido, compara três formas de classificar imagens e gera um CSV com as previsões para o Kaggle.

Execute as células de cima para baixo. As células de texto explicam o próximo passo, e as de código usam dados e funções preparados nas anteriores.

## Célula 1 · Apresentação

Explica o objetivo do projeto e como executar o notebook a partir da raiz do repositório. No Colab, a raiz precisa estar acessível ao kernel remoto. O treinamento e a previsão usam CPU, inclusive quando o Colab disponibiliza uma GPU.

## Célula 2 · Bibliotecas e caminhos

Importa as ferramentas para ler tabelas, tratar imagens, treinar modelos e desenhar gráficos. Define a semente 42 para repetir a divisão dos dados e configura o limite de threads das bibliotecas usadas no treino. `DATA_DIR` aponta para a pasta `data` na raiz, que contém os dois CSVs.

## Célula 3 · Como os dados são divididos

Explica a divisão estratificada. Cerca de 20% dos exemplos de cada dígito ficam reservados para a validação. Isso ajuda a manter proporções parecidas de zeros, uns e dos demais dígitos nos dois grupos. A validação funciona como um simulado com respostas conhecidas para comparar os modelos.

## Célula 4 · Leitura e preparação das imagens

A célula lê `data/train.csv`, transforma cada linha de 784 pixels em uma imagem de 28 × 28 e guarda o dígito correto à parte. Separa 33.600 imagens para treino e 8.400 para validação, depois mostra um exemplo de cada dígito do grupo de treino.

## Célula 5 · As três formas de reconhecer um dígito

- **Regressão logística.** Usa os valores dos pixels para escolher um dígito.
- **HOG + SVM.** O HOG resume as direções dos traços em pequenas regiões da imagem. A SVM aprende a distinguir os dígitos com base nesses resumos.
- **HOG + SVM com deslocamentos.** Usa a mesma SVM para avaliar cinco versões da imagem e combina as pontuações antes de escolher o dígito.

## Célula 6 · Funções para preparar e classificar imagens

`normalized_pixels` divide os valores dos pixels por 255 para colocá-los entre 0 e 1. `hog_vectors` transforma cada imagem em 324 números que descrevem seus traços. `shift_images` move os pixels e preenche as bordas expostas com zero, que aparece como preto. As funções `logistic_model` e `svm_model` configuram os classificadores.

Na variante com deslocamentos, `averaged_scores` calcula a pontuação média de cada dígito na imagem original e em quatro cópias, movidas um pixel para cima, para baixo, à esquerda e à direita. `svm_labels` escolhe o dígito com a maior pontuação.

## Célula 7 · Regras da comparação

Define dois testes no mesmo grupo de validação. O primeiro mede quantas imagens originais cada variante acerta. O segundo mede os acertos depois de mover cada imagem dois pixels para a direita e dois para cima. A maior acurácia nas imagens originais define a vencedora. Em caso de empate, valem os acertos nas imagens deslocadas e, se o empate continuar, a ordem das variantes na tabela.

## Célula 8 · Treino, comparação e erros

Treina uma regressão logística e uma SVM. A terceira variante reaproveita a SVM e muda a forma de fazer a previsão. A célula avalia as três variantes, imprime as porcentagens de acerto e escolhe a vencedora. Também mostra uma matriz de confusão. Cada linha indica o dígito real e cada coluna indica o dígito previsto. A diagonal mostra os acertos, e as outras posições revelam quais dígitos foram confundidos.

## Célula 9 · O que entra na submissão

Explica que, após a escolha da variante, o notebook treina de novo usando todas as 42.000 imagens rotuladas. Em seguida, gera uma previsão para cada imagem de `test.csv`, na ordem em que aparece no arquivo. O Kaggle calcula a acurácia desse conjunto após o envio.

## Célula 10 · Treino final e exportação

Lê as 28.000 imagens de `data/test.csv`, treina a variante escolhida e faz as previsões. Monta as colunas `ImageId`, com números de 1 a 28.000, e `Label`, com o dígito previsto. Por fim, grava `submission.csv` na raiz do repositório e mostra as primeiras previsões.
