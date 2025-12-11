# **Projeto de Bases de Dados (CC2005)**

**Grupo nº** 62

**Tema:** Contratos Públicos em Portugal nos primeiros dois meses de
2024

**Elementos do grupo:**

|                      |                                   |
|----------------------|-----------------------------------|
| **Nº mecanográfico** | **Nome**                          |
| 202400891            | Ezequiel Tchimbaya Cachapeu Paulo |
| 202309160            | Sérgio Gomes Pinto                |
| 202400863            | Victor De Vargas Lopes            |

## 1. **Modelação** {#modelação}

### 1.1. **Descrição do universo e modelo ER** {#descrição-do-universo-e-modelo-er}

#### 1.1.1. Descrição do Universo da Base de Dados (Requisitos Textuais) {#descrição-do-universo-da-base-de-dados-requisitos-textuais}

**Requisitos do Universo do Discurso**

Partimos por analisar a tabela única do aquivo dado no formato Excel e
em paralelo o arquivo no formato txt (contendo a informação sumária da
BD), onde derivamos o seguinte **conjunto de requisitos** que descrevem
o universo de informações representado pela nossa BD:

- O sistema armazena informação sobre **Contratos Públicos em Portugal,
  com foco nos contratos que possuem data de celebração nos primeiros
  dois meses de 2024**, cada um identificado por um *idContrato* único.

- A base de dados contém contratos no intervalo entre dezembro de 2023 e
  fevereiro de 2024, abrangendo tanto o período de divulgação quanto de
  celebração dos contratos.

- Os contratos são predominantemente em território português, mas também
  existem contratos com locais de execução em outros países, como
  Espanha, Alemanha, Estados Unidos, Arábia Saudita, Cabo Verde, dentre
  outros. Por esse motivo, os locais de execução possuem informação
  detalhada de município, distrito e país.

<!-- -->

- Cada contrato possui:

  - um **IdContrato**, contendo um número de identificação único para
    cada contrato.

  - um **tipo de contrato** (ex.: aquisição de bens móveis, aquisição de
    serviços, empreitadas de obras públicas e etc.).

  - um **tipo de procedimento**, indicando a forma de contratação.

  - uma **descrição do objeto do contrato**, correspondendo ao bem ou
    serviço adquirido.

  - uma **data de publicação** na base nacional de contratos.

  - uma **data de celebração**, que define quando o contrato foi
    formalmente celebrado.

  - um **preço contratual**.

  - um **CPV** (Código de Vocabulário Comum para Contratos Públicos).

  - um **prazo de execução**.

- Cada contrato envolve uma e **apenas uma entidade adjudicante**, a
  qual:

  - possui **número fiscal (NIF)**.

  - possui **designação oficial**.

- Um contrato pode envolver **um ou vários adjudicatários**
  (fornecedores), cada um dos quais:

  - possui **número fiscal (NIF)**.

  - possui **designação oficial**.

- O sistema armazena a **localização da execução do contrato**, devendo
  esta ser desagregada em:

  - país

  - distrito

  - município

> Cada uma destas localizações deve ser representada como entidade
> própria com **códigos oficiais** (ex.: Pais_id, Distrito_id,
> Municipio_id).
>
> O sistema permite múltiplas localizações de execução para um mesmo
> contrato.

- O sistema armazena a **fundamentação** da contratação: a justificação
  formal para a escolha do procedimento e/ou adjudicatário.

- O sistema indica se o contrato resulta de um **procedimento
  centralizado**.

- Se aplicável, o sistema regista o **Acordo Quadro** que enquadra o
  contrato, devendo armazenar a sua designação.

#### 1.1.2. Concretização em Modelo ER (Modelo conceitual derivado da forma textual) {#concretização-em-modelo-er-modelo-conceitual-derivado-da-forma-textual}

Alistando todas as colunas da tabela (dataset original), temos:

*Idcontrato, tipoContrato, tipoprocedimento, objectoContrato,
adjudicante,*

*adjudicatarios, dataPublicacao, dataCelebracaoContrato,
precoContratual, cpv,*

*prazoExecucao, localExecucao, fundamentacao, ProcedimentoCentralizado
e*

