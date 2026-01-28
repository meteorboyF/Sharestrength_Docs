# Getting Started

This guide walks you through setting up ShareStrength for local development.

## System Requirements

- PHP 8.2 or higher
- MySQL 8.0+ (or MariaDB)
- Node.js 16+ and npm
- Composer 2.0+

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/meteorboyF/ShareStrength-SL.git
cd ShareStrength-SL/backend
```

### 2. Install PHP Dependencies

```bash
composer install
```

### 3. Environment Configuration

Copy the example environment file:

```bash
cp .env.example .env
```

Update the `.env` file with your configuration:

```env
APP_NAME=ShareStrength
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=sharestrength
DB_USERNAME=your_username
DB_PASSWORD=your_password

SESSION_DRIVER=database
QUEUE_CONNECTION=database
CACHE_STORE=database
```

### 4. Generate Application Key

```bash
php artisan key:generate
```

### 5. Database Setup

Create the database:

```bash
mysql -u root -p -e "CREATE DATABASE sharestrength;"
```

Run migrations:

```bash
php artisan migrate --force
```

Optionally seed with sample data:

```bash
php artisan db:seed
```

### 6. Install Frontend Dependencies

```bash
npm install
npm run build
```

## Running the Application

### Development Server

**Option 1: Using Composer Script (Recommended)**

```bash
composer run dev
```

This runs the Laravel server, queue listener, and Vite dev server concurrently.

**Option 2: Manual Setup**

In separate terminals:

```bash
# Terminal 1: Laravel server
php artisan serve

# Terminal 2: Queue listener
php artisan queue:listen --tries=1

# Terminal 3: Vite dev server
npm run dev
```

The application will be available at `http://localhost:8000`.

## External Services

### Google Gemini API (Optional)

For AI chatbot functionality, add to `.env`:

```env
GEMINI_API_KEY=your_gemini_api_key
GEMINI_CHAT_MODEL=gemini-2.5-flash
```

### Qdrant Vector Database (Optional)

For RAG-powered search:

```env
QDRANT_URL=http://localhost:6335
QDRANT_COLLECTION=site_knowledge
```

## Composer Scripts

| Command | Description |
|---------|-------------|
| `composer run setup` | Full project setup |
| `composer run dev` | Run development server |
| `composer run test` | Run PHPUnit tests |
| `composer run build` | Build frontend assets |

## Troubleshooting

### Database Connection Issues

Ensure MySQL is running and credentials in `.env` are correct:

```bash
mysql -u your_username -p -e "SHOW DATABASES;"
```

### Permission Issues

Set proper storage permissions:

```bash
chmod -R 775 storage bootstrap/cache
```

### Cache Issues

Clear all caches:

```bash
php artisan cache:clear
php artisan config:clear
php artisan view:clear
```
