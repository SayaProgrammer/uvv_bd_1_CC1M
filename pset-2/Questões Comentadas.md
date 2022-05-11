# Correções do último PSET

Aqui, antes de tudo, tenho que deixar 2 códigos para atualizar as tabelas do meu PSET 1 para que os relatórios produzidos saiam com os dados certos.

UPDATE 1: 
~~~SQL
  UPDATE departamento SET nome_departamento = 'Matriz' where nome_departamento = 'Matris';
~~~

UPDATE 2:
~~~SQL
  UPDATE trabalha_em SET horas = '0' where horas = NULL;
~~~

Estes updates já foram aplicados ao PSET 1, mas quis deixar aqui caso você não tenha atualizado o seu -w-/

# Questão 1:

A questão 1 pedia um relatório com a média salarial de cada departamento (unindo seus funcionários e tirando a média). 

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* AVG: Comando usado para tirar a média dos salários.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Neste caso, da tabela funcionario.
* INNER JOIN: Usado para unir a tabela departamento à tabela funcionário.
* GROUP BY: Por fim, usei um group by para separar a média de cada departamento.

~~~SQL
SELECT funcionario.numero_departamento, AVG(funcionario.salario) as media_salarial
     FROM funcionario
     INNER JOIN departamento on departamento.numero_departamento = funcionario.numero_departamento
     GROUP BY funcionario.numero_departamento; 
~~~

| numero_departamento | media_salarial |
:---------------------|----------------:
|                   1 |   55000.000000 |
|                   4 |   31000.000000 |
|                   5 |   33250.000000 |

# Questão 2:

A questão 2 solicitava a média salarial divida entre os homens e as mulheres.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* AVG: Comando usado para tirar a média
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Aqui usado para pegar a tabela funcionário.
* GROUP BY: No final vem um group by para separar a média entre os sexos dos funcionários.

~~~SQL
  SELECT funcionario.sexo, AVG(funcionario.salario) as media_salarial
     FROM funcionario
     GROUP BY funcionario.sexo;
~~~
| sexo | media_salarial |
:------|----------------:
| F    |   31000.000000 |
| M    |   37600.000000 |

# Questão 3:

A questão 3 pedia a preparação de um relatório que listava os departamentos pelo nome e os dados (nome completo, data de nascimento, idade e o salário)

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Usei aqui pelas tabelas departamento e funcionario.
* WHERE: Usei para estabelecer a condição de que o número do departamento na tabela departamento deveria ser igual ao do funcionário na tabela funcionario.

~~~SQL
  SELECT nome_departamento, CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) as nome_completo, 
  data_nascimento, timestampdiff(year, data_nascimento, now()) as idade, funcionario.salario
     FROM departamento, funcionario
     WHERE departamento.numero_departamento = funcionario.numero_departamento;
~~~

| nome_departamento | nome_completo    | data_nascimento | idade | salario  |
:-------------------|------------------|-----------------|-------|----------:
| Pesquisa          | João B Silva     | 1965-01-09      |    57 | 30000.00 |
| Pesquisa          | Fernando T Wong  | 1955-12-08      |    66 | 40000.00 |
| Pesquisa          | Joice A Leite    | 1972-07-31      |    49 | 25000.00 |
| Pesquisa          | Ronaldo K Lima   | 1962-09-15      |    59 | 38000.00 |
| Matriz            | Jorge E Brito    | 1937-11-10      |    84 | 55000.00 |
| Administração     | Jennifer S Souza | 1941-06-20      |    80 | 43000.00 |
| Administração     | André V Pereira  | 1969-03-29      |    53 | 25000.00 |
| Administração     | Alice J Zelaya   | 1968-01-19      |    54 | 25000.00 |

# Questão 4:

A questão 4 pedia um relatório com os dados dos funcionários e uma aplicação de reajuste salarial ao seus salários.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* CASE/ELSE/END: Uma série de comandos que usei para criar a nova coluna de "reajuste_salarial".
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Usei aqui para puxar a tabela funcionário.