*DescrAcordoQuadro.*

Durante as aulas, aprendemos que a presença de uma única tabela
geralmente significa que **várias entidades estão "achatadas"** em uma
só. Devemos, entretanto, examinar a tabela (única), buscando entender:

- Quais colunas existem

- Que tipo de informação cada coluna contém

- Quais grupos de colunas parecem pertencer a entidades diferentes

- Como as linhas se repetem e etc.

Nesta etapa, o nosso foco está voltado para reconhecer "pistas" sobre
entidades e relacionamentos.

Para separar as devidas entidades, guiamo-nos por apectos como:

- Colunas que descrevem pessoas

- Colunas que descrevem produtos

- Colunas que descrevem eventos (ex.: vendas, celebração, publicação,
  execução)

- Colunas que pertencem a lugares, datas, categorias, etc.

A estrutura de entidades-tipo decorrentes dos requisitos é dada por:

##### **1.1.2.1. Entidades-tipo**  {#entidades-tipo}

A tabela original (dada) mistura **cinco entidades principais**
(Contrato, Entidade, CPV, Localização e AcordoQuadro) com seus
respetivos atributos:

> **1. CONTRATO**

- **Atributos:** [IdContrato]{.underline}, TipoContrato,
  > TipoProcedimento, ObjectoContrato, DataPublicacao,
  > DataCelebracaoContrato, PrecoContratual, PrazoExecucao,
  > Fundamentacao, ProcedimentoCentralizado.

> **2. ENTIDADE**
>
> (Representa adjudicantes e adjudicatários)

- **Atributos:** [Entidade_id]{.underline}, Nif, Designacao

- O atributo "Nif" é caracterizado como um valor **único**.

- Uma entidade pode desempenhar papéis diferentes em diferentes
  > contratos.

> **3. CPV**

- **Atributos:** C[pv_id]{.underline}, Codigo, Descrição

> **4. LOCALIZAÇÃO**
>
> (Seus **atributos** são multialores compostos)
>
> **a) PAÍS**

- Pais_Id, Nome

> **b) Distrito**

- Distrito_id, Nome

> **c) MUNICÍPIO**

- Municipio_id, Nome

> **5. ACORDOQUADRO**

- **Atributos:** [AcordoQuadro_id]{.underline}, DescrAcordoQuadro

##### **1.1.2.2. Relacionamentos** {#relacionamentos}

Os respetivos relacionamentos decorrentes entre as Entidades_tipo são
dadas por:

> **1. ADJUDICA (**ENTIDADE (Adjudicante), CONTRATO**)**

- Um contrato tem **um e apenas um adjudicante**

- Uma entidade pode adjudicar/conceder **vários contratos**

- **Tipo:** 1:N

> **2. ADJUDICADO (**CONTRATO, ENTIDADE (Adjudicatário))

- Um contrato pode ter **um ou vários adjudicatários**

- Uma entidade pode ser adjudicatária em **muitos contratos**

- **Tipo:** M:N via *AdjudicatarioContrato*

> **3. EXECUTADO_EM (**CONTRATO, LOCALIZAÇÃO**)**

- Um contrato pode ter **uma ou várias localizações de execução**

- Uma localização (município) pode aparecer em vários contratos

- **Tipo:** M:N via *ContratoLocalExecucao*

> **4. POSSUI (**CONTRATO, CPV**)**

- Cada contrato tem um ou mais códigos CPVs

- Um código CPV pode ser associado a muitos contratos

- **Tipo:** M:N

> **5. ENQUADRADO_POR (**CONTRATO, ACORDOQUADRO**)**

- Um contrato pode ou não estar associado a um acordo quadro

- Um acordo quadro pode enquadrar muitos contratos

- **Tipo:** N:1

> Por tabela:

BD Contratos: Cardinalidade (C) e Participação (P)

