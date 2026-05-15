PetSystemDB - Sistema de Gerenciamento para Clínicas Veterinárias e PetShops
Este projeto consiste na modelagem e desenvolvimento da camada de persistência e backend de um sistema robusto para gestão de clínicas veterinárias e petshops. O objetivo principal é gerenciar com eficiência o fluxo de clientes (tutores), pacientes (pets) e o controle da agenda de serviços.

🚀 Tecnologias Utilizadas
Banco de Dados: SQL Server

Linguagem Backend: C# (.NET) — Em desenvolvimento

IDE: SQL Server Management Studio (SSMS) & Visual Studio Community

🧠 Arquitetura e Modelagem do Banco de Dados
A fundação do sistema foi estruturada no SQL Server utilizando boas práticas de modelagem relacional, garantindo integridade e performance dos dados.

Componentes Principais:
Relacionamento 1:N (Um para Muitos): Um tutor pode possuir múltiplos pets cadastrados, vinculados via Chave Estrangeira (FOREIGN KEY), o que evita a redundância de dados.

Tabela de Agendamentos (Coração do Sistema): Uma tabela central que cruza as informações de Pets e Serviços com controle de data/hora e status dinâmico.

Integridade de Dados: Uso de restrições nativas como NOT NULL, UNIQUE para controle de CPFs, e cláusulas CHECK para validação de campos.

⚡ Recursos Avançados Implementados:
Stored Procedures: Criação da procedure sp_AgendaDoDia para otimizar o tempo de resposta do sistema ao gerar o relatório diário de atendimentos para a equipe médica.

Consultas Avançadas: Uso de múltiplos INNER JOINs para consolidação e cruzamento de relatórios gerenciais.

🛠️ Como Executar o Script SQL
Abra o SQL Server Management Studio (SSMS).

Crie um novo banco de dados ou execute o arquivo 01_estrutura_e_dados_iniciais.sql.

Para testar a listagem automática da agenda, execute o comando:

SQL
EXEC sp_AgendaDoDia;
📅 Próximos Passos (Roadmap)
[x] Modelagem do Banco de Dados Relacional (SQL Server)

[x] Criação de massa de dados para testes (INSERTs)

[x] Implementação de Stored Procedure para a agenda do dia

[ ] Desenvolvimento das classes de Entidade em C# (Backend)

[ ] Conexão do C# com o SQL Server (Camada de Acesso a Dado
