## Setup

1. Instale mini-conda (mode default): https://conda.io/miniconda.html

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

## Descomprimir os datasets

A pasta datasets contém X bases de dados de imagens comprimidas. São elas: <br />
Descomprima os datasets que deseja usar no treinamento.

#Retreinando a rede
A rede pode ser retreinada com qualquer modelo pré-pronto da Google. Mas neste workshop vamos utilizar o MobileNet. Este modelo é otimizado para ser pequeno e eficiente, mas com o custo da precisão ser um pouco menor que a dos outros.   
Outros modelos pré-treinados: https://github.com/tensorflow/models/tree/master/research/slim#pre-trained-models. 


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
