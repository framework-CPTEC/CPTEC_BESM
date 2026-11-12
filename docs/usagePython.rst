Biblioteca Python
=================

- **[Biblioteca Python])**  
  O método tradicional,  de acessar os dados do BESM (Brazilian Earth System Model) diretamente em scripts e aplicações Python.

  Ela permite consultar variáveis disponíveis, níveis atmosféricos, áreas de recorte geográfico e carregar séries temporais do modelo de forma simples e integrada ao ambiente científico Python.

.. warning::

  Alterar a data para os valores exibidos na inicialização do modelo.

.. note::

Data de Inicialização do BESM
-----------------------------

Para saber mais detalhes sobre o uso da opção date, consultar `Data de Inicialização <usage.html##horarios-de-inicializacao-do-modelo-besm-componente-atmosferico>`_

Exemplo de código para a leitura e filtragem de dados do MONAN.

.. code-block:: console

  # Import para o modelo BESM
  import besm.CPTEC_BESM as BESM

  # Durante a inicialização do construtor informações sobre os dados são exibidas
  # Entre elas informações de variaveis, niveis e frequência disponiveis para consulta
  besm = BESM.model()

  # Data da IC
  # Trocar pela data atual ou consultar dataserver do CPTEC
  date = '20261110'

  # Variaveis 
  vars = ['t2mt','prec']

  # Utizando o método load
  f = besm.load(date=date, var=vars)
  
  # Imprimir os valores recuperados
  print(f)


Período Temporal
----------------

O BESM utiliza um intervalo temporal definido pelos parâmetros start e end.

Esses parâmetros podem ser informados de duas formas:

- Índices relativos ao período da simulação;
- Datas completas no formato YYYYMMDD.

Exemplo de código para a leitura e filtragem de período temporal por índice relativo.

.. code-block:: python

  import besm.CPTEC_BESM as BESM
  besm = BESM.model()
  #### Brazilian Earth System Model - ATMOS (TQ0062L042) #####
  Forecast data available for reading between 20261101 and 20261111.
  Surface variables: ['topo', 'lsmk', 'amdl', 'amsl', 'ussl', 'uzrs', 'uzds', 'psmt', 'usmt', 'vsmt', 'spmt', 'tsmt', 'rsmt', 'pcmt', 'prec', 'prcv',         'neve', 'rnof', 'pitp', 'cssf', 'clsf', 'evap', 'cbnv', 'olis', 'oles', 'role', 'iswf', 'ocis', 'oces', 'roce', 'swae', 'ocas', 'olic', 'lwtc', 'ocic',     'swgc', 'swtc', 'saea', 'nlwb', 'tdst', 'tgsc', 'tcas', 't2mt', 'q2mt', 'u10t', 'v10t', 'w100', 'd100'].
  Level variables:   ['uvmt', 'vvmt', 'ghmt', 'atmt', 'rhmt', 'shmt', 'omtm', 'lwrh', 'swrh', 'cvlh', 'cvms', 'vdcc', 'tke2'].
  levels (hPa): ['1000', '925', '850', '775', '700', '500', '400', '300', '250', '200', '150', '100', '70', '50', '30', '20', '10', '3'].
  f = besm.load(start=0, end=10)
  Building request
  Start: 20261111 - End: 20261121
  Period: 20261111 - 20261121
  Forecast records: 10
  Requested data: 0.70 MB
  20261112: 0.07 MB, Time: 4.61s, Speed: 0.02 MB/s                                                                                                       
  20261113: 0.07 MB, Time: 4.59s, Speed: 0.02 MB/s                                                                                                       
  20261114: 0.07 MB, Time: 4.59s, Speed: 0.02 MB/s                                                                                                       
  20261115: 0.07 MB, Time: 4.65s, Speed: 0.02 MB/s                                                                                                       
  20261116: 0.07 MB, Time: 4.58s, Speed: 0.02 MB/s                                                                                                       
  20261117: 0.07 MB, Time: 4.61s, Speed: 0.02 MB/s                                                                                                       
  20261118: 0.07 MB, Time: 4.59s, Speed: 0.02 MB/s                                                                                                       
  20261119: 0.07 MB, Time: 4.63s, Speed: 0.02 MB/s                                                                                                       
  20261120: 0.07 MB, Time: 4.60s, Speed: 0.02 MB/s                                                                                                       
  20261121: 0.07 MB, Time: 4.60s, Speed: 0.02 MB/s                                                                                                       
  Download total: 100%|███████████████████████████████████████████████████████████████████████████████████████████████| 737k/737k [00:46<00:00, 16.0kB/s]
  f
  <xarray.Dataset> Size: 740kB
  Dimensions:  (time: 10, lat: 96, lon: 192)
  Coordinates:
  * time     (time) datetime64[ns] 80B 2026-11-12 2026-11-13 ... 2026-11-21
  * lat      (lat) float64 768B -88.57 -86.72 -84.86 -83.0 ... 84.86 86.72 88.57
  * lon      (lon) float64 2kB 0.0 1.875 3.75 5.625 ... 352.5 354.4 356.2 358.1
  Data variables:
    TSMT     (time, lat, lon) float32 737kB dask.array<chunksize=(1, 96, 192), meta=np.ndarray>