<table>
<colgroup>
<col style="width: 52%" />
<col style="width: 14%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Relacionamento</strong></th>
<th><strong>C</strong></th>
<th><strong>p</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><blockquote>
<p><strong>ADJUDICA</strong></p>
</blockquote>
<p><strong>(</strong>ENTIDADE (Adjudicante),
CONTRATO<strong>)</strong></p></td>
<td>1&lt;&gt;N</td>
<td>Parcial &lt;&gt; Total</td>
</tr>
<tr class="even">
<td><p><strong>ADJUDICADO</strong></p>
<p><strong>(</strong>CONTRATO, ENTIDADE (Adjudicatário))</p></td>
<td>M&lt;&gt;N</td>
<td>Total &lt;&gt; Parcial</td>
</tr>
<tr class="odd">
<td><blockquote>
<p><strong>EXECUTADO_EM</strong></p>
<p><strong>(</strong>CONTRATO, LOCALIZAÇÃO<strong>)</strong></p>
</blockquote></td>
<td>M&lt;&gt;N</td>
<td>Total &lt;&gt; Parcial</td>
</tr>
<tr class="even">
<td><blockquote>
<p><strong>POSSUI</strong></p>
<p><strong>(</strong>CONTRATO, CVP<strong>)</strong></p>
</blockquote></td>
<td>M&lt;&gt;N</td>
<td>Total &lt;&gt; Parcial</td>
</tr>
<tr class="odd">
<td><p><strong>ENQUADRADO_POR</strong></p>
<p><strong>(</strong>CONTRATO, ACORDOQUADRO<strong>)</strong></p></td>
<td>N&lt;&gt;1</td>
<td>Parcial &lt;&gt; Total</td>
</tr>
</tbody>
</table>

#### 1.1.3. Diagrama do Modelo ER {#diagrama-do-modelo-er}

