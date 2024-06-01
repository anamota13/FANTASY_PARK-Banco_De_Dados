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
    FOREIGN KEY (ID_Parque) REFERENCES Parque(ID_Parque),
);
```
**Criação da tabela Telefone_Funcionario**
```sql
CREATE TABLE Telefone_Funcionario (
    ID_Telefone INT PRIMARY KEY IDENTITY,
    ID_Funcionario INT,
    Telefone VARCHAR(20),
    FOREIGN KEY (ID_Funcionario) REFERENCES Funcionario(ID_Funcionario)
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

**Inserção dos e-mails na tabela Email_Visitante**
```sql
-- Inserindo os emails dos visitantes na tabela Email_Visitante
INSERT INTO Email_Visitante (ID_Visitante, Email) VALUES
(54, 'joaodasilva@example.com'), (54, 'joaodasilva2@example.com'),
(60, 'mariaoliveira@example.com'), (60, 'mariaoliveira2@example.com'),
(42, 'pedrosantos@example.com'), (42, 'pedrosantos2@example.com'),
(61, 'anasouza@example.com'), (61, 'anasouza2@example.com'),
(59, 'carlospereira@example.com'), (59, 'carlospereira2@example.com'),
(48, 'fernandalima@example.com'), (48, 'fernandalima2@example.com'),
(53, 'gabrielcosta@example.com'), (53, 'gabrielcosta2@example.com'),
(45, 'julianaoliveira@example.com'), (45, 'julianaoliveira2@example.com'),
(50, 'lucasmendes@example.com'), (50, 'lucasmendes2@example.com'),
(58, 'marianaalmeida@example.com'), (58, 'marianaalmeida2@example.com'),
(44, 'rafaelsilva@example.com'), (44, 'rafaelsilva2@example.com'),
(51, 'sandrasantos@example.com'), (51, 'sandrasantos2@example.com'),
(57, 'thiagooliveira@example.com'), (57, 'thiagooliveira2@example.com'),
(52, 'vanessapereira@example.com'), (52, 'vanessapereira2@example.com'),
(47, 'amandasouza@example.com'), (47, 'amandasouza2@example.com'),
(56, 'brunocosta@example.com'), (56, 'brunocosta2@example.com'),
(46, 'carolinalima@example.com'), (46, 'carolinalima2@example.com'),
(49, 'danielmendes@example.com'), (49, 'danielmendes2@example.com'),
(55, 'elainealmeida@example.com'), (55, 'elainealmeida2@example.com'),
(43, 'fabiosantos@example.com'), (43, 'fabiosantos2@example.com');
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

**Inserção na tabela Telefone_Funcionario**
```sql
INSERT INTO Telefone_Funcionario (ID_Funcionario, Telefone) VALUES
(1, '1699996666'), 
(1, '1699888888'), 
(2, '1199998888'), 
(2, '1199222222'), 
(3, '1199101010'), 
(3, '1199909090'), 
(4, '1199303030'), 
(4, '1199505050'), 
(5, '1199404040'), 
(5, '1199707070'), 
(6, '1199606060'), 
(6, '1199909090'), 
(7, '1199707070'), 
(7, '1199808080'), 
(8, '1199111111'), 
(8, '1199111112'), 
(9, '1199222223'), 
(9, '1199333334'), 
(10, '1199111111'), 
(10, '1199222222'), 
(11, '1199444445'), 
(11, '1199444444'), 
(12, '1199111111'), 
(12, '1199111112'), 
(13, '1199808080'), 
(13, '1199303030'), 
(14, '1199606060'), 
(14, '1199808080'), 
(15, '1199404040'), 
(15, '1199505050'), 
(16, '1199202020'), 
(16, '1199404040'), 
(17, '1199999999'), 
(17, '1199707070'), 
(18, '1199777777'), 
(18, '1199505050'), 
(19, '1199555555'), 
(19, '1199707070'), 
(20, '1199333333'), 
(20, '1199444444');
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
## 6. CRUD: CREATE, READ, UPDATE, DELETE

### CRUD: CREATE
Para utilizar a operação **CREATE** foi inserido um novo registro a partir do INSERT. Veja a criação de um novo registro na tabela Funcionario: 
```sql
INSERT INTO Funcionario (Nome, CPF, Data_Contratacao, Cargo, Salario, ID_Parque)
VALUES ('João da Silva', '123.456.789-00', '2024-06-01', 'Gerente de Operações', 5000.00, 1);
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/blob/main/CRUD/CREATE.png?raw=true)

### CRUD: READ
Para utilizar a operação **READ** foi feita a leitura de duas situações: 
1. Lendo o registro de todos os funcionários:
```sql
SELECT * FROM Funcionario;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/blob/main/CRUD/READ%201.png?raw=true)

