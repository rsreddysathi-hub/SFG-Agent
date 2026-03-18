# File Gateway AI Agent - Minimal Version

The simplest possible working AI agent for IBM Sterling File Gateway.

## What It Does

- Returns the latest 10 files from File Gateway
- No parameters, no complexity
- Just works

## Quick Setup (5 Minutes)

### 1. Import Workflow

1. Open n8n
2. **Workflows** → **Import from File**
3. Select `FileGateway-AI-Agent.json`
4. Import

### 2. Add OpenAI Credential

1. **Credentials** → **Add Credential**
2. Type: **OpenAI API**
3. Name: `OpenAI API`
4. API Key: Get from https://platform.openai.com/api-keys
5. Save

### 3. Add File Gateway Credential

1. **Credentials** → **Add Credential**
2. Type: **HTTP Basic Auth**
3. Name: `File Gateway API`
4. Username: Your username
5. Password: Your password
6. Save

### 4. Connect the Tool

**IMPORTANT:** n8n doesn't auto-connect tools on import.

1. Click **AI Agent** node
2. In right panel:
   - **Model:** Select "OpenAI Chat Model"
   - **Tools:** Click "Add Tool" → Select "Get Files"
3. Click **Get Files** node
4. Select credential: `File Gateway API`
5. Save

### 5. Activate & Test

1. Save workflow
2. Activate (toggle at top)
3. Click "When chat message received" node
4. Copy chat URL
5. Open in browser
6. Type: "Show me the files"

## That's It!

If this works, we can add more features. But let's get this working first.

## Troubleshooting

**Tool not connected?**
- Manually add it in AI Agent node (step 4)

**Credential error?**
- Check username/password are correct
- Ensure credential is named exactly `File Gateway API`

**No response?**
- Check OpenAI credential has valid API key
- Check you have OpenAI credits

## API Endpoint

`https://api.itz-a1ptvm.hub01-lb.techzone.ibm.com/B2BAPIs/svc/fgarrivedfiles/?limit=10`

## Next Steps

Once this works, we can add:
- More tools (update, redeliver, replay)
- Parameters for filtering
- Better error handling

But first, let's just get ONE tool working!