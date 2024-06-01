![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/472d7fff-6cfe-42b1-b2c5-55104a47633c)# FANTASY PARK: Gestão de Parque de Diversões

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
    FOREIGN KEY (ID_Parque) REFERENCES Parque(ID_Parque),
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

**Inserção na tabela Visitante**
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

**Inserção na tabela Funcionario**
```sql
INSERT INTO Funcionario (ID_Funcionario, Nome, CPF, Data_Contratacao, Cargo, Salario, ID_Parque)
VALUES
    (1, 'Ana Silva', '12345678901', '2023-01-10', 'Atendente', 2500.00, 1),
    (2, 'Carlos Oliveira', '23456789012', '2022-11-15', 'Gerente', 4500.00, 1),
    (3, 'Daniel Souza', '34567890123', '2023-03-20', 'Segurança', 2800.00, 2),
    (4, 'Elaine Santos', '45678901234', '2022-08-05', 'Limpeza', 2200.00, 2),
    (5, 'Fernanda Pereira', '56789012345', '2023-05-12', 'Atendente', 2500.00, 3),
    (6, 'Gabriel Lima', '67890123456', '2022-12-30', 'Técnico de Manutenção', 3200.00, 3),
    (7, 'Hugo Costa', '78901234567', '2023-02-18', 'Atendente', 2500.00, 4),
    (8, 'Isabela Ferreira', '89012345678', '2022-09-25', 'Vendedor de Ingressos', 2700.00, 4),
    (9, 'Juliana Almeida', '90123456789', '2023-06-08', 'Operador de Atrações', 2900.00, 5),
    (10, 'Kevin Ramos', '01234567890', '2022-10-11', 'Atendente', 2500.00, 5),
    (11, 'Lucas Oliveira', '98765432101', '2023-04-03', 'Gerente', 4500.00, 6),
    (12, 'Mariana Silva', '87654321012', '2022-07-19', 'Segurança', 2800.00, 6),
    (13, 'Natalia Costa', '76543210923', '2023-01-25', 'Limpeza', 2200.00, 7),
    (14, 'Otavio Pereira', '65432109834', '2022-11-28', 'Atendente', 2500.00, 7),
    (15, 'Pedro Santos', '54321098745', '2023-05-16', 'Técnico de Manutenção', 3200.00, 8),
    (16, 'Rafaela Oliveira', '43210987656', '2022-12-07', 'Atendente', 2500.00, 8),
    (17, 'Sandra Lima', '32109876567', '2023-02-22', 'Vendedor de Ingressos', 2700.00, 9),
    (18, 'Thiago Almeida', '21098765478', '2022-09-10', 'Operador de Atrações', 2900.00, 9),
    (19, 'Vanessa Costa', '10987654390', '2023-06-03', 'Atendente', 2500.00, 10),
    (20, 'Wagner Santos', '09876543201', '2022-10-26', 'Gerente', 4500.00, 10);
```

**Inserção na tabela Parque**
```sql
INSERT INTO Parque (ID_Parque, Nome, Rua, Numero, Cidade, Estado, Bairro, CEP, Telefone, Gerente)
VALUES
    (1, 'Parque da Aventura', 'Rua das Flores', '123', 'São Paulo', 'SP', 'Centro', '12345678', '11 12345678', 'Carlos Silva'),
    (2, 'Parque dos Sonhos', 'Avenida dos Sonhos', '456', 'Rio de Janeiro', 'RJ', 'Copacabana', '98765432', '21 98765432', 'Ana Oliveira'),
    (3, 'Parque da Diversão', 'Rua das Alegrias', '789', 'Belo Horizonte', 'MG', 'Santo Antônio', '45678912', '31 45678912', 'José Santos'),
    (4, 'Parque das Emoções', 'Avenida da Fantasia', '1011', 'Salvador', 'BA', 'Barra', '32165498', '71 32165498', 'Maria Costa'),
    (5, 'Parque da Alegria', 'Rua da Alegria', '1213', 'Florianópolis', 'SC', 'Centro', '78912345', '48 78912345', 'Paulo Oliveira'),
    (6, 'Parque Radical', 'Avenida da Aventura', '1415', 'Brasília', 'DF', 'Asa Sul', '65498732', '61 65498732', 'Carla Rodrigues'),
    (7, 'Parque do Encanto', 'Rua do Encanto', '1617', 'Porto Alegre', 'RS', 'Menino Deus', '23456789', '51 23456789', 'Rafaela Almeida'),
    (8, 'Parque dos Sonhos', 'Avenida dos Sonhos', '1819', 'Recife', 'PE', 'Boa Viagem', '87654321', '81 87654321', 'Fernando Silva'),
    (9, 'Parque da Aventura', 'Rua da Diversão', '2021', 'Fortaleza', 'CE', 'Aldeota', '34567890', '85 34567890', 'Amanda Costa'),
    (10, 'Parque do Prazer', 'Avenida do Prazer', '2223', 'Goiânia', 'GO', 'Setor Bueno', '54321098', '62 54321098', 'Gustavo Oliveira'),
    (11, 'Parque das Maravilhas', 'Rua das Maravilhas', '2425', 'Manaus', 'AM', 'Centro', '98765432', '92 98765432', 'Laura Mendes'),
    (12, 'Parque da Aventura', 'Avenida da Aventura', '2627', 'Curitiba', 'PR', 'Batel', '65432109', '41 65432109', 'Pedro Almeida'),
    (13, 'Parque Radical', 'Rua Radical', '2829', 'Belém', 'PA', 'Pedreira', '32109876', '91 32109876', 'Mariana Costa'),
    (14, 'Parque das Emoções', 'Avenida das Emoções', '3031', 'Vitória', 'ES', 'Jardim da Penha', '09876543', '27 98765432', 'Marcos Oliveira'),
    (15, 'Parque do Prazer', 'Rua do Prazer', '3233', 'Natal', 'RN', 'Ponta Negra', '87654321', '84 87654321', 'Juliana Santos'),
    (16, 'Parque da Aventura', 'Avenida da Aventura', '3435', 'Maceió', 'AL', 'Pajuçara', '76543210', '82 76543210', 'Lucas Oliveira'),
    (17, 'Parque dos Sonhos', 'Rua dos Sonhos', '3637', 'Campo Grande', 'MS', 'Centro', '54321098', '67 54321098', 'Isabela Fernandes'),
    (18, 'Parque Radical', 'Avenida Radical', '3839', 'São Luís', 'MA', 'Renascença', '43210987', '98 43210987', 'Matheus Almeida'),
    (19, 'Parque das Maravilhas', 'Rua das Maravilhas', '4041', 'Teresina', 'PI', 'Centro', '21098765', '86 21098765', 'Gabriela Santos'),
	(20, 'Parque do Prazer', 'Avenida do Prazer', '4243', 'Cuiabá', 'MT', 'Bosque da Saúde', '10987654', '65 10987654', 'Thiago Oliveira');
```

