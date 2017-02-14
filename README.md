# sqlite_day1

Explorer Mode

- How many users are there? 
  - 50
    - SELECT COUNT(id) FROM users;    
- What are the 5 most expensive items?
  - Small Cotton Gloves, Small Wooden Computer, Awesome Granite Pants, Sleek Wooden Hat, Ergonomic Steel Car
    - SELECT title FROM items ORDER BY price DESC LIMIT 5;
- What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)
  - Ergonomic Granite Chair
    - SELECT title FROM items WHERE category LIKE '%books%' ORDER BY price ASC;
- Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
  - Corrine Little
    - SELECT first_name,last_name FROM users INNER JOIN addresses ON users.id = addresses.user_id WHERE street = '6439 Zetta Hills' AND city = 'Willmouth' AND state = 'WY';
  - 54369 Wolff Forgers, Lake Bryon, CA, 31587
    - SELECT street,city,state,zip FROM addresses INNER JOIN users ON addresses.user_id = users.id WHERE first_name = 'Corrine' AND last_name = 'Little';
- Correct Virginie Mitchell's address to "New York, NY, 10108".
  - UPDATE addresses SET city = 'New York', state = 'NY', zip = '10108' WHERE user_id = 39;
- How much would it cost to buy one of each tool?
  - 46477
    - SELECT SUM(price) FROM items WHERE category LIKE '%tool%';
- How many total items did we sell?
  - 2125
    - SELECT SUM(quantity) FROM orders;
- How much was spent on books?
  - 59241
    - SELECT SUM(price) FROM items WHERE category LIKE '%books%';
- Simulate buying an item by inserting a User for yourself and an Order for that User.
  - INSERT INTO users (first_name, last_name, email) VALUES ('Laura', 'Moore', 'lmoore@email.com');
  - INSERT INTO orders (user_id, item_id, quantity, created_at) VALUES (51, 70, 4, CURRENT_TIMESTAMP);

Adventure Mode

- What item was ordered most often? Grossed the most money?
  - Tie between Ergonomic Concrete Gloves, Practical Rubber Computer, and Incredible Granite Car
    - SELECT item_id,COUNT(*) as item_count FROM orders GROUP BY item_id ORDER BY item_count DESC;
    - SELECT title FROM items WHERE id IN (10, 46, 65);
  - Incredible Granite Car grossed the most with 525240
    - SELECT (SUM(quantity) * price) AS sum, item_id FROM items INNER JOIN orders ON items.id = orders.item_id GROUP BY item_id ORDER BY sum DESC;
- What user spent the most?
  - Phoebe Kshlerin spent 676291
    - select (sum(quantity) * price) as sum, user_id from orders INNER JOIN items ON orders.item_id = items.id GROUP BY user_id ORDER BY sum DESC;
    - SELECT first_name, last_name FROM users WHERE id = 11;
- What were the top 3 highest grossing categories?
  - Top 3 grossing categories were Sports, Music, Sports & Clothing, and Beauty, Toys & Sports
    - SELECT (SUM(quantity) * price) AS sum, category FROM orders INNER JOIN items ON orders.item_id = items.id GROUP BY category ORDER BY sum DESC;
