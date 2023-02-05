# **RESUMO DOS ESTUDOS EM BEAUTIFUL SOUP(BS)**
Informações obtidas a partir de documentação e stackoverflow.

## INTRODUÇÃO
Segundo a própria documentação:
> Beautiful Soup é uma biblioteca Python de extração de dados de arquivos HTML e XML. 
> Utiliza vários parsers (interpretadores) que provêm maneiras mais intuitivas de navegar, buscar e modificar uma árvore (parse tree).

Importante salientar que a BS não transita entre páginas, por isso faz-se necessário a utilização do Selenium.

## INSTALAÇÃO E IMPORTAÇÃO
```
pip install beautifulsoup4
from bs4 import BeautifulSoup
import requests
```
OBS: importante importar a biblioteca requests, pois ela que faz o acesso ao arquivo html de alguma página. 
Também é possível que se use o método get() do Selenium para indicar a página para a BS.

## QUAL PARSER DEVO ESCOLHER?
https://www.crummy.com/software/BeautifulSoup/bs4/doc.ptbr/#instalando-um-interpretador-parser 
continuar daqui

## CRIAÇÃO DA SOUP 
```
acesso = requests.get('www.google.com')
soup = BeautifulSoup(acesso.text, 'html.parser')
```
O .text serve para transformar o arquivo para texto para que o parser funcione.

## COMANDOS BÁSICOS
Utilizando o html abaixo como exemplo:
```
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
```

Tem-se alguns comandos para navegação da página:
1. **soup.elemento**

Acessa todo o elemento
```
soup.title
# <title>The Dormouse's story</title>

soup.p
# <p class="title"><b>The Dormouse's story</b></p>
```

2. **soup.elemento.name**

Acessa à string do elemento. Ainda não vi muita utilidade nisso.
```
soup.title.name
# u'title'

# outra forma de sintaxe é colocando o "id" dentro do elemento. no caso o elemento p tem um id chamado class, retornando o nome dele
soup.p['class']
# u'title'
```

3. **soup.elemento.string**

Acessa o texto que há no elemento
```
soup.title.string
# u'The Dormouse's story'
```

4. **soup.elemento.parent.name**

Acessa o nome do elemento acima do elemento em questão. Sem o name acessaria todo o elemento que está acima.
```
soup.title.parent.name
# u'head'
```

5. **find_all()**

Nos casos acima, só se retorna o primeiro valor do elemento. 
No HTML muitas vezes há vários elementos do mesmo tipo, faz-se necessário utilizar o método find_all() se quiser navegar entre eles.
```
soup.find_all('a')
```

6. **soup_find(tag)

Pode-se usar o método find para procurar pelo tag_name, id, class, div. Há várias possibilidades de formas de procura. 

```
soup.find(id="link3") # poderia utilizar o class='sister' por exemplo
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
```

## COMANDOS MAIS AVANÇADOS
1. **Iteração de elementos**

Importante caso se deseje iterar pelos vários elementos de uma paginas, como links, listas, etc.
```
for link in soup.find_all('a'):
    print(link.get('href'))
# http://example.com/elsie
# http://example.com/lacie
# http://example.com/tillie
```
OBS: o método get('href') pega o link que há dentro do elemento a. Muito importante
