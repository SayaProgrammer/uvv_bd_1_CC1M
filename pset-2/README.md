# PSET 2 - Criação de relatórios SQL baseados no modelo Elmarsi
Esta pasta contém todo o processo da realização das tarefas exigidas no PSET.

São Elas:
1 - Preparação de relatórios SQL
2 - Aprendizado de algebra relacional em linguagem SQL

# Como identificar as questões e processo de criação do PSET
Cada questão está devidamente separada e identificada com um comentário sobre si, o código e a tabela resultante do relatório

Exemplo: 

## Questão 1:
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua...

~~~SQL
SELECT funcionario.numero_departamento, AVG(funcionario.salario) as media_salarial
     FROM funcionario
     INNER JOIN departamento ON departamento.numero_departamento = funcionario.numero_departamento
     GROUP BY funcionario.numero_departamento; 
~~~

| numero_departamento | media_salarial |
:---------------------|----------------:
|                   1 |   55000.000000 |
|                   4 |   31000.000000 |
|                   5 |   33250.000000 |

A criação desta tabela e códigos foram feitos usando o Dbeaver com a linguagem sendo o MariaDB/MySQL

# Guia de documentos
Você pode se guiar pelos seguintes documentos presentes neste pset:

* README (este documento) --> Serve como um guia para este pset
* Questões Comentadas --> Lista com as questões, suas resoluções, códigos e tabelas resultantes. Também tem updates necessários para o outro pset.