Apesar da existência de diversas ferramentas existentes para auxílio de
ilustração de diagramas do modelo ER, nós(equipa) optamos pelo uso das
ferramentas online
**[dbdia](https://colab.research.google.com/github/edrdo/dbdia/blob/master/src/main/colab/dbdia.ipynb#scrollTo=gxb3yS3XkG2M)**
e **[draw.io](https://www.drawio.com/)** disponibilizadas pelo
professor, por ser mais intuitivo (para a equipa).

##### **1.1.3.1.** Para **Entidades-tipo (**com atributos principais**)** {#para-entidades-tipo-com-atributos-principais}

> **Sintaxe Textual:**

CONTRATO([IdContrato]{.underline}, {TipoContrato(Tipo_IdContrato,
TContrato_Descricao)}, TipoProcedimento(Tipo_IdProcedimento,
TProcedimpento_Descricao), ObjectoContrato, DataPublicacao,
DataCelebracaoContrato, PrecoContratual, PrazoExecucao,
{Fundamentacao(Id_Fundamentacao, Artigo, nr, alinea, subalinea ?,
RefLegislativa)}, ProcedimentoCentralizado)

ENTIDADE([Entidade_id]{.underline}, Nif, Designacao)

CPV([Cpv_id]{.underline}, Codigo, Descrição)

LOCALIZACAO({Pais(Pais_Id, Nome)}, {Distrito(Distrito_id, Nome)},
{Municipio(Municipio_id, Nome)})

ACORDOQUADRO([AcordoQuadro_id]{.underline}, DescrAcordoQuadro ?)

**  
Diagrama:**

![](media/image1.png)

![](media/image2.png)

![](media/image3.png)

![](media/image4.png)![](media/image5.png)

##### **1.1.3.2.** Para os **Relacionamentos (**omitindo atributos internos para facilitar a visualização estrutural**)** {#para-os-relacionamentos-omitindo-atributos-internos-para-facilitar-a-visualização-estrutural}

**Sintaxe Textual:**

> ENTIDADE \-- 1 \-- \<ADJUDICA\> == N == CONTRATO
>
> CONTRATO == M == \<ADJUDICADO\> \-- N \-- ENTIDADE
>
> CONTRATO == M == \<EXECUTADO_EM\> \-- N \-- LOCALIZACAO
>
> CONTRATO == M == \<POSSUI\> \-- N \-- CPV
>
> CONTRATO \-- N \-- \<ENQUADRADO_POR\> == 1 == ACORDOQUADRO

**Diagrama:**

![](media/image6.png)

**Diagrama Completo para o modelo ER:**

![](media/image7.png)![](media/image8.png)

###  1.2**. Modelo relacional**  {#modelo-relacional}

#### 1.2.1. Explicação do Mapeamento do Modelo ER para o Modelo Relacional {#explicação-do-mapeamento-do-modelo-er-para-o-modelo-relacional}

Em perspetiva geral o **Mapeamento** do Modelo ER para o Modelo
Relacional consiste em transformar as **Entidades-tipo** e
**Relacionamentos** (do modelo ER) em **tabelas** (para o modelo
Relacional).

Para realizar tais tranformações optamos realiza-las em etapas:

##### **1.2.1.1. Mapeamento de entidades-tipo em tabelas** {#mapeamento-de-entidades-tipo-em-tabelas}

> Nesta etapa, passamos pelo mapeamento directo de atributos, excepto no
> caso de atributos multi-valor em que precisamos de recorrer a "tabelas
> auxiliares" (no caso de ***TipoContrato*** e ***Fundamentacao***),
> identificando assim no processo:

- Uma chave primária (**PK**)

- Atributos simples

- chaves externas (**FK**) apenas quando derivam de relacionamentos
  > obrigatórios

> Neste caso tornam-se tabelas:

**Sintaxe Textual:**

![](media/image9.png)![](media/image10.png)![](media/image11.png)![](media/image12.png)![](media/image13.png)![](media/image14.png)![](media/image15.png)![](media/image16.png)
![](media/image17.png)**Ilustração:**

##### **1.2.1.2. Mapeamento de relacionamentos em tabelas** {#mapeamento-de-relacionamentos-em-tabelas}

> Nesta etapa, passamos pelo mapeamento conforme a **cardinalidade** e
> **participação** das entidades-tipos de um relacionamento. Separamos
> em função de alguns critérios (para melhor entendimento):

1.  **Relacionamentos** com cardinalidade **1:N**

> Em relações com essa cardinalidade em seus relacionamentos (no modelo
> ER), realizamos o respectivo mapeamento mantendo os atributos da
> Entidade-tipo do lado (**N**) adicionado do atributo-chave da
> Entidade-tipo do lado (**1**) como chave externa **FK** na **tabela**
> do lado **N**.
>
> Sendo:

- *Entidade (**1**) -- (**N**) Contrato (adjudicante)* → **Contrato**
  > recebe adjudicante_id

- *(**N**) Contrato -- AcordoQuadrado (**1**)* → **Contrato** recebe
  > acordoquadrado_id

- *País (**1**) -- (**N**) Distrito* → **Distrito** recebe pais_id como
  > **FK**

- *Distrito (**1**) -- (**N**) Município* → **Município** recebe
  > distrito_id como **FK**

> **2. Relacionamentos** com cardinalidade **M:N**
>
> Em relações com essa cardinalidade em seus relacionamentos (no modelo
> ER), realizamos o respectivo mapeamento para uma espécie de "**Tabelas
> Associativas**". Dois relacionamentos produzem tabelas intermédias:
>
> **a) Contrato --- Entidade (Adjudicatário)**
>
> → cria tabela **ADJUDICATARIO_CONTRATO** sendo as Chaves-primária
> (**PK**) a junção das duas **PK** das duas Entidades (idContrato,
> entidade_id).
>
> **b) Contrato --- Município (Local Execução)**
>
> → cria tabela **CONTRATO_LOCAL_EXECUCAO** sendo de igual modo as
> Chaves-primária (**PK**) a junção das duas **PK** das duas Entidades
> (idContrato, municipio_id).
>
> **c) CPV --- Contrato**
>
> → cria tabela **Possui** sendo de igual modo as Chaves-primária
> (**PK**) a junção das duas **PK** das duas Entidades (CPV, Contrato).
>
> **3. Atributos não simples:** com destaque aos que exigem um
> mapeamento por "**Tabelas Associativas**":
>
> **1. Atributos compostos → desagregar**
>
> Partindo do mapeamento da Entidade-tipo **Localização** (já
> desagregado em 3 entidades) podemos obter o "localExecucao" das 3
> entidades originárias:

- País

- Distrito

- Município  
  > com relacionamentos hierárquicos 1:N.