~~~SQL
  SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) as nome_completo, data_nascimento, 
  timestampdiff(year, data_nascimento, now()) as idade, salario,
     CASE
     WHEN salario < 35000 then salario * 1.2
     ELSE salario * 1.15
     END as reajuste_salarial
     FROM funcionario;
~~~

| nome_completo    | data_nascimento | idade | salario  | reajuste_salarial |
:------------------|-----------------|-------|----------|-------------------:
| João B Silva     | 1965-01-09      |    57 | 30000.00 |        36000.0000 |
| Fernando T Wong  | 1955-12-08      |    66 | 40000.00 |        46000.0000 |
| Joice A Leite    | 1972-07-31      |    49 | 25000.00 |        30000.0000 |
| Ronaldo K Lima   | 1962-09-15      |    59 | 38000.00 |        43700.0000 |
| Jorge E Brito    | 1937-11-10      |    84 | 55000.00 |        63250.0000 |
| Jennifer S Souza | 1941-06-20      |    80 | 43000.00 |        49450.0000 |
| André V Pereira  | 1969-03-29      |    53 | 25000.00 |        30000.0000 |
| Alice J Zelaya   | 1968-01-19      |    54 | 25000.00 |        30000.0000 |

# Questão 5:

A questão 5 pedia um relatório que listava os departamentos, seus respectivos gerentes e funcionários e com seus dados, ordenando tudo pelo nome do departamento (forma crescente) e pelo salário dos funcionários (forma decrescente).

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* CASE/ELSE/END: Usei aqui para criar a coluna "cargos", separando assim quem era gerente e quem era funcionário.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Aqui foi a tabela departamento e funcionario.
* WHERE: Estabelece uma condição para puxar somente as linhas que correspondem com a condição declarada.
* ORDER BY: Ordenei a tabela pelo nome do departamento em crescente e o salário em decrescente.

~~~SQL
  SELECT nome_departamento, CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) as nome_completo, funcionario.salario,
    CASE 
    WHEN funcionario.salario >= 40000 THEN 'gerente'
    ELSE 'funcionário'
    END as cargo
    FROM departamento, funcionario 
    WHERE departamento.numero_departamento = funcionario.numero_departamento
    ORDER BY nome_departamento ASC, salario DESC;
~~~

| nome_departamento | nome_completo    | salario  | cargo        |
:-------------------|------------------|----------|--------------:
| Administração     | Jennifer S Souza | 43000.00 | gerente      |
| Administração     | André V Pereira  | 25000.00 | funcionário  |
| Administração     | Alice J Zelaya   | 25000.00 | funcionário  |
| Matriz            | Jorge E Brito    | 55000.00 | gerente      |
| Pesquisa          | Fernando T Wong  | 40000.00 | gerente      |
| Pesquisa          | Ronaldo K Lima   | 38000.00 | funcionário  |
| Pesquisa          | João B Silva     | 30000.00 | funcionário  |
| Pesquisa          | Joice A Leite    | 25000.00 | funcionário  |

# Questão 6:

Essa questão pedia um relatório listando os dados dos funcionários com dependentes e os dados destes dependentes

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* CASE/ELSE/END: Uma série de comandos que é usada para criar uma nova linha com base em condições estabelecidas no código
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha.
* INNER JOIN: Usado duas vezes. A primeira para unir os dependentes à tabela funcionário e depois, na segunda, para unir a tabela departamento à funcionário.

~~~SQL 
  SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) as nome_completo,nome_departamento, nome_dependente, dependente.data_nascimento, timestampdiff(year,     dependente.data_nascimento, now()) as idade, 
	  CASE
	  WHEN dependente.sexo = 'M' THEN 'Masculino'
	  ELSE 'Feminino'
	  End as sexo
	  FROM funcionario
	  INNER JOIN dependente on dependente.cpf_funcionario = funcionario.cpf
	  INNER JOIN departamento on departamento.numero_departamento = funcionario.numero_departamento;
~~~

