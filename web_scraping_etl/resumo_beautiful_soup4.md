# **RESUMO DOS ESTUDOS EM BEAUTIFUL SOUP(BS)**
Informações obtidas a partir de documentação e stackoverflow.

## 1 - INTRODUÇÃO
Segundo a própria documentação:
> Beautiful Soup é uma biblioteca Python de extração de dados de arquivos HTML e XML. 
> Utiliza vários parsers (interpretadores) que provêm maneiras mais intuitivas de navegar, buscar e modificar uma árvore (parse tree).

Importante salientar que a BS não transita entre páginas, por isso faz-se necessário a utilização do Selenium.

## 2 - INSTALAÇÃO E IMPORTAÇÃO
```
pip install beautifulsoup4
from bs4 import BeautifulSoup
import requests
```
OBS: importante importar a biblioteca requests, pois ela que faz o acesso ao arquivo html de alguma página. 
Também é possível que se use o método get() do Selenium para indicar a página para a BS.

### 2.1 QUAL PARSER DEVO ESCOLHER?
O Beautiful Soup não só suporta o parser HTML incluído na biblioteca padrão do Python como também inúmeros parsers de terceiros.

|Parser|Uso Padrão|Vantagens|Desvantagens|
|---|---|---|---|
|html.parser (puro)	|```BeautifulSoup(pag_html, "html.parser")```|- Velocidade Decente <br/> - Leniente (Suave?)|- Não tão rápido quanto lxml <br/> - Menos leniente que html5lib|
|HTML (lxml)|```BeautifulSoup(pag_html, "lxml")``` |- Muito rápido <br/> - Leniente|- Dependência externa de C|
|XML (lxml)|```BeautifulSoup(pag_html, "lxml-xml")```<br/> ```BeautifulSoup(pag_html, "xml")```|- Muito rápido <br/> - O único parser de XML suportado|- Dependência externa de C|
|html5lib|```BeautifulSoup(pag_html, "html5lib")```|- Muito leniente <br/> - Analisa as páginas como um navegador|- Muito Lento <br/> - Dependência externa de Python|

**OBS1:** Instalação dos outros parsers:
```
pip install lxml
pip install html5lib
```
**OBS2:** A própria _**documentação indica a utilização do lxml**_ 

## 3 - CRIAÇÃO DA SOUP 
Para analisar um documento, passe-o como argumento dentro de um construtor BeautifulSoup
```
acesso = requests.get('www.google.com')
soup = BeautifulSoup(acesso.text, 'html.parser')
```
O .text serve para transformar o arquivo para texto para que o parser funcione.

## 4 - COMANDOS BÁSICOS
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

2. **Acesso ao nome e atributos de um elemento**

Acessa o nome da tag
```
soup.title.name
# u'title'
```
As tags possuem muitas vezes vários atributos como id, class, div, etc. Pode-se acessar ao valor do atributo como um dicionário:
```
soup.p['class'] # ou soup.p.attrs. nesse caso irão mostrar todos os atributos como um dicionário.
# u'title'
```
Você pode adicionar, remover ou modificar os atributos de uma tag.

O HTML 5 define alguns atributos que podem ter múltiplos valores. como abaixo:
```
css_soup = BeautifulSoup('<p class="body strikeout"></p>')
css_soup.p['class']
# ["body", "strikeout"]
```
Caso não deseje que os atributos sejam fornecidos como lista, coloque o argumento ```multi_valued_attributes=None``` no construtor BeautifulSoup()

**OBS1:** Em arquivos XML nenhum elemento terá vários valores de atributos.

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

## 5 - COMANDOS MAIS AVANÇADOS
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
