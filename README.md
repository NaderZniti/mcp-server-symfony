# Symfony MCP Server

Examlple of a Model Context Protocol (MCP) server built with Symfony 6 and PHP, providing AI-powered tools for Symfony application management and PDF generation.

## 🚀 Features

- **Cache Management** - Clear Symfony cache via MCP tools
- **PDF Generation** - Create PDF documents with current date/time
- **HTTP Transport** - Works on Windows using ReactPHP HTTP server
- **Auto-Discovery** - Automatically discovers MCP tools from your codebase
- **VS Code Integration** - Seamlessly integrates with GitHub Copilot in VS Code

## 📋 Prerequisites

- PHP 8.1 or higher
- Composer
- Windows (configured for HTTP transport)
- VS Code with GitHub Copilot

## 🔧 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/mcp_app.git
cd mcp_app
```

### 2. Install Dependencies

```bash
composer install
```

### 3. Configure Environment

Copy the `.env` file and adjust settings if needed:

```bash
copy .env .env.local
```

### 4. Configure MCP in VS Code

Add your MCP server configuration to VS Code's `mcp.json` file.

**Location:** `%APPDATA%\Code\User\mcp.json`

```json
{
  "servers": {
    "my-mcp-server-52ab6668": {
      "url": "http://localhost:8080/mcp/sse"
    }
  },
  "inputs": []
}
```

## 🎯 Running the MCP Server

### Start the Server

```bash
php bin/mcp-server.php
```

You should see:

```
MCP Server is starting...
Discovering tools...
Base path: C:\wamp64\www\mcp_app
Scan dirs: src/Toolkit
Discovery complete
Starting HTTP server on http://127.0.0.1:8080/mcp/sse
```

### Keep the Server Running

The server must remain running for VS Code to communicate with it. Keep the terminal window open.

## 🛠️ Available MCP Tools

### 1. Clear Cache (`clear_cache`)

Clears the Symfony application cache for the dev environment.

**Usage in VS Code:**
- Ask GitHub Copilot: "Use the clear_cache tool"
- Or use Command Palette: "GitHub Copilot: Use MCP Tools"

**Example:**
```
User: "Clear the Symfony cache"
Copilot: [Executes clear_cache tool]
```

### 2. Create PDF with Date (`create_pdf_with_date`)

Creates a PDF document containing the current date and time.

**Parameters:**
- `title` (optional): Custom title for the document (default: "Date Document")
- `content` (optional): Additional content to include in the PDF

**Example:**
```
User: "Create a PDF with today's date"
Copilot: [Executes create_pdf_with_date tool]
```

**Output Location:** `var/pdfs/`

### 3. List Generated PDFs (`list_generated_pdfs`)

Lists all PDF files that have been generated.

**Example:**
```
User: "Show me all generated PDFs"
Copilot: [Executes list_generated_pdfs tool]
```

## 📁 Project Structure

```
mcp_app/
├── bin/
│   ├── console              # Symfony console
│   └── mcp-server.php       # MCP server entry point
├── config/                  # Symfony configuration
├── src/
│   ├── Kernel.php
│   └── Toolkit/             # MCP Tools directory
│       ├── AbstractToolkit.php
│       ├── CacheToolKit.php
│       ├── ConsoleRunner.php
│       └── PdfToolKit.php
├── var/
│   ├── cache/               # Symfony cache
│   ├── log/                 # Application logs
│   └── pdfs/                # Generated PDF files
├── vendor/                  # Composer dependencies
└── composer.json
```

## 🔍 Creating Custom MCP Tools

### Step 1: Create a New Toolkit Class

Create a new file in `src/Toolkit/`:

```php
<?php

namespace App\Toolkit;

use PhpMcp\Server\Attributes\McpTool;

class YourToolKit
{
    #[McpTool(
        name: 'your_tool_name',
        description: 'Description of what your tool does'
    )]
    public function yourMethod(string $param): array
    {
        // Your tool logic here
        
        return [
            'success' => true,
            'message' => 'Tool executed successfully'
        ];
    }
}
```

### Step 2: Restart the MCP Server

The server will automatically discover your new tool on restart.

```bash
php bin/mcp-server.php
```

### Step 3: Use Your Tool

Ask GitHub Copilot to use your new tool in VS Code!

## 🐛 Troubleshooting

### Server Won't Start

**Problem:** `Failed opening required autoload.php`

**Solution:** Run `composer install` to install dependencies.

### 404 Error on Connection

**Problem:** VS Code shows "404 status connecting to http://localhost:8080/mcp/sse"

**Solution:** 
1. Ensure the server is running (`php bin/mcp-server.php`)
2. Check that you're using `HttpServerTransport` (not `StdioServerTransport`)
3. Verify the URL in `mcp.json` is `http://localhost:8080/mcp/sse`

### Tools Not Discovered

**Problem:** "Discovered 0 tools"

**Solution:**
1. Check that your toolkit classes are in `src/Toolkit/`
2. Ensure the `#[McpTool]` attribute only uses valid parameters: `name`, `description`, `annotations`
3. Verify the class has a no-argument constructor or dependency injection is properly configured

### Windows STDIO Error

**Problem:** "STDIN and STDOUT are not supported on Windows"

**Solution:** Use `HttpServerTransport` instead of `StdioServerTransport` (already configured in this project).

## 📝 Development Notes

### Why HTTP Transport?

This project uses HTTP transport (`HttpServerTransport`) instead of STDIO because:
- PHP on Windows doesn't support non-blocking pipes required by STDIO transport
- HTTP transport with ReactPHP provides a reliable alternative
- SSE (Server-Sent Events) enables real-time communication

### Dependencies

Key packages:
- `php-mcp/server`: MCP server SDK for PHP
- `react/http`: ReactPHP HTTP server
- `tecnickcom/tcpdf`: PDF generation library
- `symfony/framework-bundle`: Symfony framework

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-tool`)
3. Create your MCP tool in `src/Toolkit/`
4. Commit your changes (`git commit -m 'Add amazing tool'`)
5. Push to the branch (`git push origin feature/amazing-tool`)
6. Open a Pull Request

## 📄 License

This project is licensed under the Proprietary License.

## 🔗 Resources

- [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- [PHP MCP Server Documentation](https://github.com/php-mcp/server)
- [Symfony Documentation](https://symfony.com/doc/current/index.html)
- [TCPDF Documentation](https://tcpdf.org/)

## 👤 Author

Your Name - [@your_github](https://github.com/YOUR_USERNAME)

## 🙏 Acknowledgments

- Model Context Protocol team
- Symfony community
- PHP MCP Server contributors
