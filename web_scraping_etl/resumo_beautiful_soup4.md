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

**OBS:** Caso esteja com problemas de caracteres errados, pode ser que a biblioteca do BS tenha identificado errado o encode do texto. Para isso, use o argumento ```from_encoding``` e coloque o encode se souber, ou teste os encodes (UTF-8 é a mais comum).

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

**OBS1:** Em arquivos XML nenhum elemento terá vários valores de atributos. Para isso olhe o exemplo abaixo:
```
xml_soup = BeautifulSoup('<p class="body strikeout"></p>', 'xml', multi_valued_attributes=class_is_multi) 
# aqui vale o teste se sempre é class, ou tem que colocar o atributo que se deseja
xml_soup.p['class']
# [u'body', u'strikeout']
```
3. **Acesso ao texto contido em um elemento**
```
soup.title.string
# u'The Dormouse's story'
```
Pode-se trocar a string utilizando o método abaixo:
```
soup.p.string.replace_with("No longer bold")
# <blockquote>No longer bold</blockquote>
```
OBS da doc: Se você quer utilizar uma NavigableString (classe de string da BS) fora do Beautiful Soup, você deve chamar o unicode() para transformá-la em uma string Unicode Python padrão. 
Se você não fizer isso, sua string irá carregar uma referência de toda sua árvore Beautiful Soup, mesmo que você já não esteja mais usando ela, o que é um grande desperdício de memória.

Caso a tag tenha mais de uma string utilize stripped_strings que já desconsidera os eventuais espaços que possam haver entre elas
```
for string in soup.stripped_strings:
    print(repr(string))
```

OBS.: Beautiful Soup assumirá que a string está codificada como UTF-8. Você pode evitar isso passando ao invés disso uma string Unicode.

Há ainda o método ```.get_text()```. O primeiro argumento define como você quer unir as strings, a segunda define se quer retirar os espaços entre elas.
```
# soup.get_text("|", strip=True)
u'I linked to|example.com'
```

4. **Procura de informações na página**

- find()

Pode-se usar o método find para procurar pelo tag_name, id, class, div. Há várias possibilidades de formas de procura. 
Explicação da doc:
> Você pode usá-los para realizar filtros baseados nos nomes das tags, nos seus atributos, no texto de uma strings ou em alguma combinação entre eles.

```
soup.find(id="link3") # poderia utilizar o class='sister' por exemplo
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
```

Nos casos acima, só se retorna o primeiro valor do elemento.

- find_all()

O método find_all() busca entre os descendentes de uma tag e retorna todos os descendentes que correspondem a seus filtros

Definição: find_all(name, attrs, recursive, string, limit, kwargs)

```
soup.find_all('a')

soup.find_all("p", "title")
# [<p class="title"><b>The Dormouse's story</b></p>]

soup.find_all(id="link2")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

soup.find_all(string=["Tillie", "Elsie", "Lacie"])
# [u'Elsie', u'Lacie', u'Tillie']

```

O find_all aceita o uso de vários atributos de uma vez
```
soup.find_all(href=re.compile("elsie"), id='link1')
# [<a class="sister" href="http://example.com/elsie" id="link1">three</a>]
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

2. **Navegando pela árvore**

As tags podem conter strings e outras tags. Estes elementos são as tags filhas (children).

2.1 Acessando todas as tags filhas
```
soup.head.contents
```

Com auxílio de lista é possível acessar a uma tag específica
```
soup.p.contents[2].name
```

Caso deseje iterar sobre as children, use um for
```
for filho in title_tag.filhos:
     print(filho)
```

OBS: strings não possuem o atributo .contents pois não contêm nada.

2.1.1 Acessando todas as tags abaixo
O método children e contents, somente mostra as tags filhas diretas. Exemplo: a tag head só tem como filha direta a tag title, porém essa tem várias outras abaixo.
O atributo .descendants permite que você itere sobre todas as tags filhas, recursivamente: suas filhas diretas, as filhas de suas filhas, e assim por diante:
```
for child in head_tag.descendants:
    print(child)
# <title>The Dormouse's story</title>
# The Dormouse's story
```

2.2 Acessando as tags acima
A ideia é igual. Porém agora você está subindo as tags.

Você pode acessar o elemento mãe com o atributo ```.parent```.
Signature: find_parent(name, attrs, string, **kwargs)

Para acessar todos elementos acima na árvore use ```.parents```.
Signature: find_parents(name, attrs, string, limit, **kwargs)


2.3 Acessando elementos de uma mesma tag (irmãos)
Você pode usar ```.next_sibling``` e ```.previous_sibling``` para navegar entre os elementos da página que estão no mesmo nível da árvore. 

É possível empilhar até chegar no irmão desejado:
```
soup.b.next_sibling.next_sibling
```

Também é possível iterar utilizando for:
```
for sibling in soup.a.next_siblings:
    print(repr(sibling))
