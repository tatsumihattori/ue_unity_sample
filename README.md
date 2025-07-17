# Unity WebGL Server

This is a simple HTTP server designed to properly serve Unity WebGL builds with gzip compression.

## Problem

Unity WebGL builds with compression enabled create `.gz` files that require the HTTP header `Content-Encoding: gzip` to be served correctly. Many simple web servers don't handle this automatically.

## Solution

This Node.js server automatically detects `.gz` files and serves them with the proper `Content-Encoding: gzip` header.

## Quick Start

1. Make sure you have Node.js installed (version 14 or higher)
2. Run the server:

   ```bash
   npm start
   ```

   or

   ```bash
   node server.js
   ```

3. Open your browser and go to `http://localhost:8080`

## Alternative Solutions

### Option 1: Use Python's built-in server (if you have Python installed)

```bash
# Python 3
python3 -m http.server 8080

# Python 2
python -m SimpleHTTPServer 8080
```

Note: This won't handle gzip compression properly, so you'll need to decompress the files first.

### Option 2: Decompress the files manually

If you prefer to serve uncompressed files, you can decompress the `.gz` files:

```bash
# On macOS/Linux
gunzip Build/*.gz

# Then update index.html to remove .gz extensions from URLs
```

### Option 3: Use a different web server

- **Apache**: Add to `.htaccess`:

  ```apache
  <FilesMatch "\.gz$">
    Header set Content-Encoding gzip
  </FilesMatch>
  ```

- **Nginx**: Add to config:

  ```nginx
  location ~* \.gz$ {
    gzip off;
    add_header Content-Encoding gzip;
  }
  ```

## Troubleshooting

- If you see "Unable to parse Build/ue_unity_sample.framework.js.gz" error, it means the server isn't setting the `Content-Encoding: gzip` header correctly.
- Check the browser's Network tab in Developer Tools to see if the `Content-Encoding` header is present for `.gz` files.
- Make sure you're accessing the files through the HTTP server, not directly from the file system.
