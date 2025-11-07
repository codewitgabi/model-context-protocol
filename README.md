# Model Context Protocol (MCP) Project

A production-ready implementation of the Model Context Protocol featuring a weather service MCP server and an interactive test client that integrates with Anthropic's Claude API.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Development](#development)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Overview

This project demonstrates the Model Context Protocol (MCP) through a practical weather service implementation. It consists of two main components:

1. **Weather MCP Server**: A server that exposes weather-related tools (alerts and forecasts) using the National Weather Service (NWS) API
2. **Test Client**: An interactive client that connects to MCP servers and uses Claude AI to intelligently process queries with available tools

The MCP server provides tools that can be discovered and invoked by MCP-compatible clients, enabling AI models to interact with external data sources and services.

## ✨ Features

### Weather MCP Server

- **Weather Alerts**: Retrieve active weather alerts for any US state
- **Weather Forecasts**: Get detailed forecasts for any location using latitude/longitude coordinates
- **NWS API Integration**: Direct integration with the National Weather Service API
- **Error Handling**: Robust error handling and graceful degradation
- **FastMCP Framework**: Built on FastMCP for efficient tool registration and execution

### Test Client

- **Interactive Chat Interface**: Command-line interface for querying MCP servers
- **Claude AI Integration**: Uses Anthropic's Claude Sonnet 4 for intelligent query processing
- **Tool Discovery**: Automatically discovers and lists available tools from connected servers
- **Tool Execution**: Seamlessly executes tools based on AI model decisions
- **Multi-turn Conversations**: Supports complex multi-step interactions with tool calls

## 🏗️ Architecture

```
┌─────────────────┐
│   Test Client   │
│  (Claude AI)    │
└────────┬────────┘
         │
         │ MCP Protocol (stdio)
         │
┌────────▼────────┐
│ Weather MCP     │
│    Server       │
└────────┬────────┘
         │
         │ HTTP/HTTPS
         │
┌────────▼────────┐
│   NWS API       │
│  (weather.gov)  │
└─────────────────┘
```

The architecture follows the MCP specification:

- **Client-Server Communication**: Uses stdio transport for communication
- **Tool Registration**: Server exposes tools via MCP protocol
- **Tool Invocation**: Client can discover and invoke tools dynamically
- **AI Integration**: Client uses Claude to intelligently decide when and how to use tools

## 📦 Prerequisites

- **Python 3.13+**: Both components require Python 3.13 or higher
- **Anthropic API Key**: Required for the test client (get one at [console.anthropic.com](https://console.anthropic.com))
- **Package Manager**: [uv](https://github.com/astral-sh/uv) is recommended (or pip)

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd model-context-protocol
```

### 2. Install Weather MCP Server

```bash
cd mcp-servers/weather-mcp
uv sync  # or: pip install -e .
```

### 3. Install Test Client

```bash
cd ../../mcp-clients/test-client
uv sync  # or: pip install -e .
```

## ⚙️ Configuration

### Test Client Environment Variables

Create a `.env` file in the `mcp-clients/test-client/` directory:

```env
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

**Note**: Never commit your `.env` file to version control. It's already included in `.gitignore`.

## 💻 Usage

### Running the Weather MCP Server

From the `mcp-servers/weather-mcp/` directory:

```bash
python main.py
```

The server will start and wait for client connections via stdio.

### Running the Test Client

From the `mcp-clients/test-client/` directory:

```bash
python main.py ../weather-mcp/main.py
```

Replace `../weather-mcp/main.py` with the path to your MCP server script.

### Example Interactions

Once the client is running, you can interact with it:

```
Query: What are the weather alerts for California?
Query: Get the forecast for San Francisco (latitude 37.7749, longitude -122.4194)
Query: Are there any active alerts in New York?
Query: quit
```

The client will:

1. Connect to the weather MCP server
2. Discover available tools
3. Use Claude AI to understand your query
4. Automatically invoke the appropriate tools
5. Return formatted results

## 📁 Project Structure

```
model-context-protocol/
├── README.md                    # This file
├── mcp-servers/
│   └── weather-mcp/
│       ├── main.py              # Server entry point
│       ├── pyproject.toml       # Server dependencies
│       ├── README.md            # Server-specific documentation
│       └── utils/
│           ├── __init__.py
│           ├── constants.py     # API constants (NWS base URL, user agent)
│           ├── helpers.py       # HTTP request helpers and formatting
│           └── tool_execution_handler.py  # MCP tool definitions
└── mcp-clients/
    └── test-client/
        ├── main.py              # Client entry point with chat loop
        ├── pyproject.toml       # Client dependencies
        └── README.md            # Client-specific documentation
```

### Key Files

- **`mcp-servers/weather-mcp/main.py`**: Server entry point that runs the MCP server
- **`mcp-servers/weather-mcp/utils/tool_execution_handler.py`**: Defines MCP tools (`get_alerts`, `get_forecast`)
- **`mcp-servers/weather-mcp/utils/helpers.py`**: HTTP client and data formatting utilities
- **`mcp-clients/test-client/main.py`**: Client implementation with Claude AI integration

## 🔧 Development

### Adding New Tools to the Weather Server

1. Edit `mcp-servers/weather-mcp/utils/tool_execution_handler.py`
2. Add a new function decorated with `@mcp.tool()`
3. Define the function signature and docstring (used for tool schema)
4. Implement the tool logic

Example:

```python
@mcp.tool()
async def get_current_conditions(latitude: float, longitude: float) -> str:
    """Get current weather conditions for a location.

    Args:
        latitude: Latitude of the location
        longitude: Longitude of the location
    """
    # Implementation here
    pass
```

### Testing

Run the server and client in separate terminals to test:

**Terminal 1 (Server):**

```bash
cd mcp-servers/weather-mcp
python main.py
```

**Terminal 2 (Client):**

```bash
cd mcp-clients/test-client
python main.py ../../mcp-servers/weather-mcp/main.py
```

### Code Style

- Follow PEP 8 for Python code style
- Use type hints for function parameters and return types
- Include docstrings for all public functions and classes

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Contribution Guidelines

- Write clear, descriptive commit messages
- Add tests for new features when applicable
- Update documentation as needed
- Ensure code follows existing style conventions

## 📝 License

[Add your license information here]

## 🔗 Resources

- [Model Context Protocol Specification](https://modelcontextprotocol.io)
- [Anthropic Claude API Documentation](https://docs.anthropic.com)
- [National Weather Service API](https://www.weather.gov/documentation/services-web-api)
- [FastMCP Documentation](https://github.com/jlowin/fastmcp)

## 🐛 Troubleshooting

### Client can't connect to server

- Ensure the server script path is correct
- Verify Python 3.13+ is installed
- Check that all dependencies are installed

### API errors

- Verify your Anthropic API key is set correctly in `.env`
- Check your internet connection for NWS API calls
- Ensure the NWS API is accessible (it's a public API, no key required)

### Import errors

- Run `uv sync` or `pip install -e .` in the respective directories
- Verify you're using Python 3.13 or higher
- Check that virtual environments are activated if using them

## 📧 Support

For issues, questions, or contributions, please open an issue on the repository.

---

**Built with ❤️ using the Model Context Protocol**
