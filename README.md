# IBM Sterling File Gateway AI Agent

A complete conversational AI agent for managing IBM Sterling File Gateway arrived files with natural language commands.

## 🌟 Features

### Core Capabilities
- **Natural Language Interface**: Chat with your File Gateway using plain English
- **Smart Filtering**: Filter by status, reviewed state, and date ranges
- **Batch Operations**: Process multiple files in a single command
- **Business-Friendly Display**: Clean, formatted output with emojis and visual indicators
- **SSL Certificate Handling**: Bypasses certificate validation for development environments

### Supported Operations

#### 📋 View & Search Files
- Get all files or limit results
- Search by file name, producer, consumer
- Filter by status (Failed, Routed, Routing)
- Filter by reviewed status (reviewed/unreviewed)
- Filter by date ranges (today, yesterday, between dates)
- Combine multiple filters

#### ✏️ Update Files
- Mark files as reviewed
- Update file properties
- Uses arrivedFileKey for identification

#### 🔄 Redeliver Files
- Redeliver single or multiple files
- Specify consumer and comments
- Batch redeliver support

#### ▶️ Replay Files
- Replay single or multiple files
- Add comments for audit trail
- Batch replay support

## 🚀 Quick Setup

### Prerequisites
- n8n instance (self-hosted or cloud)
- OpenAI API key
- IBM Sterling File Gateway credentials
- Access to File Gateway API endpoint

### Installation Steps

#### 1. Import Workflow
1. Open n8n
2. **Workflows** → **Import from File**
3. Select `FileGateway-AI-Agent.json`
4. Click Import

#### 2. Configure OpenAI Credential
1. **Credentials** → **Add Credential**
2. Type: **OpenAI API**
3. Name: `OpenAI API`
4. API Key: Get from https://platform.openai.com/api-keys
5. Save

#### 3. Configure File Gateway Credential
1. **Credentials** → **Add Credential**
2. Type: **HTTP Basic Auth**
3. Name: `File Gateway API`
4. Username: Your File Gateway username
5. Password: Your File Gateway password
6. Save

#### 4. Update API Endpoint (if needed)
1. Open workflow
2. Find nodes: "File Gateway API - GET", "File Gateway API - POST", "File Gateway API - PUT"
3. Update base URL if your endpoint is different:
   ```
   https://api.dev01.apps.itz-a1ptvm.hub01-lb.techzone.ibm.com:443/B2BAPIs/svc
   ```

#### 5. Activate Workflow
1. Save workflow
2. Toggle **Active** switch at top
3. Click "When chat message received" node
4. Copy the webhook URL
5. Open URL in browser to access chat interface

## 💬 Usage Examples

### View Files

```
"Show me all files"
"Get the last 20 files"
"Show me files"
```

### Filter by Status

```
"Get files with status failed"
"Show files with status error"
"Get routed files"
"Show files with status routing"
```

### Filter by Reviewed Status

```
"Show unreviewed files"
"Get reviewed files"
"Show me files that haven't been reviewed"
```

### Filter by Date

```
"Show files received today"
"Get files from yesterday"
"Show files between 2024-01-01 and 2024-01-31"
"Get files received after 2024-03-01"
"Show failed files from last week"
```

### Combine Filters

```
"Show unreviewed files with status failed"
"Get failed files from today"
"Show unreviewed files between 2024-01-01 and 2024-01-31"
```

### Update Files

```
"Update file ABC123 mark as reviewed"
"Mark file XYZ789 as reviewed"
```

### Redeliver Files

```
"Redeliver file ABC123 to consumer TestConsumer with comments Redelivering for testing"
"Redeliver files ABC123, XYZ789 to consumer Test with comments Batch redeliver"
```

### Replay Files

```
"Replay file ABC123 with comments Manual replay due to error"
"Replay files ABC123, XYZ789 with comments Batch replay"
```

## 📊 Output Format

Results are displayed in a clean, business-friendly format:

```
📋 Found 3 file(s) (state: Failed):

**1. 📄 test.txt**
   🔑 Key: `ABC123`
   ❌ Status: **Failed**
   ○ Reviewed: No
   📤 Producer: ProdA
   🕐 Arrived: Jan 15, 2024, 10:30 AM
   💾 Size: 1.25 KB

**2. 📄 data.csv**
   🔑 Key: `XYZ789`
   ✅ Status: **Routed**
   ✓ Reviewed: Yes
   📤 Producer: ProdB
   🕐 Arrived: Jan 16, 2024, 02:15 PM
   💾 Size: 3.47 KB
```

