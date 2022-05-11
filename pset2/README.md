# _Pset2_
### Nesse Pset aprendi como criar relatório utulizando os comando do MariaDB.
---

**Aluno: Thyago Silveira Rodrigues**

**Professor: Abrantes Araujo Silva Filho**

**Monitora: Suellen Miranda Amorim**

---

### Iriei falar de cada questão e o que eu fiz para resolver, além de comentar os comandos que utilizei e para que.

---

QUESTÃO 01: Prepare um relatório que mostre a média salarial dos funcionários
de cada departamento.
~~~SQL
SELECT funcionarios.numero_departamento, AVG(funcionarios.salario) AS media_do_salario
 FROM funcionarios 
  JOIN departamentos
   ON departamentos.numero_departamento = funcionarios.numero_departamento
    GROUP BY funcionarios.numero_departamento;
~~~

| numero_departamento | media_do_salario |
:---------------------|------------------:
|                   5 |     33250.000000 |
|                   1 |     55000.000000 |
|                   4 |     31000.000000 |


O comando SELECT é obrigatorio, ele selecionou a coluna "nome_departamento" e o FROM mostrou de qual tabela a coluna veio.
O JOIN serviu para combinar as colunas com a tabela departamento.
O comando GROUP BY  é utilizado em conjunto com a cláusula SELECT para agrupar registros semelhantes em uma tabela.

---

QUESTÃO 02: prepare um relatório que mostre a média salarial dos homens e das
mulheres.
~~~SQL
SELECT funcionarios.sexo, AVG(funcionarios.salario) AS media_do_salario
 FROM funcionarios
 GROUP BY funcionarios.sexo;
~~~

| sexo | media_do_salario |
:------|------------------:
| M    |     37600.000000 |
| F    |     31000.000000 |


Nessa questão o comando SELECT foi utilizado para selecionar a coluna "sexo" da tabela funcionarios (específicado com o comando FROM).
O GRUOP BY foi inserido para juntar os dados pedidos.

---

QUESTÃO 03: prepare um relatório que liste o nome dos departamentos e, para cada departamento, inclua as seguintes informações de seus funcionários: o nome completo, a data de nascimento, a idade em anos completos e o salário.
~~~SQL
SELECT nome_departamento, CONCAT(primeiro_nome,' ', nome_meio,' ',ultimo_nome) AS nome_completo,
 data_nascimento, timestampdiff(year, data_nascimento, now()) as idade, funcionarios.salario
  FROM departamentos, funcionarios
   WHERE departamentos.numero_departamento = funcionarios.numero_departamento;
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


Novamente o comando SELECT foi utilizado, mas dessa vez para selecionar o nome do departamento.
O comando CONCAT (esse para mim foi novo) serve para juntar os dados das colunas, nessa questào foi usado para juntar o nome dos funcionarios.
Já o TIMESTAMPDIFF foi muito difícil descobrir como usar ele, mas depois de muita pesquisa descobri, ele serve para retorna a diferença entre duas datas, nessa questão foi usado para mostar a idade dos funcionarios e a data de nascimento.

---

QUESTÃO 04: prepare um relatório que mostre o nome completo dos funcionários, a idade em anos completos, o salário atual e o salário com um reajuste que obedece ao seguinte critério: se o salário atual do funcionário é inferior a 35.000 o reajuste deve ser de 20%, e se o salário atual do funcionário for igual ou superior a 35.000 o reajuste deve ser de 15%.
~~~SQL
SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo,
 timestampdiff(year, data_nascimento, now()) as idade, salario, 
  CASE
   WHEN salario < 35000 then salario * 1.2
    ELSE salario * 1.15
     end as reajuste_salarial
      FROM funcionarios;
~~~~


| nome_completo    | idade | salario  | reajuste_salarial |
:------------------|-------|----------|-------------------:
| João B Silva     |    57 | 30000.00 |         36000.000 |
| Fernando T Wong  |    66 | 40000.00 |        46000.0000 |
| Joice A Leite    |    49 | 25000.00 |         30000.000 |
| Ronaldo K Lima   |    59 | 38000.00 |        43700.0000 |
| Jorge E Brito    |    84 | 55000.00 |        63250.0000 |
| Jennifer S Souza |    80 | 43000.00 |        49450.0000 |
| André V Pereira  |    53 | 25000.00 |         30000.000 |
| Alice J Zelaya   |    54 | 25000.00 |         30000.000 |


