Getting Started

# Model context protocol (MCP)

* * *

The [Model Context Protocol](https://modelcontextprotocol.io/introduction) (MCP) is a standard for connecting Large Language Models (LLMs) to external services. This guide will walk you through how to connect AI tools to Supabase using MCP.

There are a number of popular AI tools that support MCP, including:

- [Cursor](https://www.cursor.com/)
- [Claude desktop](https://claude.ai/download)
- [Cline](https://github.com/cline/cline) (VS Code extension)
- [Windsurf](https://docs.codeium.com/windsurf) (Codium)

Connecting these tools to Supabase will allow you to query your database and perform other SQL operations using natural language commands.

## Connect to Supabase using MCP [\#](https://supabase.com/docs/guides/getting-started/mcp\#connect-to-supabase-using-mcp)

We will use the [Postgres MCP server](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres) to connect AI tools to Supabase.

### Step 1: Find your database connection string [\#](https://supabase.com/docs/guides/getting-started/mcp\#step-1-find-your-database-connection-string)

To get started, you will need to retrieve your database connection string. These will differ depending on whether you are using a local or hosted instance of Supabase.

#### For a local Supabase instance [\#](https://supabase.com/docs/guides/getting-started/mcp\#for-a-local-supabase-instance)

When running a local instance of Supabase via the [CLI](https://supabase.com/docs/reference/cli/introduction), you can find your connection string by running:

`
supabase status
`

or if you are using `npx`:

`
npx supabase status
`

This will output a list of details about your local Supabase instance. Copy the `DB URL` field in the output.

#### For a hosted Supabase instance [\#](https://supabase.com/docs/guides/getting-started/mcp\#for-a-hosted-supabase-instance)

When running a hosted instance of Supabase, you can find your connection string by:

1. Navigating to your project's [Connection settings](https://supabase.com/dashboard/project/_/settings/database?showConnect=true)
2. Copying the connection string found under **Session pooler**.

### Step 2: Configure in your AI tool [\#](https://supabase.com/docs/guides/getting-started/mcp\#step-2-configure-in-your-ai-tool)

All MCP compatible tools can connect to Supabase using the [Postgres MCP server](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres). Pass the following CLI command to your tool:

`
npx -y @modelcontextprotocol/server-postgres <connection-string>
`

Replace `<connection-string>` with the connection string you retrieved in Step 1.

This assumes you have Node.js and npx installed. If you don't have Node.js or prefer to connect to the server using Docker, you can follow the instructions in the [Postgres MCP server README](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres#docker).

Below are some ways to connect to the Postgres MCP server using popular AI tools:

#### Cursor [\#](https://supabase.com/docs/guides/getting-started/mcp\#cursor)

1. Open Cursor and open `.cursor/mcp.json` file. Create the file if it doesn't exist.

2. Add the following configuration:



`
{
"mcpServers": {
       "supabase": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-postgres", "<connection-string>"]
       }
}
}
`



Replace `<connection-string>` with your connection string.

3. Save the configuration file.

4. Open Cursor and navigate to **Settings/MCP**. You should see a green active status after the server is successfully connected.


#### Claude desktop [\#](https://supabase.com/docs/guides/getting-started/mcp\#claude-desktop)

1. Open Claude desktop and navigate to **Settings**.

2. Under the **Developer** tab, tap **Edit Config** to open the configuration file.

3. Add the following configuration:



`
{
"mcpServers": {
       "supabase": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-postgres", "<connection-string>"]
       }
}
}
`



Replace `<connection-string>` with your connection string.

4. Save the configuration file and restart Claude desktop.

5. From the new chat screen, you should see a hammer (MCP) icon appear with the new MCP server available.


#### Cline [\#](https://supabase.com/docs/guides/getting-started/mcp\#cline)

1. Open the Cline extension in VS Code and tap the **MCP Servers** icon.

2. Tap **Configure MCP Servers** to open the configuration file.

3. Add the following configuration:



`
{
"mcpServers": {
       "supabase": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-postgres", "<connection-string>"]
       }
}
}
`



Replace `<connection-string>` with your connection string.

4. Save the configuration file. Cline should automatically reload the configuration.

5. You should see a green active status after the server is successfully connected.


#### Windsurf [\#](https://supabase.com/docs/guides/getting-started/mcp\#windsurf)

1. Open Windsurf and navigate to the Cascade assistant.

2. Tap on the hammer (MCP) icon, then **Configure** to open the configuration file.

3. Add the following configuration:



`
{
"mcpServers": {
       "supabase": {
         "command": "npx",
         "args": ["-y", "@modelcontextprotocol/server-postgres", "<connection-string>"]
       }
}
}
`



Replace `<connection-string>` with your connection string.

4. Save the configuration file and reload by tapping **Refresh** in the Cascade assistant.

5. You should see a green active status after the server is successfully connected.


## Next steps [\#](https://supabase.com/docs/guides/getting-started/mcp\#next-steps)

You are now connected to Supabase using MCP! You can now interact with your database using natural language commands. Try asking your AI tool to query your database, create a new table, or perform other SQL operations.

### Is this helpful?

NoYes

### On this page

[Connect to Supabase using MCP](https://supabase.com/docs/guides/getting-started/mcp#connect-to-supabase-using-mcp) [Step 1: Find your database connection string](https://supabase.com/docs/guides/getting-started/mcp#step-1-find-your-database-connection-string) [Step 2: Configure in your AI tool](https://supabase.com/docs/guides/getting-started/mcp#step-2-configure-in-your-ai-tool) [Next steps](https://supabase.com/docs/guides/getting-started/mcp#next-steps)

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings