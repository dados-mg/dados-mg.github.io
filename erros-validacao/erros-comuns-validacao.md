# Erros comuns de validação

## 1. Sobre os arquivos de metadados e seu conteúdo

#### 1.1. Sintaxe do `datapackage.json`:


i. faltou abrir ou fechar algum campo com aspas, "", colchetes '[ ]' ou chaves '{ }'

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

![](static/sintaxe2-datapackage.png)

[Relatório online Frictionless via Github Actions](https://repository.frictionlessdata.io/report/?user=dados-mg&repo=doacoes-comodatos-amigo-estado-mg&flow=frictionless&run=1963653699)


ii. faltou/sobrou alguma vírgula

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

![](static/sintaxe-datapackage.png)

[Relatório online Frictionless via Github Actions](https://repository.frictionlessdata.io/report/?user=dados-mg&repo=doacoes-comodatos-amigo-estado-mg&flow=frictionless&run=1963648000)


#### 1.2. Nome `name` do recurso contém caracteres fora da faixa permitida

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


#### 1.3. O caminho `path` incorreto

i. onde se localizam os arquivos de dados:

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

![](static/path.png)

[Relatório online Frictionless via Github Actions](https://repository.frictionlessdata.io/report/?user=dados-mg&repo=doacoes-comodatos-amigo-estado-mg&flow=frictionless&run=1963749335)

ii. onde se localizam os arquivos de metadados `datapackage.json`, o `schema.json`, ou o `dialect.json`

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

![](static/path-schema.png)

[Relatório online Frictionless via Github Actions](https://repository.frictionlessdata.io/report/?user=dados-mg&repo=doacoes-comodatos-amigo-estado-mg&flow=frictionless&run=1963769051)


#### 4. Datapackage sem a propriedade `owner_org` obrigatória ou grafada incorretamente ( hífen'-' em vez de underline "_")



## 2. Sobre os arquivos de dados e seu conteúdo

#### 1.1. Divergências de características dos dados no `datapackage.json`

i. formatos de data

ii. dado obrigatório ausente

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

![](static/dado-ausente.png)


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

[Relatório online Frictionless via Github Actions](https://repository.frictionlessdata.io/report/?user=dados-mg&repo=doacoes-comodatos-amigo-estado-mg&flow=frictionless&run=1719709178)


iii. valor numérico não inteiro ou sem a correta caracterização no datapackage.json



iv. número decimal/porcentagem (string X number)



v. valor fora das características informadas

  a. type-error

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

![](static/valor-fora.png)

[Relatório online Frictionless via Github Actions](https://repository.frictionlessdata.io/report/?user=dados-mg&repo=doacoes-comodatos-amigo-estado-mg&flow=frictionless&run=1964338865)

  
  b. constrint-error

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
![](static/valor2-fora.png)

[Relatório online Frictionless via Github Actions](https://repository.frictionlessdata.io/report/?user=dados-mg&repo=doacoes-comodatos-amigo-estado-mg&flow=frictionless&run=1964359181)


#### 2.1. Arquivo de dados sem encoding `utf-8` 




- - - 

## Lista de verificação Frictionless - 'Validation Checks'

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