### Visual Indicators
- ✅ **Routed** - Successfully processed
- ❌ **Failed** - Error occurred
- 🔄 **Routing** - In progress
- ✓ **Reviewed** / ○ **Not Reviewed**

## 🔧 Technical Details

### Architecture

The workflow uses a two-phase AI approach:

1. **Intent Classification**: Determines if the request is conversational or an API call
2. **API Configuration**: Parses natural language into API parameters

### Key Components

#### Nodes
- **Chat Trigger**: Webhook interface for browser access
- **Intent Classification**: Routes between conversation and API calls
- **API Config Builder**: Converts natural language to API parameters
- **HTTP Request Nodes**: GET, POST, PUT operations with SSL bypass
- **Filter Node**: Client-side filtering for fields not supported by API
- **Format Response**: Converts JSON to business-friendly display

#### Filtering Strategy

**API-Supported Filters** (Query Parameters):
- `fileName`
- `producerName`
- `consumerName`
- `limit`

**Client-Side Filters** (Post-API Processing):
- `arrivedFileState` (Failed, Routed, Routing)
- `reviewed` (0=unreviewed, 2=reviewed)
- `createTs` (date range filtering)

### SSL Certificate Handling

The workflow uses regular `httpRequest` nodes (not `toolHttpRequest`) with:
```javascript
options: {
  allowUnauthorizedCerts: true
}
```

This bypasses SSL certificate validation for development/test environments.

## 🔑 Important Field Names

- **arrivedFileKey**: Primary identifier (shown as 🔑 Key in results)
- **fileName**: File name (for searching only)
- **arrivedFileState**: File status (Failed, Routed, Routing)
- **reviewed**: Review status (0=unreviewed, 2=reviewed)
- **createTs**: File creation timestamp (format: "2026-03-19 14:23:41.0")
- **producerName**: Producer name
- **consumerName**: Consumer name

## 🎯 Best Practices

### For Business Users
1. Always use natural language - the AI understands context
2. Use "arrivedFileKey" (shown as 🔑 Key) for file operations
3. List files first to get arrivedFileKeys before updating/replaying/redelivering
4. Combine filters for precise results
5. Use date ranges to narrow down large result sets

### For Administrators
1. Keep OpenAI API key secure
2. Monitor API usage and costs
3. Adjust `limit` parameter in queries for performance
4. Review SSL certificate settings for production
5. Update API endpoint URL for your environment

## 🐛 Troubleshooting

### Common Issues

**"No files found"**
- Check your filters - they might be too restrictive
- Try without filters first: "Show me all files"
- Verify API credentials are correct

**SSL Certificate Error**
- Ensure `allowUnauthorizedCerts: true` is set in HTTP request nodes
- For production, use proper SSL certificates

**Wrong Results**
- Remember: "error" and "failed" both map to state "Failed"
- Date filtering uses `createTs` field, not `arrivedTime`
- Reviewed values are 0 (unreviewed) or 2 (reviewed), not 0/1

**Batch Operations Not Working**
- Ensure you provide all required fields (comments for replay/redeliver)
- Use comma-separated arrivedFileKeys: "ABC123, XYZ789"

## 📝 API Endpoints

### Base URL
```
https://api.dev01.apps.itz-a1ptvm.hub01-lb.techzone.ibm.com:443/B2BAPIs/svc
```

### Endpoints Used
- `GET /fgarrivedfiles/` - List files
- `GET /fgarrivedfiles/{arrivedFileKey}` - Get single file
- `PUT /fgarrivedfiles/{arrivedFileKey}` - Update file
- `POST /fgarrivedfiles/{arrivedFileKey}/actions/replayarrivedfile` - Replay file
- `POST /fgarrivedfiles/{arrivedFileKey}/actions/redeliverarrivedfile` - Redeliver file

## 🔄 Version History

### Current Version: Complete v1
- ✅ Natural language interface
- ✅ Multi-filter support (status, reviewed, date)
- ✅ Batch operations
- ✅ Business-friendly formatted output
- ✅ SSL certificate bypass
- ✅ Comprehensive error handling
- ✅ Date range filtering using createTs field

## 📚 Additional Resources

- [IBM Sterling File Gateway Documentation](https://www.ibm.com/docs/en/b2b-integrator)
- [n8n Documentation](https://docs.n8n.io/)
- [OpenAI API Documentation](https://platform.openai.com/docs)

## 🤝 Support

For issues or questions:
1. Check the Troubleshooting section
2. Review the Usage Examples
3. Verify your credentials and API endpoint
4. Check n8n execution logs for detailed error messages

## 📄 License

This workflow is provided as-is for use with IBM Sterling File Gateway.