| nome_completo    | nome_departamento | nome_dependente | data_nascimento | idade | sexo      |
:------------------|-------------------|-----------------|-----------------|-------|-----------:
| João B Silva     | Pesquisa          | Alicia          | 1988-12-30      |    33 | Feminino  |
| João B Silva     | Pesquisa          | Elizabeth       | 1967-05-05      |    55 | Feminino  |
| João B Silva     | Pesquisa          | Michael         | 1988-01-04      |    34 | Masculino |
| Fernando T Wong  | Pesquisa          | Alicia          | 1986-04-05      |    36 | Feminino  |
| Fernando T Wong  | Pesquisa          | Janaina         | 1958-05-03      |    64 | Feminino  |
| Fernando T Wong  | Pesquisa          | Tiago           | 1983-10-25      |    38 | Masculino |
| Jennifer S Souza | Administração     | Antonio         | 1942-02-28      |    80 | Masculino |

# Questão 7:

A questão 7 pedia um relatório dos funcionários que não tem um dependente e os dados destes funcionários.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Aqui eu puxei a funcionário, de novo.
* INNER JOIN: Usado duas vezes. A primeira para unir os dependentes à tabela funcionário e depois, na segunda, para unir a tabela departamento à funcionário.
* LEFT JOIN: Usado para relacionar dados não relacionados na tabela à esquerda. Neste caso, a tabela dependente. Neste caso, a tabela trabalha_em com a funcionario.

~~~SQL
  SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) as nome_completo, nome_departamento, salario
	  FROM funcionario
	  LEFT JOIN dependente on dependente.cpf_funcionario = funcionario.cpf
	  INNER JOIN departamento on departamento.numero_departamento = funcionario.numero_departamento
	  WHERE dependente.cpf_funcionario IS NULL;
~~~

| nome_completo    | nome_departamento | salario  |
:------------------|-------------------|----------:
| Joice A Leite    | Pesquisa          | 25000.00 |
| Ronaldo K Lima   | Pesquisa          | 38000.00 |
| Jorge E Brito    | Matriz            | 55000.00 |
| André V Pereira  | Administração     | 25000.00 |
| Alice J Zelaya   | Administração     | 25000.00 |

# Questão 8:

Esta questão pedia um relatório os projetos alocados em seus respectivos departamentos e também uma listagem dos funcionários que estão trabalhando nestes projetos e a quantidade de horas que cada um deles nos devidos projetos.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Aqui usei pra pegar a tabela funcionario.
* INNER JOIN: Usado duas vezes. A primeira para unir os dependentes à tabela funcionário e depois, na segunda, para unir a tabela departamento à funcionário.
* AND: Foi usado para dar continuidade ao mesmo processo de junção do INNER JOIN do trabalha_em.

~~~SQL
  SELECT nome_departamento, nome_projeto, CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) as nome_completo, horas
	  FROM funcionario
	  INNER JOIN departamento on departamento.numero_departamento = funcionario.numero_departamento
	  INNER JOIN projeto on departamento.numero_departamento = projeto.numero_departamento
	  INNER JOIN trabalha_em on trabalha_em.cpf_funcionario = funcionario.cpf 
	  AND projeto.numero_projeto = trabalha_em.numero_projeto;
~~~
  
| nome_departamento | nome_projeto      | nome_completo    | horas |
:-------------------|-------------------|------------------|-------:
| Pesquisa          | ProdutoX          | João B Silva     |  32.5 |
| Pesquisa          | ProdutoX          | Joice A Leite    |  20.0 |
| Pesquisa          | ProdutoY          | João B Silva     |   7.5 |
| Pesquisa          | ProdutoY          | Fernando T Wong  |  10.0 |
| Pesquisa          | ProdutoY          | Joice A Leite    |  20.0 |
| Pesquisa          | ProdutoZ          | Fernando T Wong  |  10.0 |
| Pesquisa          | ProdutoZ          | Ronaldo K Lima   |  40.0 |
| Administração     | Informatização    | André V Pereira  |  35.0 |
| Administração     | Informatização    | Alice J Zelaya   |  10.0 |
| Matriz            | Reorganização     | Jorge E Brito    |   0.0 |
| Administração     | Novos Benefícios  | Jennifer S Souza |  20.0 |
| Administração     | Novos Benefícios  | André V Pereira  |   5.0 |
| Administração     | Novos Benefícios  | Alice J Zelaya   |  30.0 |