2. Lendo o registro de apenas funcionário da área da Limpeza:
```sql
SELECT * FROM Funcionario WHERE Cargo = 'Limpeza';
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/blob/main/CRUD/READ%202.png?raw=true)

### CRUD: UPDATE
Para utilizar a operação **UPDATE** foi atualizado o salário do funcionário João da Silva: 
```sql
UPDATE Funcionario 
SET Salario = 6000.00 
WHERE Nome = 'João da Silva';
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/blob/main/CRUD/UPDATE.png?raw=true)

### CRUD: DELETE
Para utilizar a operação **DELETE** foi deletado o registro do funcionário João da Silva: 
```sql
DELETE FROM Funcionario 
WHERE Nome = 'João da Silva';
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/blob/main/CRUD/DELETE.png?raw=true)

Então, foram utilizadas todas as operações do CRUD, criando o registro do funcionário João da Silva, lendo todos os registros dos funcionários e dos que operam na área da limpeza, atualizando o salário do funcionário João da Silva e, por fim, deletando o registro dele. 

### 7. Relatórios
 Abaixo estão presentes, utilizando a Seleção, Filtro e Ordenação, 10 consultas que exibem os dados do Banco de Dados demonstrando a relação entre as tabelas.

 **Consulta 1: Listagem de todos os visitantes e seus respectivos parques**
 ```sql
SELECT Visitante.Nome AS Nome_Visitante, Parque.Nome AS Nome_Parque
FROM Visitante
JOIN Ingresso ON Visitante.ID_Visitante = Ingresso.ID_Visitante
JOIN Parque ON Ingresso.ID_Parque = Parque.ID_Parque;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/18cfe3b6-c429-4d48-991c-1d71a0af2733)

**Consulta 2: Listagem de todos os funcionários e seus respectivos parques**
```sql
SELECT Funcionario.Nome AS Nome_Funcionario, Parque.Nome AS Nome_Parque
FROM Funcionario 
JOIN Parque ON Funcionario.ID_Parque = Parque.ID_Parque;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/a76ebc12-5638-486a-b82b-ce5e5afcc1e5)

**Consulta 3: Listagem de todos os visitantes ordenados por idade**
```sql
SELECT Nome, Data_Nascimento, DATEDIFF(YEAR, Data_Nascimento, GETDATE()) AS Idade
FROM Visitante
ORDER BY Idade;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/65e6f94a-41e8-4096-8c18-0a2ce0a98a42)

