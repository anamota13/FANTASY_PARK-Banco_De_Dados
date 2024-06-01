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
SELECT
    Nome,
    CPF,
    Rua,
    Numero,
    Cidade,
    Estado,
    CEP,
    Data_Nascimento,
    DATEDIFF(YEAR, Data_Nascimento, GETDATE()) AS Idade
FROM
    Visitante;
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
## 5. Inserção de Dados

Nessa etapa foi realizada a inserção dos dados em cada uma das tabelas, contendo no mínimo 20 dados inseridos.
```sql
INSERT INTO Visitante (Nome, CPF, Rua, Numero, Cidade, Estado, CEP, Data_Nascimento)
VALUES
    ('João da Silva', '123.456.789-00', 'Rua das Flores', '123', 'São Paulo', 'SP', '12345-678', '1990-05-15'),
    ('Maria Oliveira', '987.654.321-00', 'Rua dos Ipês', '456', 'Rio de Janeiro', 'RJ', '98765-432', '1985-10-20'),
    ('Pedro Santos', '456.789.123-00', 'Avenida Central', '789', 'Belo Horizonte', 'MG', '45678-912', '2000-03-25'),
    ('Ana Souza', '321.654.987-00', 'Rua das Palmeiras', '1011', 'Salvador', 'BA', '32165-498', '1978-08-05'),
    ('Carlos Pereira', '789.123.456-00', 'Rua dos Sabiás', '1213', 'Florianópolis', 'SC', '78912-345', '1995-12-30'),
    ('Fernanda Lima', '654.987.321-00', 'Avenida das Acácias', '1415', 'Brasília', 'DF', '65498-732', '1982-06-10'),
    ('Gabriel Costa', '234.567.890-00', 'Rua das Oliveiras', '1617', 'Porto Alegre', 'RS', '23456-789', '1998-01-05'),
    ('Juliana Oliveira', '876.543.210-00', 'Rua dos Girassóis', '1819', 'Recife', 'PE', '87654-321', '1973-09-12'),
    ('Lucas Mendes', '345.678.901-00', 'Avenida dos Jasmins', '2021', 'Fortaleza', 'CE', '34567-890', '1993-04-18'),
    ('Mariana Almeida', '542.210.987-00', 'Rua das Orquídeas', '2223', 'Goiânia', 'GO', '54321-098', '1980-11-08'),
    ('Rafael Silva', '654.321.098-00', 'Rua dos Cravos', '2627', 'Curitiba', 'PR', '65432-109', '1996-02-28'),
    ('Sandra Santos', '321.098.765-00', 'Rua das Rosas', '2829', 'Belém', 'PA', '32109-876', '1975-05-22'),
    ('Thiago Oliveira', '098.765.432-00', 'Avenida dos Pinheiros', '3031', 'Vitória', 'ES', '09876-543', '1991-09-15'),
    ('Vanessa Pereira', '876.543.211-00', 'Rua dos Antúrios', '3233', 'Natal', 'RN', '87654-321', '1984-03-07'),
    ('Amanda Souza', '765.432.109-00', 'Avenida das Violetas', '3435', 'Maceió', 'AL', '76543-210', '1989-12-11'),
    ('Bruno Costa', '543.210.987-00', 'Rua das Margaridas', '3637', 'Campo Grande', 'MS', '54321-098', '1977-08-24'),
    ('Carolina Lima', '432.109.876-00', 'Avenida dos Flamboyants', '3839', 'São Luís', 'MA', '43210-987', '1994-01-17'),
    ('Daniel Mendes', '210.987.654-00', 'Rua dos Lírios', '4041', 'Teresina', 'PI', '21098-765', '1986-06-03'),
    ('Elaine Almeida', '109.876.543-00', 'Avenida das Hortênsias', '4243', 'Cuiabá', 'MT', '10987-654', '1983-04-29')
    ('Fábio Santos', '210.123.456-78', 'Rua das Magnólias', '4546', 'Porto Velho', 'RO', '21012-345', '1998-11-03');
```
Consultando a Idade de apenas um visitante específico:
```sql
SELECT
    Nome,
    Data_Nascimento,
    DATEDIFF(YEAR, Data_Nascimento, GETDATE()) AS Idade
FROM
    Visitante
WHERE
    Nome = 'João da Silva';
```