**Explicação do exemplo**

Neste exemplo, o período de interesse é definido utilizando índices relativos por meio dos parâmetros start e end:

>>> f = besm.load(start=0, end=10)

O índice 0 corresponde ao primeiro registro disponível da previsão a partir da data inicial informada pelo modelo. Já o índice 10 define o limite final do período a ser carregado.

Durante a execução, o BESM identifica que os dados de previsão estão disponíveis entre 20261111 e 20261121 e realiza o download de 10 registros diários consecutivos. Por isso, o conjunto de dados resultante possui a dimensão:

Dimensions: (time: 10, lat: 96, lon: 192)

O eixo temporal (time) contém as datas compreendidas entre 2026-11-12 e 2026-11-21, totalizando 10 registros de tempo.

Utilizar índices relativos é uma forma prática de selecionar períodos com base na posição dos registros dentro da simulação, sem a necessidade de informar datas explícitas. Essa abordagem é especialmente útil quando se deseja acessar os primeiros dias de uma previsão ou trabalhar com janelas temporais relativas aos dados disponíveis.

Resultado equivalente utilizando **datas explícitas**:

>>> f = besm.load(start="20261111", end="20261121")

Nesse caso, o período é definido diretamente pelas datas de início e fim, produzindo o mesmo intervalo temporal representado pelos índices relativos utilizados no exemplo anterior.


Variáveis e Níveis
------------------

Uma vez que o modelo é inicializado, suas informações são exibidas automaticamente.

.. code-block:: python

  import besm.CPTEC_BESM as BESM 
  besm = BESM.model()

  Brazilian Earth System Model - ATMOS (TQ0062L042)

  Forecast data available for reading between 20261031 and 20261110.

  Surface variables: ['topo', 'lsmk', 'amdl', ..., 't2mt', 'q2mt', 'u10t', 'v10t', 'w100', 'd100']

  Level variables: ['uvmt', 'vvmt', 'ghmt', 'atmt', 'rhmt', 'shmt', 'omtm', 'lwrh', 'swrh', 'cvlh', 'cvms', 'vdcc', 'tke2']

  levels (hPa): ['1000', '925', '850', '775', '700', '500', '400', '300', '250', '200', '150', '100', '70', '50', '30', '20', '10', '3']

  Variables at the surface: ['topo', 'lsmk', ..., 't2mt', 'q2mt', 'u10t', 'v10t']

  Variables at pressure levels: ['uvmt', 'vvmt', 'ghmt', 'atmt', 'rhmt', 'shmt', ...]


Descrição das Variáveis
-----------------------

>>> pd.set_option('display.max_rows', None)
>>> print(besm.get_var_description())
   Variable                                     Name              Unit