**Consulta 4: Listagem de todos os visitantes que compraram ingressos do tipo "MEIA"**
```sql
SELECT Visitante.Nome AS Nome_Visitante, Ingresso.Tipo AS Tipo_Ingresso
FROM Visitante 
JOIN Ingresso ON Visitante.ID_Visitante = Ingresso.ID_Visitante
WHERE Ingresso.Tipo = 'Meia';
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/4abde78c-acc1-4342-b994-fe6529ce4e96)

**Consulta 5: Listagem de todos os visitantes que compraram ingressos do tipo "INTEIRA"**
´´´sql
SELECT Visitante.Nome AS Nome_Visitante, Ingresso.Tipo AS Tipo_Ingresso
FROM Visitante 
JOIN Ingresso ON Visitante.ID_Visitante = Ingresso.ID_Visitante
WHERE Ingresso.Tipo = 'Inteira';
´´´
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/9f471114-63c8-439f-b654-e8c8e30094ce)

**Consulta 6: Listagem de todos os funcionários com o cargo de Segurança em ordem alfabética**
```sql
SELECT Nome, Cargo
FROM Funcionario
WHERE Cargo = 'Segurança'
ORDER BY Nome;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/664c7cd1-4cf0-4542-ab73-a5cef2bc0a1b)

**Consulta 7: Listagem dos visitantes atendidos por cada funcionário e seus respectivos parques**
```sql
SELECT Funcionario.Nome AS Nome_Funcionario, Visitante.Nome AS Nome_Visitante, Parque.Nome AS Nome_Parque
FROM Funcionario
JOIN Visitante ON Funcionario.ID_Funcionario = Visitante.ID_Funcionario
JOIN Parque ON Funcionario.ID_Parque = Parque.ID_Parque;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/c8b1acae-afd8-443d-9d80-f00403fb685c)

**Consulta 8: Listagens dos funcionários que possuem maior tempo de "casa" em ordem decrescente**7
```sql
SELECT 
    Nome, 
    Data_Contratacao, 
    Cargo, 
    ID_Parque
FROM 
    Funcionario
ORDER BY 
    Data_Contratacao ASC;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/52b50ffc-8fe5-41ae-9274-3f8e40e9860a)

**Consulta 9: Listagem de número de visitantes atendidos por funcionário e nome do parque**
```sql
SELECT 
    Funcionario.Nome AS Nome_Funcionario,
    Parque.Nome AS Nome_Parque,
    COUNT(Visitante.ID_Visitante) AS Numero_Visitantes_Atendidos
FROM 
    Funcionario
LEFT JOIN 
    Parque ON Funcionario.ID_Parque = Parque.ID_Parque
LEFT JOIN 
    Visitante ON Funcionario.ID_Funcionario = Visitante.ID_Funcionario
GROUP BY 
    Funcionario.Nome, Parque.Nome
ORDER BY 
    Numero_Visitantes_Atendidos DESC;
```
![Consulta 9(P1)](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/c3fb14c2-e3d4-4e07-a16d-878929640de7)
![Consulta 9(P2)](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/d199c699-0da7-4e8b-aec4-4741140f22c1)

**Consulta 10: Detalhes de Visitantes, Ingressos, Funcionários e Parques**
OBS:A consulta vai mostrar o nome do visitante, o tipo de ingresso que ele comprou, o nome do funcionário que o atendeu, e o nome do parque onde tudo isso ocorreu.
```sql
SELECT 
    Visitante.Nome AS Nome_Visitante,
    Ingresso.Tipo AS Tipo_Ingresso,
    Funcionario.Nome AS Nome_Funcionario,
    Parque.Nome AS Nome_Parque,
    Ingresso.Data_Compra,
    Atração.Nome AS Nome_Atracao,
    Atração.Capacidade,
    Atração.Tempo_Duracao
FROM 
    Visitante
JOIN 
    Ingresso ON Visitante.ID_Visitante = Ingresso.ID_Visitante
JOIN 
    Funcionario ON Visitante.ID_Funcionario = Funcionario.ID_Funcionario
JOIN 
    Parque ON Ingresso.ID_Parque = Parque.ID_Parque
JOIN 
    Atração ON Parque.ID_Parque = Atração.ID_Parque
ORDER BY 
    Visitante.Nome, Ingresso.Data_Compra;
```
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/eb5d83f5-634e-40d8-a095-f951b5bc2b11)
![image](https://github.com/anamota13/FANTASY_PARK-Banco_De_Dados/assets/110187484/c92160b7-7fc2-47a6-9074-9e72561b610b)


