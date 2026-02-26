# 🚀 Antigravity Claude Proxy

A powerful local API proxy server for Claude and other AI models with built-in account management, rate limiting, and a beautiful web interface.

## ✨ Features

- 🔄 **Multi-Account Management**: Seamlessly switch between multiple Claude accounts
- 🌐 **Web UI Dashboard**: Beautiful web interface for managing accounts and monitoring usage
- ⚡ **Real-time Streaming**: Server-Sent Events (SSE) support for real-time responses
- 🔐 **OAuth Authentication**: Built-in OAuth2 flow for secure account management  
- 📊 **Rate Limit Management**: Intelligent rate limit handling across accounts
- 🎛️ **CLI Interface**: Command-line interface for account management
- 🧠 **Smart Account Selection**: Automatic account selection based on rate limits
- 📱 **Modern Web Interface**: Responsive dashboard with real-time logs

## 📋 Prerequisites

- **Node.js**: Version 18 or higher
- **npm**: Latest version
- **Linux/macOS/Windows**: Any modern operating system
- **Claude API Access**: Anthropic account with API access

## 📦 Installation

### Quick Install

```bash
# Step 1: Clone the repository
git clone https://github.com/vaibhavmaurya20/antigravity-local-api-proxy.git
cd antigravity-local-api-proxy

# Step 2: Install dependencies
npm install

# Step 3: Configure the proxy
cp config.example.json config.json

# Step 4: Start the server
npm start
```

### Detailed Installation Steps

#### 1. Extract the Project

If you have the zip file:

```bash
unzip antigravity-claude-proxy.zip
cd antigravity-claude-proxy
```

#### 2. Install Node.js Dependencies

```bash
# Install all required packages
npm install

# This will install:
# - express: Web server framework
# - anthropic: Official Claude SDK
# - express-rate-limit: Rate limiting middleware
# - sqlite3: Database for account storage
# - passport: Authentication middleware
# - winston: Logging system
# - And other dependencies...
```

#### 3. Configure the Server

Copy the example configuration file:

```bash
cp config.example.json config.json
```

Edit `config.json` with your settings:

```json
{
  "server": {
    "port": 3000,
    "host": "localhost"
  },
  "anthropic": {
    "apiVersion": "v1",
    "streaming": true,
    "timeout": 30000
  },
  "rateLimiting": {
    "windowMs": 60000,
    "maxRequests": 100
  },
  "logging": {
    "level": "info",
    "format": "combined"
  }
}
```

#### 4. Initialize the Database

The server will automatically create the SQLite database on first run.

## 🔐 Authentication Setup

### Method 1: OAuth Authentication (Recommended)

1. **Access the web UI**: Open `http://localhost:3000` in your browser
2. **Navigate to Accounts**: Click on "Accounts" in the navigation menu
3. **Initialize OAuth Flow**: Click "Add Account" and select "OAuth Login"
4. **Authorize Access**: Follow the Anthropic OAuth flow to authorize the proxy
5. **Save Credentials**: The proxy will securely store your authentication tokens

### Method 2: API Key Authentication