0      topo                               TOPOGRAPHY                 M
1      lsmk                            LAND SEA MASK            NO DIM
2      amdl                        STORAGE ON CANOPY                 M
3      amsl                        STORAGE ON GROUND                 M
4      ussl                  SOIL WETNESS OF SURFACE            No Dim
5      uzrs                SOIL WETNESS OF ROOT ZONE            No Dim
6      uzds            SOIL WETNESS OF DRAINAGE ZONE            No Dim
7      psmt               TIME MEAN SURFACE PRESSURE                Cb
8      usmt         TIME MEAN SURFACE ZONAL WIND (U)             M/Sec
9      uvmt                 TIME MEAN ZONAL WIND (U)             M/Sec
10     vsmt    TIME MEAN SURFACE MERIDIONAL WIND (V)             M/Sec
11     vvmt            TIME MEAN MERIDIONAL WIND (V)             M/Sec
12     ghmt            TIME MEAN GEOPOTENTIAL HEIGHT                 M
13     spmt             TIME MEAN SEA LEVEL PRESSURE                Mb
14     tsmt   TIME MEAN SURFACE ABSOLUTE TEMPERATURE                 K
15     atmt           TIME MEAN ABSOLUTE TEMPERATURE                 K
16     rsmt      TIME MEAN SURFACE RELATIVE HUMIDITY            No Dim
17     rhmt              TIME MEAN RELATIVE HUMIDITY            No Dim
18     shmt              TIME MEAN SPECIFIC HUMIDITY            No Dim
19     pcmt                  TIME MEAN PRECIP. WATER          Kg M**-2
20     omtm                          TIME MEAN OMEGA            Cb/Sec
21     prec                      TOTAL PRECIPITATION  Kg M**-2 Day**-1
22     prcv                 CONVECTIVE PRECIPITATION  Kg M**-2 Day**-1
23     neve                                 SNOWFALL  Kg M**-2 Day**-1
24     rnof                                   RUNOFF  Kg M**-2 Day**-1
25     pitp                        INTERCEPTION LOSS           W M**-2
26     cssf          SENSIBLE HEAT FLUX FROM SURFACE           W M**-2
27     clsf            LATENT HEAT FLUX FROM SURFACE           W M**-2
28     evap                              EVAPORATION  Kg M**-2 Day**-1
29     cbnv                              CLOUD COVER            No Dim
30     olis             DOWNWARD LONG WAVE AT BOTTOM           W M**-2
31     oles               UPWARD LONG WAVE AT BOTTOM           W M**-2
32     role                OUTGOING LONG WAVE AT TOP           W M**-2
33     iswf                 INCIDENT SHORT WAVE FLUX           W M**-2
34     ocis            DOWNWARD SHORT WAVE AT GROUND           W M**-2
35     oces              UPWARD SHORT WAVE AT GROUND           W M**-2
36     roce                 UPWARD SHORT WAVE AT TOP           W M**-2
37     swae  SHORT WAVE ABSORBED BY EARTH/ATMOSPHERE           W M**-2
38     ocas            SHORT WAVE ABSORBED AT GROUND           W M**-2
39     lwrh              LONG WAVE RADIATIVE HEATING             K/Sec
40     swrh             SHORT WAVE RADIATIVE HEATING             K/Sec
41     cvlh                CONVECTIVE LATENT HEATING             K/Sec
42     cvms               CONVECTIVE MOISTURE SOURCE             1/Sec
43     olic     DOWNWARD LONG WAVE AT BOTTOM (CLEAR)           W M**-2
44     lwtc        OUTGOING LONG WAVE AT TOP (CLEAR)           W M**-2
45     ocic    DOWNWARD SHORT WAVE AT GROUND (CLEAR)           W M**-2
46     swgc      UPWARD SHORT WAVE AT GROUND (CLEAR)           W M**-2
47     swtc         UPWARD SHORT WAVE AT TOP (CLEAR)           W M**-2
48     saea   SHORT WV ABSRBD BY EARTH/ATMOS (CLEAR)           W M**-2
49     nlwb         NET LONG WAVE AT BOTTOM (CLEAR)            W M**-2
50     tdst         TIME MEAN DEEP SOIL TEMPERATURE                  K
51     tgsc         GROUND/SURFACE COVER TEMPERATURE                 K
52     tcas         TEMPERATURE OF CANOPY AIR SPACE                  K
53     vdcc         VERTICAL DIST TOTAL CLOUD COVER             No Dim
54     t2mt         TIME MEAN TEMP AT 2-M FROM SFC                   K
55     q2mt  TIME MEAN SPEC HUMIDITY AT 2-M FROM SFC            No Dim
56     u10t   TIME MEAN AT 10 METRE U-WIND COMPONENT             M/Sec
57     v10t   TIME MEAN AT 10 METRE V-WIND COMPONENT             M/Sec
58     tke2                 TURBULENT KINETIC ENERGY             M/Sec
59     w100   SPEED WIND AT 100-M FROM SURFACE LAYER             M/Sec
60     d100     DIR WIND AT 100-M FROM SURFACE LAYER           Degrees


Consultar uma variável específica:

>>> besm.get_var_description("t2mt")
  Variable                              Name Unit
0     t2mt  TIME MEAN TEMP AT 2-M FROM SFC      K


Listar Níveis Disponíveis
-------------------------

>>> besm.list_levels()
Available levels: ['1000', '925', '850', '775', '700', '500', '400', '300', '250', '200', '150', '100', '70', '50', '30', '20', '10', '3']


Listar Áreas Disponíveis
------------------------

Para consultar os tipos de áreas disponíveis:

