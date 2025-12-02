Atividade Prática 4 – Modelagem e SQL

1. Fundamentos e Preparação do Ambiente

Nesta etapa foi feita a preparação completa do ambiente para o desenvolvimento do banco de dados PetLar.
Foram revisados conceitos essenciais:

A linguagem SQL é usada exclusivamente para criar, consultar, modificar e excluir dados.

Linguagens como Python, Java, C# e JavaScript implementam lógica, aplicativos e interfaces.

O ambiente utilizado foi MySQL Workbench, onde foram realizados:

Criação do banco petlar;

Configuração das tabelas conforme o modelo lógico;

Definição de tipos de dados adequados (VARCHAR, INT, DATE…);

Aplicação de chaves primárias, estrangeiras e demais regras de integridade.

Essa base garantiu um ambiente estruturado para iniciar a manipulação de dados.

📗 2. Implementação e Manipulação de Dados

A segunda etapa envolveu o uso direto da SQL, aplicando comandos DDL e DML para manipulação e consulta de dados no sistema PetLar.

2.1 Criação das Tabelas – create_tables.sql
CREATE DATABASE IF NOT EXISTS petlar;
USE petlar;

-- Tabela ONG
CREATE TABLE ONG (
    id_ong INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cnpj VARCHAR(18) UNIQUE NOT NULL,
    telefone VARCHAR(20),
    cidade VARCHAR(50)
);

-- Tabela Adotante
CREATE TABLE Adotante (
    id_adotante INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    telefone VARCHAR(20),
    cidade VARCHAR(50)
);

-- Tabela Animal
CREATE TABLE Animal (
    id_animal INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(80) NOT NULL,
    especie VARCHAR(50) NOT NULL,
    idade INT,
    status_adocao VARCHAR(20) DEFAULT 'Disponível',
    id_ong INT NOT NULL,
    FOREIGN KEY (id_ong) REFERENCES ONG(id_ong)
);

-- Tabela Adoção
CREATE TABLE Adocao (
    id_adocao INT AUTO_INCREMENT PRIMARY KEY,
    data_adocao DATE NOT NULL,
    id_animal INT NOT NULL,
    id_adotante INT NOT NULL,
    FOREIGN KEY (id_animal) REFERENCES Animal(id_animal),
    FOREIGN KEY (id_adotante) REFERENCES Adotante(id_adotante)
);

2.2 Inserção de Dados – insert_data.sql
-- Inserir ONGs
INSERT INTO ONG (nome, cnpj, telefone, cidade) VALUES
('Amigos dos Animais', '12.345.678/0001-90', '11988887777', 'São Paulo'),
('Patinhas Felizes', '98.765.432/0001-10', '21999996666', 'Rio de Janeiro');

-- Inserir Adotantes
INSERT INTO Adotante (nome, email, telefone, cidade) VALUES
('Maria Silva', 'maria@gmail.com', '11911112222', 'São Paulo'),
('João Pereira', 'joao@gmail.com', '21922223333', 'Rio de Janeiro'),
('Ana Costa', 'ana@gmail.com', '31933334444', 'Belo Horizonte');

-- Inserir Animais
INSERT INTO Animal (nome, especie, idade, status_adocao, id_ong) VALUES
('Bidu', 'Cachorro', 3, 'Disponível', 1),
('Mia', 'Gato', 2, 'Disponível', 1),
('Thor', 'Cachorro', 5, 'Disponível', 2),
('Luna', 'Gato', 1, 'Adotado', 2);

-- Inserir Adoção
INSERT INTO Adocao (data_adocao, id_animal, id_adotante) VALUES
('2024-10-10', 4, 2);

2.3 Consultas SQL – select_queries.sql
-- 1. Selecionar todos os animais disponíveis
SELECT nome, especie, idade
FROM Animal
WHERE status_adocao = 'Disponível';

-- 2. Animais ordenados por idade
SELECT nome, especie, idade
FROM Animal
ORDER BY idade DESC;

-- 3. Limitar resultados
SELECT nome, especie
FROM Animal
LIMIT 2;

-- 4. JOIN: ver adoções completas
SELECT A.nome AS Animal, AD.nome AS Adotante, O.nome AS ONG, AC.data_adocao
FROM Adocao AC
JOIN Animal A ON A.id_animal = AC.id_animal
JOIN Adotante AD ON AD.id_adotante = AC.id_adotante
JOIN ONG O ON O.id_ong = A.id_ong;

-- 5. Animais de uma ONG específica
SELECT A.nome, A.especie, O.nome AS ONG
FROM Animal A
JOIN ONG O ON O.id_ong = A.id_ong
WHERE O.nome = 'Amigos dos Animais';

2.4 Atualização e Remoção – update_delete.sql
-- UPDATE 1: Alterar status de adoção
UPDATE Animal
SET status_adocao = 'Adotado'
WHERE id_animal = 1;

-- UPDATE 2: Atualizar telefone do adotante
UPDATE Adotante
SET telefone = '11955556666'
WHERE id_adotante = 1;

-- UPDATE 3: Alterar cidade da ONG
UPDATE ONG
SET cidade = 'Campinas'
WHERE id_ong = 1;

-- DELETE 1: Remover adoção (exemplo seguro)
DELETE FROM Adocao
WHERE id_adocao = 1;

-- DELETE 2: Remover um animal sem adoção registrada
DELETE FROM Animal
WHERE id_animal = 3;

-- DELETE 3: Remover adotante sem adoções
DELETE FROM Adotante
WHERE id_adotante = 3;

🧾 3. Considerações Finais

A atividade consolidou o uso da linguagem SQL em um projeto real, integrando modelagem lógica, criação de tabelas, manipulação de dados e consultas avançadas.
O banco PetLar foi totalmente implementado, incluindo:

Estrutura relacional completa;

Chaves e integridade referencial aplicadas;

Dados inseridos e manipulados corretamente;

Consultas SQL com JOIN, filtros, ordenações e limites;

Atualizações e exclusões controladas;

Versionamento do projeto no GitHub.

O projeto demonstra domínio das operações fundamentais de um banco de dados relacional e aplicação prática de SQL em um cenário realista.
