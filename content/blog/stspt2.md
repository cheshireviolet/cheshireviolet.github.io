+++
date = '2025-03-13T18:14:35-03:00'
draft = true
title = 'Slay the Spire e Análise de Dados - Parte 2'
tags = ['Data Analysis']
+++

Bom dia amigos! Aqui vou revelar um segredo: Tenho vários rascunhos com posts pela metade, sobre as coisas que estou fazendo. Escrever sobre o que motiva no momento faz mais sentido na minha cabeça, mesmo que fosse possível finalizar e postar sobre outros assuntos.

Caso tenha interesse:

- [Parte 1](https://cheshireviolet.github.io/blog/sixth/)

## HERE COMES A NEW CHALLENGER

Já havia escrito a introdução deste post antes, e o que vem depois também já foi escrito. Talvez eu tenha que editar.

Decidi comparar novamente as listas de certos atributos: `path_per_floor`, `path_taken`,`gold_per_floor`,`current_hp_per_floor` e `max_hp_per_floor`.

Juntando hp/gold, dão 56 entradas. Estudando-as com o histórico de run aberto do lado, começa no **floor 1** (index 0 nessa lista) e vai até o **floor 56** (o último boss, heart). Okay, pela última vez, isso está decidido.

Agora precisamos juntar `path_per_floor` e `path_taken`. Adicionando um **None** toda vez que esse é o valor em `path_per_floor` conseguimos achar um padrão. Acredito ter explicado da última vez, mas:

`path_per_floor` é o "evento" em cada andar e `path_taken` é a decisão do player. Os andares **None** são, em sua maioria, andares que no **histórico de runs** do jogo mostram como os baús após os bosses, ou o andar entre o terceiro boss e o estágio secreto (aonde seu personagem junta as chaves e abre a porta).

Sendo assim.......path_per_floor tem..........56 entradas. Comparando com as outras listas por andar:

![Table Engraçada com os valores](/static/stspt2/table.jpg)

O **Index 56 (ou Floor 57)** está sobrando? É possível que sim. No **histórico de runs**, existe um andar 57 que seria a animação ao conseguir o final verdadeiro.

Após testar no meu sample de quase 600 runs, toda vitória tem esse "andar extra". Só tirar. Ufa. Estava ficando doida.

## Preparação para subir os andares

Assim como no jogo, precisamos nos preparar para essa jornada (que será longa e árdua). Fiz uma longa pesquisa, e basicamente ninguém acredita ser possível fazer isso. **Ir com muita sede ao pote** é um tema recorrente em minha vida, no final das contas.

Bom, vamos lá.

Uma das melhores formas de lidar com problemas grandes é ir por partes. ~~Como jack o estripador kkkkkkk salve professora de matemática da sexta série.~~

Indo de andar para andar, primeiramente precisamos saber todos os eventos do `Neow`, e seus custos.

Os dados já foram extraídos anteriormente e salvos em uma única base, em JSON. Através dela é possível filtrar todos os eventos.

Fiz um script curtinho em Python para salvar os valores únicos:

```Python
bonusList = []
costList = []

for i in data:
    if data[i]["neow_bonus"] not in bonusList:
        bonusList.append(data[i]["neow_bonus"])
    if data[i]["neow_cost"] not in costList:
        costList.append(data[i]["neow_cost"])
```

```Python
bonusList: ['TRANSFORM_CARD', 'ONE_RANDOM_RARE_CARD', 'THREE_ENEMY_KILL', 'REMOVE_TWO', 'TWO_FIFTY_GOLD', 'THREE_RARE_CARDS', 'ONE_RARE_RELIC', 'RANDOM_COMMON_RELIC', 'HUNDRED_GOLD', 'TRANSFORM_TWO_CARDS', 'RANDOM_COLORLESS_2', 'TEN_PERCENT_HP_BONUS', 'BOSS_RELIC', 'UPGRADE_CARD', 'RANDOM_COLORLESS', 'THREE_SMALL_POTIONS', 'REMOVE_CARD', 'TWENTY_PERCENT_HP_BONUS', 'THREE_CARDS']
costList: ['NONE', 'PERCENT_DAMAGE', 'TEN_PERCENT_HP_LOSS', 'NO_GOLD', 'CURSE']
```

Existem alguns poréns, nisso. O Neow te dá escolhas, o que significa que podem ter bonus e custos que não foram escolhidos nas runs do meu sample. Vamos ignorar isso, por enquanto.

Indo pelos custos, que são mais simples. `NONE` é ignorável.

`NO_GOLD` é só ajustar o valor do gold inicial para zero, no neow. O que gera a pergunta, quanto de gold a gente começa em Slay The Spire?

De cabeça, eu lembro que é `99 gold` na Ascension 20, que é a maior dificuldade. Não é bom fazer coisas de cabeça.

Com um script parecido com o anterior, verifiquei os valores únicos em `gold_per_floor[0]`:

```Python
starting Gold: [113, 117, 116, 112, 366, 114, 118, 109, 119, 213, 111, 219, 359, 110, 211, 17, 19, 115, 131, 15, 128, 214, 99, 364, 13, 361, 20, 209, 367, 363, 218, 212, 14, 123, 369, 16, 18, 10, 215, 34, 210, 216, 12, 365, 368, 11, 360, 31, 417, 154]
```

Isso nos dá uma informação: `gold_per_floor`, e provavelmente `max_hp_per_floor` e `current_hp_per_floor` registram o `resultado` do andar, a partir do andar **1**, que é uma batalha.

**CECI DO FUTURO:** Oie, Ceci do futuro aqui, com notícias do futuro! `gold_per_floor`, `max_hp_per_floor` e `current_hp_per_floor` realmente contam apenas do primeiro andar, ainda estou pensando quão necessário é ter essa informação no Neow (fora o gold).
