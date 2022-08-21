# SQLPROJETOBASEDEDATOS-BANCO

1.Tem alguns Clientes que são dependentes. Quero que vocês me digam de que clientes eles são dependentes.


select conta.numero as conta,cliente_pai ,nome as cliente_dependente
from cliente
join cliente_conta on cliente_conta.id_cliente=cliente.id 
join (select cliente.nome as cliente_pai,cliente_conta.id_conta as conta_pai
      from cliente 
      join cliente_conta on cliente_conta.id_cliente=cliente.id 
      where dependente=false) as clientes_pai
	on conta_pai = cliente_conta.id_conta
join conta on conta.id=cliente_conta.id_conta
where dependente=true
order by conta;

2. Quais foram as 5 contas que:
○ Mais fizeram transações
○ Menos fizeram transações

SELECT
	cliente_conta.id_conta, conta.numero as "numero de conta",
    COUNT(transacao.id) AS "Quantidade_de_trasacao"
FROM transacao
JOIN cliente_conta
	ON transacao.id_cliente_conta = cliente_conta.id
JOIN conta
	ON cliente_conta.id = conta.id
GROUP BY conta.id
ORDER BY Quantidade_de_trasacao DESC
Limit 5;
SELECT
	cliente_conta.id_conta, conta.numero as "numero de conta",
    COUNT(transacao.id) AS "Quantidade_de_trasacao"
FROM transacao
JOIN cliente_conta
	ON transacao.id_cliente_conta = cliente_conta.id
JOIN conta
	ON cliente_conta.id = conta.id
GROUP BY conta.id
ORDER BY Quantidade_de_trasacao asc
Limit 5;

3. Tivemos uma perda de dados e não sabemos qual é o saldo de cada conta, mas temos todas as transações
efetuadas.
○ Queremos saber qual saldo total das contas registradas em banco!
■ Reparem que temos alguns tipos de transações que subtraem dinheiro e outros que
somam.

CREATE VIEW tb_creditos  as select id_cliente_conta as id_temp, SUM(transacao.valor) as creditos
from transacao
where id_tipo_transacao = 1
group by id_cliente_conta;

select id_cliente_conta, tb_creditos.creditos - SUM(transacao.valor)  as saldo_conta
from transacao
inner join tb_creditos on tb_creditos.id_temp = id_cliente_conta
where id_tipo_transacao in (2,3,4)
group by id_cliente_conta
order by saldo_conta asc;

DDLTOTI.SQL
https://gist.github.com/tiagoamaro/5c3977aa1f3fe9087914b5f86faf9db5