O SELECT junto com o CONCAT foi utilizado para selecionar e juntar as colunas dos nomes.
o TIMESTAMPDIFF foi usado para dizer a idade e os 5 ultimos comandos foram utilizados para fazer uma conta matematica, na qual queria o salario com um reajuste, que deveria ser. Se o salario for inferior a 35 mil ele teria um aumento de 20%, e se fosse superior a 35 mil, teria um aumento de 15%.

---

QUESTÃO 05: prepare um relatório que liste, para cada departamento, o nome do gerente e o nome dos funcionários. Ordene esse relatório por nome do departamento (em ordem crescente) e por salário dos funcionários (em ordem decrescente).
~~~SQL
SELECT nome_departamento,                   
 CASE
  WHEN funcionarios.cpf = departamentos.cpf_gerente THEN 'gerente'
   ELSE 'funcionarios'
    END AS cargo,
     CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo, salario
      FROM departamentos, funcionarios
       WHERE departamentos.numero_departamento = funcionarios.numero_departamento
        ORDER BY nome_departamento, salario DESC;
~~~


| nome_departamento | cargo        | nome_completo    | salario  |
:-------------------|--------------|------------------|----------:
| Administração     | gerente      | Jennifer S Souza | 43000.00 |
| Administração     | funcionarios | André V Pereira  | 25000.00 |
| Administração     | funcionarios | Alice J Zelaya   | 25000.00 |
| Matriz            | gerente      | Jorge E Brito    | 55000.00 |
| Pesquisa          | gerente      | Fernando T Wong  | 40000.00 |
| Pesquisa          | funcionarios | Ronaldo K Lima   | 38000.00 |
| Pesquisa          | funcionarios | João B Silva     | 30000.00 |
| Pesquisa          | funcionarios | Joice A Leite    | 25000.00 |


Nessa questão o SELECT foi utilizado para selecionar a coluna que mostra o nome dos departamentos. Os proximos comandos foram para ordenar o relatório em ordem crescente e os salarios por ordem decrescente.

---

QUESTÃO 06: prepare um relatório que mostre o nome completo dos funcionários que têm dependentes, o departamento onde eles trabalham e, para cada funcionário, também liste o nome completo dos dependentes, a idade em anos de cada
dependente e o sexo (o sexo NÃO DEVE aparecer como M ou F, deve aparecer
como “Masculino” ou “Feminino”).
~~~SQL
SELECT CASE
 WHEN funcionarios.cpf = dependentes.cpf_funcionario THEN 
  CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome)
   END AS nome_funcionarios,
    funcionarios.numero_departamento, departamentos.nome_departamento, nome_dependente,
     TIMESTAMPDIFF(year, dependentes.data_nascimento, now()) AS idade,
      CASE
       WHEN dependentes.sexo = 'M' THEN 'Masculino'
        ELSE 'Feminino'
         END AS sexo
          FROM funcionarios, departamentos, dependentes
           WHERE funcionarios.cpf = dependentes.cpf_funcionario AND
            funcionarios.numero_departamento = departamentos.numero_departamento;
~~~

| nome_funcionarios | numero_departamento | nome_departamento | nome_dependente | idade | sexo      |
:-------------------|---------------------|-------------------|-----------------|-------|-----------:
| João B Silva      |                   5 | Pesquisa          | Alícia          |    33 | Feminino  |
| João B Silva      |                   5 | Pesquisa          | Elizabeth       |    55 | Feminino  |
| João B Silva      |                   5 | Pesquisa          | Michael         |    34 | Masculino |
| Fernando T Wong   |                   5 | Pesquisa          | Alícia          |    36 | Feminino  |
| Fernando T Wong   |                   5 | Pesquisa          | Janaína         |    64 | Feminino  |
| Fernando T Wong   |                   5 | Pesquisa          | Tiago           |    38 | Masculino |
| Jennifer S Souza  |                   4 | Administração     | Antonio         |    80 | Masculino |

O SELECT junto com o CONCAT foi utilizado para selecionar e juntar o nome completo dos funcionarios, logo depois juntei o cpf dos funcionarios que tem dependentes e o departamento onde eles trabalham, e pra finalizar, use os comandos para dizer a idade em anos e o sexo dos dependentes.

---

