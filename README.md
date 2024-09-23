-- Create a table named "users"
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITHOUT TIME ZONE DEFAULT NOW()
);

-- Insert a new user record
INSERT INTO users (username, email, password) VALUES
    ('johndoe', 'johndoe@example.com', 'password123');

-- Select all users from the "users" table
SELECT * FROM users;

-- Update a user's email address
UPDATE users SET email = 'johndoe.updated@example.com' WHERE id = 1;

-- Delete a user from the "users" table
DELETE FROM users WHERE id = 1;

-- Create a table named "products" with a foreign key referencing the "users" table
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    price NUMERIC(10, 2) NOT NULL,
    user_id INT REFERENCES users(id),
    created_at TIMESTAMP WITHOUT TIME ZONE DEFAULT NOW()
);

-- Insert a new product record associated with a user
INSERT INTO products (name, description, price, user_id) VALUES
    ('Laptop', 'Powerful laptop for work and play', 1200, 1);

-- Retrieve products associated with a specific user
SELECT * FROM products WHERE user_id = 1;

-- Create a table named "orders" with foreign keys referencing the "users" and "products" tables
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id),
    product_id INT REFERENCES products(id),
    quantity INT NOT NULL,
    order_date DATE NOT NULL,
    status VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITHOUT TIME ZONE DEFAULT NOW()
);

-- Insert a new order record
INSERT INTO orders (user_id, product_id, quantity, order_date, status) VALUES
    (1, 1, 2, '2023-10-26', 'pending');

-- Retrieve orders placed by a specific user
SELECT * FROM orders WHERE user_id = 1;

-- Retrieve order details including user and product information
SELECT 
    orders.*,
    users.username,
    products.name AS product_name,
    products.description AS product_description,
    products.price
FROM orders
JOIN users ON orders.user_id = users.id
JOIN products ON orders.product_id = products.id;
