<div align="center">

# Rayphoenix by Ray Laboratories
### A lightning-fast URL shortening service powered by Go & Ruby

[![Version](https://img.shields.io/badge/version-1.0.0-purple)](https://github.com/yourusername/rayphoenix)
[![License](https://img.shields.io/badge/license-MIT-magenta)](LICENSE)
[![Go](https://img.shields.io/badge/go-1.20+-violet)](https://go.dev/)
[![Ruby](https://img.shields.io/badge/ruby-2.7+-violet)](https://www.ruby-lang.org/)

**Perfect for high-traffic redirects** • **Beautiful management interface**

---

</div>

<div align="left">

## Installation & Setup

### Prerequisites

#### For Go Service:
```bash
# Go 1.20 or higher required
go version

# Initiate the modules
go mod init rayphoen

# Install SQLite3 driver
go get github.com/mattn/go-sqlite3
```

#### For Ruby Service:
```bash
# Ruby 2.7 or higher required
ruby --version

# Install Sinatra
gem install sinatra
```

## Running the System

### Step 1: Start the Go Redirect Service

```bash
cd backend/go
go run main.go
```
### Step 2: Start the Ruby Management Interface

```bash
cd backend/ruby
ruby app.rb
```
### Step 3: Access the Dashboard

Open your browser and navigate to:
```
http://localhost:4567
```

## Configuration

### Go Service Configuration

Edit `main.go` to modify server settings:

```go
// Change server port
log.Fatal(http.ListenAndServe(":8080", enableCORS(http.DefaultServeMux)))

// Change database location
db, err = sql.Open("sqlite3", "./rayphoenix.db")
```

### Ruby Service Configuration

Edit `app.rb` to modify settings:

```ruby
# Change Go service URL
GO_SERVICE_URL = ENV['GO_SERVICE_URL'] || 'http://localhost:8080'

# Change server port (at bottom of file)
set :port, 4567
```

### Database Schema

The SQLite database uses the following schema:

```sql
CREATE TABLE links (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    short_code TEXT UNIQUE NOT NULL,
    long_url TEXT NOT NULL,
    clicks INTEGER DEFAULT 0,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

## API Reference

### Go Service Endpoints

#### GET /{short_code}
Redirect to the long URL associated with the short code.

**Response**: `302 Found` redirect to long URL

---

#### GET /api/links
Retrieve all short links.

**Response**:
```json
[
  {
    "id": 1,
    "short_code": "github",
    "long_url": "https://github.com",
    "clicks": 42,
    "created_at": "2024-12-16T10:30:00Z"
  }
]
```

---

#### POST /api/links
Create a new short link.

**Request Body**:
```json
{
  "short_code": "github",
  "long_url": "https://github.com"
}
```

**Response**: `201 Created`
```json
{
  "id": 1,
  "short_code": "github",
  "long_url": "https://github.com",
  "clicks": 0,
  "created_at": "2024-12-16T10:30:00Z"
}
```

---

#### DELETE /api/links/{short_code}
Delete a short link.

**Response**: `204 No Content`

---

### Ruby Service Endpoints

#### GET /
Access the web-based management interface.

---

#### GET /api/links
Proxy to Go service - retrieve all links.

---

#### POST /api/links
Create a new link with validation.

**Request Body**:
```json
{
  "short_code": "github",
  "long_url": "https://github.com"
}
```

**Validation Rules**:
- Short code: alphanumeric, underscores, and hyphens only
- Long URL: must be a valid HTTP/HTTPS URL

---

#### DELETE /api/links/{short_code}
Proxy to Go service - delete a link.

---

#### GET /api/generate-code
Generate a random 6-character short code.

**Response**:
```json
{
  "code": "xk9p2m"
}
```
