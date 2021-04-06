Часть 1. Презентация
Мы выпустили приложение Ленточка. После неплохого старта у нас начало сокращаться количество заказов из приложения.
Задача – выявить причины падения и предложить меры для решения этой ситуации. Необходимые данные можно придумать.

Часть 2. SQL
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
- Выведете клиентов, которые не посещали форматы «У Дома» и
«Гипермаркет» более 8 недель и формат «Авто» более 4 недель;
- Выведете клиентов, у которых 80% чеков содержат от 10 шт. каждого
товара в чеке.

select acc_id, frmt_name, trn_date
from transactions join warehouses on warehouses.whs_id = transactions.whs_id
where trn_date in (
	select min(trn_date)
	from transactions
	group by acc_id
	)

SELECT acc_id
from warehouses join transactions on
	 warehouses.whs_id = transactions.whs_id
where (frmt_name in ('У Дома', 'Гипермаркет') and trn_date < dateadd(week,-8,getdate())) or (frmt_name like 'Авто' and trn_date < dateadd(week,-4,getdate()))
group by acc_id

drop table #t
select
	acc_id, 
	count(transactions.trn_id) as trn_total, 
	count(case when qnty >=10 then 1 else NULL END) as trn_count
into #t
from transactions join products on transactions.trn_id = products.trn_id
group by acc_id

select acc_id from #t where trn_count / 0.8 > trn_total

Часть 3. Python

3.1 Напишите программу, которая фильтрует спам, поступающий на корпоративную почту. Спам – письма, в тексте которых часть букв в
словах заменена на цифры. На вход подается файл с сообщениями.
Выход – файл без спам-писем.
Программа должна работать с файлами, размер которых превышает объем оперативной памяти. Файл подается в виде текстового файла.  Одно письмо = одна строка. 

3.2 Напишите программу для базовой линейной алгебры без использования сторонних библиотек (напр. NumPy).
Требуемые функции: сложение, вычитание, умножение, деление, транспонирование и определитель.

