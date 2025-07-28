# Google Analytics MCP Server 📊

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![FastMCP](https://img.shields.io/badge/FastMCP-v2.8.0-green.svg)](https://github.com/jlowin/fastmcp)

**A FastMCP-powered Model Context Protocol server for Google Analytics 4 API integration with automatic OAuth 2.0 authentication**

Connect Google Analytics 4 data directly to Claude Desktop and other MCP clients with seamless OAuth 2.0 authentication, automatic token refresh, comprehensive reporting, and analytics capabilities.

## ✨ Features

- 🔐 **Automatic OAuth 2.0** - One-time browser authentication with auto-refresh
- 🔄 **Smart Token Management** - Handles expired tokens automatically
- 📊 **Comprehensive Reporting** - Access all GA4 metrics and dimensions
- 🏢 **Property Management** - List and manage Google Analytics properties
- 📈 **Advanced Analytics** - Page views, users, events, traffic sources, and more
- 🚀 **FastMCP Framework** - Built on the modern MCP standard
- 🖥️ **Claude Desktop Ready** - Direct integration with Claude Desktop
- 🛡️ **Secure Local Storage** - Tokens stored locally, never exposed

## 📋 Available Tools

| Tool | Description | Parameters | Example Usage |
|------|-------------|------------|---------------|
| `list_properties` | List all GA4 accounts and properties | `account_id` (optional) | "List all my Google Analytics properties" |
| `get_page_views` | Get page view metrics | `property_id`, `start_date`, `end_date`, `dimensions` (optional) | "Show me page views for last month" |
| `get_active_users` | Get active users metrics | `property_id`, `start_date`, `end_date`, `dimensions` (optional) | "Get active users by day for last week" |
| `get_events` | Get event metrics | `property_id`, `start_date`, `end_date`, `dimensions` (optional) | "Show me events data for property 123456789" |
| `get_traffic_sources` | Get traffic source data | `property_id`, `start_date`, `end_date`, `dimensions` (optional) | "Analyze traffic sources for last 30 days" |
| `get_device_metrics` | Get device-based metrics | `property_id`, `start_date`, `end_date`, `dimensions` (optional) | "Show device breakdown for last month" |
| `run_report` | Comprehensive custom reporting | `property_id`, `start_date`, `end_date`, `metrics`, `dimensions`, filters, etc. | "Create custom report with sessions and conversions by country" |

**Note:** All tools automatically handle authentication - no token parameters required!

## 🚀 Quick Start

### Prerequisites

Before setting up the MCP server, you'll need:
- Python 3.10+ installed
- A Google Cloud Platform account
- A Google Analytics 4 property with data access

## 🔧 Step 1: Google Cloud Platform Setup

### 1.1 Create Google Cloud Project

1. **Go to [Google Cloud Console](https://console.cloud.google.com/)**
2. **Create a new project:**
   - Click "Select a project" → "New Project"
   - Enter project name (e.g., "Google Analytics MCP")
   - Click "Create"

### 1.2 Enable Google Analytics APIs

1. **In your Google Cloud Console:**
   - Go to "APIs & Services" → "Library"
   - Search for "Google Analytics Data API" and enable it
   - Search for "Google Analytics Admin API" and enable it

### 1.3 Create OAuth 2.0 Credentials

1. **Go to "APIs & Services" → "Credentials"**
2. **Click "+ CREATE CREDENTIALS" → "OAuth 2.0 Client ID"**
3. **Configure consent screen (if first time):**
   - Click "Configure Consent Screen"
   - Choose "External" (unless you have Google Workspace)
   - Fill required fields:
     - App name: "Google Analytics MCP"
     - User support email: Your email
     - Developer contact: Your email
   - Add scopes:
     - `https://www.googleapis.com/auth/analytics`
     - `https://www.googleapis.com/auth/analytics.readonly`
   - Click "Save and Continue" through all steps
4. **Create OAuth Client:**
   - Application type: **"Desktop application"**
   - Name: "Google Analytics MCP Client"
   - Click "Create"
5. **Download credentials:**
   - Click "Download JSON" button
   - Save file as `client_secret_[long-string].json` in your project directory

## 🔧 Step 2: Google Analytics Access

### 2.1 Ensure Analytics Access

1. **Sign in to [Google Analytics](https://analytics.google.com)**
2. **Verify you have access to GA4 properties**
3. **Note your property IDs** (found in GA4 Admin → Property Settings)
4. **Ensure your Google account has at least Viewer access** to the properties you want to query

## 🔧 Step 3: Installation & Setup

### 3.1 Clone and Install

```bash
# Clone the repository
git clone https://github.com/yourusername/google-analytics-mcp-server.git
cd google-analytics-mcp-server

# Create virtual environment (recommended)
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 3.2 Environment Configuration

Create a `.env` file in your project directory:

```bash
# Copy the example file
cp .env.example .env
```

Edit `.env` with your credentials:

```bash
# Required: Path to OAuth credentials JSON file (downloaded from Google Cloud)
GOOGLE_ANALYTICS_OAUTH_CONFIG_PATH=/full/path/to/your/client_secret_file.json
```

**Example `.env` file:**
```bash
GOOGLE_ANALYTICS_OAUTH_CONFIG_PATH=/Users/john/google-analytics-mcp/client_secret_138737274875-abc123.apps.googleusercontent.com.json
```

## 🖥️ Step 4: Claude Desktop Integration

### 4.1 Locate Claude Configuration

Find your Claude Desktop configuration file:

**macOS:**
```bash
~/Library/Application Support/Claude/claude_desktop_config.json
```

**Windows:**
```bash
%APPDATA%\Claude\claude_desktop_config.json
```

### 4.2 Add MCP Server Configuration

Edit the configuration file and add your Google Analytics MCP server:

```json
{
  "mcpServers": {
    "google-analytics": {
      "command": "/full/path/to/your/project/.venv/bin/python",
      "args": [
        "/full/path/to/your/project/server.py"
      ]
    }
  }
}
```

**Real Example:**
```json
{
  "mcpServers": {
    "google-analytics": {
      "command": "/Users/marble-dev-01/workspace/google_analytics_mcp/.venv/bin/python",
      "args": [
        "/Users/marble-dev-01/workspace/google_analytics_mcp/server.py"
      ]
    }
  }
}
```

**Important:** 
- Use **absolute paths** for all file locations
- On Windows, use forward slashes `/` or double backslashes `\\` in paths

### 4.3 Restart Claude Desktop

Close and restart Claude Desktop to load the new configuration.

## 🔐 Step 5: First-Time Authentication

### 5.1 Trigger OAuth Flow

1. **Open Claude Desktop**
2. **Try any Google Analytics command**, for example:
   ```
   "List all my Google Analytics properties"
   ```

### 5.2 Complete Authentication

1. **Browser opens automatically** to Google OAuth page
2. **Sign in** with your Google account (the one with Analytics access)
3. **Grant permissions** by clicking "Allow"
4. **Browser shows success page**
5. **Return to Claude** - your command will complete automatically!

### 5.3 Verify Setup

After authentication, you should see:
- A `google_analytics_token.json` file created in your project directory
- Your Google Analytics properties listed in Claude's response

## 📖 Usage Examples

### Property Management

```
"List all my Google Analytics properties"

"Show me properties for account 123456789"

"What GA4 properties do I have access to?"
```

### Page View Analysis

```
"Get page views for property 421301275 from 2025-01-01 to 2025-01-31"

"Show me top pages by page views for last month for property 421301275"

"Analyze page performance by country for property 421301275"
```

### User Analytics

```
"Get active users for property 421301275 in the last 7 days"

"Show me user metrics by device category for property 421301275"

"Compare new vs returning users for last month"
```

### Traffic Source Analysis

```
"Analyze traffic sources for property 421301275 from 2025-01-01 to 2025-01-31"

"Show me which channels drive the most users to my site"

"Compare organic vs paid traffic performance"
```

### Event Tracking

```
"Get events data for property 421301275 in the last 30 days"

"Show me conversion events by source/medium"

"Which events are most popular on my site?"
```

### Custom Reports

```
"Create a report for property 421301275 with sessions, users, and page views by country from 2025-01-01 to 2025-01-31"

"Run a custom report showing bounce rate and engagement rate by device category"

"Generate a comprehensive traffic report with sessions, conversions, and revenue by source/medium"
```

## 🔍 Advanced GA4 Examples

### Sessions and Users by Country
```python
run_report(
    property_id="421301275",
    start_date="2025-01-01",
    end_date="2025-01-31",
    metrics=["sessions", "totalUsers", "screenPageViews"],
    dimensions=["country"],
    limit=20
)
```

### Device Performance Analysis
```python
run_report(
    property_id="421301275",
    start_date="2025-01-01",
    end_date="2025-01-31",
    metrics=["sessions", "bounceRate", "engagementRate"],
    dimensions=["deviceCategory", "operatingSystem"],
    limit=50
)
```

### Traffic Sources with Conversions
```python
run_report(
    property_id="421301275",
    start_date="2025-01-01",
    end_date="2025-01-31",
    metrics=["sessions", "conversions", "totalRevenue"],
    dimensions=["source", "medium", "campaignName"],
    limit=100
)
```

### Daily Trend Analysis
```python
run_report(
    property_id="421301275",
    start_date="2025-01-01",
    end_date="2025-01-31",
    metrics=["sessions", "activeUsers", "screenPageViews"],
    dimensions=["date"],
    limit=31
)
```

## 📁 Project Structure

```
google-analytics-mcp-server/
├── server.py                              # Main MCP server
├── oauth/
│   ├── __init__.py                        # Package initialization
│   └── google_auth.py                     # OAuth authentication logic
├── google_analytics_token.json            # Auto-generated token storage (gitignored)
├── client_secret_[long-string].json       # Your OAuth credentials (gitignored)
├── .env                                   # Environment variables (gitignored)
├── .env.example                           # Environment template
├── .gitignore                             # Git ignore file
├── requirements.txt                       # Python dependencies
├── LICENSE                                # MIT License
└── README.md                              # This file
```

## 🔒 Security & Best Practices

### File Security
- ✅ **Credential files are gitignored** - Never committed to version control
- ✅ **Local token storage** - Tokens stored in `google_analytics_token.json` locally
- ✅ **Environment variables** - Sensitive data in `.env` file
- ✅ **Automatic refresh** - Minimal token exposure time

### Recommended File Permissions
```bash
# Set secure permissions for sensitive files
chmod 600 .env
chmod 600 google_analytics_token.json
chmod 600 client_secret_*.json
```

### Production Considerations
1. **Use environment variables** instead of `.env` files in production
2. **Implement rate limiting** to respect API quotas
3. **Monitor API usage** in Google Cloud Console
4. **Secure token storage** with proper access controls
5. **Regular token rotation** for enhanced security

## 🛠️ Troubleshooting

### Authentication Issues

| Issue | Symptoms | Solution |
|-------|----------|----------|
| **No tokens found** | "Starting OAuth flow" message | ✅ Normal for first-time setup - complete browser authentication |
| **Token refresh failed** | "Refreshing token failed" error | ✅ Delete `google_analytics_token.json` and re-authenticate |
| **OAuth flow failed** | Browser error or no response | Check credentials file path and internet connection |
| **Permission denied** | "Access denied" in browser | Ensure Google account has Analytics access |

### Configuration Issues

| Issue | Symptoms | Solution |
|-------|----------|----------|
| **Environment variables missing** | "Environment variable not set" | Check `.env` file and Claude config `env` section |
| **File not found** | "FileNotFoundError" | Verify absolute paths in configuration |
| **Module import errors** | "ModuleNotFoundError" | Run `pip install -r requirements.txt` |
| **Python path issues** | "Command not found" | Use absolute path to Python executable |

### Claude Desktop Issues

| Issue | Symptoms | Solution |
|-------|----------|----------|
| **Server not connecting** | No Google Analytics tools available | Restart Claude Desktop, check config file syntax |
| **Invalid JSON config** | Claude startup errors | Validate JSON syntax in config file |
| **Permission errors** | "Permission denied" on startup | Check file permissions and paths |

### API Issues

| Issue | Symptoms | Solution |
|-------|----------|----------|
| **Invalid property ID** | "Property not found" | Use numeric format: `421301275` |
| **API quota exceeded** | "Quota exceeded" error | Wait for quota reset or request increase |
| **Invalid date format** | "Invalid date" | Use YYYY-MM-DD format: `2025-01-31` |
| **No data returned** | Empty results | Check date range and property access |

### Debug Mode

Enable detailed logging for troubleshooting:

```python
# Add to server.py for debugging
import logging
logging.basicConfig(level=logging.DEBUG)
```

## 🚀 Advanced Configuration

### HTTP Transport Mode

For web deployment or remote access:

```bash
# Start server in HTTP mode
python3 server.py --http
```

**Claude Desktop config for HTTP:**
```json
{
  "mcpServers": {
    "google-analytics": {
      "url": "http://127.0.0.1:8000/mcp"
    }
  }
}
```

### Custom Token Storage

Modify token storage location in `oauth/google_auth.py`:

```python
# Custom token file location
def get_token_path():
    return "/custom/secure/path/google_analytics_token.json"
```

## 🤝 Contributing

We welcome contributions! Here's how to get started:

### Development Setup

```bash
# Fork and clone the repository
git clone https://github.com/yourusername/google-analytics-mcp-server.git
cd google-analytics-mcp-server

# Create development environment
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Set up development environment
cp .env.example .env
# Add your development credentials to .env
```

### Making Changes

1. **Create a feature branch:** `git checkout -b feature/amazing-feature`
2. **Make your changes** with appropriate tests
3. **Test thoroughly** with different property configurations
4. **Update documentation** as needed
5. **Commit changes:** `git commit -m 'Add amazing feature'`
6. **Push to branch:** `git push origin feature/amazing-feature`
7. **Open a Pull Request** with detailed description

## 📊 API Limits and Quotas

### Google Analytics API Quotas

- **Core Reporting API:** 100,000 requests per day per project
- **Realtime API:** 10,000 requests per day per project
- **Request rate:** 10 queries per second per project

### Best Practices for API Usage

1. **Cache results** when possible to reduce API calls
2. **Use appropriate date ranges** to limit data volume
3. **Batch requests** when supported
4. **Monitor usage** in Google Cloud Console
5. **Implement retry logic** for rate limit errors

### Quota Management

```bash
# Monitor usage in Google Cloud Console
# Go to APIs & Services → Quotas
# Search for "Google Analytics" to see current usage
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

### MIT License

```
Copyright (c) 2025 Google Analytics MCP Server Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## 📈 Roadmap

### Upcoming Features
- 🔄 **Enhanced real-time analytics** with streaming data
- 📊 **Built-in data visualization** with charts and graphs
- 🤖 **AI-powered insights** and anomaly detection
- 📝 **Custom dashboard creation** tools
- 🔍 **Advanced segmentation** capabilities
- 🌐 **Multi-property reporting**

---

**Made with ❤️ for the MCP community**

*Connect your Google Analytics 4 data directly to AI assistants and unlock powerful web analytics insights through natural language conversations.*
