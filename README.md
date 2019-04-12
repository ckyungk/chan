# PortuGUIs

Um simples compilador para converter arquivos descritivos em portuguès estruturado para geração de interfaces web semi-dinâmicas que visa facilitar a prototipação para usuários sem conhecimentos técnicos em programação. Tem estrutura simples, legível e o mais importante, gera protótipos conforme descritos.
Você pode ver um exemplo de arquivo de entrada em [exemplo.pg](exemplo.pg)

## Começando

Estas instruções visam auxiliá-lo a configurar o software completamente funcional em sua máquina local. Para instruções de implantação em produção, veja a seção abaixo.

### Pré-requisitos

Os seguintes softwares ou programas são pré-requisitos:

* Python 3
* Virtualenv (módulo de Python)
* PLY (módulo de Python)
* Selenium (módulo de Python)
* Firefox ou Chrome

Em um sistema linux, instale os pacotes com o gerenciador de pacotes disponível. Abaixo segue exemplo para Ubuntu (que já vem com Python instalado)
```bash
$ sudo apt update
$ sudo apt install wget git
$ sudo apt upgrade -y
$ pip install virtualenv
$ virtualenv venv
$ cd venv
$ source bin/activate
$ git clone <url do repositório>
$ pip install -r portuguis/requirements.txt
$ wget https://github.com/mozilla/geckodriver/releases/download/v0.18.0/geckodriver-v0.18.0-linux64.tar.gz
$ tar -xvzf geckodriver*
$ chmod +x geckodriver
$ export PATH=$PATH:</path-to-extracted-file>/geckodriver
```

Se quiser, temos uma imagem do docker pré-configurada com todos os componentes. Para isso, instale Docker, e acesse a pasta `docker` na raíz do projeto, e leia o `README.md` do diretório.

### Instalação

O sistema não exige muita configuração, apenas, com o python instalado, e com todos os módulos instalados, execute `python3 portuguis.py arquivo1.txt arquivo2.txt arquivo3.txt`

A saída serão arquivos no diretório `outputs` que podem ser abertos no navegador de sua escolha.

## Rodando os testes

Para rodar os testes unitários e de comportamento, execute:
`python test_main.py`

## Implantação

Conteúdo a ser inserido.

## Ferramentas

* [Python](https://www.python.org/) - A linguagem do compilador
* [HTML5](https://www.w3.org/html/) - A linguagem para a estrutura da saída
* [CSS3](https://www.w3.org/Style/CSS/Overview.en.html) - A linguagem para os estilos da saída
* [javascript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript) - A linguagem para dinamização do sistema
* [React](https://reactjs.org/) - Framework de javascript para padronização de comportamento
* [Material](https://material.io/) - Principal biblioteca de estilos para padronização de experiência do usuário
* [PLY](https://www.dabeaz.com/ply/) - Módulo Python para implementação de Lexer
* [Unittest](https://docs.python.org/3/library/unittest.html) - Módulo Python para testes unitários
* [Selenium](https://www.seleniumhq.org/) - Módulo para testes de comportamento.

## Contribuindo / Desenvolvendo

Por favor leia [CONTRIBUTING.md](CONTRIBUTING.md) para detalhes do nosso código de conduta e o processo para submissão de Merge Requests e execução de tarefas/planejamento.

## Versionamento

Nós utilizamos [SemVer](https://semver.org/) para versionamento. Para as versões disponíveis, veja as [tags]() do repositório.

## Autores

* **André Gonçalves** - [ra145280](https://gitlab.ic.unicamp.br/ra145280)
* **Celso Mizerani** - [ra145711](https://gitlab.ic.unicamp.br/ra145711)
* **Chan Kim** - [ra145720](https://gitlab.ic.unicamp.br/ra145720)
* **Daniel Travieso** - [ra145280](https://gitlab.ic.unicamp.br/ra145767)
* **João Victor Fernandes** - [ra158043](https://gitlab.ic.unicamp.br/ra158043)
* **Leandro Magalhães** - [ra171836](https://gitlab.ic.unicamp.br/ra171836)
* **Raissa Cavalcante** - [ra150619](https://gitlab.ic.unicamp.br/ra150619)

Veja também a lista de todos os [contribuidores](https://gitlab.ic.unicamp.br/mc911/2019-s1/grupo-04/project_members) que partiparam do projeto

## Licença

Este projeto é licenciado sobre a licença MIT. Para mais informações, leia [LICENSE.md](LICENSE.md).

## Agradecimentos

* Ao professor pela inspiração
* Às designers entrevistadas
* Aos colegas pela ajuda
