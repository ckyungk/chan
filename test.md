# Sprint 1

## Objetivos da sprint
* Pesquisar diferentes estruturas de linguagem
* Desenvolver um protótipo da linguagem a ser usada
    + Definição do layout e componentes
* MVP do compilador:
    + Definição dos tokens
    + Desenvolvimento do lexer e parser básico
* Montar os testes automatizados do compilador

### Pesquisar diferentes estruturas de linguagem
Realizamos um estudo quanto às diferentes estruturas de linguagens para tentar mapear os pontos em comuns e utilizar como base para a criação do protótipo da nossa linguagem. Tivemos as seguintes conclusões:
* A compilação deve inserir automaticamente a tag <!DOCTYPE html> no início da página HTML.
* Por se tratar de um protótipo para visualização, a maioria das propriedades em head podem ser desconsideradas (ex. title, description, etc.). No máximo setamos o charset para utf-8.
* Estilos (CSS) e comportamentos (JavaScript) podem ser definidos ou dentro do próprio componente (exemplo, um botão que ao clicado abre uma página qualquer), ou através de variáveis (exemplo, uma função que faz um elemento clicado ser removido da tela).
* Componentes podem possuir identificadores únicos para serem usados em blocos lógicos ou em comportamento.

### Protótipo da linguagem
Também começamos a desenvolver um [protótipo da linguagem](https://docs.google.com/document/d/1t0b1E6K36lQq4PxxtTQ3FU_Oi0dOoAsT-Ejq8-Isw6g/edit#heading=h.enbvjduue693), incluindo a estrutura inicial, coloração, componentes, etc. Assim, como estrutura inicial, definimos que um documento PortuGUIs será dividido em duas seções, sendo a primeira de configuração da página (em geral, trechos que farão parte da tag head) e a segunda de páginas e componentes em si (tag body).

A coloração foi dividida em configurações de cor primária e secundária que incluem as seguintes cores:
```
Vermelho (red), Rosa (pink), Roxo (purple), RoxoForte (deep_purple), Anil (indigo), Azul (blue), AzulClaro (light_blue), Ciano (cyan), AzulPetróleo (teal), Verde (green), VerdeClaro (light_green), Lima (lime), Amarelo (yellow),  mbar (amber), Marrom (brown), CinzaAzulado (blue_gray) e Cinza (grey)
```
Além disso, como parte da linguagem, definimos os [diversos componentes](https://docs.google.com/document/d/1S5TeIMEuP306ARC4vMzfA_vryocFq247LHRd9eCOxQU/edit#) a serem utilizadas, como botões, inputs, listas, layouts em geral, menus, tabelas, tipografias.

### Lexer
Quanto ao compilador em si, para começar o desenvolvimento do Lexer e Parser, nos baseamos [neste tutorial](https://blog.usejournal.com/writing-your-own-programming-language-and-compiler-with-python-a468970ae6df). Utilizando Python e bibliotecas como RPLY e AST.
Para o lexer, começamos definindo os tokens no arquivo lexer.py
```python
from rply import LexerGenerator

class Lexer():
    def __init__(self):
        self.lexer = LexerGenerator()

    def _add_tokens(self):
        # Pagina:
        self.lexer.add('PAGINA', r'Pagina::')
        # ABRECHAVE:
        self.lexer.add('ABRECHAVE', r'{')
        # FECHACHAVE:
        self.lexer.add('FECHACHAVE', r'}')
        # Dois pontos:
        self.lexer.add('DOISPONTOS', r':')
        #Ponto e Virgula:
        self.lexer.add('PONTOVIRGULA', r';')
        # Frase:
        self.lexer.add('FRASE', r'".*"')
        #EOF
        self.lexer.add('$end', r'"$end"')
        # Number
        # self.lexer.add('NUMBER', r'\d+')
        #ID
        self.lexer.add('ID', r'[a-zA-Z][a-zA-Z0-9_]*')
        #ID
        self.lexer.add('NOVALINHA', r'\n+')
        # Ignore spaces
        self.lexer.ignore('[^\S\r\n]+')
        # Ignore comments
        self.lexer.ignore('\#.*')

    def get_lexer(self):
        self._add_tokens()
        return self.lexer.build()
```

### Parser
Para a segunda parte do compilador, criamos o arquivo parser.py, tendo como objetivo verificar a sintaxe da linguagem. Para isso, criamos um arquivo ast.py onde denifimos as classes de componentes da linguagem, instanciando-as em parser.py.
```python
from rply import ParserGenerator
from ast import Component, Property, ColorProperty, Statement, Base, Root


class Parser():
    def __init__(self):
        self.pg = ParserGenerator(
            # A list of all token names accepted by the parser.
            ['PAGINA', 'ABRECHAVE', 'FECHACHAVE', 'DOISPONTOS',
             'FRASE', 'ID', 'NOVALINHA', 'PONTOVIRGULA'],
        )

    def parse(self):
        @self.pg.production('root : base')
        def root(p):
            print(f"root: p[0]: {p[0]}")
            return Root(p[0])
        @self.pg.production('base : NOVALINHA base')
        def newline_base(p):
            print('newline base: %s' % (p[1]))
            return Base(p[1])
        @self.pg.production('base : NOVALINHA')
        def base_newline(p):
            print('base newline: %s' % (p[0]))
            return Base(p[0])
        @self.pg.production('base : property base')
        def base_property(p):
            print(f"base property: {p[0]}, p1: {p[1]}")
            return Base(p[0], p[1])
        @self.pg.production('base : color_property base')
        def base_color_property(p):
            print(f"base color property: p[0]: {p[0]}, p[1]: {p[1]}")
            return Base(p[0], p[1])
        @self.pg.production('color_property : ID DOISPONTOS ID')
        def color_property(p):
            print("color propery: %s %s %s" % (p[0], p[1], p[2]))
            return ColorProperty(p[0], p[2])
        @self.pg.production('property : ID DOISPONTOS statement')
        def property(p):
            print("property: %s %s %s" % (p[0], p[1], p[2]))
            return Property(p[0], p[2])
        @self.pg.production('statement : FRASE')
        def statement(p):
            print("statement: %s" % (p[0], ))
            return Statement(p[0])
        @self.pg.error
        def error_handle(token):
            raise ValueError(token)
            
    def get_parser(self):
        return self.pg.build()
```
No arquivo ast.py definimos as regras de compilação para o arquivo a ser lido, no momento interpretando o texto e div.
```python
html_base_head = f"""<!DOCTYPE html>
<html>
  <head>
    <title>PortuGUIS</title>
    <meta name="viewport" content="initial-scale=1">
  </head>
  <body>
    """

html_base_tail = f"""  </body>
</html>"""

html_body = """"""

class Component:
  def __init__(self, components):
    self.components = components

class Root:
    def __init__(self, base):
        self.base = base

    def eval(self):
        file = open("test.html", "w")
        if self.base:
            print("Root Eval")
            self.base.eval()
            file.write(html_base_head + html_body + html_base_tail)
            file.close()
            return f"\n\n{html_base_head + html_body + html_base_tail}"
        return "Nothing to be done\n"

class Base:
    def __init__(self, base1, base2=None):
        self.base1 = base1
        self.base2 = base2

    def eval(self):
        print("Base Eval")
        print(self.base1)
        print(self.base2)
        if self.base2:
            return f"base1: {self.base1.eval()}, base2: { self.base2.eval()}"
        else:
            try:
                return "%s" % (self.base1.eval(),)
            except:
                return f"{self.base1.value}"

class ColorProperty:
    def __init__(self, token_left, token_right):
        self.token_left = token_left
        self.token_right = token_right

    def eval(self):
        print("ColorProperty Eval")
        r = self.token_right.value
        l = self.token_left.value
        return f"CorIndice: {l} será: {r}"

class Property:
    def __init__(self, token_left, expression_right):
        self.token_left = token_left
        self.expression_right = expression_right
    def eval(self):
        global html_body
        print("Property Eval")
        #print("eval property: %s:%s" % (self.left, self.right.eval()))
        print(self.token_left)
        print(self.expression_right)
        r = self.expression_right.eval()
        l = self.token_left.value
        if l == "texto":
            # html_body += f"<h1>{r}</h1>\n"

            html_body = html_body + f"<h1>{r}</h1>\n"
        if l == "div":
            # html_body += f"<h1>{r}</h1>\n"
            html_body = html_body + f"<div style='height: 50px; width:100px; background-color: red'=><p>{r}</p></div>\n"
        return "%s:%s" % (l, r)

class Statement:
    def __init__(self, token):
        self.value = token.value

    def eval(self):
        print("Statement Eval")
        returnstring = str(self.value).replace('"', '')
        print(returnstring)
        #print("eval statement %s" % (returnstring,))
        return returnstring
```

Após isso, criamos um arquivo main.py onde instanciamos um input para compilação:
```python
from lexer import Lexer
from parser import Parser

file = open('entrada.txt', 'r')
text_input = file.read()
file.close()

lexer = Lexer().get_lexer()
tokens = lexer.lex(text_input)

pg = Parser()
pg.parse()
parser = pg.get_parser()

print(parser.parse(tokens).eval())
```

Input: entrada.txt
```
#this is a comment
texto: "Hello World"
texto: "Hello Man"
texto: "Balboa is Estudioso!"
div: "Axl Rose"
#this is a comment


#this is a comment
```

### Resultados
![HTML](https://imgur.com/owpod6B)

## Lições aprendidas
* Por termos utilizado boa parte dos membros da equipe na criação do protótipo da linguagem, tivemos poucas pessoas desenvolvendo o compilador em si, tendo sido um limitante para desenvolvermos os componentes esperados para essa Sprint.
* A falta de priorização das tarefas durante a sprint trouxe uma sobrecarga no trabalho do desenvolvimento do lexer e parser. Deveríamos ter criado um protótipo mais simples e ter começado o desenvolvimento de maneira mais gradual.

## Próximos passos
Devemos, na Sprint 2, focar nossos esforços em desenvolver um MVP, trazendo mais recursos para o desenvolvimento do lexer e parser

