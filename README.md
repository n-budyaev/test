##### Часть SQL
Дана следующая схема данных

		whs_id – id магазина
		frmt – id формата магазина
		frmt_name – название формата магазина
		trn_id – id транзакции
		acc_id – id клиента
		trn_date – дата транзакции
		total – сумма транзакции
		art_id – id товара
		qnty – количество товара
		value – стоимость товара

Для данной схемы напишите следующие запросы:
- Для каждого клиента выведете магазин, в котором он совершил
первую покупку, и ее дату;

		select acc_id, frmt_name, trn_date 
		from transactions join warehouses on warehouses.whs_id = transactions.whs_id
		where trn_date in ( 
			select min(trn_date) 
			from transactions 
			group by acc_id 
		)

- Выведете клиентов, которые не посещали форматы «У Дома» и
«Гипермаркет» более 8 недель и формат «Авто» более 4 недель;

		SELECT acc_id
		from warehouses join transactions on warehouses.whs_id = transactions.whs_id
		where (frmt_name in ('У Дома', 'Гипермаркет') and trn_date < dateadd(week,-8,getdate()))
			or (frmt_name like 'Авто' and trn_date < dateadd(week,-4,getdate()))
		group by acc_id

- Выведете клиентов, у которых 80% чеков содержат от 10 шт. каждого
товара в чеке.

		drop table #t
		select
			acc_id, 
			count(transactions.trn_id) as trn_total, 
			count(case when qnty >=10 then 1 else NULL END) as trn_count
		into #t
		from transactions join products on transactions.trn_id = products.trn_id
		group by acc_id
		select acc_id from #t where trn_count / 0.8 > trn_total

