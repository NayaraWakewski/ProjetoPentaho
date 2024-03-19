# Data Warehouse - PROJETO PENTAHO - UNIDADE 2.

Prof. Alex Sousa.

## 🚀 Visão Geral

Este projeto envolve a construção de um Data Warehouse (DW) para a Base de Dados Corporativa, utilizada na Unidade 2 do Curso de Data Analytics. Foi utilizado o Pentaho Data Integration (PDI) para processos ETL (Extract, Transform, Load) e o VSCode com a biblioteca psycopg2 para consultas SQL no banco de dados. O projeto visa criar um Data Warehouse, um Job para execução da Transformação e um Agendamento deste Job, via agendador de tarefas do Windows.
Toda a documentação consta no arquivo ipynb em anexo neste repositório.

### 🚀 Análise Geral

Inicialmente foi feita uma análise geral da base de dados de produção.
A análise foi realizada dentro de um jupyter notebook, utilizando o VS Code.

## 🚀 Estrutura do Projeto

O DW é modelado usando um esquema estrela, com tabelas de fato e dimensão projetadas para otimizar consultas analíticas. As principais tabelas incluem:

##### Dimensões e Fatos

##### Banco de Dados `corporativo_dw` e schema `public`

**Dimensão Estado - dim_estado** 
- `id_estado`: SERIAL PRIMARY KEY
- `sigla`: TEXT
- `descricao`: TEXT

**Dimensão Cidade - dim_cidade** 
- `id_cidade`: SERIAL PRIMARY KEY
- `id_estado`: INTEGER, FOREIGN KEY REFERENCES `dim_estado(id_estado)`
- `descricao`: TEXT

**Dimensão Bairro - dim_bairro** 
- `id_bairro`: SERIAL PRIMARY KEY
- `id_cidade`: INTEGER, FOREIGN KEY REFERENCES `dim_cidade(id_cidade)`
- `descricao`: TEXT

**Dimensão Endereço - dim_endereco** 
- `id_endereco`: SERIAL PRIMARY KEY
- `id_bairro`: INTEGER, FOREING KEY REFERENCES  `dim_bairro(bairro)`
- `rua`: TEXT
- `numero`: TEXT
- `cep`: TEXT
- `complemento`: TEXT
- `id_cidade`: INTEGER, FOREIGN KEY REFERENCES `dim_cidade(id_cidade)` -- Alteração posterior
- `id_estado`: INTEGER, FOREIGN KEY REFERENCES `dim_estado(id_estado)` -- Alteração posterior

**Dimensão Contato**
- `id_contato`: SERIAL PRIMARY KEY
- `descricao`: TEXT
- `principal`: BOOLEAN
- `sigla`: TEXT
- `valor`: TEXT
- `data_cadastro`: TIMESTAMP WITH TIME ZONE
- *Associar a `pessoa_fisica_id` ou `pessoa_juridica_id` conforme necessário.*

**Dimensão Pessoa Física**
- `pessoa_fisica_id`: SERIAL PRIMARY KEY
- `nome`: TEXT
- `cpf`: TEXT UNIQUE
- `nascimento`: TIMESTAMP WITH TIME ZONE
- `id_contato`: TIMESTAMP WITH TIME ZONE
- `endereco_id`: INTEGER, FOREIGN KEY REFERENCES `dim_endereco(endereco_id)`

**Dimensão Pessoa Jurídica**
- `pessoa_juridica_id`: SERIAL PRIMARY KEY
- `razao_social`: TEXT
- `cnpj`: TEXT UNIQUE
- `id_contato`: TIMESTAMP WITH TIME ZONE
- `endereco_id`: INTEGER, FOREIGN KEY REFERENCES `dim_endereco(endereco_id)`

**Dimensão Cargo - dim_cargo** 
- `id_cargo`: SERIAL PRIMARY KEY
- `descricao`: TEXT

**Dimensão Departamento - dim_departamento** 
- `id_departamento`: SERIAL PRIMARY KEY
- `descricao`: TEXT
- `sigla`: TEXT

