+++
date = '2025-08-04T18:53:16-03:00'
draft = true
title = 'Testando a Teoria do Buffet'
tags = ['Python', 'Data Analysis', 'Web Scrapping']
+++

Bom dia, amigos! Cá estou eu, depois de meses. Apesar de estar programando como nunca (estou empregada yay :D), estou meio sem tempo para fazer coisas interessantes e postar aqui.

Vi um video que falava sobre a teoria Buy and Hold, que é a que eu já adoto naturalmente, mas falando algo do tipo **"se comprar cada ativo baseado em seu valor de mercado e deixar lá você vence de qualquer índice"**, ou algo assim.

Decidi que queria testar isso com a nossa queridissima bolsa brasileira, porém existe um grande problema. Obviamente não tenho o dinheiro necessário para fazer isso, muito menos para fazer isso pra testar algo. Queria simular usando a API da B3, mas descobri que a mesma é paga...Mas nem tudo está perdido.

## Parte 1: Pegando dados com Web Scraping

Okay, peguei todas as ações da bolsa de um site duvidoso na data mais antiga que achei (junho de 2024), e utilizando Selenium peguei a quantidade de papéis por ticker do site do investidor10. Claro que os números podem e em vários casos vão estar bem diferentes mas pegar esses números de datas antigas seria muito trabalho.
