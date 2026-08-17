FoodHub is a full-stack food delivery application built with React, Spring Boot, MySQL, and JWT authentication.

It allows users to create an account, log in securely, and view their authenticated profile. The backend provides REST APIs with password encryption, JWT-based security, input validation, centralized error handling, and role-based access control scaffolding for customers, restaurant owners, and administrators.

The project includes:

React and Node.js frontend

Spring Boot Java 17 backend

MySQL database schema and Docker setup

Secure registration and login with JWT

Protected user-profile endpoint

Role-based authorization foundation

Postman API collection

STS/Eclipse-ready Maven project

FoodHub is designed as a clean foundation for expanding into a complete food ordering platform with restaurants, menus, cart management, orders, payments, and delivery tracking.

## Project folders

| Folder/file | Purpose |
| --- | --- |
| `frontend` | React + Node.js website for registration, login, and profile viewing. |
| `backend` | Spring Boot REST API, JWT security, MySQL integration, and role-based access scaffolding. |
| `database/foodhub_db.sql` | MySQL database schema. |
| `docker-compose.yml` | Starts a ready-to-use local MySQL database with Docker. |

## Features

- Customer registration and secure login
- Password encryption with BCrypt
- JWT token authentication
- Protected profile endpoint: `GET /api/users/me`
- MySQL user storage
- Role structure for `CUSTOMER`, `RESTAURANT_OWNER`, and `ADMIN`
- React interface for creating an account, logging in, and logging out

## Requirements

Install these applications before running the project:

1. **Java JDK 17** — required for the backend.
2. **Spring Tool Suite 4 (STS)** — used to import and run the backend.
3. **Node.js 20.19 or newer** — required for the frontend. Download from [nodejs.org](https://nodejs.org/).
4. **Docker Desktop** — recommended and easiest way to run MySQL. Download from [docker.com](https://www.docker.com/products/docker-desktop/).

> You may use an existing MySQL Server instead of Docker. Instructions are included below.

## Step 1 — Extract the ZIP

1. Download `FoodHub-Complete-Ready.zip`.
2. Right-click the ZIP file and select **Extract All…**.
3. Choose a simple folder, such as `Documents\FoodHub`.
4. Open the extracted `foodhub-complete` folder.

Do not run the project directly from inside the ZIP file.

## Step 2 — Start the MySQL database with Docker (recommended)

1. Open **Docker Desktop** and wait until it says Docker is running.
2. Open the extracted `foodhub-complete` folder in File Explorer.
3. Click the address bar, type `powershell`, and press Enter. A terminal opens in this folder.
4. Run:

```bash
docker compose up -d database
```

5. Wait a few seconds, then confirm the database is running:

```bash
docker compose ps
```

The MySQL connection details are:

```text
Host: localhost
Port: 3306
Database: foodhub_db
Username: root
Password: foodhub
```

The database and `users` table are created automatically on the first startup.

## Step 3 — Run the backend in STS

1. Open **Spring Tool Suite 4**.
2. Select **File → Import…**.
3. Select **Maven → Existing Maven Projects**, then select **Next**.
4. Click **Browse…** and select the extracted `foodhub-complete\backend` folder.
5. Confirm that the `foodhub` project appears, then select **Finish**.
6. Wait for STS to download the Maven dependencies. The first import can take a few minutes.
7. In the left Project Explorer, open:

```text
backend
  → src/main/java
  → com.foodhub
  → FoodhubApplication.java
```

8. Right-click `FoodhubApplication.java` → **Run As → Spring Boot App**.

The backend is ready when the STS Console shows that it started on port `8080`.

Keep STS running while you use the frontend.

## Step 4 — Run the frontend

1. Open File Explorer and go to the extracted `foodhub-complete\frontend` folder.
2. Click the address bar, type `powershell`, and press Enter.
3. Run these commands one at a time:

```bash
npm install
npm run dev
```

4. The terminal shows a local address similar to:

```text
http://localhost:5173/
```

5. Hold `Ctrl` and click that address, or copy it into your browser.

## Step 5 — Test the application

1. On the FoodHub website, select **Create one**.
2. Enter your full name, email address, and a password of at least six characters.
3. Select **Create account**.
4. Your account is saved in MySQL, a JWT token is created, and your profile page opens automatically.
5. Select **Log out**, then use the login form to sign in again.

## Using your own MySQL installation instead of Docker

1. Start MySQL Server and open MySQL Workbench.
2. Open and run `database/foodhub_db.sql`.
3. Open `backend/src/main/resources/application.properties`.
4. Replace the default database username and password using either of these options:

   - Change the default values after the `:` in `DB_USERNAME` and `DB_PASSWORD`.
   - Or set `DB_USERNAME` and `DB_PASSWORD` environment variables before starting the backend.

For example, if your MySQL root password is `mypassword`, set:

```properties
spring.datasource.password=${DB_PASSWORD:mypassword}
```

## API testing with Postman

Import `backend/FoodHub.postman_collection.json` in Postman. It includes requests for registration, login, and the protected profile endpoint.

After logging in, copy the response `token` into the collection's `token` variable. Then send **My Profile**.

## Common problems

### `npm` is not recognized

Node.js is missing or was installed after the terminal was opened. Install Node.js, close the terminal, open a new terminal, and try again.

### Port 3306 is already in use

Another MySQL service is using the port. Stop that service, or edit `docker-compose.yml` and change `"3306:3306"` to `"3307:3306"`. If you use port `3307`, also update the backend database URL to use `localhost:3307`.

### Port 8080 is already in use

Stop the other Java application using port 8080, or change this line in `backend/src/main/resources/application.properties`:

```properties
server.port=8080
```

If you change it, update `frontend/src/api.js` to use the same backend port.

### The frontend cannot log in or shows a connection error

Make sure all three parts are running:

1. MySQL database (`docker compose ps` shows it running)
2. Spring Boot backend in STS (`http://localhost:8080`)
3. Frontend (`http://localhost:5173`)

## Stop the project

- Stop the frontend by pressing `Ctrl + C` in its terminal.
- Stop the backend using the red Stop button in STS.
- Stop MySQL from the root project folder:

```bash
docker compose down
```

This keeps the database data. To permanently remove its database data, use:

```bash
docker compose down -v
```

## Security note

The included MySQL password and JWT secret are safe only for local development. Replace them with secure values before deploying the application publicly.
