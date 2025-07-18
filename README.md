# rangerMCP
# Ranger MCP Server

A Model Context Protocol (MCP) server that connects to public Slack channels for Merchant Feedback on the Shop App to fetch messages, threads, and conversations. Designed to work with LibreChat and other agents (eg: Scout) for creating merchant feedback digests.

## Features

- **Channel Access**: Connect to public Shopify Slack channels
- **Message Retrieval**: Fetch messages, threads, and conversations
- **Search & Filter**: Search messages by content, date, and user
- **Thread Support**: Access full conversation threads
- **Integration Ready**: Works with LibreChat and other MCP agents

## Prerequisites

- Python 3.8+
- Slack Bot Token (for public channels)
- Required Python packages (see `requirements.txt`)

## Installation

1. Clone or download this MCP server
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Configure your Slack bot token (see Configuration section)

## Configuration

### Slack Bot Setup

1. Create a Slack app at https://api.slack.com/apps
2. Add the following OAuth scopes:
   - `channels:read`
   - `groups:read`
   - `im:read`
   - `mpim:read`
   - `chat:write`
   - `users:read`
   - `users:read.email`
3. Install the app to your workspace
4. Copy the Bot User OAuth Token

### Environment Variables

Create a `.env` file in the server directory:

```env
SLACK_BOT_TOKEN=xoxb-your-bot-token-here
SLACK_APP_TOKEN=xapp-your-app-token-here
```

### LibreChat Configuration

Add this to your LibreChat MCP configuration:

```json
{
  "mcpServers": {
    "shopify-slack": {
      "command": "python",
      "args": ["/path/to/shopify-slack-mcp/server.py"],
      "env": {
        "SLACK_BOT_TOKEN": "xoxb-your-bot-token-here"
      }
    }
  }
}
```

## Available Tools

### `list_channels`
Lists all accessible Slack channels.

**Parameters:**
- `include_private` (optional): Include private channels (default: false)

**Returns:** List of channel information including ID, name, and member count.

### `get_channel_messages`
Retrieves messages from a specific channel.

**Parameters:**
- `channel_id` (required): Slack channel ID
- `limit` (optional): Number of messages to retrieve (default: 100, max: 1000)
- `oldest` (optional): Start time as Unix timestamp
- `latest` (optional): End time as Unix timestamp

**Returns:** List of messages with metadata.

### `search_messages`
Search for messages across channels or within a specific channel.

**Parameters:**
- `query` (required): Search query
- `channel_id` (optional): Limit search to specific channel
- `limit` (optional): Number of results (default: 20, max: 100)

**Returns:** Search results with message content and metadata.

### `get_thread_replies`
Retrieves all replies in a thread.

**Parameters:**
- `channel_id` (required): Channel ID
- `thread_ts` (required): Thread timestamp

**Returns:** All messages in the thread.

### `get_user_info`
Retrieves information about a Slack user.

**Parameters:**
- `user_id` (required): Slack user ID

**Returns:** User profile information.

## Usage Examples

### Basic Channel Access
```
List all public Shopify channels
```

### Message Retrieval
```
Get the last 50 messages from #merchant-feedback channel
```

### Search Functionality
```
Search for messages containing "checkout issues" in the last 30 days
```

### Thread Analysis
```
Get all replies in the thread starting with message timestamp 1234567890.123456
```

## Integration with LibreChat

This MCP server is designed to work seamlessly with LibreChat. You can:

1. **Chain with Scout**: Use Scout MCP to analyze merchant feedback patterns
2. **Create Digests**: Automatically generate merchant feedback summaries
3. **Monitor Trends**: Track recurring issues and concerns
4. **Generate Reports**: Create structured reports from Slack conversations

### Example LibreChat Agent Chain

```
Shopify Slack MCP → Scout MCP → Report Generator
```

1. **Shopify Slack MCP**: Fetches relevant messages from merchant channels
2. **Scout MCP**: Analyzes patterns and categorizes feedback
3. **Report Generator**: Creates structured digest reports

## Error Handling

The server includes comprehensive error handling for:
- Invalid Slack tokens
- Rate limiting
- Channel access permissions
- Network connectivity issues

## Rate Limits

Slack API has rate limits. The server implements:
- Automatic retry with exponential backoff
- Rate limit detection and handling
- Request throttling to stay within limits

## Security

- Bot tokens are stored securely
- Only public channel access (no private data)
- No message content is stored locally
- All requests are logged for debugging

## Troubleshooting

### Common Issues

1. **"Invalid token" error**
   - Verify your Slack bot token is correct
   - Ensure the bot has been added to your workspace

2. **"Channel not found" error**
   - Check that the channel ID is correct
   - Ensure the bot has access to the channel

3. **Rate limiting**
   - The server will automatically retry
   - Consider reducing request frequency

### Debug Mode

Enable debug logging by setting:
```env
DEBUG=true
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

MIT License - see LICENSE file for details.

## Support

For issues and questions:
- Check the troubleshooting section
- Review Slack API documentation
- Open an issue in the repository
