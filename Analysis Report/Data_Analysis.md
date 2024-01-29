**Who is the senior most employee based on job title?**
```
SELECT *
FROM employee
ORDER BY levels DESC
LIMIT 1
```
| employee_id | last_name | first_name | title                 | reports_to | levels | birthdate         | hire_date         | address           | city     | state | country | postal_code | phone            | fax              | email                      |
|-------------|-----------|------------|-----------------------|------------|--------|-------------------|-------------------|-------------------|----------|-------|---------|-------------|-----------------|-----------------|----------------------------|
| 9           | Madan     | Mohan      | Senior General Manager| NULL       | L7     | 1/26/1961 0:00    | 1/14/2016 0:00    | 1008 Vrinda Ave MT| Edmonton | AB    | Canada  | T5K 2N1     | +1 (780) 428-9482 | +1 (780) 428-3457 | madan.mohan@chinookcorp.com |

---
---

**Which countries have the most Invoices?**
```
SELECT billing_country, Count(invoice_id) AS countofinvoices
FROM invoice
GROUP BY billing_country
ORDER BY countofinvoices DESC
```
| billing_country | countofinvoices |
|-----------------|-----------------|
| USA             | 131             |
| Canada          | 76              |
| Brazil          | 61              |
| France          | 50              |
| Germany         | 41              |
| Czech Republic  | 30              |
| Portugal        | 29              |
| United Kingdom  | 28              |
| India           | 21              |
| Chile           | 13              |
| Ireland         | 13              |
| Spain           | 11              |
| Finland         | 11              |
| Australia       | 10              |
| Netherlands     | 10              |
| Sweden          | 10              |
| Poland          | 10              |
| Hungary         | 10              |
| Denmark         | 10              |
| Austria         | 9               |
| Norway          | 9               |
| Italy           | 9               |
| Belgium         | 7               |
| Argentina       | 5               |

<img width="699" alt="image" src="https://github.com/Himanshu2902/SQL_Project-Digital-Music-Store-Analysis/assets/91212648/58c3c88a-bd94-403d-8b11-4a46fe35c955">

---
---

**What are top 3 values of total invoice?**
```
SELECT total as top_3_values_of_total_invoices
FROM invoice
ORDER BY total desc
LIMIT 3
```
| Top 3 Values of Total Invoices |
|--------------------------------|
| 23.76                          |
| 19.8                           |
| 19.8                           |

---
---


**Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name & sum of all invoice totals**
```
SELECT billing_city as best_city, SUM(total) AS sum_of_invoice_totals
FROM invoice
GROUP BY billing_city
ORDER BY sum_of_invoice_totals DESC
LIMIT 1
```
| Best City | Sum of Invoice Totals |
|-----------|-----------------------|
| Prague    | 273.24                |

---
---

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
| Customer ID | Full Name           | Total  |
|-------------|---------------------|--------|
| 5           | František Wichterlová | 144.54 |

---
---
**Let's invite the artists who have written the most rock music in our dataset. Write a query that returns the Artist name and total track count of the top 10 rock bands**
```
SELECT artist.artist_id,artist.name AS artist_name, Count(artist.artist_id) as no_of_tracks
FROM track
JOIN album ON album.album_id = track.album_id
JOIN artist ON album.artist_id = artist.artist_id
JOIN genre ON track.genre_id = genre.genre_id
WHERE genre.genre_id IN (
		SELECT genre_id 
		FROM genre
		WHERE name LIKE 'Rock' 
	)
GROUP BY artist.artist_id
ORDER BY no_of_tracks DESC
LIMIT 10
```
| Artist ID | Artist Name                 | No. of Tracks |
|-----------|-----------------------------|---------------|
| 22        | Led Zeppelin                | 114           |
| 150       | U2                          | 112           |
| 58        | Deep Purple                 | 92            |
| 90        | Iron Maiden                 | 81            |
| 118       | Pearl Jam                   | 54            |
| 152       | Van Halen                   | 52            |
| 51        | Queen                       | 45            |
| 142       | The Rolling Stones          | 41            |
| 76        | Creedence Clearwater Revival| 40            |
| 52        | Kiss                        | 35            |

<img width="706" alt="image" src="https://github.com/Himanshu2902/SQL_Project-Digital-Music-Store-Analysis/assets/91212648/a1f8eaa6-2bd6-4b5f-b33b-2ba783a17321">

---
---