# Questão 9:

A questão 9 pedia um relatório com a soma total de horas gasta em cada projeto de cada departamento, mostrando também os nomes dos departamentos e projetos

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Peguei a tabela departamento.
* INNER JOIN: Usado duas vezes. A primeira para unir os dependentes à tabela funcionário e depois, na segunda, para unir a tabela departamento à funcionário.
* GROUP BY: Usei esse comando no final pra separar as linhas pelo nome do projeto.

~~~SQL
  SELECT SUM (horas) as hora_total, nome_departamento, nome_projeto
	  FROM departamento 
	  INNER JOIN projeto on departamento.numero_departamento = projeto.numero_departamento
	  INNER JOIN trabalha_em on projeto.numero_projeto = trabalha_em.numero_projeto
	  GROUP BY nome_projeto;
~~~

| hora_total | nome_departamento  | nome_projeto     |
:------------|--------------------|------------------:
| 55         | Administração      | Informatização   |
| 55         | Administração      | Novos benefícios |
| 52.5       | Pesquisa           | ProdutoX         |
| 37.5       | Pesquisa           | ProtudoY         |
| 25         | Pesquisa           | Produto Z        |
| 50         | Matriz             | Reorganização    |

# Questão 10:

A questão 10 pedia um relatório com a média salarial de cada departamento (unindo seus funcionários e tirando a média). Sim, é uma questão repetida, a original é a Questão 1.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* AVG: Comando usado para tirar a média salarial.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha.
* INNER JOIN: Usado para unir a tabela departamento à tabela funcionário.
* GROUP BY: Por fim, usei um group by para separar a média de cada departamento.

~~~SQL
SELECT funcionario.numero_departamento, AVG(funcionario.salario) as media_salarial
     FROM funcionario
     INNER JOIN departamento on departamento.numero_departamento = funcionario.numero_departamento
     GROUP BY funcionario.numero_departamento; 
~~~

| numero_departamento | media_salarial |
:---------------------|----------------:
|                   1 |   55000.000000 |
|                   4 |   31000.000000 |
|                   5 |   33250.000000 |

# Questão 11:

A questão 11 pedia um relatório que listaria quanto de salário um funcionário receberia por trabalhar tantas horas num projeto, contando que a hora trabalhada valia 50 reais.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Peguei três, sendo a funcionario, projeto e trabalha_em.
* WHERE: Impus a condição que o cpf do funcionario deveria ser igual ao cpf_funcionario na tabela trabalha_em.
* AND: Foi usado para aplicar mais uma condição ao WHERE, a de que o numero do projeto na tabela projeto deveria ser igual ao da trabalha_em.

~~~SQL
  SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo,
	projeto.nome_projeto, salario * 50 * horas AS salario_total
    	FROM funcionario, projeto, trabalha_em
    	WHERE funcionario.cpf = trabalha_em.cpf_funcionario 
    	AND projeto.numero_projeto = trabalha_em.numero_projeto;
~~~

| nome_completo    | nome_projeto      | salario_total |
:------------------|-------------------|---------------:
| João B Silva     | ProdutoX          |        1625.0 |
| João B Silva     | ProdutoY          |         375.0 |
| Fernando T Wong  | ProdutoY          |         500.0 |
| Fernando T Wong  | ProdutoZ          |         500.0 |
| Fernando T Wong  | Informatização    |         500.0 |
| Fernando T Wong  | Reorganização     |         500.0 |
| Joice A Leite    | ProdutoX          |        1000.0 |
| Joice A Leite    | ProdutoY          |        1000.0 |
| Ronaldo K Lima   | ProdutoZ          |        2000.0 |
| Jorge E Brito    | Reorganização     |           0.0 |
| Jennifer S Souza | Reorganização     |         750.0 |
| Jennifer S Souza | Novos Benefícios  |        1000.0 |
| André V Pereira  | Informatização    |        1750.0 |
| André V Pereira  | Novos Benefícios  |         250.0 |
| Alice J Zelaya   | Informatização    |         500.0 |
| Alice J Zelaya   | Novos Benefícios  |        1500.0 |

