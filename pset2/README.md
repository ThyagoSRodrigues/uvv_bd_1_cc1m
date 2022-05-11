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

SELECT funcionarios.numero_departamento, AVG(funcionarios.salario) AS media_do_salario

FROM funcionarios 

JOIN departamentos

ON departamentos.numero_departamento = funcionarios.numero_departamento

GROUP BY funcionarios.numero_departamento;

O comando SELECT é obrigatorio, ele selecionou a coluna "nome_departamento" e o FROM mostrou de qual tabela a coluna veio.
O JOIN serviu para combinar as colunas com a tabela departamento.
O comando GROUP BY  é utilizado em conjunto com a cláusula SELECT para agrupar registros semelhantes em uma tabela.

---

QUESTÃO 02: prepare um relatório que mostre a média salarial dos homens e das
mulheres.

SELECT funcionarios.sexo, AVG(funcionarios.salario) AS media_do_salario

FROM funcionarios

GROUP BY funcionarios.sexo;

Nessa questão o comando SELECT foi utilizado para selecionar a coluna "sexo" da tabela funcionarios (específicado com o comando FROM).
o GRUOP BY foi inserido para juntar os dados pedidos.

---

QUESTÃO 03: prepare um relatório que liste o nome dos departamentos e, para cada departamento, inclua as seguintes informações de seus funcionários: o nome completo, a data de nascimento, a idade em anos completos e o salário.

SELECT nome_departamento, CONCAT(primeiro_nome,' ', nome_meio,' ',ultimo_nome) AS nome_completo,

data_nascimento, timestampdiff(year, data_nascimento, now()) as idade, funcionarios.salario

FROM departamentos, funcionarios

WHERE departamentos.numero_departamento = funcionarios.numero_departamento;

Novamente o comando SELECT foi utilizado, mas dessa vez para selecionar o nome do departamento.
O comando CONCAT (esse para mim foi novo) serve para juntar os dados das colunas, nessa questào foi usado para juntar o nome dos funcionarios.
Já o TIMESTAMPDIFF foi muito difícil descobrir como usar ele, mas depois de muita pesquisa descobri, ele serve para retorna a diferença entre duas datas, nessa questão foi usado para mostar a idade dos funcionarios e a data de nascimento.

---

QUESTÃO 04: prepare um relatório que mostre o nome completo dos funcionários, a idade em anos completos, o salário atual e o salário com um reajuste que obedece ao seguinte critério: se o salário atual do funcionário é inferior a 35.000 o reajuste deve ser de 20%, e se o salário atual do funcionário for igual ou superior a 35.000 o reajuste deve ser de 15%.

SELECT CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo,
timestampdiff(year, data_nascimento, now()) as idade, salario, 
CASE
WHEN salario < 35000 then salario * 1.2
ELSE salario * 1.15
end as reajuste_salarial
FROM funcionarios;

O SELECT junto com o CONCAT foi utilizado para selecionar e juntar as colunas dos nomes.
o TIMESTAMPDIFF foi usado para dizer a idade e os 5 ultimos comandos foram utilizados para fazer uma conta matematica, na qual queria o salario com um reajuste, que deveria ser. Se o salario for inferior a 35 mil ele teria um aumento de 20%, e se fosse superior a 35 mil, teria um aumento de 15%.

---

QUESTÃO 05: prepare um relatório que liste, para cada departamento, o nome do gerente e o nome dos funcionários. Ordene esse relatório por nome do departamento (em ordem crescente) e por salário dos funcionários (em ordem decrescente).

SELECT nome_departamento,                   
CASE
WHEN funcionarios.cpf = departamentos.cpf_gerente THEN 'gerente'
ELSE 'funcionarios'
END AS cargo,
CONCAT(primeiro_nome, ' ', nome_meio, ' ', ultimo_nome) AS nome_completo, salario
FROM departamentos, funcionarios
WHERE departamentos.numero_departamento = funcionarios.numero_departamento
ORDER BY nome_departamento, salario DESC;

Nessa questão o SELECT foi utilizado para selecionar a coluna que mostra o nome dos departamentos.