**Inserção na tabela Atracao**
```sql
INSERT INTO Atracao (ID_Atracao, Nome, Capacidade, Tempo_Duracao, ID_Parque)
VALUES
    (1, 'Montanha-Russa Radical', 20, 120, 1),
    (2, 'Carrossel dos Sonhos', 30, 10, 2),
    (3, 'Casa dos Horrores', 15, 15, 3),
    (4, 'Roda-Gigante do Encanto', 40, 15, 4),
    (5, 'Barco Pirata', 25, 8, 5),
    (6, 'Cinema 5D', 20, 20, 6),
    (7, 'Trem Fantasma', 30, 12, 7),
    (8, 'Bumper Cars', 20, 5, 8),
    (9, 'Canoa Selvagem', 15, 10, 9),
    (10, 'Carrinho de Bate-Bate', 20, 5, 10),
    (11, 'Teleférico do Prazer', 25, 10, 11),
    (12, 'Torre do Terror', 10, 15, 12),
    (13, 'Tiro ao Alvo', 20, 5, 13),
    (14, 'Barco Viking', 30, 8, 14),
    (15, 'Circuito Radical', 20, 30, 15),
    (16, 'Carrossel dos Sonhos', 25, 10, 16),
    (17, 'Montanha-Russa Maluca', 20, 90, 17),
    (18, 'Casa dos Espelhos', 15, 10, 18),
    (19, 'Balão dos Sonhos', 30, 15, 19),
    (20, 'Gira-Gira do Prazer', 15, 8, 20);
```

**Inserção na tabela Ingresso**
```sql
INSERT INTO Ingresso (ID_Ingresso, Tipo, Preco, Data_Compra, ID_Visitante, ID_Parque)
VALUES
    (1, 'Meia', 25.00, '2024-05-01', 54, 1),
    (2, 'Meia', 25.00, '2024-05-02', 60, 2),
    (3, 'Meia', 25.00, '2024-05-03', 42, 3),
    (4, 'Meia', 25.00, '2024-05-04', 61, 4),
    (5, 'Meia', 25.00, '2024-05-05', 59, 5),
    (6, 'Meia', 25.00, '2024-05-06', 48, 6),
    (7, 'Meia', 25.00, '2024-05-07', 53, 7),
    (8, 'Meia', 25.00, '2024-05-08', 45, 8),
    (9, 'Meia', 25.00, '2024-05-09', 50, 9),
    (10, 'Meia', 25.00, '2024-05-10', 58, 10),
    (11, 'Inteira', 50.00, '2024-05-11', 44, 11),
    (12, 'Inteira', 50.00, '2024-05-12', 51, 12),
    (13, 'Inteira', 50.00, '2024-05-13', 57, 13),
    (14, 'Inteira', 50.00, '2024-05-14', 52, 14),
    (15, 'Inteira', 50.00, '2024-05-15', 47, 15),
    (16, 'Inteira', 50.00, '2024-05-16', 56, 16),
    (17, 'Inteira', 50.00, '2024-05-17', 46, 17),
    (18, 'Inteira', 50.00, '2024-05-18', 49, 18),
    (19, 'Inteira', 50.00, '2024-05-19', 55, 19),
    (20, 'Inteira', 50.00, '2024-05-20', 43, 20);
```



