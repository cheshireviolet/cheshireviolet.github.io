+++
date = '2025-05-21T18:35:34-03:00'
draft = false
title = 'Data Lake com estatísticas do Last.fm'
tags = ['Python', 'Data Analysis']
+++

Bom dia amigos! Fazia tempo que não postava nada. Últimos 2 meses foram extremamente corridos, muita bagunça e adversidades, mas consegui fazer algo a tempo de manter 1 post por mês.

Uma das trends que sigo no BlueSky (que persiste por uns 10 anos?) é a de postar uma colagem dos álbuns mais ouvidos. Existem vários sites que fazem isso, como TapMusic. Gosto bastante, mas gostaria que tivessem mais númerozinhos engraçadinhos.

![Colagem TapMusic](https://github.com/cheshireviolet/cheshireviolet.github.io/blob/main/content/blog/static/lastfm/collage.jpg)

Foi aí que decidi adicionar estatísticas a respeito dos álbuns que escuto! Apesar de ser relativamente simples, decidi utilizar uma abordagem mais técnica e escalável, para o futuro.

## Camada Bronze - O E de ETL

Antes de tudo, precisamos alimentar nossa base. O Last.fm disponibiliza uma API gratuita, que possui um método `user.gettopalbums`. Bem conveniente, basta colocar usuário, api key, período de tempo e recebemos tudo que precisamos.

A API deles retorna um JSON que tem vários dados que são irrelevantes, então pego apenas **artista**, **nome do album**, **playcount**, **rank** e um **link da imagem de capa do album** e salvo, junto a data do dia da extração em um JSON.

## Camada Prata - O T DE ETL

Para os dados brutos, um simples JSON já resolve tudo. Para a segunda camada, decidi utilizar um banco de dados simples em **SQLite**, utilizando **SQLAlchemy** para a conexão e queries, visto que funciona perfeitamente com **pandas**.

Criei uma tabela para os artistas e uma tabela para os álbuns, o que facilita agregar dados específicos.

Por último, criei uma tabela com algumas estatísticas, que é atualizada com o tempo, conforme ganhamos novas entradas.

## O L de ETL

Como o projeto não é suficientemente grande, a camada Ouro e a camada Prata são a mesma.

Para finalizar o projeto, basta baixar as imagens e usar a biblioteca **PIL** para gerar a colagem e adicionar o texto com as estatísticas.

![Colagem Final!](https://github.com/cheshireviolet/cheshireviolet.github.io/blob/main/content/blog/static/lastfm/collage2.jpg)

Existem alguns ajustes a serem feitos, como uma fonte melhor e alguns status extras, mas pretendo atualizar nas próximas semanas.

Também pretendo, assim que tiver tempo, colocar todo esse serviço na nuvem, possibilitando outras pessoas de utilizarem o serviço. Fazer uma WebAPI em Azure e talvez usar algum banco gratuito ,visto que mesmo com licença de estudante acaba ficando caro a curto prazo.

Gostei muito de fazer esse projeto, mas a minha parte preferida dele é que não ficou só num protótipo. :P

Até a próxima!