> ***Obs.:*** Por termos desagregado anteriormente os atributos
> ***TipoContrato*** e ***Fundamentacao*** (da entidade Contratos) dando
> origem a duas novas tabelas (como visto na ilustração anterior), as
> mesmas darão agora origem a duas novas "**tabelas associativas**"
> frutos da relação com as outras:

a)  **ClassificacaoContratos**(TipoContrato, Contratos)

- **Tipo** M:N

- Um contrato pode ser clasificado por **um ou vários tipos de
  Contratos.**

- Um tipo de contrato pode classificar um ou vários contratos.

b)  **FundamentacaoContratos**(Fundamentacao, Contratos)

- **Tipo** M:N

- Um contrato pode ser fundamentado por **uma ou mais fundamentações.**

- Uma fundamentação pode fundamentar um ou vários contratos.

> **2. Atributos multivalorados → nova tabela**
>
> O atributo multivalor do *localExecução* gera a tabela associativa
> **CONTRATO_LOCAL_EXECUCAO** sendo as Chaves-primária (**PK**) a junção
> das duas **PK** das duas Entidades (idContrato, municipio_id), com
> Chaves-externa (**FK**) de: CONTRATO (idContrato),
> MUNICIPIO(municipio_id).

##### **1.2.1.3. MODELO RELACIONAL FINAL (Tabelas com PK e FK explícitas)** {#modelo-relacional-final-tabelas-com-pk-e-fk-explícitas}

> Neste caso, tornam-se tabelas resultantes do mapeamento:

**Sintaxe Textual:**

**Diagrama:**

