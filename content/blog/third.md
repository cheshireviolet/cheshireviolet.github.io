+++
date = '2024-12-03T01:43:07-03:00'
draft = false
title = 'Compilando a Darknet no Windows 10 e treinando modelo com a YOLOv3'
tags = ['Machine Learning']
+++

## Introdução

**AVISO:** Por mais que seja possível treinar um modelo utilizando a CPU, é um processo extremamente mais demorado do que poderia ser, pela sua própria sanidade, usa o Google Colab se possível. Ou alguma distro de linux.

Um dos exercícios do Bootcamp de machine learning que estou realizando é realizar exatamente o que diz no título desse post. No caso, a parte de ser no windows é por eu ser cabeça dura demais para só usar o Google Collab e não ter problemas.

Se você tentar seguir o tutorial da própria [Darknet](https://pjreddie.com/darknet/yolo/), é possível que você encare os mesmos problemas que eu encarei, então vou explicar como fiz tudo funcionar.

Além disso, essa também é a própria resolução do exercício, uau estamos unindo o útil ao agradável.

Antes de prosseguirmos, caso tenha interesse no artigo sobre a YOLOv3 escrito pelos seus criadores, recomendo [YOLOv3: An Incremental Improvement](https://arxiv.org/abs/1804.02767) (está em inglês).

## Requerimentos

- [Microsoft Visual Studio](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community) (Não testei o code, mas talvez funcione)
- [CMake GUI](https://cmake.org/download/)
- A própria [Darknet](https://github.com/AlexeyAB/darknet/archive/master.zip)
- [OpenCV](https://sourceforge.net/projects/opencvlibrary/) (Opcional)
- [CUDA](https://developer.nvidia.com/cuda-downloads) (Opcional, precisa ter **GPU DA NVIDIA**, é doloroso se não tiver)

## Compilando a Darknet

1. Extraia o darknet-master.zip
2. Instale os programas da lista que não foram instalados
3. Abra o CMake GUI
4. Browse source -> Selecione a pasta aonde extraiu a darknet.
5. Browse Build ->  Selecione a pasta aonde quer que seja criada a solução (.sln)
6. Configure irá carregar todas as opções de configuração da Solução.
7. **Opcional:** Se não for usar CUDA/OpenCV, desmarque as opções ENABLE_CUDA e ENABLE_OPENCV
8. Generate irá criar a solução no diretório especificado -> Open Project irá abrir a solução no visual studio.
9. Build Solution (F6) como Release para x64.

Os binários estarão na pasta Release no diretório do .sln.

## Baixando o COCO Dataset

Pelo próprio [site do COCO](https://cocodataset.org/#download) eles tem alguns tutoriais de como baixar usando a COCO API, MASK API e através da FiftyOne. Para fins práticos (ou não), estarei seguindo o tutorial do próprio site da Darknet.

Dentro do *darknet-master/scripts/* tem um arquivo chamado *get_coco_dataset.sh*, abrindo com um editor de texto, você vai ver que primeiro o arquivo clona a COCO API e cria uma pasta chamada *images* logo após. Se não tiver git instalado, basta baixar o repositório zipado, igual feito com a darknet.

```shell
git clone https://github.com/pdollar/coco
cd coco
mkdir images
```

Então ele usa **wget** para baixar alguns arquivos, por fins práticos é mais rápido baixar só clicando neles, mesmo (se não tiver wget para Windows instalado, claro)

- Os arquivos abaixo são extraídos dentro da pasta images, que acabamos de criar.
  - train2014.zip
  - val2014.zip
- E os arquivos abaixo ficam dentro do diretório da Coco
  - instances_train-val2014.zip
  - 5k.part
  - trainvalno5k.part
  - labels.tgz

Não colocarei links pois é possível que na data que esteja lendo eles tenham expirado ou até mesmo os trocado (até porque o dataset de 2014 está meio desatualizado, né?). Baixe tudo, é possível que **demore um pouco** (os datasets de treino e validação são bem pesadinhos)

Extraia que foi baixado tudo na pasta aonde Buildou a Darknet. O arquivo labels.tgz gera um arquivo labels.tar. É basicamente um Zip dentro de um Zip, 7Zip descomprime. :

Para finalizar, o script concatena o diretório atual com cada linha dos arquivos e coloca tudo em dois arquivos de texto.

Você pode usar os seguintes comandos no Powershell para realizar essa parte final do script:

```Powershell
Get-Content .\5k.part | ForEach-Object { "$PWD\$($_ -replace '/', '\')" -replace '\\+', '\' } > 5k.txt
Get-Content .\trainvalno5k.part | ForEach-Object { "$PWD\$($_ -replace '/', '\')" -replace '\\+', '\' } > trainvalno5k.txt
```

## Configurando a darknet para treinamento com a COCO

Dentro de *darknet-master* existe uma pasta chamada *cfg* e outra chamada *data*. Copie ambas e cole-as dentro de *Release*. Dentro de *cfg*, nós precisamos editar o arquivo *coco.data*

```text
classes = 80
train = Caminho até trainvalno5k.txt
valid = Caminho até 5k.txt
names = Caminho para data/coco.names (A pasta que acabou de copiar)
backup = backup
eval = coco
```

Também precisamos editar o arquivo *yolo.cfg*, já que está configurado para testes, e não treinamento.

Altere as linhas abaixo:

```cfg
batch=64
subdivisions=8
```

Copie o arquivo *pthreadVC2.dll* da pasta *darknet-master\3rdparty\pthreads\bin* para a pasta Release.

Só falta um ultimo arquivo, o [darknet53.conv.74](https://pjreddie.com/media/files/darknet53.conv.74), que é um arquivo pre-treinado de pesos. Coloque-o em Releases.

## Treinando o Modelo, finalmente

No powershell, basta executar:

```Powershell
./darknet detector train *caminho completo*\cfg\coco.data *caminho completo*\cfg\yolov3.cfg darknet53.conv.74
```

Trocando o caminho completo pelo...caminho completo...

Esse processo vai demorar...muitos dias...por favor não cometa o mesmo erro que eu.

Após treinado, será gerado um arquivo .weight, que pode ser utilizado para detecções, como no exemplo abaixo:

```shell
./darknet detect cfg/yolov3.cfg *arquivo.weight* data/dog.jpg
```

## Bônus: AH MAS EU TENHO UMA PLACA DA AMD

Existe um repositório feito pra isso, mas não sei se é por estar desatualizado ou outro motivo, simplesmente não consigo compilar.

Talvez faça um post no futuro com uma solução, mas até lá, se quiser se aventurar é só [clicar aqui](https://github.com/sowson/darknet?tab=readme-ov-file).

## Conclusão

Da próxima vez irei comprar uma placa da NVIDIA.
