# Resumos dos estudos da biblioteca Selenium e Beatiful Soup 4
Baseado nas documentações das bibliotecas

## Selenium
Biblioteca que permite e suporta a automação de navegadores.
Nas experiências que tive e checando em fóruns, é importante destacar que Selenium não é tão boa para retirar as informações das páginas, mas é ótima para fazer a transição entre páginas e links. Por esse motivo que se utiliza Beautiful Soup ou Scrapy para realizar o web scraping de fato.

### 1 - Instalação e importação
```pip install
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
driver = webdriver.Chrome() #ou .Firefox()
```          
               

- Link para download do chrome driver para selenium: https://sites.google.com/chromium.org/driver/

No caso do firefox: https://github.com/mozilla/geckodriver/releases

Caso use o Colab, é possível instalar o driver mediante os comandos abaixo:

```
!apt-get update
!apt-get install chromium chromium-driver
```

**OBS:** Para utilizar no colab é necessário também colocar algumas opções para que funcione
```
from selenium.webdriver.chrome.options import Options
def web_driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--verbose")
    options.add_argument('--no-sandbox')
    options.add_argument('--headless')
    options.add_argument('--disable-gpu')
    options.add_argument("--window-size=1920, 1200")
    options.add_argument('--disable-dev-shm-usage')
    driver = webdriver.Chrome( options=options)
    return driver
driver = web.driver()
```
**OBS2:** Há também um código para alterar a versão do ubuntu, ao que parece o Selenium não funciona ainda com a versão mais nova do Ubuntu utilizado na VM do colab. 
Você pode checar tal código no início do notebook dessa pasta.

### 2 - Comandos Básicos

**1. Acesso a uma URL e verificação da página que o browser está**
```
driver.get("http://www.python.org")
driver.current_url 
```

**OBS:** Alguns problemas podem surgir com URLs incompletas ou mostradas diferentes. Nesses casos utilizar a biblioteca urllib
```
from urllib.parse import urlparse
o = urlparse("http://docs.python.org:80/3/library/urllib.parse.html?"
             "highlight=params#url-parsing")
```
A saída será algo do tipo:
```
ParseResult(scheme='http', netloc='docs.python.org:80',
            path='/3/library/urllib.parse.html', params='',
            query='highlight=params', fragment='url-parsing')
```
Por fim pegue a URL corretamente com:
```
o._replace(fragment="").geturl()
```

**2. Fechamento de browser e aba, respectivamente**
```
driver.quit()
driver.close()
```

**3. Interação com uma página**

Selenium permite a identificação de um elemento de uma página de várias formas - HTML, seletor de CSS e XPATH. 
No dois primeiros exemplos, só se retorna o primeiro elemento. Já nos dois últimos, retorna-se uma lista com todos os elementos contidos na página.

```
elemento = driver.find_element(By.ID, "passwd-id")
elemento = driver.find_element(By.TAG_NAME, "li")
elemento = driver.find_element(By.LINK_TEXT, "https://www.google.com")
elementos = driver.find_elements(By.XPATH, "//input[@id='passwd-id']")
elementos = driver.find_elements(By.CSS_SELECTOR, "input#passwd-id")
```

3.1 Pegar atributos
Por muitas vezes os elementos possuem atributos atrelados como nome, id, link, etc. Nesses casos, pode-se acessá-los utilizando o método abaixo:
    ```
    elemento = driver.find_element(By.TAG_NAME, "li")
    elemento.get_attribute('href') # retorna o link atrelado àquele elemento
    ```

**4. Escrever algo na tela**

Pode-se fazer que seja digitado algo em um elemento de página. e por meio do módulo Keys há uma variedade de possibilidades de teclas simuladas.
```
elemento.send_keys("some text")
element.send_keys(" and some", Keys.ARROW_DOWN)
element.clear() # para limpeza do que foi digitado, não entendi muito bem isso ainda.
```
Para checar quais as possibilidades de teclas possíveis checar esse [link](https://selenium-python.readthedocs.io/api.html#module-selenium.webdriver.common.keys)

**5. Iteração de elementos**

O HTML é feito por elementos que muitas vezes se repetem como listas (li, ul), links (a href), divisões (div), etc.
Há duas formas principais de iterar sobre esses elementos.
    - Utilizando for.
    A própria documentação diz que não é o método mais eficiente de se utilizar.
    ```
    all_options = element.find_elements(By.TAG_NAME, "option")
    for opcao in all_options:
        print("Value is: %s" % option.get_attribute("value"))
        opcao.click()
    ```
    
    - Utilizando o método Select.
    Não consegui utilizá-lo por acusar erros, não aceitar nenhum localizar no find_elements
    ```
    from selenium.webdriver.support.ui import Select
    select = Select(driver.find_element(By.NAME, 'name'))
    select.select_by_index(index)
    select.select_by_visible_text("text")
    select.select_by_value(value)

    opcoes = select.options
    ```

**6. Clique em elementos**

Após achar algum elemento utilizando find_element() pode-se clicar nele.
```
driver.find_element(By.ID("submit")).click()
```

**7. Alternar entre abas**
```
driver.switch_to.window("windowName")
```

**8. Retornar e avançar em um histórico de página**
```
driver.forward()
driver.back()
```

### 3 - Paradas (waits)
Algumas vezes as páginas demoram a carregar. Então, para esses casos faz-se necessários os 'waits'.
Waits são métodos de espera, para que haja tempo para que os elementos desejados carreguem.
Há duas formas: waits explícitos e implícitos. (Há ainda outra forma não indicada utilizando o método sleep de python puro)

_Waits explícitos_
Explicita qual elemento e pelo que se espera.
An explicit wait is a code you define to wait for a certain condition to occur before proceeding further in the code

    ```
    from selenium.webdriver.support.wait import WebDriverWait
    from selenium.webdriver.support import expected_conditions as EC

    driver = webdriver.Firefox()
    driver.get("http://somedomain/url_that_delays_loading")
    try:
        element = WebDriverWait(driver, 10).until( # esse 10 significa que se esperará 10s
            EC.presence_of_element_located((By.ID, "myDynamicElement"))
        )
    finally:
        driver.quit()
    ```

Pode-se utilizar waits explícitos com essas formas
- title_is
- title_contains
- presence_of_element_located
- visibility_of_element_located
- visibility_of
- presence_of_all_elements_located
- text_to_be_present_in_element
- text_to_be_present_in_element_value
- frame_to_be_available_and_switch_to_it
- invisibility_of_element_located
- element_to_be_clickable
- staleness_of
- element_to_be_selected
- element_located_to_be_selected
- element_selection_state_to_be
- element_located_selection_state_to_be
- alert_is_present

_Waits implícitos_
Não define elemento, apenas diz quanto tempo se espera antes de acessar algum elemento

    ```
    driver.implicitly_wait(10) # seconds
    driver.get("http://somedomain/url_that_delays_loading")
    myDynamicElement = driver.find_element_by_id("myDynamicElement")
    ```


## Beautiful Soup 4
