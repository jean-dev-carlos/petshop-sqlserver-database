USE [PetSytem .NET];

-- APAGAR TABELAS NA ORDEM CORRETA
DROP TABLE IF EXISTS Agendamentos;
DROP TABLE IF EXISTS Pets;
DROP TABLE IF EXISTS Servicos;
DROP TABLE IF EXISTS Tutores;

-- TABELA TUTORES
CREATE TABLE Tutores (
    IdTutor INT PRIMARY KEY IDENTITY(1,1), --ID único que se auto-incrementa (, 2, 3...)
    Nome VARCHAR(100) NOT NULL, --Nome Completo (obrigatório)
    CPF VARCHAR(14) UNIQUE NOT NULL, --CPF com máscara (ex: 000.000.000-00)
    Telefone VARCHAR(20) NOT NULL, --Telefone para contatos/recados
    Email VARCHAR(100), --Email (obrigatório)
    Endereco VARCHAR(250) --Endereço do cliente
);

-- TABELA PETS
CREATE TABLE Pets (
    IdPet INT PRIMARY KEY IDENTITY(1,1), --ID único do animal
    Nome VARCHAR(100) NOT NULL, --Nome do pet (Mel, Mimosa, etc.)
    Especie VARCHAR(50) NOT NULL, --Se é Cachorro, Gato, AVE, ETC.
    Raca VARCHAR(50), --Vira-lata, Poodle, Siamês, etc.
    Sexo CHAR(1)
        CHECK (Sexo IN ('M', 'F')), --Só aceita 'M' (Macho) ou 'F' (Fêmea)
    DataNascimento DATE, --Guarda apenas (dia/mês/ano)
    Peso DECIMAL(5,2), --Aceita apenas números como 12.50 (ate 999.99kg)
    Observacoes VARCHAR(MAX), --Texto livre para informações importantes

    IdTutor INT NOT NULL,

    CONSTRAINT FK_Pets_Tutores
        FOREIGN KEY (IdTutor)
        REFERENCES Tutores(IdTutor)
);

-- Criando Tabela de Serviços (Catálago de procedimentos e preços)
CREATE TABLE Servicos (
    IdServico INT PRIMARY KEY IDENTITY(1,1), --ID único do Serviço
    NomeServico VARCHAR(100) NOT NULL, --Ex: Consulta Geral, Banho, e Tosa, Vacina V10, etc.
    Descricao VARCHAR(250), -- Detalhes sobre o que inclui o serviço
    Preco DECIMAL(10,2) NOT NULL --Valor Cobrado (ex: 150,00$)
);

-- Criando a Tabela de Agendamentos (A Agenda PETSHOP) 
CREATE TABLE Agendamentos (

    IdAgendamento INT PRIMARY KEY IDENTITY(1,1), --ID único da consulta/serviço marcado
    IdPet INT NOT NULL,
    IdServico INT NOT NULL,
    DataAgendamento DATETIME NOT NULL, --Data e hora do compromisso (ex: 15/05/2026)
    StatusAgendamento VARCHAR(20)
        CHECK (StatusAgendamento IN
        ('Agendado', 'Concluido', 'Cancelado')),  

    Observacoes VARCHAR(250), --Aviso rápidos(ex: "Tutor vai trazer a propria ração?")

    CONSTRAINT FK_Agendamentos_Pets
        FOREIGN KEY (IdPet)
        REFERENCES Pets(IdPet),

    CONSTRAINT FK_Agendamentos_Servicos
        FOREIGN KEY (IdServico)
        REFERENCES Servicos(IdServico)
);

-- 1. Inserindo Tutores de teste (Juntei o Paranoá e Brasília em um campo só)
INSERT INTO Tutores (Nome, CPF, Telefone, Email, Endereco) VALUES
('Jean Carlos', '071.119.555-53', '(61) 99505-2524', 'jean@gmai.com', 'Paranoá, Brasília-DF'),
('Ana Clara', '024.546.369-56', '(61) 99525-2426', 'anaclara@gmail.com', 'Paranoá, Brasília-DF');
GO

-- 2. Inserindo os Pets (Adicionei a vírgula que faltava entre os registros)
INSERT INTO Pets (Nome, Especie, Raca, Sexo, DataNascimento, Peso, Observacoes, IdTutor) VALUES
('Mimosa', 'Cachorro', 'Vira-Lata', 'F', '2023-05-10', 32.50, 'Sem Alergia', 1), -- Vírgula aqui no final!
('Mel', 'Cachorro', 'Vira-Lata', 'F', '2023-05-10', 32.50, 'Alergia a sabao de coco', 2);
GO

-- 3. Inserindo os Serviços (Troquei as vírgulas dos preços por pontos)
INSERT INTO Servicos (NomeServico, Descricao, Preco) VALUES
('Consulta Geral', 'Avaliação Médica de rotina', 150.00),
('Banho e Tosa', 'Higienização completa e corte de pelagem', 100.00),
('Vacina V10', 'Imunização anal para cães', 120.00);
GO

-- 4. Criando os Agendamentos
INSERT INTO Agendamentos
(DataAgendamento, StatusAgendamento, Observacoes, IdPet, IdServico)
VALUES
('15-05-2026 14:30:00', 'Agendado',
'Tutor vai trazer a própria ração', 1, 2),

('20-07-2026 09:00:00', 'Agendado',
'Retorno da vacina', 2, 1);
GO

SELECT

    A.IdAgendamento,

    P.Nome AS Pet,

    S.NomeServico AS Servico,

    A.DataAgendamento,

    A.StatusAgendamento,

    T.Nome AS Tutor

FROM Agendamentos A

INNER JOIN Pets P
    ON A.IdPet = P.IdPet

INNER JOIN Tutores T
    ON P.IdTutor = T.IdTutor

INNER JOIN Servicos S
    ON A.IdServico = S.IdServico;

   
