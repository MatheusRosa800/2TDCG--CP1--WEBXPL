# FIAP - 1° Checkpoint - Pesquisa Vulns Web & Hacker101 CTF

## Introdução
Checkpoint realizado com o intuito de colocar em prática todos os conhecimentos sobre vulnerabilidades web na matéria de Web Exploit, ministrada pelo [Professor Rafael Trassi](https://www.linkedin.com/in/rafael-trassi/).

## Feito por

- Matheus Rosa


## 1ª Parte - Report das vulnerabilidades

### Modelo seguido

1. Nome da falha
2. Impacto
3. Dois exemplos onde elas ocorreram no mundo real
4. Código vulnerável à esta falha 
5. Exploits publicados 


## 1ª Vulnerabilidade

## 1.1 - Nome da falha

- Reflected Cross-site scripting (XSS)

Nesse tipo de vulnerabilidade, o atacante consegue injetar scripts maliciosos no lado do cliente (navegador) através de parâmetros fornecidos pela URL ou formulários. A vulnerabilidade "refletida" ocorre quando os dados enviados pelo usuário são devolvidos pelo servidor sem validação

## 1.2 - Impacto

### TOP 3 no OWASP 10

Se um invasor puder controlar um script executado no navegador da vítima, normalmente poderá comprometer totalmente esse usuário. Entre outras coisas, o invasor pode:

- Executar qualquer ação dentro do aplicativo.
- Visualizar qualquer informação.
- Modificar qualquer informação.
- Iniciar interações com outros usuários do aplicativo, incluindo ataques maliciosos, que parecerão originar-se do usuário vítima inicial.

OBS: O nivel do impacto de um ataque XSS depende das permissoes e liberacoes que o usuario atacado tiver. 

## 1.3 - Dois exemplos onde elas ocorreram no mundo real

Reflected XSS in https://www.waze.com

Reflected XSS POST method at partners.uber.com

## 1.4 - Código vulnerável à esta falha 

```
from flask import Flask, request

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        nome = request.form['nome']

        # vvv Aqui está a linha vulneravel!! vvv

        return f"<h1>Olá, {nome}!</h1>"
    else:
        return '''
        <form method="post">
            <label for="nome">Digite seu nome:</label><br>
            <input type="text" id="nome" name="nome"><br>
            <input type="submit" value="Enviar">
        </form>
        '''

if __name__ == '__main__':
    app.run(debug=True)
```
## 1.4.2 - Bonus! Código mitigado

```
from flask import Flask, request
import html

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':

        # vvv Aqui está a linha vulneravel!! vvv

        nome = html.escape(request.form['nome'])
        return f"<h1>Olá, {nome}!</h1>"
    else:
        return '''
        <form method="post">
            <label for="nome">Digite seu nome:</label><br>
            <input type="text" id="nome" name="nome"><br>
            <input type="submit" value="Enviar">
        </form>
        '''

if __name__ == '__main__':
    app.run(debug=True)
```

## 1.5 - Exploits publicados 

1 -  Yahoo User Interface library - Multiple Reflected Cross Site Scripting (XSS)

URL: https://www.exploit-db.com/exploits/51198

2 -  Art Gallery Management System - Reflected Cross-Site Scripting (XSS)

URL: https://www.exploit-db.com/exploits/51214


