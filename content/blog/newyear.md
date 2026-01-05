+++
date = '2026-01-05T20:00:51-03:00'
draft = false
title = 'Arrumando a Bagunça dos Projetos'
tags = ['Personal']
+++

Olá amigos, tudo bem? Feliz ano novo, espero que tenha ocorrido tudo bem nessa virada, 2026 já começou turbulentissimo mas no fim do dia seguimos nossas pequenas vidinhas. 

Não era uma promessa para o ano nem nada, mas resolvi dar uma limpa no dropbox, mais especificamente na parte de projetos relacionados a programação. Apesar de não postá-los aqui, tem pelo menos uns 10 posts nos rascunhos de projetos que comecei (e nunca terminei), alguns projetos que não eram grandes o bastante pra render um post, continuação de outros projetos, enfim.

Esse post vai ser um adeus para vários deles, com resumos e coisas **possivelmente** legais sobre.

## Projetos com posts de blog salvo nos rascunhos

### Provando a teoria do Warren Buffet de tempo no Mercado

Esse é um projeto que não era complicado, mas precisaria precisa de certa manutenção. 

Um dos caras que ficou bizentonario com investimentos foi o Warren Buffet, que diz que **"Time in the market > Timing the market"**, investir a longo prazo vai ser sempre mais rentável do que investir na hora certa nos ativos certos.

Tem uma história sobre uma certa aposta e basicamente se comprasse uma % de todos os ativos da bolsa baseado no tamanho deles no mercado, essa carteira hipotetica venceria de qualquer stock picking a longo prazo. 

Na época, peguei todos os tickers da bolsa brasileira e então fiz web scraping no investidor10 para pegar o Valor Patrimonial, Número de papéis e valor de mercado do dia. A ideia seria acompanhar esses números (e dividendos) com o passar do tempo e ver a progressão da carteira. Fiquei com preguiça, mas foi legalzinho.

### Project Mono Pt2

Uma das coisas que mudou esse ano foi o meu sistema operacional de preferência, migrei 100% pro linux, logo sem acesso ao .net perdi minha interface bonitinha em WPF e fiquei com preguiça de fazer algo novo. Ironicamente provavelmente é mais fácil fazer isso funcionar direito no linux, enfim.

### Slay the Spire Pt2

A parte boa é que eu tenho um backup de algumas coisas, a parte ruim é que o histórico de run não fica salvo na nuvem da steam. Perdi a motivação, mas a pt2 estava quase pronta, talvez eu poste no futuro!

## Projetos que tem pasta própria

Sim, isso é um critério pro projeto não ser "tão pequeno".

### Img2Video

Projeto que eu fiz pra minha mãe há uns 8~10 anos atrás. Pega várias imagens usando ffmpeg e converte em um video. Tem opção de adicionar legenda e música de fundo. Mega simples, tá no meu github, lol.

### MacroHelper

Nem lembrava desse, era um gerador de macro de AHK, projeto besta salvar tempo pra salvar tempo. Nem sei se tem AHK no linux mas hoje em dia sou mais tryhard com automações (em partes porque trabalho com isso).

### Image Simplification

Projeto rapido pra um bootcamp que deixa uma imagem colorida em grayscale, pela emoção fiz mudar vários pixels por vez como uma animaçãozinha pra ficar legal. Mas foi extremamente simples

### Last.fm

Já tem um post sobre, pretendo continuar o projeto no futuro, mas no momento preguicinha.

### MagiCat Route Analyzer

Lá por 2018~2020 eu fazia speedruns, um desses jogos se chamava MagiCat. é um platformer que você pode escolher as fases que vai, comprando itens que permitem pular estágios e coisas assim. A rota de Any% não era (e potencialmente ainda não é, ninguém jogou muito esse jogo além de mim e um japonês muito foda) optimizada, então na época eu peguei dados de tempo de fase considerando pegar certos itens pra tentar otimizar runs. Projeto legal, hoje em dia eu faria MUITO melhor. 

### Infernoble Knights (Yu-Gi-Oh! 1)