>>> besm.list_areas()
Usage: list_areas( 'continentes'/'paises'/'regioes'/'estados'/'bacias'/'biomas') e.g. besm.list_areas('paises') 
>>> besm.list_areas('continentes')
Usage: type_abbreviation e.g. shp='estados_sp'
Area    -   type_abbreviation
Africa - continentes_af
Antartica - continentes_an
Asia - continentes_as
Australia - continentes_au
Europa - continentes_eu
Oceania - continentes_oc
America do Sul - continentes_sa
America do Norte - continentes_na
>>> 


Ler Dados do BESM
-----------------

O pacote BESM oferece duas formas principais de leitura de dados:

**load**

Utilizado para recuperar dados sem recorte espacial, abrangendo todo o domínio disponível.

>>> f = besm.load( date="20261110", var=["atmt", "uvmt", "vvmt"], level=[1000,850] )
Building request
Start: 20261110 - End: 20261120
Period: 20261110 - 20261120
Forecast records: 10
Requested data: 4.22 MB
20261111: 0.42 MB, Time: 14.10s, Speed: 0.03 MB/s                                                                                                      
20261112: 0.42 MB, Time: 14.11s, Speed: 0.03 MB/s                                                                                                      
20261113: 0.42 MB, Time: 14.13s, Speed: 0.03 MB/s                                                                                                      
20261114: 0.42 MB, Time: 15.51s, Speed: 0.03 MB/s                                                                                                      
20261115: 0.42 MB, Time: 19.68s, Speed: 0.02 MB/s                                                                                                      
20261116: 0.42 MB, Time: 20.47s, Speed: 0.02 MB/s                                                                                                      
20261117: 0.42 MB, Time: 21.61s, Speed: 0.02 MB/s                                                                                                      
20261118: 0.42 MB, Time: 24.70s, Speed: 0.02 MB/s                                                                                                      
20261119: 0.42 MB, Time: 21.36s, Speed: 0.02 MB/s                                                                                                      
20261120: 0.42 MB, Time: 21.38s, Speed: 0.02 MB/s                                                                                                      
Download total: 100%|█████████████████████████████████████████████████████████████████████████████████████████████| 4.42M/4.42M [03:07<00:00, 23.6kB/s]
>>> 

**load_shape**

Utilizado quando se deseja aplicar um recorte espacial nos dados.

Permite selecionar áreas específicas (estados, países, regiões, continentes, bacias ou biomas) utilizando abreviações de shape.


Salvando arquivos NetCDF por período com save_by_period
-------------------------------------------------------

O método save_by_period() permite salvar um xarray.Dataset retornado pelo BESM em arquivos NetCDF (.nc) agrupados por período de tempo. É uma forma conveniente de organizar os resultados da previsão para posterior processamento, arquivamento ou distribuição.

help do método

>>> help(besm.save_by_period)

Exemplo de leitura dos dados

.. code-block:: python

  import besm.CPTEC_BESM as BESM
  besm = BESM.model()
  f = besm.load(start=0, end=10)
  print(f)

Resultado:

.. code-block:: python
  <xarray.Dataset>
  Dimensions: (time: 10, lat: 96, lon: 192)
 
  Coordinates:
  * time (time) datetime64[ns] 2026-11-13 ... 2026-11-22
  * lat (lat) float64 -88.57 ... 88.57
  * lon (lon) float64 0.0 ... 358.1
 
  Data variables:
  TSMT (time, lat, lon) float32
 
- Salvamento padrão (frequency="day")

O valor padrão de frequency é "day". Portanto, basta chamar o método informando apenas o dataset:

.. code-block:: python

  arquivos = besm.save_by_period(f)
  print(arquivos)

Resultado:

.. code-block:: python

  [
  './BESM_20261112_20261113.nc',
  './BESM_20261112_20261114.nc',
  './BESM_20261112_20261115.nc',
  './BESM_20261112_20261116.nc',
  './BESM_20261112_20261117.nc',
  './BESM_20261112_20261118.nc',
  './BESM_20261112_20261119.nc',
  './BESM_20261112_20261120.nc',
  './BESM_20261112_20261121.nc',
  './BESM_20261112_20261122.nc'
  ]

- Salvamento de todo o período (frequency="all")

Caso seja desejado gerar apenas um arquivo contendo todos os registros carregados, utilize a opção frequency="all".

.. code-block:: python

  arquivos = besm.save_by_period(f, frequency="all")
  print(arquivos)

Resultado:

.. code-block:: python

  [
  './BESM_20261112_20261113_20261122.nc'
  ]




