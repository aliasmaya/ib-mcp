# ib-mcp: Interactive Brokers in FastMCP

## Overview

The `ib-mcp` project is a Model Context Protocol (MCP) server built using FastMCP that interfaces with the Interactive Brokers (IB) Trader Workstation (TWS) API via the `ib_insync` library. It provides a set of RESTful-like tools to interact with IB TWS for tasks such as connecting/disconnecting, retrieving account and market data, qualifying contracts, and placing orders. The server is designed to be used with MCP-compatible clients (e.g., Claude Desktop) and supports configuration via environment variables stored in a `.env` file.

This project aims to simplify the integration of IB TWS API functionality into applications that require trading, market data retrieval, and account management through a standardized MCP interface.

![GitHub](https://img.shields.io/github/license/aliasmaya/ib-mcp) 
![GitHub last commit](https://img.shields.io/github/last-commit/aliasmaya/ib-mcp) 
![Python](https://img.shields.io/badge/python-3.10%2B-blue)

## Features

- **Connection Management**: Connect to and disconnect from IB TWS or Gateway using environment-configured host, port, and client ID.
- **Market Data**: Request real-time market data for stocks and other securities.
- **Order Placement**: Place limit orders for trading.
- **Account Information**: Retrieve account values and current positions.
- **Contract Qualification**: Qualify contracts to ensure valid trading symbols and parameters.
- **Standardized Responses**: All tools return a consistent format with "result" (success/failed) and "message" (details or error).

## Installation

1. Clone this repository:

   ```bash
   git clone https://github.com/aliasmaya/ib-mcp.git
   cd ib-mcp
   ```

2. Install the required dependencies:

   ```bash
   pip install fastmcp ib_insync python-dotenv
   ```

3. Create a `.env` file in the project root directory with the following content:

   ```ini
   IB_HOST=127.0.0.1
   IB_PORT=7497
   IB_CLIENT=1
   ```

   - `IB_HOST`: The host address of your IB TWS or Gateway (default: "127.0.0.1").
   - `IB_PORT`: The port number for API access (default: 7497).
   - `IB_CLIENT`: The client ID for the API connection (default: 1).

   Adjust these values based on your IB TWS/Gateway configuration.

## Usage

Run the MCP server:

```bash
python ib.py
```

The server will start and expose the following tools via MCP:

- `connect()`: Establishes a connection to IB TWS.
- `disconnect()`: Terminates the IB TWS connection.
- `qualifyContracts(symbol, secType="STK", exchange="SMART", currency="USD")`: Qualifies a contract.
- `reqMktData(symbol, secType="STK", exchange="SMART", currency="USD")`: Requests market data.
- `placeOrder(symbol, action, quantity, limitPrice, secType="STK", exchange="SMART", currency="USD")`: Places a limit order.
- `positions(account="")`: Retrieves current account positions.
- `accountValues(account="")`: Retrieves account values.

Each tool returns a dictionary with two keys:

- `result`: Either "success" (if the operation succeeded) or "failed" (if it failed).
- `message`: A free-text message or data structure containing the result details or error description.

### Example Response

For `reqMktData("AAPL")`:

```json
{
    "result": "success",
    "message": {
        "symbol": "AAPL",
        "bid": 150.0,
        "ask": 150.5,
        "last": 150.2,
        "volume": 1000000
    }
}
```

Or for a failed operation:

```json
{
    "result": "failed",
    "message": "Not connected to IB TWS. Use 'connect' first."
}
```

## Contributing

Contributions are welcome! Please fork the repository, make your changes, and submit a pull request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- [FastMCP](https://github.com/jlowin/fastmcp) for the MCP server framework.
- [ib_insync](https://github.com/erdewit/ib_insync) for the IB TWS API wrapper.
