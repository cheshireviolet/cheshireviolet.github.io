+++
date = '2024-12-02T01:50:58-03:00'
draft = false
title = 'Leetcode 128. Longest Consecutive Sequence'
tags = ['Leetcode', 'Python', 'Data Structure']
mermaid = true
+++

## Intro

Recentemente eu comecei a fazer os desafios do leetcode criado como [problemas de entrevistas da FAANG](https://leetcode.com/studyplan/top-interview-150/), tanto pra refrescar noções de estruturas de dados quanto para praticar python, já que de modo geral por mim faço tudo em C#.

Aproveitar a introdução para agradecer a [este post](https://navendu.me/posts/adding-diagrams-to-your-hugo-blog-with-mermaid/) pelo tutorial de como implementar mermaid em um fórum feito em hugo, graças ao post não precisei desenhar as árvores, e sim usar markdown :D

## O problema da vez

[Dada uma array não ordenada de inteiros *nums*, retorne o tamanho da sequencia mais longa de valores consecutivos.](https://leetcode.com/problems/longest-consecutive-sequence/description/?envType=study-plan-v2&envId=top-interview-150)

Exemplo:

```
Input: nums = [100,4,200,1,3,2]
Output: 4
Explicação: A sequencia mais longa de valores consecutivos é [1,2,3,4], o que torna a resposta 4.
```

**Todos os exemplos desse post vão usar o input acima como base** (sim, adicionei essa linha de texto após repetir algumas vezes que tava usando o exemplo acima)

A solução mais simples é ordenar a array, mas além de chata, é uma resposta pouco eficiente.

Na classificação do plano de estudos, esse problema está na lista de Hashmaps, porém, nas tags, está o termo *Union Find*, que eu nunca tinha ouvido falar antes.

## Union Find: Teoria

O resumo é que você separa os valores por critérios, gerando uma árvore muito eficiente de ser percorrida.

Exemplo:

{{< mermaid >}}
flowchart TD
  1 --> 2 --> 3 --> 4
  100
  200
{{< /mermaid >}}

*Tá, mas e aí?* Esse algoritmo é interessante porque ele possui duas funções: Union e Find (criatividade torando). É aí que vem a brincadeira de verdade:

### Find(n)

Sempre irá retornar a raiz do nodo. Find(4) retornaria 1, Find(1) retornaria 1.

Uma coisa importante dessa estrutura é que para fins práticos, o parente da raiz será sempre a própria raiz.

Para implementar, é bem simples:

```python
def find(x, Parent):
    if Parent[x] != x: #se não for a raiz
        return find(Parent[x]) #repete a função, porém com o Parent de parâmetro
    else:
        return x #se for a raiz, retorna ela mesma
```

### Union(x,y)

Junta dois grupos, a partir de suas raízes.

Exemplo: Union(4,200) alteraria a raiz de 200 para 1, que é a raiz de 4.

{{< mermaid >}}
flowchart TD
  1 --> 2 --> 3 --> 4
  1 --> 200
  100
{{< /mermaid >}}

Outra implementação simples:

```python
def union(x,y,Parent):
    Parent[Find(y)] = Find(x)
```

## Resolvendo o problema

É minha primeira vez resolvendo um problema usando Union Find, então é possível que minha resposta não esteja optimizada, ainda assim fiquei contente com o resultado.

A galera do leetcode gosta bastante de colocar casos excepcionais, porém válidos, nos testes, então logo de cara:

```python
if nums == []:
        return 0
```

Ok, podemos começar a resolução real, agora. Declarando as 3 variáveis que serão usadas:

- *graph* é um dicionário aonde sua chave é *nums[i]*, e o valor é a raiz de *nums[i]*
- *graph_sum* é um dicionario aonde sua chave é *nums[i]* e o valor é o tamanho da sua sequência
- *longest* é o tamanho da sequencia mais longa

```python
graph = {}
graph_sum = {}
longest = 0
```

Find e Union, conforme a estrutura que estamos lidando

```python
def find(i):
    if graph[i] == i:
        return i
    if graph[i] != i:
        return find(graph[i])

def union(i, j):
    root_i = find(i)
    root_j = find(j)
    graph[root_i] = root_j
```

E agora a parte que importa, que é bem pequena na verdade:

```python
for i in nums:
    if i not in graph:
        graph[i] = i
        graph_sum[i] = 1
        if i+1 in graph:
            union(i,i+1)
            graph_sum[graph[i]] += graph_sum[i+1]
        if i-1 in graph:
            union(i-1,i)
            graph_sum[graph[i]] += graph_sum[i-1]
    if longest < graph_sum[graph[i]]:
        longest = graph_sum[graph[i]]
return longest
```

Vamos quebrar por partes, pra simplificar. Primeiro, se o item ainda não existe em nossa árvore, adicionamos ele, com a raiz sendo ele mesmo.
Também adicionamos no dicionário de sequencias, sua sequencia sendo 1.

```python
if i not in graph:
    graph[i] = i
    graph_sum[i] = 1
```

Ainda dentro do if acima, para evitar duplicidade, se suas sequencias existem (i-1 e i+1) na arvore, realizamos o Union, e então adicionamos o tamanho da sequencia que foi adicionada a raiz.

```python
if i+1 in graph:
    union(i,i+1)
    graph_sum[graph[i]] += graph_sum[i+1]
if i-1 in graph:
    union(i-1,i)
    graph_sum[graph[i]] += graph_sum[i-1]
```

E por fim, se o tamanho da sequencia atual for maior que a maior sequencia registrada, atualizamos o *longest* para a nova maior sequencia. Então o loop continua com o próximo item da lista de entrada, e retorna o valor de *longest*

```python
if longest < graph_sum[graph[i]]:
        longest = graph_sum[graph[i]]
return longest
```

## Conclusão

Sinto que realizei algum teste de entrevista que tinha um problema parecido, ou que pelo menos eu poderia ter usado Union Find pra chegar em uma solução mais eficiente.

Após uma pesquisada, realmente é algo que é visto com certa frequência em entrevistas, então vale muito a pena dar uma olhada e brincar!
