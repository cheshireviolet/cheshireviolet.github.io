+++
date = '2025-03-04T18:25:36-03:00'
draft = false
title = 'Slay the Spire e Análise de Dados - Parte 1'
tags = ['Python', 'Data Analysis', 'Slay the Spire']
+++

Bom dia amigos! Novamente, eu devia escrever o primeiro DevLog do Choncho Adventure, mas novamente me peguei em uma situação aonde estou mexendo em **outro projeto**.

Tive alguns avanços trágicos no Project Mono, que vão mudar um pouco como eu levo o projeto. Aprendi muito maaaaaaaaas ficará para outro post.

### Tudo que foi falado aqui vai ser divulgado bonitinho no github mesmo, soon ™

## Introdução

[Slay the Spire](https://store.steampowered.com/app/646570/Slay_the_Spire/) é um jogo da categoria Roguelike Deckbuilder, não sei dizer se foi o primeiro, mas foi o que influenciou tantos jogos no futuro a ponto de não ter outro tipo de recomendação na minha Steam.

Comecei a jogar lá quando o jogo estava em Early Access, a atualmente tenho aproximadamente **870 horas.** É meu jogo de conforto e facilmente um dos meus jogos preferidos de todos os tempos.

Logo quando o jogo saiu, os desenvolvedores pegavam dados das runs (**com seu consentimento**) para realizar o balanceamento do jogo, mas os mesmos também ficam guardados localmente, em arquivos **.run** que na verdade são JSON.

Foi uma ideia que deu super certo para os desenvolvedores, o jogo é ridiculamente bem balanceado, com exceção da última personagem lançada, a Watcher, que não recebeu o mesmo carinho de anos de Análise de Dados e patches.

Como é um formato super acessível e leve, houveram diversos projetos ao longo do tempo, super legais, que envolvem esses arquivos.

Existia o SpireLogs, aonde usuários podiam fazer upload de suas runs e então ver dados sobre suas vitórias, derrotas, personagens, cartas, relíquias, mas também podia comparar com todas as outras runs que haviam sido carregadas.

Atualmente existe o [Slay the Report](https://slaythereport.kojim.net/)(em jp), aonde usuários também podem fazer upload e download de runs.

Sempre houve esse carinho, tanto pela parte dos desenvolvedores quanto pelos jogadores.

Estou lentamente retornando para a área de dados, então resolvi realizar projetos relacionados, e nada melhor do que pegar uma paixão para trabalhar em cima, né?

## Objetivo

Meus objetivos iniciais eram, em nenhuma ordem de importância: Aprender Pandas, Seaborn e PowerBI/Tableau.

Na maior parte da minha vida, em projetos pessoais envolvendo dados eu sempre usei Excel/Google Spreadsheets, mas nisso meus relatórios, apesar de pontuais, não são tão bonitos e engraçadinhos como querem atualmente no mercado de trabalho. Ao mesmo tempo, usar alguma linguagem de programação e bibliotecas relacionadas pode agilizar bastante o processo. Pensei em aprender R, mas eu já mexo com python, então deixa o R pro futuro próximo, a fila está meio grande e o tempo é curto.

## Estrutura do Projeto

Não gosto de revelar a grande ideia final por trás de todos os passos, mas dá para já dar uma palhinha inicial. A complexidade não vem dos passos em si.

- **Extração:** Para algumas coisas, posso utilizar os .run que ficam guardados localmente, para projetos maiores posso pegar runs de lugares como Slay the Report ou datasets espalhados por aí. Para o primeiro caso, já criei um script curtinho em Python que lê todas as runs e as guarda sem edição em um arquivo só, para facilitar processos futuros.
- **Normalização:** O post desse mês basicamente será quase inteiro sobre normalização dos dados, já que apesar de extremamente útil, os dados da run são...bagunçados...para poupar as palavras que usei pra reclamar sobre reclamando com amigos.
- **Análise:** Por que a gente precisa usar os dados para alguma coisa né? Eu tenho algumas ideias básicas e outras mais ousadas sobre o que quero fazer, mas fica pra depois. :p
- **Visualização:** Vai ser o ponto que eu vou tentar ignorar ao máximo mesmo sendo algo que eu precisava aprender meio com urgência. Pelo lado bom, decidi que vou focar em Tableau e Seaborn para isso, PowerBI por algum motivo trava quando eu tento exportar os dados de **meras** 600 runs, pffft.

## Extração

Não tem muito a ser falado aqui, começando com projetos menores, em poucas linhas consegui pegar todos os .run e salvar tudo em um grande .json com os dados intactos, usando o **run_id**(uuid) dos próprios arquivos para evitar duplicidade. Também só guardei runs que não forem dailies, visto que os modificadores impactam bastante quase tudo que acontece.

## Normalização - Parte 1 (a parte fácil)

Apesar de todos os arquivos serem .run que vieram do mesmo jogo, é necessária uma normalização, já que tem várias chaves que são inúteis ou redundantes, como `is_prod`, que era usada durante a alpha. Como não tenho runs o bastante pois sempre esqueço de ligar, também removi as chaves relacionadas a mods, como os status específicos de cada relíquia.

Outra parte importante é normalizar tipagem de algumas coisas, visto que em alguns lugares os andares do jogo estão como inteiros, e em outros como float (sempre vai ser um número inteiro).

Também tem algumas coisas engraçadinhas, as classes estão definidas como `DEFECT, IRONCLAD, WATCHER`, mas a Silent está como `THE_SILENT`.

Várias cartas, relíquias e eventos estão com nomes da Beta, coisas com apóstrofo as vezes perdem espaçamento, enfim.

Não é tão difícil, mas é super importante, para que a base de dados seja reliable a longo prazo sem manutenção.

## Normalização - Parte 2 (eu vou explodir)

Nem tudo são flores. Um dos meus objetivos é converter as runs em um formato que seja possível ver EXATAMENTE qual foi a decisão do player e suas alternativas (quando possível). Slay the Spire é um jogo cheio de decisões e possibilidades, e o log das decisões é baseado, em sua grande parte, em como vai aparecer no **Match History**.

Então vamos supor que eu queira recriar os dados do andar **7**. Okay, segundo a chave `path_per_floor[7]`, o resultado é `?`, que significa que pode ser um evento, ou outra coisa como um combate ou um baú do tesouro.

Ainda tenho que ver com calma se vai ser **sempre** assim, mas nesse caso era um Tesouro, que é possível verificar pelo `path_taken[7]`, que retorna `T`.

Até aí okay, mas e se eu te falar que `path_per_floor` tem **56** valores nessa run, enquanto **path_taken** tem **52**? É explicável, mas para automatizar isso você já tem que criar a primeira exceção. Mesmo que fosse usado só o `path_taken`, que é mais preciso, a run teve, segundo `max_floor`, **57** floors. Ironicamente, outros valores que são separados por andar possuem no máximo **55**.

Deixe-me te explicar enquanto eu crio mais uma dúvida na sua cabeça:

Pelo próprio jogo, no match history, uma run começa no andar **1** e vai até o andar **57**. No Andar **1** sempre vai começar com um `M` (batalha contra monstro normal).

`path_per_floor` considera todos os andares do jogo, incluindo os que não foram tomadas decisões, que estão ali como `null`. Em uma run na dificuldade mais alta, que vai o mais longe possível, as ocorrências de `null` são após o primeiro e segundo bosses, aonde você vai para um andar abrir um baú de tesouro, após o terceiro boss, em que você irá usar chaves para abrir o caminho secreto, e logo após o heart, que representa o fim do jogo. `56 - 4 = 52`, a conta bate, graças a deus.

Mas e por que `max_floor` é 57? ÓTIMA PERGUNTA, não existe um motivo, mas minha interpretação pessoal é que o jogo considera o evento do `Neow`, que precede todas as runs, como um andar.

Antes de toda run começar, a grandiosa baleia Neow lhe oferece algumas opções para facilitar sua vida na run. Quanto melhor a recompensa, maior o risco, podendo lhe dar dano, retirar seu gold, e, às vezes, lhe dar uma curse. A blessing fica em `neow_bonus`, enquanto o custo fica em `neow_cost`.

Parece simples né? NÃO.

Vou usar uma run de exemplo, o `neow_bonus` foi `ONE_RARE_RELIC`. Qual relic? Vamos descobrir!

existe o `relics_obtained`, que nesse caso está assim:

```JSON
"relics_obtained": [
    {
        "floor": 6,
        "key": "Turnip"
    },
    {
        "floor": 8,
        "key": "Boot"
    },
    {
        "floor": 9,
        "key": "Gremlin Horn"
    }
]
```

A primeira relíquia da lista é uma `Turnip`, só que ela foi adquirida no andar `6`, em um tesouro.

Existe algum outro lugar? Graças a deus existe! A chave `relics`:

```JSON
"relics": [
    "Cracked Core",
    "Gambling Chip",
    "Turnip",
    "Boot",
    "Gremlin Horn"
]
```

Com as informações passadas, é possível deduzir que a relíquia obtida foi `Gambling Chip`, já que a `Cracked Core` é a relíquia inicial da classe `DEFECT`.

Para automatizar isso, uma pessoa poderia dizer para usar o segundo item da lista ao detectar o evento, mas infelizmente existe uma exceção que tornaria essa ideia errada:

- Existe um evento aonde pedem uma relíquia sua em troca de outra relíquia, é possível que a relíquia obtida nesse evento suma, a nova relíquia vai para o final da fila.

Isso torna fazer um track apropriado das relíquias uma tarefa bem difícil. E a Curse?

`card_choices` começa com a escolha do primeiro combate, e possui apenas escolhas que envolvem opções.

Vamos ver todas as cartas em `master_deck`:

```JSON
"master_deck": [
    "AscendersBane",
    "Strike_B",
    "Strike_B",
    "Strike_B",
    "Strike_B",
    "Defend_B",
    "Defend_B",
    "Defend_B",
    "Defend_B",
    "Zap+1",
    "Dualcast",
    "Doom and Gloom",
    "Coolheaded",
    "Coolheaded",
    "Undo"
]
```

A única curse no deck é `AscendersBane`, que é adicionada em todo deck após uma certa dificuldade. Para não extender demais, encontramos em `event_choices`, que segue esse formato:

```JSON
"event_choices": [
        {
            "cards_removed": [
                "Doubt"
            ],
            "damage_healed": 0,
            "gold_gain": 0,
            "player_choice": "Card Removal",
            "damage_taken": 7,
            "max_hp_gain": 0,
            "max_hp_loss": 0,
            "event_name": "Golden Wing",
            "floor": 2,
            "gold_loss": 0
        },
```

Achamos a curse! Dá para deduzir pois ela foi removida logo cedo, no andar 2 (o andar 1 sempre vai ser um combate). Mas não existia menção a ela em nenhum lugar.

Ainda sobre `event_choices`, existe um evento com vampiros, que substituem cartas `strike` no seu deck por cartas `bite`. Apesar de no exemplo acima ter `cards_removed`, nesse outro já não tem.

Um dos meus objetivos é conseguir guardar as `decisões` tomadas em todos os andares e o `game state`, na medida do possível (não tem log detalhado das batalhas, por exemplo), mas essas inconsistências geram várias mini-exceções chatas de lidar.

Existem dois jeitos de guardar o `game state` e as `decisões` por turno de forma prática:

- Ir de trás pra frente, pegar o estado do jogo no ultimo andar da run, e tentar fazer backtrack para preencher possíveis lacunas.

- Preencher os dados com Place Holders, que serão meticulosamente investigados e substituídos conforme a run avança.

Enfim, esse é outro projeto que parecia muito mais simples na minha cabeça.

Mais um projeto que será lentamente atualizado, mas pelo menos agora estou anotando o progresso com esses posts.

Obrigada a todos que leram, até a próxima
