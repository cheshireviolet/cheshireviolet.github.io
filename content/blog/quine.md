+++
date = '2025-04-01T14:13:08-03:00'
draft = false
title = 'Quine ofuscada'
tags = ['Silly']
+++

Olá amigos! Novamente estou aqui, com algo que não é relacionado aos posts anteriores :D

O prazo para o projeto e esse post é de **24 horas**.

Antes de dormir, estava pensando em **Quines**, que são códigos que retornam eles mesmos (sem ler o próprio arquivo), e como gostava muito desse conceito que quebrava minha cabecinha simples quando era jovem.

Aí pensei um pouco mais, e como seria uma **quine ofuscada**?

Recentemente tenho visto muitos videos do [**John Rammond**](https://www.youtube.com/@_JohnHammond), aonde ele desbrava no mundo de análise de malware, tenho que interpretar código ofuscado. Não sei **nada** real sobre ofuscação de código, mas vi bastante videos dele para entender que dá pra brincar bastante com isso!

O desafio de fazer uma Quine ofuscada envolve fazer algo que seja consistente e confuso.

## A Quine

Peguei da wikipedia, será a base. Fazendo em python pra salvar tempo usando Jupyter no VSCode.

`c = 'c = %r; print(c %% c)'; print(c % c)`

A ideia inicial era pegar e criar métodos, então renomear tudo pra ficar confuso, pensando nisso, cheguei no seguinte código:

```python
def asd():
    qwe = 'def asd()\n\tqwe = %r;\n\tprint(qwe %% qwe)\nasd()'
    print(qwe % qwe)
asd()
```

Não é muito inspirador, é só uma quine levemente mais longa. Então pensei em fazer uma função que substitui os \n:

```python
def barraN():
    _temp = 55
    _num = int(chr(_temp)) * int(chr(_temp))
    _str = chr(_num) + chr(int(_num - 1))
    return chr(int(_str))
```

Fazer uma Quine com isso é meio longo e trabalhoso, então simplifiquei um pouco, por agora:

```python
def barraN():
    return '\n'
a = "def barraN():{0}\treturn '\\n'{0}a = {1}{0}print(a.format(barraN(),repr(a)))"
print(a.format(barraN(),repr(a)))
```

Cheatando levemente por usar repr(a), que retorna a propria string de forma pura, com aspas, por exemplo. Mas o ponto não é ser boa em quines, e sim ofuscar isso tudo.

## Tentativa 1: Compressão

Vendo o código acima, dá pra brincar um pouco mais com a string final, usando a mesma lógica do barraN. Se fossemos comprimir a string **a**, daria pra, por exemplo, fazer algo assim:

```python
def n():
    return '\n'
def t():
    return '\t'
def d():
    return 'def '
def r():
    return 'return '
a = "{3}n():{0}{1}{4}'\\n'{0}{3}t():{0}{1}{4}'\\t'{0}{3}d():{0}{1}{4}'{3}'{0}{3}r():{0}{1}{4}'{4}'{0}a = {2}{0}print(a.format(n(),t(),repr(a),d(),r()))"
print(a.format(n(),t(),repr(a),d(),r()))
```

Todas as repetições de 3 ou mais caracteres foram comprimidas, e a string em si já está levemente confusa.

Indo por essa linha, podemos ver que existem duas repetições grandes na string **a**: **{0}{3}** e **{0}{1}{4}**

```python
def n():
    return '\n'
def t():
    return '\t'
def d():
    return 'def '
def r():
    return 'return '
def z():
    return '{0}{1}{4}'
def y():
    return '{0}{3}'
a = "{{3}}n():{0}'\\n'{1}t():{0}'\\t'{1}d():{0}'{{3}}'{1}r():{0}'{{4}}'{1}z():{0}'{{5}}'{1}y():{0}'{{6}}'{{0}}a = {{2}}{{0}}print(a.format(z(),y(),repr(a)).format(n(),t(),repr(a),d(),r(),z(),y()))"
print(a.format(z(),y(),repr(a)).format(n(),t(),repr(a),d(),r(),z(),y()))
```

Bom. Agora a string está gigantesca, e tudo está confuso, fiquei embasbacada em como prosseguir.

O problema é que para conseguir comprimir o z() e o y(), tivemos que criar um novo format, por cima do format antigo.

```python
def n():
    return '\n'
def t():
    return '\t'
def d():
    return 'def '
def r():
    return 'return '
def z():
    return '{0}{1}{4}'
def y():
    return '{0}{3}'
def f():
    return '.format('
a = "{{3}}n():{0}'\\n'{1}t():{0}'\\t'{1}d():{0}'{{3}}'{1}r():{0}'{{4}}'{1}z():{0}'{{5}}'{1}y():{0}'{{6}}'{1}f():{0}'{{7}}'{{0}}a = {{2}}{{0}}print(a{{7}}z(),y(),repr(a)){{7}}n(),t(),repr(a),d(),r(),z(),y(),f()))"
print(a.format(z(),y(),repr(a)).format(n(),t(),repr(a),d(),r(),z(),y(),f()))
```

Enquanto o que exatamente está sendo printado é confuso, não diria que o código está ofuscado, e os milhões de método entregam, mais ou menos.

## Tentativa 2: Exec

Tentei, para fins de teste, fazer uma das versões simples, utilizando exec para criar funções em tempo real:

```python
def n():
    return '\n'
a = "def n():{0}\treturn '\\n'{0}a = {1}{0}print(a.format(n(),repr(a)))"
print(a.format(n(),repr(a)))
```

```python
b = 'def a(x,y): exec(f\'def {x}(): return \"{y}\"\',globals())'
exec(b)
a('n','\\n')
q = '{0}exec(b){0}a(\'n\',\'\\\\n\'){0}q = {1}{0}print(b + q.format(n(),repr(q)))'
print(b + q.format(n(),repr(q)))
```

Não estou exatamente impressionada, mas dá para melhorar.

Comparando essa versão base, são **126** caracteres contra **205**.

Uma coisa engraçadinha que eu percebi nisso tudo.........é que uma função para imprimir **\n** vai sempre aumentar em 1 caractere. **"NOSSAAAAAAAAA SÓ PERCEBEU AGORA??"** Pergunta leitor frustrado. Sim, só agora. Sou besta.

Após ter oitenta bilhões de problemas com \, " e  ', decidi que vou me contentar com essa versão mesmo. Mas estou longe de terminar.

## Ultima tentativa, juro

Peguei o algoritmo mais bestinha de criptografia que não usasse biblioteca:

```python
a = "def e(t): return ''.join(chr(ord(c)^456)for c in t)"
exec(a)
q = 'ƩǨǵǨƳǸƵǂƭưƭƫǠƩǡǂƹǨǵǨƳǹƵǂƸƺơƦƼǠƭǠƹǡǦƮƧƺƥƩƼǠƺƭƸƺǠƩǡǤƺƭƸƺǠƹǡǡǡ'
print(e(q).format(repr(a),repr(q)))
```

Gostei bastante de ter feito essa quine, basicamente aquele bloco estranho é igual a `'a = {0}\nexec(a)\nq = {1}\nprint(e(q).format(repr(a),repr(q)))'`

Está simples e engraçadinho, mas ainda dá pra ver que no final das contas estamos printando algo que veio de **q**.

```python
a = "def e(t): return ''.join(chr(ord(c)^456)for c in t)\nc = repr(a)"
exec(a)
a = 'ƭưƭƫǠǪƪǨǵǨǯƩǨǵǨƳǸƵƔƔƦƭưƭƫǠƩǡƔƔƦƩǨǵǨƳǹƵƔƔƦƭưƭƫǠƭǠƩǡǡǯƔƦƸƺơƦƼǠƪǦƮƧƺƥƩƼǠƫǤƺƭƸƺǠƩǡǡǡǪǤƯƤƧƪƩƤƻǠǡǡ'
exec(e(a))
```

Levemente mais engraçado, agora não dá pra saber o que vai ser executado. Estou feliz, PUTA MERDA ESQUECI DO EPISÓDIO DE HOJE DA NOVELA.

Espero que tenham gostado fuiiiiiiii!