# Questão 12:

A questão 12 pedia um relatório onde estariam listados o nome do departamento, o nome do projeto e o nome dos funcionários que, apesar de alocados em um projeto, não registraram nenhuma hora neste.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Peguei 4 dessa vez, departamento, projeto, trabalha_em e funcionario.
* WHERE: Coloquei para, primeiro, dizer que o numero do departamento da tabela departamento deveria ser igual ao numero do departamento da tabela projeto.
* AND: Foi usado para aplicar mais uma condição ao WHERE quatro vezes. Uma para aplicar que o numero do departamento na departamento deveria ser igual ao da funcionario, outra pra dizer que o numero do projeto em projeto deveria ser igual ao da trabalha_em, mais um pra determinar que o cpf do funcionario em funcionario tinha de ser igual ao cpf na trabalha_em e, pro fim, que as horas na trabalha_em deveriam ser igual a 0.
* GROUP BY: Por fim, usei um group by para ordenar as coisas direitinho.

~~~SQL
  SELECT nome_departamento, nome_projeto, CONCAT(primeiro_nome, ' ', nome_meio, ' ',ultimo_nome) AS nome_completo, horas
	  FROM departamento, projeto, trabalha_em, funcionario
	  WHERE departamento.numero_departamento = projeto.numero_departamento 
    AND departamento.numero_departamento = funcionario.numero_departamento 
    AND projeto.numero_projeto = trabalha_em.numero_projeto 
    AND funcionario.cpf = trabalha_em.cpf_funcionario 
    AND trabalha_em.horas = 0 
	  GROUP BY nome_completo, nome_departamento, nome_projeto;
~~~

| nome_departamento | nome_projeto    | nome_completo | horas |
:-------------------|-----------------|---------------+-------:
| Matriz            | Reorganização   | Jorge E Brito |   0.0 |

# Questão 13:

A questão 13 solicitava que todas as pessoas, funcionários e seus dependentes, fossem listadas para que todos fossem presenteados, separando pelo nome completo, a data do nascimento, a idade e o sexo da pessoa, ordenando tudo em ordem decrescente na idade.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Aqui puxei duas, uma em cada SELECT diferente: funcionario e dependente.
* UNION: Utilizado para unir os dois selects em um único relatório. Aqui eu uni o SELECT do funcionario e o do depenendete.
* INNER JOIN: Utilizado para puxar a tabela funcionario para a dependente.
* ORDER BY: Só foi um Order By para fechar o maior comando desta lista com chave de ouro, ordenando tudo em forma decrescente pela idade.

~~~SQL
  SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) as nome_completo, 
funcionario.data_nascimento as data_nascimento,
timestampdiff(year, data_nascimento, now()) as idade, funcionario.sexo as sexo
  FROM funcionario

 UNION

SELECT CONCAT(nome_dependente, ' ', funcionario.nome_meio, ' ', funcionario.ultimo_nome) as nome_completo, 
dependente.data_nascimento as data_nascimento, timestampdiff(year, dependente.data_nascimento, now()) as idade, 
dependente.sexo as sexo
  FROM dependente
  INNER JOIN funcionario on funcionario.cpf = dependente.cpf_funcionario
  ORDER BY idade DESC;
~~~

| nome_completo     | data_nascimento | idade | sexo |
:-------------------|-----------------|-------|------:
| Jorge E Brito     | 1937-11-10      |    84 | M    |
| Antonio S Souza   | 1942-02-28      |    80 | M    |
| Jennifer S Souza  | 1941-06-20      |    80 | F    |
| Fernando T Wong   | 1955-12-08      |    66 | M    |
| Janaina T Wong    | 1958-05-03      |    64 | F    |
| Ronaldo K Lima    | 1962-09-15      |    59 | M    |
| João B Silva      | 1965-01-09      |    57 | M    |
| Elizabeth B Silva | 1967-05-05      |    55 | F    |
| Alice J Zelaya    | 1968-01-19      |    54 | F    |
| André V Pereira   | 1969-03-29      |    53 | M    |
| Joice A Leite     | 1972-07-31      |    49 | F    |
| Tiago T Wong      | 1983-10-25      |    38 | M    |
| Alicia T Wong     | 1986-04-05      |    36 | F    |
| Michael B Silva   | 1988-01-04      |    34 | M    |
| Alicia B Silva    | 1988-12-30      |    33 | F    |