***Obs.:*** Para auxílio em ilustração (explicitando as respectiva
chaves primárias e externas) de diagramas do Modelo Relacional,
nós(equipa) optamos pelo uso da ferramenta online
**[draw.io](https://www.drawio.com/)** disponibilizada pelo professor,
por ser mais intuitivo (para a equipa).

![](media/image18.png)

O respetivo modelo Relacional (final) fornece-nos segurança pois, para
além de ser fruto do mapeamento directo do modelo ER, este está também
apresentado na **3ª FN (Terceira Forma Normal)**, pois obece as
seguintes condições:

> **1º --- Está em 1FN,** isto é:

- Todos os atributos têm valores atómicos (sem atributos compostos nem
  > multi-valores).

> **2º --- Está em 2FN,** isto é:

- Não existe dependência parcial (em nenhuma das tabelas com chave
  > composta).

> **3º --- Está em 3FN,** isto é:

- **Não há dependências transitivas**  
  Ou seja:  
  *um atributo não-chave não depende de outro atributo não-chave.*

## 2. **Implementação** {#implementação}

### 2.1. **Povoamento de tabelas** {#povoamento-de-tabelas}

O povoamento da base de dados (contratos_publicos.db) foi realizado
através do *script* Seed.py, seguindo rigorosamente a metodologia **ETL
(Extract, Transform, Load)**. Este processo garantiu a migração de dados
do *dataset* Excel para o modelo relacional normalizado.

O processo de Extract, Transform, Load foi implementado utilizando a
biblioteca pandas para a fase de Extração, garantindo uma leitura rápida
e eficiente do ficheiro Excel. A fase de Transformação foi a mais
crítica, dado o formato não estruturado de vários campos da fonte de
dados.

A criação das tabelas na base de dados foi realizada utilizando a
linguagem SQL através de scripts Python, conforme definido no ficheiro
scheme.py. Este processo envolveu a definição rigorosa da estrutura das
tabelas, incluindo as chaves primárias (PK) e as chaves estrangeiras
(FK), para garantir a integridade referencial dos dados (por exemplo, a
ligação entre municipio, distrito e pais).

Exemplo de criação de tabela simples:

![](media/image19.png)

Criação de uma tabela com uma chave estrangeira:

![](media/image20.png)

A normalização da BD resultou na criação de tabelas intermediárias para
modelar as relações (N:M). Estas tabelas contêm apenas duas chaves
estrangeiras, sendo o par de chaves a sua Chave Primária Composta.

**Normalização de Tipos:** A conversão do campo precoContratual de texto
para REAL implicou o tratamento explícito de separadores decimais
(substituição de vírgula por ponto) e a gestão de valores nulos.

- **Parsing de Estruturas Múltiplas:** Campos como adjudicatários
  continham múltiplas entidades separadas por delimitadores (\| ou ;).
  Foi desenvolvida uma função específica (split_adjudicatarios) que
  utiliza expressões regulares (re) para dividir o *string* em entidades
  individuais.

- **Tratamento de Entidades Complexas (Fundamentação):** A coluna
  fundamentacao exigiu a criação de uma função de *parsing* avançada
  (parse_fundamentacao) para isolar, via re, os constituintes (Artigo,
  Número, Alínea, etc.), garantindo que cada componente fosse armazenado
  na sua coluna dedicada na tabela fundamentação.

![](media/image21.png)

- **Prevenção de Duplicidade (Controlo de Unicidade):** A função
  **insert_return_id** assegura que valores em tabelas de domínio (e.g.,
  pais, cpv) são inseridos apenas uma vez. Se o registo já existir, o
  código recupera o ID existente (SELECT) em vez de tentar uma nova
  inserção, prevenindo erros de violação de UNIQUE e garantindo que
  todas as chaves estrangeiras (FK) apontam para um único registo.

![](media/image22.png)

1.  **Tratamento de Entidades (Adjudicante/Adjudicatários)**

> A tabela entidade foi especialmente adaptada para o tratamento de
> dados confidenciais (RGPD) e inconsistências no NIF:

- A **Chave Primária** passou a ser **entidade_id**, e o NIF tornou-se
  uma coluna de dados regular.

- A lógica de inserção foi ajustada para, na ausência de NIF, tentar
  associar o contrato à entidade existente usando apenas a designacao
  (nome), minimizando a criação de registos duplicados no caso de o NIF
  estar omisso.

|                         |                    |
|-------------------------|--------------------|
| **Nome da tabela**      | **Nº de entradas** |
| Contrato                | 21748              |
| Entidade                | 11698              |
| Adjudicatario_Contrato  | 23282              |
| CPV                     | 2211               |
| Possui                  | 22067              |
| Acordo_Quadro           | 256                |
| Tipo_Contrato           | 8                  |
| ClassificacaoContratos  | 21911              |
| Pais                    | 18                 |
| Distrito                | 40                 |
| Municipio               | 348                |
| Contrato_Local_Execucao | 23317              |
| Tipo_Procedimento       | 11                 |
| Fundamentacao           | 88                 |
| Fundamentacao_Contrato  | 21611              |

### 2.2. **Interrogações SQL** {#interrogações-sql}

**Dividimos as 10 perguntas em 3 níveis de dificuldade, Fáceis, Médias e
Difíceis**

1.  **Fáceis**

- **Quais os tipos de contratos por ordem Alfabética?**

> SELECT nome
>
> FROM tipo_contrato
>
> ORDER BY nome
>
> ![](media/image23.png)

- **Quais as entidades que comecem por F?**

> select e.designacao
>
> FROM entidade e
>
> WHERE e.designacao LIKE \'F%\'
>
> ![](media/image24.png)

- **Soma dos Preços Contratuais por País?**

> SELECT
>
> p.nome AS pais,
>
> SUM(c.precoContratual) AS total_preco
>
> FROM contrato c
>
> JOIN contrato_local_execucao cle
>
> ON c.idContrato = cle.idContrato
>
> JOIN municipio m
>
> ON cle.municipio_id = m.municipio_id
>
> JOIN distrito d
>
> ON m.distrito_id = d.distrito_id
>
> JOIN pais p
>
> ON d.pais_id = p.pais_id
>
> GROUP BY p.nome
>
> ![](media/image25.png)

1.  **Médias**

- **Quantos contratos existem por tipo de contrato?**

> SELECT tc.nome,
>
> COUNT(c.idContrato) AS quantidade
>
> FROM contrato c
>
> JOIN
>
> ClassificacaoContratos ctc ON c.idContrato = ctc.idContrato
>
> JOIN
>
> tipo_contrato tc ON ctc.tipoContrato_id = tc.tipoContrato_id
>
> GROUP BY tc.tipoContrato_id
>
> ![](media/image26.png)

- **Total de Contratos por Entidade Adjudicante?**

> SELECT e.designacao AS entidade,
>
> COUNT(c.idContrato) AS total
>
> FROM contrato c
>
> JOIN
>
> entidade e ON c.adjudicante_id = e.entidade_id
>
> GROUP BY c.adjudicante_id
>
> ORDER BY total DESC
>
> ![](media/image27.png)

- **Média do preço contratual por tipo de procedimento?**

> SELECT tp.descricao AS procedimento,
>
> ROUND(AVG(c.precoContratual), 2) AS media_preco
>
> FROM contrato c
>
> JOIN tipo_procedimento tp ON c.tipoProcedimento_id =
> tp.tipoProcedimento_id
>
> GROUP BY tp.tipoProcedimento_id
>
> ![](media/image28.png)

- **Número de contratos por mês?**

> SELECT SUBSTR(c.dataCelebracaoContrato, 6, 2) AS mes,
>
> COUNT(\*) AS total
>
> FROM contrato c
>
> GROUP BY mes
>
> ORDER BY mes
>
> ![](media/image29.png)

- **Contratos com preço superior à média global?**

> WITH media_global AS (
>
> SELECT AVG(precoContratual) AS mg FROM contrato
>
> )
>
> SELECT c.idContrato,
>
> e.designacao AS entidade,
>
> c.precoContratual
>
> FROM contrato c
>
> JOIN entidade e ON c.adjudicante_id = e.entidade_id
>
> WHERE c.precoContratual \> (SELECT mg FROM media_global)
>
> ORDER BY c.precoContratual DESC
>
> ![](media/image30.png)

1.  **Difíceis**

- **Top 10 contratos mais caros com cpv associado?**

> SELECT c.idContrato,
>
> c.precoContratual,
>
> cpv.designacao AS cpv_nome
>
> FROM contrato c
>
> JOIN possui p ON c.idContrato = p.idContrato
>
> JOIN cpv ON p.cpv_id = cpv.cpv_id
>
> ORDER BY c.precoContratual DESC
>
> LIMIT 10
>
> ![](media/image31.png)

- **Top 10 municípios por soma do preço contratual?**

> WITH contratos_por_municipio AS (
>
> SELECT DISTINCT idContrato, municipio_id
>
> FROM contrato_local_execucao
>
> )
>
> SELECT m.nome AS municipio,
>
> ROUND(SUM(c.precoContratual), 2) AS total
>
> FROM contratos_por_municipio cm
>
> JOIN contrato c ON cm.idContrato = c.idContrato
>
> JOIN municipio m ON cm.municipio_id = m.municipio_id
>
> GROUP BY m.municipio_id
>
> ORDER BY total DESC
>
> LIMIT 10
>
> ![](media/image32.png)

### 2.3. **Aplicação Python** {#aplicação-python}

A componente final do projeto consiste num site, desenvolvido em Python,
que serve como interface de consulta e visualização dos dados contidos
na base de dados SQLite (contratos_publicos.db). Esta aplicação
demonstra a funcionalidade analítica do modelo relacional através da
execução interativa das interrogações SQL definidas.

1.  **Arquitetura da aplicação**

> O desenvolvimento recorreu ao Flask, reconhecido pela sua leveza e
> flexibilidade. A arquitetura da aplicação é dividida em três camadas
> essenciais:

- **Servidor:** (server.py) O ponto de entrada da aplicação. Este
  ficheiro é responsável por inicializar o ambiente de execução e
  estabelecer a **conexão inicial** com a base de dados antes de iniciar
  o servidor web.

![](media/image33.png)

- **Lógica da Aplicação:** (app.py) Contém o código central, onde são
  definidas todas as rotas (*endpoints*) e as funções que executam a
  lógica. este caso, as consultas SQL.

- **Apresentação:** (Templates HTML) Utiliza o motor de *templating*
  Jinja2 (integrado no Flask) para renderizar a estrutura HTML. Os dados
  obtidos das consultas SQL são passados a estes *templates* para serem
  formatados e apresentados ao utilizador.

  1.  **Interface da app**

<!-- -->

- **/** (index.html)

> Página inicial da aplicação, de Título 'Base de Dados de Contratos
> Públicos', com informação sobre o número total de contratos e valor
> total da soma de todos os contratos, uma opção para listar todos os
> contratos (/contratos.html), podendo depois obter informação sobre um
> contrato apenas clicando no seu ID (/contrato.html), ou na página
> inicial no canto superior direito indicar o idContrato e ser logo
> direcionado para o mesmo.
>
> ![](media/image34.png)
>
> Código para listar todos os contratos e informação sobre um contrato
> em específico.
>
> (index.html)
>
> ![](media/image35.png)

- / (Q1 ... /Q10.html)

> No index.html temos acesso a 10 perguntas de dificuldades variadas
> que, escolhendo uma, direciona para uma nova página com a pergunta e o
> resultado tabelado.
>
> Exemplo da pergunta Q6
>
> ![](media/image36.png)
>
> Código .html para a pergunta:
>
> ![](media/image37.png)

|                |                                                                                                              |
|----------------|--------------------------------------------------------------------------------------------------------------|
| **"Endpoint"** | **Funcionalidade**                                                                                           |
| **/**          | Página de entrada com acesso a número total de contratos assim como todos os contratos, e lista de perguntas |
| /contratos     | Tabela com todos os contratos                                                                                |
| /contrato      | Tabela de um contrato só, previamente selecionado                                                            |
| /Q1.../Q10     | Tabela com a resposta para as perguntas selecionadas                                                         |

## 3. **Conclusão** {#conclusão}

Para o trabalho realizado foi feita uma compreensão sólida e sistemática
no desenvolvimento de um sistema de bases de dados para armazenar e
consultar informações sobre Contratos Públicos em Portugal 2024.

O projeto começou com uma análise detalhada dos requisitos, na criação
de um Modelo Entidade-Relacionamento (ER), que foi então rigorosamente
mapeado para um Modelo Relacional.

A decisão de desagregar a tabela única original em entidades como
*Contrato*, *Entidade*, *CPV*, *Localização* e *Acordo Quadro* foi
crucial para a correta normalização. O Modelo Relacional final está em
conformidade com a Terceira Forma Normal (**3FN**), garantindo a
integridade e a ausência de dependências transitivas e parciais.

A complexidade foi gerenciada com a utilização da biblioteca pandas para
a extração e o desenvolvimento de funções de parsing avançadas em Python
para lidar com dados não estruturados, como *adjudicatários* e
*fundamentação*.

A eficácia do modelo relacional foi comprovada pela capacidade de
executar interrogações SQL, divididas por níveis de dificuldade (Fáceis,
Médias e Difíceis), que abrangem desde consultas simples até análises
mais complexas.

A componente final, o site desenvolvido em Python usando Flask,
proporciona uma interface de consulta e visualização dos dados, tornando
o projeto funcional e acessível ao utilizador.

Em última instância, decidimos deixar nosso projecto disponível no
**Github** para eventuais consultas e futuras melhorias (caso sejam
necessárias):
<https://github.com/ezequielcabeja/BD-Projecto-Contratos-Publicos-2024>

## **Referências**

Ferramentas:

Link da ferramenta (**dbdia**) usada para ilustração dos diagramas do
modelo ER:
<https://colab.research.google.com/github/edrdo/dbdia/blob/master/src/main/colab/dbdia.ipynb>

Link da ferramenta (**draw.io**) usada para ilustração dos diagramas do
modelo Relacional:

<https://app.diagrams.net/?src=about>

Slides das aulas:

O **Modelo ER** - Bases de Dados (CC2005), *Eduardo R. B. Marques,
DCC/FCUP*

O **Modelo Relacional** - Bases de Dados (CC2005), *Eduardo R. B.
Marques, DCC/FCUP*

**Normalização** - Bases de Dados (CC2005), Departamento de Ciência de
Computadores Faculdade de Ciências da Universidade do Porto, *Eduardo R.
B. Marques --- DCC/FCUP*

**SQL Gabriel** - Bases de Dados, *Gabriel David, <gtd@fe.up.pt>*

**Aplicações BD com SQL embebido** - Bases de Dados (CC2005), *Eduardo
R. B. Marques, DCC/FCUP*
