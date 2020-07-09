# sql-2-afternoon

Practice joins

1-Get all invoices where the unit_price on the invoice_line is greater than $0.99.

select  * from invoice i
join invoice_line il on il.invoice_id =i.invoice_id
where il.unit_price > 0.99;

2-Get the invoice_date, customer first_name and last_name, and total from all invoices.

select i.invoice_date, c.first_name, c.last_name, i.total
from invoice i
join customer c on i.customer_id = c.customer_id;

3-Get the customer first_name and last_name and the support rep's first_name and last_name from all customers.

select c.first_name, c.last_name, e.first_name, e.last_name
from customer c
join employee e on c.support_rep_id = employee_id;

4-Get the album title and the artist name from all albums.

select al.title, ar.name 
from albums al
join artist ar on al.artist_id = ar.artist_id;

5-Get all playlist_track track_ids where the playlist name is Music.

select pt.track_id
from playlist_track pt
join playlist p on p.playlist_id = pt.playlist_id
where p.name = 'Music';

6-Get all track names for playlist_id 5.

select t.name
from track t
join playlist_track_id pt on pt.track_id = track_id
where pt.playlist_id = 5;

7-Get all track names and the playlist name that they're on ( 2 joins ).

select t.name, p.name
from track t
join playlist_track pt on t.track_id = pt.track_id
join playlost p on pt.playlist_id = p.playlist_id;

8-Get all track names and album titles that are the genre Alternative & Punk ( 2 joins ).

select t.name, a.title
from track t
join album a on t.album_id = a.album_id
join genre g on g.genre_id = t.genre_id
where g.name = 'Alternative & Punk';

Practice nested queries

1-Get all invoices where the unit_price on the invoice_line is greater than $0.99.

select * 
from invoice
where invoice_id in (select invoice_id from invoice_line where unit_price >0.99);

2-Get all playlist tracks where the playlist name is Music.

select * 
from playlist_track
where playlist_id in (select playlist_id from playlist where name = 'Music');

3-Get all track names for playlist_id 5.

select name
from track
where track_id in (select track_id from playlist_track where playlist_id = 5 );

4-Get all tracks where the genre is Comedy.

select * 
from track 
where genre_id in (select genre_id from genre where name = 'Comedy');

5-Get all tracks where the album is Fireball.

select *
from track
where album_id in (select album_id from album where title = 'Fireball');

6-Get all tracks for the artist Queen ( 2 nested subqueries ).

select *
from track
where album_id in (select album_id from album where artist_id in (select artist_id from artist where name = 'Queen')
);

Practice updating Rows

1-Find all customers with fax numbers and set those numbers to null.

update customer 
set fax = null
where fax is not null;

2-Find all customers with no company (null) and set their company to "Self".

update customer 
set company = 'Self'
where company is null;

3-Find the customer Julia Barnett and change her last name to Thompson.

update customer 
set last_name = 'Thompson'
where first_name = 'Julia' and last_name = 'Barnett';

4-Find the customer with this email luisrojas@yahoo.cl and change his support rep to 4.

update customer 
set suppport_rep_id = 4
where email = 'luisrojas@yahoo.cl';

5-Find all tracks that are the genre Metal and have no composer. Set the composer to "The darkness around us".

update track
set composer = 'The darkness around us'
where genre_id =(select genre_id from genre where name = 'Metal')
and composer is null;


Group by

1-Find a count of how many tracks there are per genre. Display the genre name with the count.

select count (*), g.name
from track t
join genre g on t.genre_id = g.genre_id
group by g.name;

2-Find a count of how many tracks are the "Pop" genre and how many tracks are the "Rock" genre.

select count(*), g.name
from track t
join genre g on g.genre_id = t.genre_id
where g.name = 'Pop' or g.name = 'Rock'
group by g.name;

3-Find a list of all artists and how many albums they have.

select ar.name, count(*)
from album al
join artist ar on ar.artist_id = al.atrist_id
group by ar.name;

Use Distinct

1-From the track table find a unique list of all composers.

select distinct composer
from track;

2-From the invoice table find a unique list of all billing_postal_codes.

select distinct billing_postal_codes
from invoice;


3-From the customer table find a unique list of all companys.

select distinct company
from customer;


Delete Rows

1-Copy, paste, and run the SQL code from the summary.
2-Delete all 'bronze' entries from the table.

delete from practice_delete
where type = 'bronze';

3- Delete all 'silver' entries from the table.

delete from practice_delete
where type = 'silver';

4-Delete all entries whose value is equal to 150.

delete from practice_delete
where value = 150;


eCommerce Simulation - No Hints

1-Create 3 tables following the criteria in the summary.
Let's simulate an e-commerce site. We're going to need users, products, and orders.

users need a name and an email.
products need a name and a price
orders need a ref to product.
All 3 need primary keys.

create table users(
user_id serial primary key,
user_name varchar(40),
email varchar(40)
);

create table product(
product_id serial primary key,
product_name varchar(30),
product_price int
);

create table orders(
-- 	order_id serial primary key,
    product_id int references product(product_id),
--   );


Get all products for the first order.
Get all orders.
Get the total cost of an order ( sum the price of all products on an order ).

select *
from product
where product_id in (select order_id from orders);

select * from orders;