QUESTÃO 07: prepare um relatório que mostre, para cada funcionário que NÃO TEM dependente, seu nome completo, departamento e salário.
~~~SQL
SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo,
 departamentos.nome_departamento, salario
  FROM departamentos, funcionarios
   LEFT JOIN dependentes ON funcionarios.cpf = dependentes.cpf_funcionario
    WHERE dependentes.cpf_funcionario IS NULL
     GROUP BY nome_completo;
~~~


| nome_completo   | nome_departamento | salario  |
:-----------------|-------------------|----------:
| Joice A Leite   | Pesquisa          | 25000.00 |
| Ronaldo K Lima  | Pesquisa          | 38000.00 |
| Jorge E Brito   | Pesquisa          | 55000.00 |
| André V Pereira | Pesquisa          | 25000.00 |
| Alice J Zelaya  | Pesquisa          | 25000.00 |

Nesse relatório, usei os comandos para mostrar quem são os funcionários que não tem denpendentes, fiz usando os comando SELECT e CONCAT, usando o comando FROM disse mde qual tabale são.

---

QUESTÃO 08: prepare um relatório que mostre, para cada departamento, os projetos desse departamento e o nome completo dos funcionários que estão alocados
em cada projeto. Além disso inclua o número de horas trabalhadas por cada funcionário, em cada projeto.

~~~SQL
SELECT nome_departamento, nome_projeto,
 CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo, horas
  FROM departamentos, projeto, funcionarios, trabalha_em
   WHERE departamentos.numero_departamento = projeto.numero_departamento 
    AND funcionarios.cpf = trabalha_em.cpf_funcionario 
     AND projeto.numero_projeto = trabalha_em.numero_projeto
      ORDER BY nome_departamento, nome_projeto;
   ~~~
       
| nome_departamento | nome_projeto    | nome_completo    | horas |
:-------------------|-----------------|------------------|-------:
| Administração     | Informatização  | Fernando T Wong  |  10.0 |
| Administração     | Informatização  | André V Pereira  |  35.0 |
| Administração     | Informatização  | Alice J Zelaya   |  10.0 |
| Administração     | Novosbenefícios | Jennifer S Souza |  20.0 |
| Administração     | Novosbenefícios | André V Pereira  |   5.0 |
| Administração     | Novosbenefícios | Alice J Zelaya   |  30.0 |
| Matriz            | Reorganização   | Fernando T Wong  |  10.0 |
| Matriz            | Reorganização   | Jorge E Brito    |   0.0 |
| Matriz            | Reorganização   | Jennifer S Souza |  15.0 |
| Pesquisa          | ProdutoX        | João B Silva     |  32.5 |
| Pesquisa          | ProdutoX        | Joice A Leite    |  20.0 |
| Pesquisa          | ProdutoY        | João B Silva     |   7.5 |
| Pesquisa          | ProdutoY        | Fernando T Wong  |  10.0 |
| Pesquisa          | ProdutoY        | Joice A Leite    |  20.0 |
| Pesquisa          | ProdutoZ        | Fernando T Wong  |  10.0 |
| Pesquisa          | ProdutoZ        | Ronaldo K Lima   |  40.0 |

Nessa questão eu usei os comandos para mostrar os departamentos, os projetos e o nome dos funcionarios, utilizei os comando SELECT e CONCAT para juntar, o FROM para dizer a tabela, logo depois como foi pedido, inclui as horas trabalhadas.

---

QUESTÃO 09: prepare um relatório que mostre a soma total das horas de cada projeto em cada departamento. Obs.: o relatório deve exibir o nome do departamento, o nome do projeto e a soma total das horas.
~~~SQL
SELECT departamentos.nome_departamento, projeto.numero_projeto, nome_projeto, SUM(horas) AS horas_trabalhadas
 FROM departamentos, projeto, trabalha_em
  WHERE projeto.numero_projeto = trabalha_em.numero_projeto
   AND departamentos.numero_departamento = projeto.numero_departamento  
    GOUP BY nome_projeto;
~~~


| nome_departamento | numero_projeto | nome_projeto    | horas_trabalhadas |
:-------------------|----------------|-----------------|-------------------:
| Pesquisa          |              1 | ProdutoX        |              52.5 |
| Pesquisa          |              2 | ProdutoY        |              37.5 |
| Pesquisa          |              3 | ProdutoZ        |              50.0 |
| Administração     |             10 | Informatização  |              55.0 |
| Matriz            |             20 | Reorganização   |              25.0 |
| Administração     |             30 | Novosbenefícios |              55.0 |