Gosto de estudar decklists diversas de pessoas bem sucedidas com baralhos que quero aprender. Na época tava afim de aprender Infernoble, que são os cavaleiros Yaoi e sua princesa Fujoshi. O programa fazia Web Scraping do Master Duel Meta (não tem muito como achar boas decklists de infernoble, no MDM pelo menos a galera pegou rank bom com o deck) fazendo comparativos de decisões de tech para entender porque usar ou não usar certa carta ou combinações de carta.

Isso sempre ajuda e eu sempre fiz manualmente, foi fácil mas bem divertido! 

### Yu-Gi-Oh! 2

Usando a api do tcg player peguei dados de todas as cartas do jogo, pra um possivel projeto futuro que ainda não desisti! Novidades em breve (talvez)

### Yu-Gi-Oh! 3

Se você acompanha o r/masterduel vai perceber que o chapéu de alumínio é super popular por lá, principalmente sobre o resultado da moeda (que decide quem joga primeiro) e a mão inicial.

O projeto usaria Pytesseract pra anotar automaticamente as minhas mãos iniciais e moedas pra gerar dados.

É MUITO CHATO FAZER ISSO ENTÃO DESISTI. (além disso, tem métodos melhores do que pytesseract, já que o jogo é em Unity e dá pra fazer dll injection porque usa mono hihi)

### Peixes

Só pra não deixar batido, uma colega e eu tivemos essa ideia que envolve ter uma base de peixes brasileiros e fotos deles. Não está morto mas não estou fazendo nada com isso no momento. NÃO IRÁ MORRER

## Jogos 

### Choncho Adventure

Meu anjo querido me perdoe eu não quero mais continuar a pixel art do jogo então desisti na metade (protótipo nem ficou pronto). 

Também tem o negócio aonde eu não sabia se queria transformar em um roguelike ou um joguinho de arcade. Talvez reviva pra alguma jam, usando Godot.

### Project Box

Esse joguinho tem potencial mas não mexi em quase nada nele. Incrementalzinho besta que também não desisti

### Momo Adventure

Fiz pra uma entrevista que seria para ensinar programação para crianças. Fui forçada a usar PyGameZero. Espero nunca mais ter que usar isso.

### Pseudoclicker

Originalmente ia ser meu projeto pra gamejam de 20 segundos, mas acabei desistindo porque não era divertido.

Era um clicker, mas você podia fazer combos, usar outros botões, segurar o botão, etc. Teria um sistema de progressão meio de RPG, classes, etc.

Mas não era divertido então xauuuu

### Tamagochi Match-3

Sinceramente eu não lembro de nada a respeito, além de ser um tamagochi com match-3 envolvido. Era incremental?? Talvez???

Talvez faça um tamagochizinho no futuro mas não vai ser nada a ver com isso aí

### Mind Defrag

Esse projeto aqui é brabo, estava usando sadconsole+monogame pra fazer esse joguinho de texto super inspirado em Severance. Scope Creep me destruiu mas talvez eu volte

## Projetos pequenos

Metade está em uma pasta "outros" e a outra metade em uma pasta "small stuff". NÃO SEI DIZER A DIFERENÇA. ORGNIZAÇÃO NOTA 10

### bruteforce.py / diary.py

Dois scriptzinhos pra CTFs!

### tati.py

Plotei uns gráficos, não lembro o porquê.

### risos.py

Tava tentando burlar um app aí...Merecia o próprio blog post mas não funciona no linux então nem tenho como fazer mais isso

### nloth x question card.py

No Slay the Spire após cada batalha você pode escolher entre 3 cartas. Elas tem raridades diferentes. Queria testar as % de vir cartas melhores usando reliquias diferentes que alteram essas chances. Foi legal, rapidinho e direto.

### palindromo.py

Verifica se uma frase é palindromo. Eu faço Palindromos.

## Conclusão

No final das contas acabei deletando quase nada, mas dei uma microlimpa e surgiu esse postzinho engraçado.

Até a próxima! 