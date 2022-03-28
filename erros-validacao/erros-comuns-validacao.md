# Erros comuns de validação:

## Índice:

 1. [Sobre os arquivos de metadados e seu conteúdo](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#1-sobre-os-arquivos-de-metadados-e-seu-conte%C3%BAdo)

  1.1. [Sintaxe do `datapackage.json`](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#11-sintaxe-do-datapackagejson)

  1.2. [Nome `name` do recurso contém caracteres fora da faixa permitida](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#12-nome-name-do-recurso-cont%C3%A9m-caracteres-fora-da-faixa-permitida)
  
  1.3. [O caminho `path` incorreto](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#13-o-caminho-path-incorreto)

2. [Sobre os arquivos de dados e seu conteúdo](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#2-sobre-os-arquivos-de-dados-e-seu-conte%C3%BAdo)

  2.1. [Divergências de características dos dados no `datapackage.json`](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#21-diverg%C3%AAncias-de-caracter%C3%ADsticas-dos-dados-no-datapackagejson)

  2.1.1. formatos de data

  2.1.2. dado obrigatório ausente

  2.1.3. valor numérico não inteiro

  2.1.4. valor fora das características informadas

  2.2. [Valores fora dos campos delimitadores (, ou ;)](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#22-valores-fora-dos-campos-delimitadores--ou-)

  2.3. [Arquivo de dados sem encoding `utf-8`](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#23-arquivo-de-dados-sem-encoding-utf-8)

[Appendix - Lista de verificação Frictionless - 'Validation Checks'](https://github.com/dados-mg/dados-mg.github.io/blob/erros-valiacao/erros-validacao/erros-comuns-validacao.md#lista-de-verifica%C3%A7%C3%A3o-frictionless---validation-checks)

---

Este documento segue, tanto quanto possível, a estrutura de classificação hierárquica por assunto e método de descrição seguinte:

---
## Natureza/grupo do erro

## Título do erro

Caso de uso

Item

Descrição sucinta

Exemplo copiado de mensagem de erro após `frictionless validate` na `bash cell`

Exemplo copiado de mensagem de erro de relatório online (`github actions`)

Print do arquivo e/ou pasta mostrando o erro

Solução proposta

---



## 1. Sobre os arquivos de metadados e seu conteúdo
<a href="#top">(inicio)</a>

#### 1.1. Sintaxe do `datapackage.json`:

**a. faltou abrir ou fechar algum campo com aspas, "", colchetes '[ ]' ou chaves '{ }'**

Esse problema pode acontecer na inclusão ou edição de qualquer propriedade ou seu valor, como por exemplo:

- edição do `name`, `title`, ou `description`, do conjunto ou dos `resources`;

- nas propriedades das variáveis/colunas de cada `resource` (`schema`);

- na inclusão de pastas para `resource` (i.e.: 'data/resource.csv'), `schema.json`, `dialect.json`, etc;

- na inclusão de novo recurso;

- na inclusão da propriedade `owner_org`; dentre outros ...


````$ frictionless validate datapackage.json
# -------
# invalid: datapackage.json
# -------
=============  ===================================================================================================================================
code           message                                                          
=============  ===================================================================================================================================
package-error  The data package has an error: cannot extract metadata "datapackage.json" because "Expecting value: line 101 column 28 (char 3812)"
=============  ===================================================================================================================================
````

![](static/sintaxe2-datapackage-frictionless.png)

![](static/sintaxe2-datapackage.png)


* Solução: corrigir a sintaxe do arquivo datapackage.json na linha indicada pela mensagem de erro, utilizando o editor de texto ou o editor de arquivos do github (que também podem fazer marcações visuais de erros de sintaxe)

**b. faltou/sobrou alguma vírgula**

Esse problema pode acontecer em diversas situações, tal qual exemplificado no caso de uso anterior.

````$ frictionless validate datapackage.json
# -------
# invalid: datapackage.json
# -------
=============  ==========================================================================================================================================
code           message                                                          
=============  ==========================================================================================================================================
package-error  The data package has an error: cannot extract metadata "datapackage.json" because "Expecting ',' delimiter: line 96 column 11 (char 3621)"
=============  ==========================================================================================================================================

````

![](static/sintaxe-datapackage-frictionless.png)

![](static/sintaxe-datapackage.png)

* **Solução**: corrigir a sintaxe do arquivo datapackage.json na linha indicada pela mensagem de erro, utilizando o editor de texto ou o editor de arquivos do github (que também podem fazer marcações visuais de erros de sintaxe)

#### 1.2. Nome `name` do recurso contém caracteres fora da faixa permitida
<a href="#top">(inicio)</a>

A especificação `Frictionless` não aceita espaços, letras maiúsculas ou caracteres especiais no valor deste campo:

````$ frictionless validate datapackage.json
# -------
# invalid: datapackage.json
# -------
=============  ================================================================================================================================================================================================================
code           message                                                                                                                                          
=============  ================================================================================================================================================================================================================
package-error  The data package has an error: "'doa▒▒es-comodatos-amigo-estado-mg' does not match '^([-a-z0-9._/])+$'" at "resources/0/name" in metadata and at "properties/resources/items/properties/name/pattern" in profile
=============  ================================================================================================================================================================================================================
````

![](static/name.png)

* **Solução**: corrigir a propriedade `name` contendo [especificações legíveis por máquina](https://specs.frictionlessdata.io/data-resource/#metadata-properties)


#### 1.3. O caminho `path` incorreto
<a href="#top">(inicio)</a>

**a. onde se localizam os arquivos de dados:**

````$ frictionless validate datapackage.json
# -------
# invalid: doacoes-comodatos-amigo-estado-mg.csv
# -------

===  =====  ============  ==============================================================================================================================
row  field  code          message                                               
===  =====  ============  ==============================================================================================================================
            scheme-error  The data source could not be successfully loaded: [Errno 2] No such file or directory: 'doacoes-comodatos-amigo-estado-mg.csv'
===  =====  ============  ==============================================================================================================================
````

![](static/path-frictionless.png)

![](static/path.png)

* **Solução**: corrigir o valor da propriedade `path` incorporando o nome da pasta ou URL onde se localiza o recurso, ou corrigindo o nome do recurso, se for o caso

**b. onde se localizam os arquivos de metadados `datapackage.json`, o `schema.json`, ou o `dialect.json`**

````$ frictionless validate datapackage.json
# -------
# invalid: datapackage.json
# -------
============  =======================================================================================================================
code          message                                                           
============  =======================================================================================================================
schema-error  Schema is not valid: cannot extract metadata "schema.json" because "[Errno 2] No such file or directory: 'schema.json'"
============  =======================================================================================================================
````

![](static/path-schema-frictionless.png)

![](static/path-schema.png)

* **Solução**: corrigir o valor da propriedade `path` incorporando o nome da pasta ou URL onde se localiza o `schema` ou `dialect`, ou corrigindo seu nome, se for o caso


## 2. Sobre os arquivos de dados e seu conteúdo
<a href="#top">(inicio)</a>

#### 2.1. Divergências de características dos dados no `datapackage.json`

**2.1.1. formatos de data**

Observar se a estrutura física do dado está coerente com sua representação lógica no arquivo de metadados `datapackage.json`, ex:

- AAAA/MM/DD (formato `default` para data na especificação `Frictionless`);

- dd/mm/aaaa (formato usual no Brasil e como geralmente esse tipo de variável é exportado do excel)

![](static/data-goodtables1.png)

![](static/data-goodtables2.png)

````
  {
    "name": "date",
    "type": "date",
    "format": "default",
    "title": "Date"
  },
````


* **Solução**: adequar o formato da data no `datapackage.json` ou no arquivo de dados:

![](static/data-commit.png)

**2.1.2. dado obrigatório ausente**
<a href="#top">(inicio)</a>

````$ frictionless validate datapackage.json
# -------
# invalid: data/doacoes-comodatos-amigo-estado-mg.csv
# -------

===  =====  ==========  =================================================================================================
row  field  code        message                                                 
===  =====  ==========  =================================================================================================
 51      9  type-error  Type error in the cell "" in row "51" and field "VALOR" at position "9": type is "number/default"
===  =====  ==========  =================================================================================================
````

![](static/dado-ausente-frictionless.png)


````          {
            "name": "VALOR",
            "type": "number",
            "format": "default",
            "title": "Valor",
            "description": "Valor estimado do bem ou serviço doado ou oferecido em comodato. Pode ser em moeda nacional ou estrangeira.",
            "groupChar": ".",
            "decimalChar": ","
          }
        ]
        "missingValues": [
          "NA"
        ]
````

![](static/dado-ausente.png)

* **Solução**: completar o valor ausente indicado no arquivo do recurso. Se ele não for obrigatório, aplicar 'NA' na célula indicada no arquivo do recurso


**2.1.3. valor numérico não inteiro**
<a href="#top">(inicio)</a>

- faltou informar os separadores de milhar (groupChar) e decimais (decimalChar):            

````$ frictionless validate datapackage.json
# -------
# invalid: data/doacoes-comodatos-amigo-estado-mg.csv
# -------

===  =====  ==========  =============================================================================================================
row  field  code        message
===  =====  ==========  =============================================================================================================
  2      9  type-error  Type error in the cell "300.000,00" in row "2" and field "VALOR" at position "9": type is "number/default"
  3      9  type-error  Type error in the cell "220.000,00" in row "3" and field "VALOR" at position "9": type is "number/default"
  4      9  type-error  Type error in the cell "347.000,00" in row "4" and field "VALOR" at position "9": type is "number/default"
````
![](static/number-default-frictionless.png)

![](static/number-default.png)

* **Solução**: aplicar as propriedades de separador de milhar `groupChar` e/ou separador de decimais `decimalChar`, sempre quando o valor numérico utilizar essa grafia. Alterar o valor da propriedade `type` de `integer` para `number`:

````
"type": "number",
"format": "default",
"title": "Valor",
"groupChar": ".",
"decimalChar": ","
````

**2.1.4. valor fora das características informadas**
<a href="#top">(inicio)</a>

  **a. type-error**

````$ frictionless validate datapackage.json
# -------
# invalid: data/doacoes-comodatos-amigo-estado-mg.csv
# -------

===  =====  ==========  =======================================================================================================
row  field  code        message                                                 
===  =====  ==========  =======================================================================================================
  2      9  type-error  Type error in the cell "300000%" in row "2" and field "VALOR" at position "9": type is "number/default"
===  =====  ==========  =======================================================================================================
````
![](static/valor-fora-frictionless.png)

![](static/valor-fora.png)

* **Solução**: alterar os valores no arquivo do recurso, observando as características das variáveis numéricas indicadas no `datapackage.json`. Caso o problema tenha sido na descrição do dado, alterar as propriedades necessárias no `datapackage.json`  
  
  **b. constraint-error - pattern**

````
$ frictionless validate datapackage.json
# -------
# invalid: data/doacoes-comodatos-amigo-estado-mg.csv
# -------

===  =====  ================  ==============================================================================================================================================
row  field  code              message                                                                                                                           
===  =====  ================  ==============================================================================================================================================
  2     10  constraint-error  The cell "R$" in row at position "2" and field "MOEDA" at position "10" does not conform to a constraint: constraint "pattern" is "^[A-Z]{3}$"
===  =====  ================  ==============================================================================================================================================

````
![](static/valor2-fora-frictionless.png)

![](static/valor2-fora.png)

* **Solução**: alterar os valores no arquivo do recurso, observando as características das variáveis indicadas no `datapackage.json`. Caso o problema tenha sido na descrição do dado, alterar as propriedades necessárias no `datapackage.json`


#### 2.2. valores fora dos campos delimitadores (, ou ;):
<a href="#top">(inicio)</a>

Na conversão de tipos de arquivo, texto ou valores de uma coluna podem 'vazar'. Observar se há sinais de pontuação (',', ';') que também funcionam como delimitadores, e podem redundar no erro:

````
$ frictionless validate datapackage.json
# -------
# invalid: datapackage.json
# -------
==========  ======================================
code        message
==========  ======================================
task-error  The task has an error: 'fieldPosition'
==========  ======================================
````

![](static/escaping-frictionless.png)

![](static/escaping.png)

* Solução: verificar se colunas de texto contêm ',' e/ou ';' que possam estar sendo interpretados como delimitadores de coluna. Delimitar os valores das colunas de texto em suas respsectivas células, utilizando aspas no editor de `csv`)


#### 2.3. Arquivo de dados sem encoding `utf-8` 
<a href="#top">(inicio)</a>

a. encoding Western, Latin, Windows...


b. encoding UTF-8 sem Byte Order Mask (BOM)

A assinatura 'Byte Order Mask (BOM)' preserva a visualiação adequada dos caracteres especiais, quando houve conversão de arquivo tabular do formato `excel` para `csv`. Quando ela não está aplicada, pode ocorrer o problema em outras ferramentas de edição de `csv`, como o `excel`:

![](static/bom.png)

![](static/bom-comparado.png)

* **Solução**: gerar o arquivo `csv` com o BOM. No editor de texto Sublime, 'Save with Encoding --> UTF-8 with BOM'. Via Python, usando o pacote 'Pandas', aplicar `read_file.to_csv (csv_file_path, encoding = 'utf-8-sig')`

- - - 

## Appendix - Lista de verificação Frictionless - 'Validation Checks'
<a href="#top">(inicio)</a>

````
['hash-count-error',
 'byte-count-error',
 'field-count-error',
 'row-count-error',
 'blank-header',
 'extra-label',
 'missing-label',
 'blank-label',
 'duplicate-label',
 'incorrect-label',
 'blank-row',
 'primary-key-error',
 'foreign-key-error',
 'extra-cell',
 'missing-cell',
 'type-error',
 'constraint-error',
 'unique-error']
 ````