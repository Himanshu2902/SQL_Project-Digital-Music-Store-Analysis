**Who is the senior most employee based on job title?**
```
SELECT *
FROM employee
ORDER BY levels DESC
LIMIT 1
```

**Which countries have the most Invoices?**
```
SELECT billing_country, Count(invoice_id) AS countofinvoices
FROM invoice
GROUP BY billing_country
ORDER BY countofinvoices DESC
```

**What are top 3 values of total invoice?**
```
SELECT total as top_3_values_of_total_invoices
FROM invoice
ORDER BY total desc
LIMIT 3
```

**Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals**
```
SELECT billing_city , SUM(total) AS s
FROM invoice
GROUP BY billing_city
ORDER BY s DESC
LIMIT 1
```


**Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money**
```
SELECT  c.customer_id,CONCAT(c.first_name,c.last_name) as fullname ,Sum(i.total) as total 
FROM invoice AS i 
JOIN customer AS c
ON i.customer_id = c.customer_id
GROUP BY c.customer_id
ORDER BY total DESC
LIMIT 1
```

**Let's invite the artists who have written the most rock music in our dataset. Write a query that returns the Artist name and total track count of the top 10 rock bands**
```
Select artist.artist_id,artist.name, Count(artist.artist_id) as no_of_tracks
from track
join album on album.album_id = track.album_id
join artist on album.artist_id = artist.artist_id
join genre on track.genre_id = genre.genre_id
Where genre.genre_id IN (
		SELECT genre_id 
		FROM genre
		WHERE name LIKE 'Rock' 
	)
GROUP BY artist.artist_id
ORDER BY no_of_tracks DESC
```
**Return all the track names that have a song length longer than the average song length.Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first**
```
SELECT name , milliseconds
FROM track
WHERE milliseconds > (SELECT AVG(milliseconds) FROM track)
ORDER BY milliseconds DESC
```

**What are the most popular countries for music purchases?**
```
select billing_country , sum(total) as total from invoice
group by billing_country 
order by total desc
limit 10
```

**Top Selling Genres: Find the top 3 genres based on the total number of tracks sold. Include the genre name and the total number of tracks sold for each genre**
SELECT g.name , SUM(il.quantity) as total_number_of_track_sold 
FROM track AS t
JOIN genre AS g ON t.genre_id = g.genre_id
JOIN invoice_line AS il ON il.track_id = t.track_id
GROUP BY g.name
ORDER BY total_number_of_track_sold  DESC 
LIMIT 3

**Sales Analysis: Retrieve the total number of invoices, the total amount billed, and the average invoice amount for each customer. Sort the results by the total amount billed in descending order.**
```
SELECT 
c.customer_id,
c.first_name,
c.last_name,
COUNT(i.invoice_id) as total_number_of_invoices ,
SUM(i.total) AS total_amount_billed ,
AVG(i.total) AS average_invoice_amount
FROM invoice AS i
JOIN customer AS c ON c.customer_id = i.customer_id
GROUP BY c.customer_id
ORDER BY  total_amount_billed DESC
```

**Playlist Analysis:Find the names of playlists along with the total number of unique tracks in each playlist. Include only those playlists that have more than 10 unique tracks.**
```
SELECT p.name , count(t.track_id) as count_of_track
FROM playlist as p
JOIN playlist_track AS pt ON p.playlist_id = pt.playlist_id
JOIN track as t ON t.track_id = pt.track_id
GROUP BY p.name
HAVING COUNT(t.track_id) > 10
ORDER BY count_of_track DESC;
```

**Retrieve the top 5 best-selling tracks based on the total number of units sold.Display the track name, album name, artist name, and the total number of units sold for each track.**
```
SELECT t.name AS track_name,
al.title as album_title,
ar.name AS artist_name ,
SUM(il.quantity) as total_number_units_sold
FROM track as t
JOIN album as al ON t.album_id = al.album_id
JOIN artist as ar ON al.artist_id = ar.artist_id
JOIN invoice_line as il ON il.track_id =t.track_id
GROUP BY t.name,al.title,ar.name
ORDER BY total_number_units_sold DESC
LIMIT 5
```












