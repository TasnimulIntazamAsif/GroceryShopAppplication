## Grocery Shop Application

This project is a simple **Grocery Store Management** web application built with:

- **Frontend**: HTML, CSS, JavaScript, Bootstrap (in `ui/`)
- **Backend**: Python + Flask (in `backend/`)
- **Database**: MySQL (see `backend/sql_connection.py`)

The app lets you manage products (with units of measure) and create/view customer orders.

---

## Project Structure

- **backend/**
  - `server.py`: Flask application exposing REST endpoints for products, units of measure, and orders.
  - `product_dao.py`: Product CRUD operations.
  - `orders_dao.py`: Order and order‑details persistence and querying.
  - `uom_dao.py`: Units of measure (UOM) queries.
  - `sql_connection.py`: MySQL connection helper (update credentials here).
- **ui/**
![bg](https://github.com/user-attachments/assets/970a420b-a95d-4f6f-b168-21ff3a09bcce)
<img width="1914" height="946" alt="Screenshot 2025-12-04 111453" src="https://github.com/user-attachments/assets/aea2cc82-e1dd-4660-a00f-61a967550006" />
<img width="1919" height="947" alt="Screenshot 2025-12-04 111544" src="https://github.com/user-attachments/assets/9d0b9bdf-15b9-4dae-88da-8247a05e1388" />
<img width="1915" height="937" alt="Screenshot 2025-12-04 111600" src="https://github.com/user-attachments/assets/7dbff594-15e4-4164-bee0-05c62e1de888" />
<img width="1914" height="938" alt="Screenshot 2025-12-04 111624" src="https://github.com/user-attachments/assets/edb11370-a467-4154-b614-c2ed413e537d" />

- 
  - `index.html`: Dashboard / landing page.
  - `manage-product.html`: Product listing and management UI.
  - `order.html`: Create and view orders UI.
  - `css/`, `js/`, `images/`: Static assets (Bootstrap, jQuery, custom scripts, styles, and images).

---

## Prerequisites

- **Python 3.8+**
- **MySQL Server**
- **pip** (Python package manager)

Python packages (install via `pip`):

```bash
pip install flask mysql-connector-python
```

---

## Database Setup

1. **Create a database** (default name used in code is `grocery`):

```sql
CREATE DATABASE grocery;
```

2. **Create required tables** (simplified example – adjust to match your own schema):

```sql
USE grocery;

CREATE TABLE uom (
  uom_id INT AUTO_INCREMENT PRIMARY KEY,
  uom_name VARCHAR(50) NOT NULL
);

CREATE TABLE products (
  product_id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  uom_id INT NOT NULL,
  price_per_unit DECIMAL(10,2) NOT NULL,
  FOREIGN KEY (uom_id) REFERENCES uom(uom_id)
);

CREATE TABLE orders (
  order_id INT AUTO_INCREMENT PRIMARY KEY,
  customer_name VARCHAR(100) NOT NULL,
  total DECIMAL(10,2) NOT NULL,
  datetime DATETIME NOT NULL
);

CREATE TABLE order_details (
  id INT AUTO_INCREMENT PRIMARY KEY,
  order_id INT NOT NULL,
  product_id INT NOT NULL,
  quantity DECIMAL(10,2) NOT NULL,
  total_price DECIMAL(10,2) NOT NULL,
  FOREIGN KEY (order_id) REFERENCES orders(order_id),
  FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

3. **Configure DB credentials** in `backend/sql_connection.py`:

```python
__cnx = mysql.connector.connect(
    user='root',
    password='YOUR_PASSWORD',
    database='grocery'
)
```

---

## Running the Backend (Flask API)

1. Open a terminal and go to the `backend` folder:

```bash
cd backend
```

2. Run the Flask server:

```bash
python server.py
```

3. By default the API runs at:

- `http://localhost:5000`

Key endpoints (used by the UI JavaScript):

- `GET /getUOM` – list units of measure.
- `GET /getProducts` – list products.
- `POST /insertProduct` – create a new product.
- `POST /deleteProduct` – delete a product.
- `GET /getAllOrders` – list all orders (with details).
- `POST /insertOrder` – create a new order.

---

## Running the Frontend

1. Open the `ui` folder in your file explorer.
2. Open one of these files in a browser (Chrome/Edge/Firefox, etc.):
   - `index.html`
   - `manage-product.html`
   - `order.html`
3. Make sure the Flask server is running on `http://localhost:5000` so that AJAX calls from the UI succeed.

If you prefer, you can serve `ui/` via a simple static server instead of opening HTML files directly (for example `python -m http.server` inside `ui/`), but it is not strictly required.

---

## Notes and Customization

- **Change port**: Edit `app.run(port=5000)` in `backend/server.py` if you want a different port.
- **Change DB connection**: Update `backend/sql_connection.py` for your MySQL host/user/password/database.
- **Security**: This is a learning/demo app and does not include authentication or authorization; do not expose it directly to the internet without hardening.

---

## License

See `LICENSE` file for licensing details.