1. **Get your API key**: Visit [Anthropic Console](https://console.anthropic.com/)
2. **Access CLI**: Use the command-line interface

```bash
# Add account with API key
node bin/cli.js accounts add --name "My Account" --api-key "sk-ant-xxxx"

# Verify the account
node bin/cli.js accounts list
```

### Method 3: Web UI Configuration

1. **Open Dashboard**: Navigate to `http://localhost:3000`
2. **Go to Settings**: Click on "Settings" in the menu
3. **Add API Key**: Enter your Claude API key in the "API Configuration" section
4. **Test Connection**: Click "Test Connection" to verify
5. **Save Settings**: Click "Save Settings" to persist your configuration

## 🔄 Model Management & Switching

### Available Models

The proxy supports the latest Antigravity model lineup:

- `gemini-3-1-pro-high` - Gemini 3.1 Pro (high)
- `gemini-3-1-pro-low` - Gemini 3.1 Pro (low)
- `gemini-3-flash` - Gemini 3 Flash
- `claude-sonnet-4-5-thinking` - Claude Sonnet 4.5 (thinking)
- `claude-opus-4-5-thinking` - Claude Opus 4.5 (thinking)
- `claude-sonnet-4-6-thinking` - Claude Sonnet 4.6 (thinking)
- `claude-opus-4-6-thinking` - Claude Opus 4.6 (thinking)
- `gpt-oss-120b` - GPT-OSS-120b

### Switching Models

#### Via Web UI:

1. **Open Dashboard**: `http://localhost:3000`
2. **Navigate to Models**: Click "Models" in the top navigation
3. **Select Model**: Choose your desired model from the dropdown
4. **Set as Default**: Toggle "Set as Default" if desired
5. **Apply Changes**: Click "Apply" to save

#### Via API Request:

```bash
curl -X POST http://localhost:3000/api/chat   -H "Content-Type: application/json"   -d '{
    "model": "claude-sonnet-4-6-thinking",
    "messages": [
      {
        "role": "user",
        "content": "Hello, Claude!"
      }
    ],
    "max_tokens": 1024
  }'
```

#### Via CLI:

```bash
# Set default model
node bin/cli.js config set defaultModel "claude-sonnet-4-6-thinking"

# List available models
node bin/cli.js models list

# Get current model
node bin/cli.js config get defaultModel
```

### Model Selection Strategy

The proxy implements intelligent model selection:

1. **Default Model**: Used when no model is specified
2. **Request-Specific**: Model can be specified per-request
3. **Fallback Chain**: Automatically falls back if a model is unavailable
4. **Cost Optimization**: Can be configured to use cost-effective models by default

## 🚀 Usage Examples

### Basic Chat Completion

```bash
# Send a simple message
curl -X POST http://localhost:3000/api/chat   -H "Content-Type: application/json"   -d '{
    "model": "claude-sonnet-4-6-thinking",
    "messages": [
      {
        "role": "user", 
        "content": "Write a haiku about artificial intelligence."
      }
    ],
    "max_tokens": 150
  }'
```

### Streaming Response

```bash
# Enable streaming for real-time responses
curl -X POST http://localhost:3000/api/chat   -H "Content-Type: application/json"   -H "Accept: text/event-stream"   -d '{
    "model": "claude-sonnet-4-6-thinking",
    "messages": [{"role": "user", "content": "Count to 10 slowly"}],
    "max_tokens": 100,
    "stream": true
  }'
```

### Multi-Turn Conversation

```javascript
const conversation = [
  {
    role: "user",
    content: "What is machine learning?"
  },
  {
    role: "assistant", 
    content: "Machine learning is a type of artificial intelligence..."
  },
  {
    role: "user",
    content: "Can you give me a simple example?"
  }
];

// Send conversation history
fetch('http://localhost:3000/api/chat', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    model: "claude-sonnet-4-6-thinking",
    messages: conversation,
    max_tokens: 200
  })
});
```

### Using the CLI

```bash
# Send a message via CLI
node bin/cli.js chat "Explain quantum computing in simple terms"

# Send with specific model
node bin/cli.js chat --model "gemini-3-flash" "What is the weather like?"

# Interactive chat mode
node bin/cli.js chat --interactive

# Get account usage
node bin/cli.js accounts usage

# Monitor real-time logs
node bin/cli.js logs --tail
```

### Web UI Usage

1. **Access the Dashboard**: Open `http://localhost:3000`
2. **Send Messages**: Use the chat interface at the bottom
3. **View History**: Browse conversation history in the main pane
4. **Manage Accounts**: Switch between different Claude accounts
5. **Monitor Usage**: View token usage and API calls in real-time
6. **Settings**: Configure models, rate limits, and other options

### Advanced Configuration

#### Custom Model Settings

```json
{
  "models": {
    "claude-sonnet-4-6-thinking": {
      "maxTokens": 8192,
      "temperature": 0.7,
      "topP": 1.0
    },
    "gemini-3-flash": {
      "maxTokens": 4096,
      "temperature": 0.5,
      "topP": 1.0
    }
  }
}
```

#### Rate Limit Configuration

```json
{
  "rateLimiting": {
    "windowMs": 60000,
    "maxRequests": 100,
    "skipSuccessfulRequests": false,
    "skipFailedRequests": true
  }
}
```

#### Account Rotation

Enable automatic account switching when rate limits are reached:

```bash
# Enable account rotation
node bin/cli.js config set enableAccountRotation true

# Set rotation threshold (percentage of rate limit)
node bin/cli.js config set rotationThreshold 80
```

## 📊 Web UI Features

The web dashboard provides:

- **Real-time Chat**: Send messages and receive responses in real-time
- **Account Management**: Add, remove, and switch between Claude accounts
- **Model Selection**: Easy dropdown to switch between Claude models
- **Usage Analytics**: Visual charts showing token usage and costs
- **Log Viewer**: Real-time streaming logs of all API activity
- **Settings Panel**: Configure models, rate limits, and server settings
- **Conversation History**: Browse and search through past conversations

## 🔧 Configuration Options

### Server Configuration

Edit `config.json`:

```json
{
  "server": {
    "port": 3000,
    "host": "localhost",
    "ssl": {
      "enabled": false,
      "cert": "/path/to/cert.pem",
      "key": "/path/to/key.pem"
    }
  }
}
```

### Logging Configuration

```json
{
  "logging": {
    "level": "info",
    "format": "combined",
    "file": "logs/server.log",
    "maxSize": "10m",
    "maxFiles": 5
  }
}
```

### Anthropic API Configuration

```json
{
  "anthropic": {
    "apiVersion": "v1",
    "baseURL": "https://api.anthropic.com",
    "timeout": 30000,
    "maxRetries": 3,
    "streaming": true
  }
}
```

## 🐳 Docker Deployment

### Using Docker

```bash
# Build the Docker image
docker build -t antigravity-claude-proxy .

# Run the container
docker run -p 3000:3000   -v $(pwd)/config.json:/app/config.json   -v $(pwd)/data:/app/data   antigravity-claude-proxy
```

### Using Docker Compose

```yaml
version: '3.8'
services:
  claude-proxy:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - ./config.json:/app/config.json
      - ./data:/app/data
      - ./logs:/app/logs
    environment:
      - NODE_ENV=production
    restart: unless-stopped
```

Run with:

```bash
docker-compose up -d
```

## 🛠️ Development

### Project Structure

```
antigravity-claude-proxy/
├── src/
│   ├── server.js              # Main Express server
│   ├── cli/                   # Command-line interface
│   ├── webui/                 # Web interface handlers
│   ├── account-manager/       # Multi-account management
│   ├── cloudcode/             # Cloud API integration
│   └── utils/                 # Utility modules
├── public/                    # Static web assets
├── tests/                     # Test suite
├── bin/                       # Executable scripts
├── config.example.json        # Configuration template
└── package.json              # Dependencies and scripts
```

### Available Scripts

```bash
# Install dependencies
npm install

# Start the server
npm start

# Start with auto-restart (development)
npm run dev

# Run tests
npm test

# Run linter
npm run lint

# View logs
npm run logs
```

### Environment Variables

You can also configure via environment variables:

```bash
export ANTHROPIC_API_KEY="sk-ant-xxxx"
export SERVER_PORT=3000
export LOG_LEVEL=info
export NODE_ENV=production
```

Then run:

```bash
npm start
```

## 🔍 Troubleshooting

### Common Issues

#### "Authentication Failed"

- Verify your API key is correct and active
- Check that your Anthropic account has API access
- Ensure the API key hasn't exceeded rate limits

#### "Rate Limit Exceeded"

- The proxy will automatically switch accounts if multiple are configured
- Check the web UI for account usage statistics
- Consider upgrading your Anthropic account for higher limits

#### "Server Won't Start"

- Check that port 3000 is not in use:
  ```bash
  lsof -i :3000
  # Or use a different port
  export SERVER_PORT=3001
  ```

- Verify Node.js version:
  ```bash
  node --version  # Should be 18+
  ```

#### "Database Errors"

- Ensure write permissions in the data directory
- Check SQLite3 installation:
  ```bash
  npm rebuild sqlite3
  ```

### Debug Mode

Enable debug logging:

```bash
# Set debug mode
export DEBUG=claude-proxy*

# Or in config.json
{
  "logging": {
    "level": "debug"
  }
}
```

## 📚 API Reference

### POST /api/chat

Main chat completion endpoint.

**Request Body:**
```json
{
  "model": "claude-sonnet-4-6-thinking",
  "messages": [
    {"role": "user", "content": "Hello!"}
  ],
  "max_tokens": 1024,
  "temperature": 0.7,
  "stream": false
}
```

**Response:**
```json
{
  "id": "msg_12345",
  "model": "claude-sonnet-4-6-thinking",
  "content": "Hello! How can I help you today?",
  "usage": {
    "input_tokens": 10,
    "output_tokens": 25
  }
}
```

### GET /api/models

List available models.

**Response:**
```json
{
  "models": [
    {"id": "claude-sonnet-4-6-thinking", "name": "Claude Sonnet 4.6 (thinking)"},
    {"id": "gemini-3-flash", "name": "Gemini 3 Flash"}
  ]
}
```

### GET /api/usage

Get account usage statistics.

**Response:**
```json
{
  "account": "My Account",
  "usage": {
    "today": {"requests": 150, "tokens": 45000},
    "this_month": {"requests": 3200, "tokens": 980000}
  }
}
```

## 🔒 Security Features

- **Token Encryption**: API keys are encrypted at rest
- **Rate Limiting**: Built-in rate limiting per account
- **CORS Protection**: Configurable CORS policies
- **Request Validation**: Input sanitization and validation
- **Audit Logging**: All API calls are logged for security monitoring

## 📱 Web UI Interface

The web dashboard includes:

- **Clean Chat Interface**: Modern, responsive design
- **Real-time Updates**: Live updates of responses and logs
- **Account Switcher**: Dropdown to switch between configured accounts
- **Model Selector**: Easy model switching
- **Usage Charts**: Visual representation of token usage
- **Log Viewer**: Filter and search through API logs
- **Settings Panel**: Configure all server options

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch:
   ```bash
   git checkout -b feature/amazing-feature
   ```
3. Make your changes and add tests
4. Run tests:
   ```bash
   npm test
   ```
5. Commit your changes:
   ```bash
   git commit -m 'Add amazing feature'
   ```
6. Push to the branch:
   ```bash
   git push origin feature/amazing-feature
   ```
7. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Anthropic for the Claude API
- Express.js community
- All contributors and testers

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/vaibhavmaurya20/antigravity-local-api-proxy/issues)
- **Documentation**: [Wiki](https://github.com/vaibhavmaurya20/antigravity-local-api-proxy/wiki)
- **Discussions**: [GitHub Discussions](https://github.com/vaibhavmaurya20/antigravity-local-api-proxy/discussions)

---

**Made with ❤️ by the Antigravity Team**

"Defying gravity, one API call at a time! 🚀"