```

3. **Procura de informações na página (avançado)**
- RegEx

Se você passar um objeto regex, o Beautiful Soup irá realizar um filtro com ela utilizando seu método search().
```
import re
for tag in soup.find_all(re.compile("^b")):
    print(tag.name)
# body
# b
```

- Lista

Se você passar uma lista, o Beautiful Soup irá buscar uma correspondência com qualquer item dessuma lista.
```
soup.find_all(["a", "b"])
# [<b>The Dormouse's story</b>,
#  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

- O valor True

Caso coloque True em algum filtro de busca, ele irá buscar todos os casos desejados.
```
soup.find_all(id=True)
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

**OBS1:** caso um elemento tenha atributo chamado name ou qualquer outro utilizado pelo BS pode dar um erro, para solucionar use o exemplo abaixo. 
```
name_soup = BeautifulSoup('<input name="email"/>')
name_soup.find_all(name="email")
# []
name_soup.find_all(attrs={"name": "email"})
# [<input name="email"/>]
```
No caso acima 'name' seria utilizado para encontrar a palavra 'input' porém usando o argumento attrs e um dicionário, e possível contornar esse problema.

- Classe CSS

É muito útil buscar por uma tag que tem uma certa classe CSS, mas o nome do atributo CSS, “class”, é uma palavra reservada no Python. 
Utilizar class como um argumento palavra-chave lhe trará um erro de sintaxe. Você pode buscar por uma classe CSS utilizando o argumento palavra-chave class_.
```
soup.find_all("a", class_="sister")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

4. **Seletores CSS**

A partir da versão 4.7.0, o Beautiful Soup suporta a maior parte dos seletores CSS4 através do projeto _SoupSieve_. 
Se você instalou o Beautiful Soup através do pip,o SoupSieve foi instalado ao mesmo tempo.

BeautifulSoup possui um método ```.select()``` o qual utiliza o SoupSieve para executar um seletor CSS selector sobre um documento a ser analisado e retorna todos os elementos correspondentes.

```
soup.select("title")
# [<title>The Dormouse's story</title>]

soup.select("p:nth-of-type(3)")
# [<p class="story">...</p>]

soup.select("p > a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

Se quiser buscar por tags que correspondem a duas ou mais classes CSS, você deverá utilizar um seletor CSS:
```
css_soup.select("p.strikeout.body")
# [<p class="body strikeout"></p>]
```

Há outros exemplos bem mais avançados nesse [link](https://www.crummy.com/software/BeautifulSoup/bs4/doc.ptbr/#seletores-css)

Há outro método chamado ```select_one()```, o qual encontra somente a primeira tag que combina com um seletor:
```
soup.select_one(".sister")
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
```
5. Modificando a árvore

Não faz sentido para mim, mas o BS4 consegue modificar a estrutura, apagar elementos, etc. Caso tenha interesse, cheque esse [link](https://www.crummy.com/software/BeautifulSoup/bs4/doc.ptbr/#modificando-a-arvore)

## 6 Analisando apenas parte do documento
Suponhamos que você queira apenas as tags 'a' do documento. 
É um desperdício de tempo e memória analisar repetitivamente todo o documento para apenas buscar tais tags.
Seria muito mais rápido ignorar tudo o que não for 'a' em primeiro lugar. A classe ```SoupStrainer``` permite que você escolha qual partes do documento serão analisadas.
Você deverá penas criar uma instância de ```SoupStrainer``` e passá-la ao construtor BeautifulSoup no argumento ```parse_only```.

OBS: esta característica não funcionará se você estiver utilizando o html5lib.

```
from bs4 import SoupStrainer
so_tags_a = SoupStrainer("a")
so_tags_com_id = SoupStrainer(id="link2")
BeautifulSoup(html_doc, parse_only=so_tags_a)
```

## 7 Solucionando problemas
Utilize a função ```.diagnose()``` para saber o que o Beautiful Soup está fazendo com o documento.
```
from bs4.diagnose import diagnose
with open("bad.html") as fp:
    data = fp.read()
diagnose(data)

# Diagnostic running on Beautiful Soup 4.2.0
# Python version 2.7.3 (default, Aug  1 2012, 05:16:07)
# I noticed that html5lib is not installed. Installing it may help.
# Found lxml version 2.3.2.0
#
# Trying to parse your data with html.parser
# Here's what html.parser did with the document:
```

## 8 Conclusão
Creio que esse seja o compêndio principal para se entender BS4. O que não estiver aqui pode ser checado diretamente na documentação. 
