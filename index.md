## Setup

1. Instale mini-conda (mode default): [miniconda](https://conda.io/miniconda.html)

2. Se você está na pasta local deste repositório, digite o seguinte comando no terminal para criar um environment com todas as dependencias necessárias:

    2.1. Para Linux:
    ```markdown
        conda env create -f  environments/Linux/tensorflowEnvironmentLinux.yaml
    ```

    2.2. Para Mac:
    ```markdown
        conda env create -f  environments/Mac/tensorflowEnvironmentMac.yaml
    ```

3. Para ativar e entrar no environment:
```markdown
    source activate tensorflowEnv
```

## Clone o repositório

```markdown
    git clone https://github.com/larissalages/TensorFLow_Workshop.git
```

## Descomprimindo os datasets

A pasta datasets contém X bases de dados de imagens comprimidas. São elas: <br />
Descomprima os datasets que deseja usar no treinamento.

## Retreinando a rede

A rede pode ser retreinada com qualquer modelo pré-pronto da Google. Mas neste workshop vamos utilizar o MobileNet. Este modelo é otimizado para ser pequeno e eficiente, mas com o custo da precisão ser um pouco menor que a dos outros.
<br /> 
Você pode encontrar outros modelos pré-treinados neste repositório: [pre-trained-models](https://github.com/tensorflow/models/tree/master/research/slim#pre-trained-models).
<br />
<br />
Escolha uma das opções de resolução de imagem: 128,160,192, ou 224px. Melhores resoluções precisam de mais tempo de processamento, mas resultam em uma acurácia melhor.
<br />
<br />
Inicialize as seguintes variáveis shell para configurar a rede:
```markdown
IMAGE_SIZE=224
ARCHITECTURE="mobilenet_1.0_${IMAGE_SIZE}
```
## Executando o treinamento

Para começar a retreinar a rede, execute o seguinte comando:
```markdown
python -m scripts.retrain \
  --bottleneck_dir=tf_files/bottlenecks \
  --model_dir=tf_files/models/ \
  --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" \
  --output_graph=tf_files/retrained_graph.pb \
  --output_labels=tf_files/retrained_labels.txt \
  --architecture="${ARCHITECTURE}" \
  --image_dir=datasets
```
Este script baixa o modelo pré-treinado, adiciona uma camada final e treina esta camada com os datasets escolhidos. <br />
O script vai gerar primeiramente todos os arquivos de bottleneck, que são todas as camadas antes da camada final, depois disso o treinamento da camada final começa. 
O script retrain escreve os dados nos seguintes arquivos:

**tf_files/retrained_graph.pb**: Contém uma versão do modelo selecionado com a camada final retreinada com as suas categorias. <br />
**tf_files/retrained_labels.txt**: Arquivo de texto contendo as labels.

## Testando o modelo

A pasta test_set contém algumas imagens para teste. Para testar todas as imagens da pasta no modelo, execute o seguinte comando:

```markdown
python test_all.py
```
Para testar uma imagem especifica, faça:
```markdown
python -m scripts.label_image \
    --graph=tf_files/retrained_graph.pb  \
    --image=path_para_imagem/nome_da_imagem
```
O script retrain tem várias outras opções de linha de comando que você pode usar para tentar aumentar a acurácia do modelo.
Você pode ler sobre essas opções no help do script:

```markdown
python -m scripts.retrain -h
```

## Otimizando o modelo para rodar em dispositivos móveis

Uma maneira pela qual a biblioteca TensorFlow é mantida pequena, para dispositivos móveis, é porque ela suporta apenas um subconjunto de operações que são comumente usadas durante a fase de validação e teste. Essa é uma abordagem razoável, já que o treinamento não é realizado nas plataformas móveis. Você pode ver a lista de operações suportadas no arquivo tensorflow/contrib/makefile/tf_op_files.txt.
<br />
Para evitar problemas causados por operações de treinamento não suportadas, a instalação do TensorFlow inclui uma ferramenta, optimize_for_inference, que remove tudo que não é necessário para um determinado conjunto de entradas e saídas.
O script também faz algumas outras otimizações que ajudam a acelerar o modelo. Ele pode acelerar em até 30%, dependendo do modelo de entrada. Veja como você executa o script:

```markdown
python -m tensorflow.python.tools.optimize_for_inference \
  --input=tf_files/retrained_graph.pb \
  --output=tf_files/optimized_graph.pb \
  --input_names="input" \
  --output_names="final_result"
```
Este script vai criar um novo arquivo em tf_files/optimized_graph.pb.<br />
O modelo treinado ainda tem em torno de 80 MB de tamanho. Esse tamanho ainda pode ser um fator limitante para o aplicativo que o inclua. A maior parte do espaço ocupado pelo modelo é pelos pesos, que são grandes blocos de números de ponto flutuante. Então, para fazer a compressão dos pesos, é utilizado o arquivo **quantize_graph.py**.

```markdown
python -m scripts.quantize_graph \
  --input=tf_files/optimized_graph.pb \
  --output=tf_files/rounded_graph.pb \
  --output_node_names=final_result \
  --mode=weights_rounded
```
# Android APP

Abra o Android Studio. Depois que ele carregar, selecione "Abrir um projeto existente do Android Studio".
No seletor de arquivos, escolha TensorFLow_Workshop/android/tfmobile. <br />
Se aparecer um pop-up "Gradle Sync", a primeira vez que abrir o projeto, clique OK".
<br />
Se o seu celular ainda não estiver com o modo desenvolvedor ativado, [siga estas instruções](http://www.techtudo.com.br/dicas-e-tutoriais/noticia/2014/10/como-ativar-o-modo-desenvolvedor-no-android.html)
```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Thaysfsil/TensorFlow-Lite/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
