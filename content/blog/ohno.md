+++
date = '2025-10-05T22:07:02-03:00'
draft = false
title = 'Um blog no neocities me fez voltar a fazer ctfs'
tags = ['CTF']
+++

Bom dia amigos! Minha marida há algum tempo resolveu aprender HTML/CSS/JS e acabou descobrindo o incrível mundo do neocities! 

Nessa aventura ela me mandou um blog chamado [hello room](https://hello-room.neocities.org/). Um blog de 'puzzle' (nome oficial da categoria no neocities). E quando vi estava resolvendo CTFs tal qual fazia há uns 10 anos atrás. Note que, apesar de achar muito maneiro, não fui muito longe, tanto por preguiça quanto por falta de interesse **o bastante**.

Mas esse me prendeu, por um tempo. Esse post contém spoilers, então se você quer jogar, deixa pra ler depois! :D 

## big_cat.png

Após encontrar o link em uma porta, tem um gato e um texto em base64. Basicamente diz INVESTIGUE A MINHA IMAGEM. 

A imagem em questão é o big_cat.png 

usando um simples `strings` você acaba encontrando a solução, o link pro próximo puzzle! Bem simples, mas algo que eu não esperaria de um blog no neocities

## big_cat_8.png

Eu fiquei MUITO tempo nesse, e foi aonde minha inexperiência realmente ficou bem clara.

O **big_cat** perdeu uma vida, nos dá parabéns e dá uma dica: **11 números em hex**.

Usei strings, file, identify, exiftool, hexdump, e nada. Tentei stegsolve, absolutamente nada, nadinha.

Acabei entrando no discord do blog. Descobri que dá para ir na página /hint em um puzzle para descobrir algo que pode ajudar.

"A imagem vai ser importante". PUTA QUE PARIU. Completamente perdida. Tentei transformar o hex em caracteres, deu ruim, varios são além de caracteres legíveis.

Minha pista é que o link do puzzle atual **também** tem 11 caracteres. Tentei achar uma relação entre os dois, sem sucesso.

Pensei em usar hexdump para ver se tinham caracteres nas posições em hex que o gato falou. AÍ EU COMETI UM GRANDE ERRO: como eu migrei recente pro linux, nunca tinha usado o hexdump e o código que achei na internet não mostrava o arquivo todo (sabe-se lá o porquê), então como haviam posições que iam além do arquivo, desisti disso.

Pedi ajuda no discord, falaram que eu estava correta em querer usar o hexdump. AÍ EU ENTENDI QUE NÃO ERA O DUMP COMPLETO. Completamente besta, mas minhas intenções eram boas. Ao tentar novamente, com o dump completo, consegui chegar no próximo, MAS por um erro de atenção faltou um caractere, o que me fez perguntar no discord novamente e a pessoa ficar muito "putz tão perto acho q vc esqueceu de uma letra aí". **Que vergonha, sério**.

## big_cat_7.png

**"Okay, no more clues, now I want to watch you struggle."**

EU JÁ ESTAVA STRUGGLING, mas tudo bem! Ao clicar no **big_cat**, é baixado um struggle.zip, que contém uma história e uma ASCII gigantesca de um gato que parece muito o gato de cheshire.

No **big_cat_7.png** não havia nada. No hints, uma imagem que tinha sua origem a página de [Vigenère](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher), que é um método de encriptação baseado em uma chave. A chave está na história, bem simples.

Fiz um scriptzinho em python super simples pra aplicar na ascii art, que era gigantesca. O efeito foi bem legal, comparar os dois arquivos visualmente, a resposta no minimap do vscode fica mt mais nítida, e então encontramos o link pro próximo gato!

## six_hearts.png

Não achei nada nos arquivos **cheshire.png** ou **six_hearts.png**, mas havia algo cifrado no HTML da página.

Não era césar, a key não funcionou, depois de uma pesquisa descobri que poderia ser uma cifra de substituição monoalfabética, que basicamente é manualmente escolher letras para virarem outras. Por ex: se `B = Q, O = W, L = E`, `LOBO BOBO` vira `EWQW QWQW`.

Claro que fazer isso manualmente é uma loucurada que só, encontrei um site que faz brute [force aqui](https://www.guballa.de/substitution-solver). Ele usa um dicionario pra fazer assimilações e decriptar o texto. Achamos o link do próximo puzzle.

## big_cat_5.png

Nada na imagem, um link para um arquivo `.pcapng`, que descobri que é basicamente um log de tráfego de rede, se eu entendi certo. Usei o **wireshark** para ler, não sei se era a melhor opção ou não, mas deu conta do recado. 

No log basicamente é um chat entre dois gatos, e a lore desse puzzle, e o link do próximo puzzle. Nada escondido, diria que esse foi puramente lore dump.

## big_cat_4.png

**Coming soon**.

Bom, o puzzle ainda não está completo e tem mais pela frente, mas ainda não saiu. Esse puzzlezinho me fez aprender várias coisas sobre criptografia, e me deu vontade de voltar a fazer CTFs.

Queria que tivessem mais CTFs com lore interessante assim, no momento estou fazendo alguns do picoCTF. 

Tentei ao máximo não dar spoilers da história, mesmo falando como resolver acho que é legal ver por conta!

## Nada a ver com os puzzles

Obrigada a todos que leram, ultimamente não tenho tido muito tempo/cabeça para fazer posts, tenho uns 5 rascunhos que escrevo enquanto mexo em projetos pessoais, mas até terminar os projetos em si não acho que eles vão pro forno. :p

E estou com outros projetos em mente, mas espero que ainda esse ano saia alguma 'parte 2' de um dos projetos que já fiz post aqui.