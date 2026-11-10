Biblioteca Python

A biblioteca Python é a forma tradicional de acessar os dados do BESM (Brazilian Earth System Model) diretamente em scripts e aplicações Python.

Ela permite consultar variáveis disponíveis, níveis atmosféricos, áreas de recorte geográfico e carregar séries temporais do modelo de forma simples e integrada ao ambiente científico Python.

.. warning::

Alterar a data para os valores exibidos na inicialização do modelo.

.. note::

Data de Inicialização do BESM

Para saber mais detalhes sobre o uso da opção date, consulte a documentação sobre disponibilidade de dados do BESM no Dataserver do CPTEC.

Período Temporal

O BESM utiliza um intervalo temporal definido pelos parâmetros start e end.

Esses parâmetros podem ser informados de duas formas:

Índices relativos ao período da simulação;
Datas completas no formato YYYYMMDDHH.

Exemplo:

.. code-block:: python

start = 0 end = 10

ou

.. code-block:: python

start = "20261110" end = "20261120"

Exemplo Básico

Exemplo de código para leitura de dados do BESM.

.. code-block:: python

Import do modelo BESM

import besm.CPTEC_BESM as BESM

Inicialização do modelo

besm = BESM.model()

Data da rodada

date = "20261110"

Variáveis

vars = ["t2mt"]

Carregar dados

f = besm.load( date=date, var=vars )

print(f)

Variáveis e Níveis

Uma vez que o modelo é inicializado, suas informações são exibidas automaticamente.

.. code-block:: python

import besm.CPTEC_BESM as BESM besm = BESM.model()

Saída:

.. code-block:: text

Brazilian Earth System Model - ATMOS (TQ0062L042)

Forecast data available for reading between 20261031 and 20261110.

Surface variables: ['topo', 'lsmk', 'amdl', ..., 't2mt', 'q2mt', 'u10t', 'v10t', 'w100', 'd100']

Level variables: ['uvmt', 'vvmt', 'ghmt', 'atmt', 'rhmt', 'shmt', 'omtm', 'lwrh', 'swrh', 'cvlh', 'cvms', 'vdcc', 'tke2']

levels (hPa): ['1000', '925', '850', '775', '700', '500', '400', '300', '250', '200', '150', '100', '70', '50', '30', '20', '10', '3']

Listar Variáveis Disponíveis

.. code-block:: python

besm.list_variables()

Saída:

.. code-block:: text

Variables at the surface: ['topo', 'lsmk', ..., 't2mt', 'q2mt', 'u10t', 'v10t']

Variables at pressure levels: ['uvmt', 'vvmt', 'ghmt', 'atmt', 'rhmt', 'shmt', ...]

Descrição das Variáveis

Para visualizar a descrição completa das variáveis disponíveis:

.. code-block:: python

besm.get_var_description()

Exemplo:

.. code-block:: text

Variable Name Unit

topo TOPOGRAPHY M lsmk LAND SEA MASK NO DIM t2mt TIME MEAN TEMP AT 2-M FROM SFC K u10t TIME MEAN AT 10 METRE U-WIND COMPONENT M/Sec v10t TIME MEAN AT 10 METRE V-WIND COMPONENT M/Sec ...

Consultar uma variável específica:

.. code-block:: python

besm.get_var_description("t2mt")

Saída:

.. code-block:: text

Variable Name Unit

t2mt TIME MEAN TEMP AT 2-M FROM SFC K

Listar Níveis Disponíveis

.. code-block:: python

besm.list_levels()

Saída:

.. code-block:: text

Available levels: ['1000', '925', '850', '775', '700', '500', '400', '300', '250', '200', '150', '100', '70', '50', '30', '20', '10', '3']

Listar Áreas Disponíveis

Para consultar os tipos de áreas disponíveis:

.. code-block:: python

besm.list_areas()

Saída:

.. code-block:: text

Usage: list_areas('continentes'/'paises'/'regioes'/ 'estados'/'bacias'/'biomas')

Exemplo:

.. code-block:: python

besm.list_areas("continentes")

Saída:

.. code-block:: text

Usage: type_abbreviation e.g. shp='estados_sp'

Area - type_abbreviation Africa - continentes_af Antartica - continentes_an Asia - continentes_as Australia - continentes_au Europa - continentes_eu Oceania - continentes_oc America do Sul - continentes_sa America do Norte - continentes_na

Ler Dados do BESM

O pacote BESM oferece duas formas principais de leitura de dados:

load_shape

Utilizado quando se deseja aplicar um recorte espacial nos dados.

Permite selecionar áreas específicas (estados, países, regiões, continentes, bacias ou biomas) utilizando abreviações de shape.

Exemplo:

.. code-block:: python

shp = "estados_sp"

load

Utilizado para recuperar dados sem recorte espacial, abrangendo todo o domínio disponível.

Baixar Variáveis de Superfície com Recorte de Área

.. code-block:: python

f = besm.load_shape( date="20261110", var=["t2mt", "u10t", "v10t"], shp="estados_sp" )

Baixar Variáveis em Níveis Atmosféricos

.. code-block:: python

f = besm.load( date="20261110", var=["atmt", "uvmt", "vvmt"], level=[1000,850] )

Baixar um Período Temporal Específico

.. code-block:: python

f = besm.load( date="20261110", start="20261110", end="20261120", var=["t2mt"] )

Baixar um Intervalo Utilizando Índices

.. code-block:: python

f = besm.load( start=0, end=10, var=["t2mt"] )

Saída típica:

.. code-block:: text

Building request

Start: 20261110 - End: 20261120 Period: 20261110 - 20261120 Forecast records: 10 Requested data: 0.70 MB

Download total: 100%