Foi pedido para preparar um relatório mostrando a soma total das horas trabalhadas em cada departamento. Utilizando novamente o comando SELECT selecionei as colunas das tabelas ditas no FROM, logo depois, usando o comando WHERE como um filtro para extrair somente o que eu quero e o comando GROUP BY para juntar tudo.

---

QUESTÃO 10: prepare um relatório que mostre a média salarial dos funcionários de cada departamento.
~~~SQL
SELECT funcionarios.numero_departamento, AVG(funcionarios.salario) AS media_salarial
FROM funcionarios
JOIN departamentos
ON departamentos.numero_departamento = funcionarios.numero_departamento
GROUP BY funcionarios.numero_departamento;
~~~


| numero_departamento | media_salarial |
:---------------------|----------------:
|                   5 |   33250.000000 |
|                   1 |   55000.000000 |
|                   4 |   31000.000000 |

Para fazer essa questão eu usei o comando SELECT para selecionar as colunas, e o FROM para dizer as tabelas, o JOIN foi utilizado para combinar duas tabelas diferente, nesse caso, a tabela funcionario com a tabela departamento. E para finalizar usei o GROUP BY foi usado para juntar a tabela funcionario com o numero do departamento.

---

QUESTÃO 11: considerando que o valor pago por hora trabalhada em um projeto é de 50 reais, prepare um relatório que mostre o nome completo do funcionário, o
nome do projeto e o valor total que o funcionário receberá referente às horas trabalhadas naquele projeto.
~~~SQL
SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo,
projeto.nome_projeto, salario * 50 * horas AS salario_total
FROM funcionarios, projeto, trabalha_em
WHERE funcionarios.cpf =trabalha_em.cpf_funcionario 
AND projeto.numero_projeto = trabalha_em.numero_projeto;
~~~


| nome_completo    | nome_projeto    | salario_total |
:------------------|-----------------|---------------:
| Fernando T Wong  | Informatização  |  20000000.000 |
| André V Pereira  | Informatização  |  43750000.000 |
| Alice J Zelaya   | Informatização  |  12500000.000 |
| Jennifer S Souza | Novosbenefícios |  43000000.000 |
| André V Pereira  | Novosbenefícios |   6250000.000 |
| Alice J Zelaya   | Novosbenefícios |  37500000.000 |
| João B Silva     | ProdutoX        |  48750000.000 |
| Joice A Leite    | ProdutoX        |  25000000.000 |
| João B Silva     | ProdutoY        |  11250000.000 |
| Fernando T Wong  | ProdutoY        |  20000000.000 |
| Joice A Leite    | ProdutoY        |  25000000.000 |
| Fernando T Wong  | ProdutoZ        |  20000000.000 |
| Ronaldo K Lima   | ProdutoZ        |  76000000.000 |
| Fernando T Wong  | Reorganização   |  20000000.000 |
| Jorge E Brito    | Reorganização   |         0.000 |
| Jennifer S Souza | Reorganização   |  32250000.000 |

O comando SELECT foi utilizado junto com o comando CONCAT para juntar o nome dos funcionarios, nome dos porojetos e o salario total, o FROM foi usado para dizer de quais tabela essas colunas pertencem, o WHERE para extrair somente  o que foi pedido.

---

QUESTÃO 12: seu chefe está verificando as horas trabalhadas pelos funcionários nos projetos e percebeu que alguns funcionários, mesmo estando alocadas à algum
projeto, não registraram nenhuma hora trabalhada. Sua tarefa é preparar um relatório que liste o nome do departamento, o nome do projeto e o nome dos funcionários
que, mesmo estando alocados a algum projeto, não registraram nenhuma hora trabalhada.

~~~SQL
SELECT departamentos.nome_departamento, projeto.nome_projeto, 
CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo,
trabalha_em.horas
FROM departamentos, projeto, funcionarios, trabalha_em
WHERE trabalha_em.horas = 0
GROUP BY trabalha_em.horas;
~~~


| nome_departamento | nome_projeto  | nome_completo | horas |
:-------------------|---------------|---------------|-------:
| Administração     | Reorganização | João B Silva  |   0.0 |

Foi pedido para que descubra qual o funcionário não trabalhou nenhuma hora e o nome do projeto e do departamento, usei novamente o comando SELECT