**Return all the track names that have a song length longer than the average song length.Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first**
```
SELECT name , milliseconds
FROM track
WHERE milliseconds > (SELECT AVG(milliseconds) FROM track)
ORDER BY milliseconds DESC
```
*Sample Data*
| Name                           | Milliseconds |
|--------------------------------|--------------|
| Occupation / Precipice         | 5286953      |
| Through a Looking Glass        | 5088838      |
| Greetings from Earth, Pt. 1    | 2960293      |
| The Man With Nine Lives        | 2956998      |
| Battlestar Galactica, Pt. 2    | 2956081      |
| Battlestar Galactica, Pt. 1    | 2952702      |
| Murder On the Rising Star      | 2935894      |
| Battlestar Galactica, Pt. 3    | 2927802      |
| Take the Celestra              | 2927677      |

---
---

**What are the most popular countries for music purchases?**
```
SELECT billing_country, SUM(total) AS total
FROM invoice
GROUP BY billing_country
ORDER BY total DESC
LIMIT 10
```
| Billing Country  | Total  |
|------------------|--------|
| USA              | 1040.49|
| Canada           | 535.59 |
| Brazil           | 427.68 |
| France           | 389.07 |
| Germany          | 334.62 |
| Czech Republic   | 273.24 |
| United Kingdom   | 245.52 |
| Portugal         | 185.13 |
| India            | 183.15 |
| Ireland          | 114.84 |

---
---

**Top Selling Genres: Find the top 3 genres based on the total number of tracks sold. Include the genre name and the total number of tracks sold for each genre**
```
SELECT g.name as genre_name, SUM(il.quantity) as total_number_of_track_sold 
FROM track AS t
JOIN genre AS g ON t.genre_id = g.genre_id
JOIN invoice_line AS il ON il.track_id = t.track_id
GROUP BY g.name
ORDER BY total_number_of_track_sold  DESC 
LIMIT 3
```
| Genre Name         | Total Number of Tracks Sold |
|--------------------|-----------------------------|
| Rock               | 2635                        |
| Metal              | 619                         |
| Alternative & Punk| 492                         |

---
---

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
*Sample data*
| Customer ID | First Name | Last Name | Total Number of Invoices | Total Amount Billed | Average Invoice Amount |
|-------------|------------|-----------|--------------------------|----------------------|------------------------|
| 5           | František  | Wichterlová| 18                       | 144.54               | 8.03                   |
| 6           | Helena     | Holý      | 12                       | 128.7                | 10.725                 |
| 46          | Hugh       | O'Reilly  | 13                       | 114.84               | 8.833846154            |
| 58          | Manoj      | Pareek    | 13                       | 111.87               | 8.605384615            |
| 1           | Luís       | Gonçalves | 13                       | 108.9                | 8.376923077            |
| 13          | Fernanda   | Ramos     | 15                       | 106.92               | 7.128                  |

---
---

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
| Name                        | Count of Tracks |
|-----------------------------|-----------------|
| Music                       | 6580            |
| 90’s Music                  | 1477            |
| TV Shows                    | 426             |
| Classical                   | 75              |
| Brazilian Music             | 39              |
| Heavy Metal Classic         | 26              |
| Classical 101 - The Basics  | 25              |
| Classical 101 - Deep Cuts   | 25              |
| Classical 101 - Next Steps  | 25              |
| Grunge                      | 15              |

---
---

**Retrieve the top 5 best-selling tracks based on the total number of units sold.Display the track name, album name, artist name, and the total number of units sold for each tr-ck.**
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
| Track Name            | Album Title             | Artist Name   | Total Number of Units Sold |
|-----------------------|-------------------------|---------------|-----------------------------|
| War Pigs              | Cake BSides and Rarities| Cake          | 31                          |
| Highway Chile         | Are You Experienced     | Jimi Hendrix  | 14                          |
| Are You Experienced?  | Are You Experienced     | Jimi Hendrix  | 14                          |
| Hey Joe               | Are You Experienced     | Jimi Hendrix  | 13                          |
| Third Stone From The Sun | Are You Experienced  | Jimi Hendrix  | 13                          |

---
---


**Artist Popularity Analysis:
Identify the top 3 most popular artists based on the total number of tracks sold across all albums.Display the artist names along with the total number of tracks sold for each artist.**
```
SELECT ar.artist_id, ar.name as artist_name, COUNT(il.quantity) as total_track_sold
FROM artist AS ar
JOIN album as al ON al.artist_id = ar.artist_id
JOIN track as t ON t.album_id = al.album_id
JOIN invoice_line as il ON il.track_id = t.track_id
GROUP BY ar.artist_id, ar.name
ORDER BY total_track_sold DESC
LIMIT 3
```

| Artist ID | Artist Name   | Total Tracks Sold |
|-----------|---------------|-------------------|
| 51        | Queen         | 192               |
| 94        | Jimi Hendrix  | 187               |
| 110       | Nirvana       | 130               |

<img width="687" alt="image" src="https://github.com/Himanshu2902/SQL_Project-Digital-Music-Store-Analysis/assets/91212648/ce6c290e-f3d7-41c5-a031-7d479d325083">


---
---







