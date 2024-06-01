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

**Criação da tabela Visitante**
``` sql
USE Fantasy_Park;
CREATE TABLE Visitante (
    ID_Visitante INT IDENTITY(1,1) PRIMARY KEY,
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

O próximo passo foi criar as tabelas dos atributos multivalorados, por exemplo, Telefone e E-mail: 

**Telefone_Visitante:**
``` sql
CREATE TABLE Telefone_Visitante (
    ID_Telefone INT IDENTITY(1,1) PRIMARY KEY,
    ID_Visitante INT,
    Telefone VARCHAR(11),
    FOREIGN KEY (ID_Visitante) REFERENCES Visitante(ID_Visitante)
);
```

**Email_Visitante:**
```sql
CREATE TABLE Email_Visitante (
    ID_Email INT IDENTITY(1,1) PRIMARY KEY,
    ID_Visitante INT,
    Email VARCHAR(60),
    FOREIGN KEY (ID_Visitante) REFERENCES Visitante(ID_Visitante)
);
```

Para concluir a criação da estrutura da tabela Visitante, foi calculado o atributo Idade (Atributo Derivado): 
``` sql
CREATE VIEW VisitanteComIdade AS
SELECT
    ID_Visitante,
    Data_Nascimento,
    CASE 
        WHEN MONTH(Data_Nascimento) < MONTH(GETDATE()) 
            OR (MONTH(Data_Nascimento) = MONTH(GETDATE()) AND DAY(Data_Nascimento) <= DAY(GETDATE())) 
        THEN DATEDIFF(YEAR, Data_Nascimento, GETDATE()) 
        ELSE DATEDIFF(YEAR, Data_Nascimento, GETDATE()) - 1 
    END AS Idade
FROM Visitante;
```

**Criação da tabela Funcionário**
``` sql
CREATE TABLE Funcionario (
    ID_Funcionario INT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    CPF CHAR(14) NOT NULL UNIQUE,
    Data_Contratacao DATE NOT NULL,
    Cargo VARCHAR(30) NOT NULL,
    Salario DECIMAL(10, 2) NOT NULL,
    ID_Parque INT,
    ID_Funcionario_Atendimento INT,
    FOREIGN KEY (ID_Parque) REFERENCES Parque(ID_Parque),
    FOREIGN KEY (ID_Funcionario_Atendimento) REFERENCES Funcionario(ID_Funcionario)
);
```

**Criação da tabela Parque**
``` sql
CREATE TABLE Parque (
    ID_Parque INT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Rua VARCHAR(50) NOT NULL,
    Numero VARCHAR(10) NOT NULL,
    Cidade VARCHAR(30) NOT NULL,
    Estado CHAR(2) NOT NULL,
    Bairro CHAR(15) NOT NULL,
    CEP CHAR(8) NOT NULL,
    Telefone VARCHAR(11) NOT NULL,
    Gerente VARCHAR(100) NOT NULL
);
```

**Criação da tabela Atração**
``` sql
CREATE TABLE Atracao (
    ID_Atracao INT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Capacidade INT NOT NULL,
    Tempo_Duracao INT NOT NULL,
    ID_Parque INT,
    FOREIGN KEY (ID_Parque) REFERENCES Parque(ID_Parque)
);
```

**Criação da tabela Ingresso**
``` sql
CREATE TABLE Ingresso (
    ID_Ingresso INT PRIMARY KEY,
    Tipo VARCHAR(50) NOT NULL,
    Preco DECIMAL(10, 2) NOT NULL,
    Data_Compra DATE NOT NULL,
    ID_Visitante INT,
    ID_Parque INT,
    FOREIGN KEY (ID_Visitante) REFERENCES Visitante(ID_Visitante),
    FOREIGN KEY (ID_Parque) REFERENCES Parque(ID_Parque)
);
```