**Dimensão Escolaridade - dim_escolaridade** 
- `id_escolaridade`: SERIAL PRIMARY KEY
- `descricao`: TEXT
- `codigo`: TEXT

**Dimensão Funcionário - dim_funcionario** 
- `id_funcionario`: INTEGER PRIMARY KEY
- `id_pessoa`: INTERGER
- `id_escolaridade`: INTEGER, FOREIGN KEY REFERENCES `dim_escolaridade(id_escolaridade)`
- `pretensao_salarial`: NUMERIC
- `nome`: TEXT
- `pcd`: BOOLEAN
- `id_cargo`: INTEGER, FOREIGN KEY REFERENCES `dim_cargo(id_cargo)`
- `id_departamento`: INTEGER, FOREIGN KEY REFERENCES `dim_departamento(id_departamento)`
- `data_cadastro`: TIMESTAMP WITH TIME ZONE
- `data_desligamento`: TIMESTAMP WITH TIME ZONE
- `salario`: NUMERIC

**Dimensão Categoria - dim_categoria** 
- `id_categoria`: SERIAL PRIMARY KEY
- `descricao`: TEXT

**Dimensão Forma Pagamento - dim_forma_pagamento** 
- `id_forma_pagamento`: SERIAL PRIMARY KEY
- `descricao`: TEXT

**Dimensão Produto - dim_produto**  
- `id_produto`: SERIAL PRIMARY KEY
- `id_fornecedor`: INTEGER -- não existe tabela fornecedor então não tem fk.
- `id_categoria`: INTEGER, FOREIGN KEY REFERENCES `dim_categoria(id_categoria)`
- `nome`: TEXT
- `valor_venda`: NUMERIC
- `valor_custo`: NUMERIC

**Fato Vendas - fato_vendas** 
- `id_produto`: INTEGER, FOREIGN KEY REFERENCES `dim_produto(produto_id)`
- `id_nota_fiscal`: INTEGER
- `quantidade`: INTEGER
- `valor_venda_real`: NUMERIC
- `id_funcionario`: INTEGER, FOREIGN KEY REFERENCES `dim_funcionario(id_funcionario)`
- `id_pessoa_juridica`: INTEGER, FOREIGN KEY REFERENCES `dim_pessoa_juridica(id_pessoa_juridica)`
- `id_forma_pagamento`: INTEGER, FOREIGN KEY REFERENCES `dim_forma_pagamento(id_forma_pagamento)`
- `data_venda`: TIMESTAMP WITH TIME ZONE
- `numero_nf`: TEXT
- `total`: NUMERIC

## 🚀 Processo ETL

O processo ETL é realizado através do PDI, envolvendo as seguintes etapas:

1. **Extração**: Dados são extraídos do Banco de Dados `corporativo_db`
2. **Transformação**: Não foram feitas etapas de transformação, apenas modelagem.
3. **Carga**: Dados transformados são carregados no DW nas tabelas de fato e dimensão apropriadas.

## 🚀 JOB

Foi feito um Job para execução da transformação e caso tivesse retorno de SUCESSO, enviar um e-mail para o Professor da Unidade.

Nessa etapa de configuração do e-mail, tiveram alguns erros de configuração do arquivo de certificação do smpt. do Gmail no Java, sendo necessário baixar esse arquivo e fazer a configuração dentro da pasta de segurança do Java.

## 🚀 Agendamento de Execução do JOB

Foi feito um agendamento de execução do JOB no dia 19/03/2024 as 12:00, através do agendador de tarefas do Windows. O qual será enviado um email para o Professor da Unidade como teste do Agendamento.

## 🚀 Tecnologias Utilizadas

- VsCode (Jupyter Notebook)
- Pentaho Data Integration (PDI)
- PostgreSQL
- SQL
- Python

  ## 🎁 Expressões de gratidão

* Compartilhe com outras pessoas esse projeto 📢;
* Quer saber mais sobre o projeto? Entre em contato para tomarmos um :coffee:;

---
⌨️ com ❤️ por [Nayara Vakevskii](https://github.com/NayaraWakewski) 😊
