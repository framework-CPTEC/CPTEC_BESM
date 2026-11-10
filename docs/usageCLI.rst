Interface de Linha de Comando (CLI)

A interface de linha de comando (CLI) é uma forma de interagir com programas digitando comandos diretamente em um terminal ou console; diferente de uma interface gráfica (GUI), onde o usuário clica em elementos visuais, na CLI as instruções são escritas em texto. Esse método é amplamente utilizado em ambientes de desenvolvimento, servidores e ferramentas técnicas porque permite maior automação, rapidez e controle sobre as operações.




Exemplos práticos

[Git] → git clone, git commit, git push
[Conda] → conda install pacote
[Pip] → pip install cptec-besm
BESM (via CLI) → besm_load -d 2026092800 -v t2mt


Opções do CLI besm_load

O comando besm_load é a interface de linha de comando para acessar e baixar dados do BESM. Após a instalação do pacote, o comando fica automaticamente disponível no ambiente de instalação, permitindo que seja executado diretamente no terminal.

Ele oferece um conjunto completo de parâmetros que permitem configurar data da rodada, intervalo temporal de interesse, variáveis e níveis atmosféricos, recortes espaciais (shapes) e opções de saída.

A seguir estão as opções disponíveis, organizadas por categoria:



Opções gerais

-h, --help

Mostra a ajuda e encerra.

--date DATE, -d DATE

Define a data da condição inicial no formato YYYYMMDDHH.

Exemplo:

Shell
--date 2026092800

.. warning::

Trocar 2026092800 por uma data disponível no Dataserver do CPTEC.

Controle do período temporal

--start START, -s START

Define a data inicial do período a ser carregado.

Pode ser informada como:

índice temporal (ex.: 0)
data completa no formato YYYYMMDDHH

Exemplo:

Shell
--start 0

ou

Shell
--start 2026010100

--end END, -r END

Define a data final do período a ser carregado.

Pode ser informada como:

índice temporal (ex.: 10)
data completa no formato YYYYMMDDHH

Exemplo:

Shell
--end 10

ou

Shell
--end 2026123100
Variáveis e níveis

--var VAR [VAR ...], -v VAR [VAR ...]

Variáveis a carregar.

Exemplo:

Shell
--var t2mt atmt
 

--level LEVEL [LEVEL ...], -l LEVEL [LEVEL ...]

Níveis atmosféricos a carregar.

Exemplo:

Shell
--level 1000 850

--list_levels

Lista todos os níveis disponíveis.

Shell
console não tem suporte total. O realce de sintaxe é baseado em Shell.
besm_load --list_levels

--list_vars

Lista todas as variáveis disponíveis.

Shell
console não tem suporte total. O realce de sintaxe é baseado em Shell.
besm_load --list_vars
Áreas e shapes

--shape SHAPE, -shp SHAPE

Define a abreviação da área (shape).

Exemplo:

Shell
--shape estados_sp

--list_areas {continentes/paises/regioes/estados/bacias/biomas}

Lista as áreas disponíveis por categoria.

Exemplo:

Shell
--list_areas estados
Shell
console não tem suporte total. O realce de sintaxe é baseado em Shell.
besm_load --list_areas estados
Saída / Output arquivo

--prefix PREFIX, -p PREFIX

Prefixo do nome do arquivo NetCDF salvo.

Valor padrão:

Plain Text
BESM

Exemplo:

Shell
--prefix MEUARQUIVO

--path PATH, -o PATH

Diretório onde os arquivos NetCDF serão salvos.

Valor padrão: diretório atual.

Exemplo:

Shell
--path ./dados
Exemplos de uso
Baixar variáveis de superfície
Shell
besm_load \
--date 2026092800 \
--var t2mt prect \
--start 0 \
--end 12 \
--shape estados_sp
Baixar variáveis em níveis atmosféricos
Shell
besm_load \
--date 2026092800 \
--var atmt shum \
--level 1000 850 \
--start 0 \
--end 12
 
Definir período usando datas completas
Shell
besm_load \
--date 2026092800 \
--var t2mt \
--start 2026010100 \
--end 2026123100
 

.. warning::

É possível solicitar, em um mesmo pedido, variáveis de superfície e variáveis de níveis atmosféricos. Dessa forma, diferentes tipos de dados do BESM podem ser combinados em uma única requisição.

.. warning::

--date 2026092800 - Trocar por uma data disponível ou consultar as datas existentes no Dataserver do CPTEC.

Ajuda da linha de comando
Shell
console não tem suporte total. O realce de sintaxe é baseado em Shell.
besm_load -h
 
Plain Text
usage: besm_load [-h] [--date DATE] [--start START] [--end END]
[--var VAR [VAR ...]] [--level LEVEL [LEVEL ...]]
[--shape SHAPE] [--prefix PREFIX] [--path PATH]
[--list_areas {continentes,paises,regioes,estados,bacias,biomas}]
[--list_levels] [--list_vars]
 
besm_load - load BESM data from CPTEC dataserver.