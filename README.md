# FANTASY PARK: Gestão de Parque de Diversões

## Descrição do Projeto

Esse projeto se baseia em uma avaliação para a criação de uma Modelagem de Banco de Dados em que alguns requisitos foram exigidos, tais como: 
1. Descrição do Cenário;
2. Modelagem Conceitual;
3. Modelagem Lógica;
4. Modelagem Física;
5. Inserção de Dados;
6. CRUD;
7. Relatórios: Criação de Consultas;

## 1. Descrição do Cenário
A empresa Fantasy Parks administra diversos parques de diversões espalhados pelo país, cada um oferecendo uma variedade de atrações e eventos únicos, além de vender ingressos e pacotes promocionais. Para proporcionar uma experiência personalizada e eficiente tanto para visitantes quanto para os administradores dos parques, é essencial implementar um sistema de banco de dados robusto. Este sistema busca gerenciar as informações de **visitantes** (ID_Visitante, Nome, CPF, Endereço, Data_Nascimento, Idade, Telefone, Email, ID_Funcionário), **funcionários** (ID_Funcionario, Nome, CPF, Data_Contratacao, Cargo, Salário, ID_Parque), **atrações** (ID_Atracao, Nome, Tipo, Capacidade, Tempo_Duracao, ID_Parque), e **ingressos** (ID_Ingresso, Tipo, Preço, Data_Compra, ID_Visitante, ID_Parque) de maneira integrada e eficiente.


### Entidades

### 1. Visitante
- **ID_Visitante**: Chave primária que identifica unicamente cada visitante. (Atributo Simples)
- **Nome**: Nome completo do visitante. (Atributo Simples)
- **CPF**: Cadastro de Pessoa Física do visitante. (Atributo Simples)
- **Endereço**: Endereço completo, incluindo Rua, Número, Cidade, Estado, e CEP. (Atributo Composto)
- **Data_Nascimento**: Data de nascimento do visitante. (Atributo Simples)
- **Idade**: Idade do visitante, calculada a partir da Data_Nascimento. (Atributo Derivado)
- **Telefones**: Lista de números de telefone do visitante, podendo haver mais de um. (Atributo Multivalorado)
- **Email**: Endereço de e-mail do visitante. (Atributo Multivalorado)

### 2. Funcionário
- **ID_Funcionario**: Chave primária que identifica unicamente cada funcionário. (Atributo Simples)
- **Nome**: Nome completo do funcionário. (Atributo Simples)
- **CPF**: Cadastro de Pessoa Física do funcionário. (Atributo Simples)
- **Data_Contratacao**: Data em que o funcionário foi contratado. (Atributo Simples)
- **Cargo**: Cargo ocupado pelo funcionário. (Atributo Simples)
- **Salário**: Salário do funcionário. (Atributo Simples)
- **ID_Parque**: Identificação do parque onde o funcionário trabalha. (Chave Estrangeira)

### 3. Parque
- **ID_Parque**: Chave primária que identifica unicamente cada parque. (Atributo Simples)
- **Nome**: Nome do parque de diversões. (Atributo Simples)
- **Endereço**: Endereço completo do parque, incluindo Rua, Número, Cidade, Estado, e CEP. (Atributo Composto)
- **Telefone**: Número de telefone de contato do parque. (Atributo Simples)
- **Gerente**: Nome do gerente responsável pelo parque. (Atributo Simples)

### 4. Atração
- **ID_Atracao**: Chave primária que identifica unicamente cada atração. (Atributo Simples)
- **Nome**: Nome da atração. (Atributo Simples)
- **Tipo**: Tipo da atração, como Montanha-Russa, Carrossel etc. (Atributo Simples)
- **Capacidade**: Capacidade máxima de pessoas que a atração pode acomodar. (Atributo Simples)
- **Tempo_Duracao**: Duração média da atração em minutos. (Atributo Simples)
- **ID_Parque**: Identificação do parque onde a atração está localizada. (Chave Estrangeira)

### 5. Ingresso
- **ID_Ingresso**: Chave primária que identifica unicamente cada ingresso. (Atributo Simples)
- **Tipo**: Tipo do ingresso, como Diário, Anual, VIP etc. (Atributo Simples)
- **Preço**: Preço do ingresso. (Atributo Simples)
- **Data_Compra**: Data em que o ingresso foi comprado. (Atributo Simples)
- **ID_Visitante**: Identificação do visitante que comprou o ingresso. (Chave Estrangeira)
- **ID_Parque**: Identificação do parque ao qual o ingresso dá acesso. (Chave Estrangeira)

## 2. Modelagem Conceitual

Veja abaixo a Modelagem Conceitual do projeto: 


## 3. Modelagem Lógica

Veja abaixo a Modelagem Lógica do projeto: 

## 4. Modelagem Física 

Para contruir a Modelagem Física do Fantasy Park foi criado o banco de dados:
``` sql
CREATE DATABASE Fantasy_Park
```

Agora, para que nosso banco fique estruturado, foi efetuado a criação das tabelas da seguinte maneira: 
``` sql
CREATE TABLE Visitante (
    ID_Visitante INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    CPF CHAR(14) NOT NULL UNIQUE,
    Rua VARCHAR(50) NOT NULL,
    Numero VARCHAR(10) NOT NULL,
    Cidade VARCHAR(50) NOT NULL,
    Estado CHAR(2) NOT NULL,
    CEP CHAR(9) NOT NULL,
    Data_Nascimento DATE NOT NULL,
);
```


