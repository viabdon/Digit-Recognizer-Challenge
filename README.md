# Digit Recognizer

O objetivo do projeto é reconhecer dígitos manuscritos de 0 a 9 em imagens de 28 × 28 pixels do MNIST e gerar previsões para o desafio Digit Recognizer do Kaggle. O experimento também mede quanto os acertos mudam quando o dígito sai um pouco do lugar.

## Como funciona

O [notebook](solucao-digit-recognizer.ipynb) separa as imagens rotuladas em 80% para treino e 20% para validação. Em seguida, compara três variantes nas imagens originais e nas mesmas imagens deslocadas.

- **Regressão logística** usa os valores dos pixels.
- **HOG + SVM** resume a direção dos traços e usa esse resumo para classificar o dígito.
- **HOG + SVM com deslocamentos** combina as pontuações da imagem original e de quatro cópias levemente deslocadas.

A variante com deslocamentos teve o melhor resultado nas imagens originais, com **99,21% de acurácia na validação local**. Nas imagens deslocadas, o HOG + SVM simples foi melhor, com **93,10%**. Depois da comparação, o notebook treina novamente a variante escolhida com todas as imagens rotuladas e gera um CSV com as 28.000 previsões para o Kaggle.

## Como executar

Abra o [notebook](solucao-digit-recognizer.ipynb) e execute as células em ordem a partir da raiz do repositório. Ele lê `data/train.csv` e `data/test.csv` e grava `submission.csv` na raiz. O experimento roda em CPU. No Colab, use como diretório de trabalho a raiz da cópia do repositório no kernel remoto.

Para mais detalhes, consulte o [guia do notebook](GUIA-NOTEBOOK.md), o [relatório](docs/relatorio-digit-recognizer.docx) e o [CSV de submissão](docs/submission.csv) gerado no experimento.