# Questão 14:

A questão 14 pedia um relatório que contava quantos funcionários cada departamento tinha.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* CASE/WHEN/THEN/COUNT/END: Foi com esta série de comandos que consegui contar quantos funcionarios tinha em cada departamento. 
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Dei from na funcionario.
* INNER JOIN: Usado para puxar a tabela departamento para a funcionario.
* WHERE: Estabeleci que o numero do departamento na tabela departamento deveria ser igual na tabela funcionario.
* GROUP BY: Por fim, usei um group by para ordenar as coisas direitinho.

~~~SQL
  SELECT nome_departamento,
    	CASE 
       WHEN funcionario.numero_departamento = departamento.numero_departamento 
       THEN COUNT(departamento.numero_departamento)
    	 END AS soma
    	 FROM funcionario
	     INNER JOIN departamento on departamento.numero_departamento = funcionario.numero_departamento
   	   WHERE departamento.numero_departamento = funcionario.numero_departamento 
    	 GROUP BY nome_departamento;
~~~

| nome_departamento | soma |
:-------------------|------:
| Administração     |    3 |
| Matriz            |    1 |
| Pesquisa          |    4 |

# Questão 15: 

Ao final de tudo isso, a questão 15 pedia um relatório onde mostrasse o nome completo dos funcionários, os departamentos destes funcionários e em quais projetos eles estão trabalhando.

### Comandos e seus papéis:
* SELECT: O principal comando para gerar o relatório, sendo ele usado para selecionar as linhas que aparecem no relatório.
* FROM: Dá o caminho ao SELECT de onde ele deve puxar a linha. Puxei 4 de novo: funcionario, departamento, projeto e trabalha_em.
* WHERE: Disse que o cpf do funcionario na tabela do mesmo deveria ser igual ao da tabela trabalha_em.
* AND: Utilizado no relatório duas vezes para adicionar mais condições ao WHERE, sendo elas: que o numero do departamento na tabela do próprio deveria ser igual ao da tabela funcionario e que o numero do projeto da tabela projeto deveria ser igual ao da trabalha_em.

~~~SQL
  SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo, departamento.nome_departamento, projeto.nome_projeto
   	FROM funcionario, departamento, projeto, trabalha_em
    WHERE funcionario.cpf = trabalha_em.cpf_funcionario 
   	AND departamento.numero_departamento = funcionario.numero_departamento 
    AND projeto.numero_projeto = trabalha_em.numero_projeto;
~~~

| nome_completo    | nome_departamento | nome_projeto      |
:------------------|-------------------|-------------------:
| André V Pereira  | Administração     | Informatização    |
| Alice J Zelaya   | Administração     | Informatização    |
| Jennifer S Souza | Administração     | Novos Benefícios  |
| André V Pereira  | Administração     | Novos Benefícios  |
| Alice J Zelaya   | Administração     | Novos Benefícios  |
| Fernando T Wong  | Pesquisa          | Informatização    |
| João B Silva     | Pesquisa          | ProdutoX          |
| Joice A Leite    | Pesquisa          | ProdutoX          |
| João B Silva     | Pesquisa          | ProdutoY          |
| Fernando T Wong  | Pesquisa          | ProdutoY          |
| Joice A Leite    | Pesquisa          | ProdutoY          |
| Fernando T Wong  | Pesquisa          | ProdutoZ          |
| Jennifer S Souza | Administração     | Reorganização     |
| Jorge E Brito    | Matriz            | Reorganização     |
| Ronaldo K Lima   | Pesquisa          | ProdutoZ          |
| Fernando T Wong  | Pesquisa          | Reorganização     |
