# File Gateway AI Agent

A conversational AI agent for IBM Sterling File Gateway Arrived File API.

## Features

- 💬 Chat interface with natural language
- 🔍 Query and search files
- ✅ Update file review status
- 🔄 Redeliver files
- ▶️ Replay files
- 🧠 Remembers conversation context

## Quick Setup

### 1. Import Workflow

1. Open n8n
2. Go to **Workflows** → **Import from File**
3. Select `FileGateway-AI-Agent.json`
4. Click Import

### 2. Configure Credentials

#### OpenAI API
1. Go to **Credentials** → **Add Credential**
2. Type: **OpenAI API**
3. Name: `OpenAI API`
4. API Key: Your key from https://platform.openai.com/api-keys
5. Save

#### File Gateway API
1. Go to **Credentials** → **Add Credential**
2. Type: **HTTP Basic Auth**
3. Name: `File Gateway API`
4. Username: Your File Gateway username
5. Password: Your File Gateway password
6. Save

### 3. Connect Tools Manually

**Important:** n8n doesn't auto-connect LangChain nodes on import. You must connect them manually:

1. Click on the **AI Agent** node
2. In the right panel:
   - **Model:** Select "OpenAI Chat Model"
   - **Memory:** Click "Add Memory" → Select "Window Buffer Memory"
   - **Tools:** Click "Add Tool" 4 times and select:
     - Query Files
     - Update File
     - Redeliver File
     - Replay File

### 4. Verify Tool Credentials

For each tool node (Query Files, Update File, Redeliver File, Replay File):
1. Click on the tool node
2. Select credential: `File Gateway API`
3. Save

### 5. Activate & Test

1. Save the workflow
2. Activate it (toggle at top)
3. Click "When chat message received" node
4. Copy the chat URL
5. Open URL in browser
6. Test with: "Hello!" then "Show me error files"

## API Endpoints

- **Base URL:** `https://api.dev01.apps.itz-a1ptvm.hub01-lb.techzone.ibm.com/B2BAPIs/svc/fgarrivedfiles/`
- **GET** `/` - Query files
- **PUT** `/:id` - Update file
- **POST** `/:id/actions/redeliverarrivedfile` - Redeliver
- **POST** `/:id/actions/replayarrivedfile` - Replay

## Example Conversations

```
You: Hello!
Agent: Hello! 👋 I'm your File Gateway AI Assistant...

You: Show me error files
Agent: I can search for files with ERROR state. Would you like me to show all error files?

You: Yes
Agent: [Calls queryFiles tool with arrivedFileState=ERROR]
Found 5 files...

You: Update file 12345 to reviewed
Agent: I can update file 12345. Should I mark it as reviewed?

You: Yes
Agent: [Calls updateFile tool]
Successfully updated!
```

## Troubleshooting

### Tools not connected
- Manually connect them in AI Agent node (see step 3)
- This is normal - n8n doesn't auto-connect on import

### Credentials not found
- Ensure credentials are named exactly as shown
- Check credential IDs match in the JSON

### API errors
- Verify File Gateway credentials are correct
- Check API endpoint is accessible
- Ensure you have proper permissions

## Cost

Using GPT-4o-mini:
- ~$0.0002-0.0005 per message
- Very affordable for most use cases

## Support

For issues or questions, check the n8n documentation or File Gateway API docs.