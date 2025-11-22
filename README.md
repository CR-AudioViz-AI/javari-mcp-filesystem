# File System MCP Server

Model Context Protocol (MCP) server for file system operations. Enables Javari AI to autonomously create, read, update, and validate files with security sandboxing.

## Features

- ✅ Create and manage workspaces (project directories)
- ✅ Create files with content validation
- ✅ Read file contents
- ✅ Update files with automatic backups
- ✅ Delete files safely
- ✅ List directory contents
- ✅ Validate TypeScript compilation
- ✅ Generate Next.js project templates
- ✅ Security sandboxing (prevents directory traversal)
- ✅ File type whitelist
- ✅ Size limits
- ✅ Comprehensive logging

## Installation

```bash
npm install
```

## Configuration

Copy `.env.example` to `.env` and configure:

```bash
cp .env.example .env
```

Required variables:
- `MCP_API_KEY`: Secure key for MCP authentication
- `STORAGE_PATH`: Directory for workspaces (default: /tmp/javari-builds)
- `MAX_FILE_SIZE`: Maximum file size in bytes (default: 10MB)

## Development

```bash
npm run dev
```

## Production

```bash
npm run build
npm start
```

## API Endpoints

### Health Check
```
GET /health
```

### Create Workspace
```
POST /api/workspace/create
Headers: x-api-key: YOUR_MCP_KEY
Body: {
  "workspaceId": "project-123"
}
```

### Create File
```
POST /api/files/create
Headers: x-api-key: YOUR_MCP_KEY
Body: {
  "workspaceId": "project-123",
  "filePath": "src/index.ts",
  "content": "console.log('Hello');"
}
```

### Read File
```
GET /api/files/read?workspaceId=project-123&filePath=src/index.ts
Headers: x-api-key: YOUR_MCP_KEY
```

### Update File
```
PUT /api/files/update
Headers: x-api-key: YOUR_MCP_KEY
Body: {
  "workspaceId": "project-123",
  "filePath": "src/index.ts",
  "content": "console.log('Updated');"
}
```

### Delete File
```
DELETE /api/files/delete
Headers: x-api-key: YOUR_MCP_KEY
Body: {
  "workspaceId": "project-123",
  "filePath": "src/index.ts"
}
```

### List Directory
```
GET /api/dirs/list?workspaceId=project-123&dirPath=src
Headers: x-api-key: YOUR_MCP_KEY
```

### Validate TypeScript
```
POST /api/validate/typescript
Headers: x-api-key: YOUR_MCP_KEY
Body: {
  "workspaceId": "project-123"
}
```

### Generate Next.js Template
```
POST /api/template/nextjs
Headers: x-api-key: YOUR_MCP_KEY
Body: {
  "workspaceId": "project-123",
  "name": "my-app",
  "typescript": true
}
```

### Clean Workspace
```
DELETE /api/workspace/clean
Headers: x-api-key: YOUR_MCP_KEY
Body: {
  "workspaceId": "project-123"
}
```

## Security Features

### Sandboxing
- All file operations are restricted to workspace directories
- Path traversal attacks are prevented
- No access to system files

### File Type Whitelist
Allowed extensions:
- TypeScript: `.ts`, `.tsx`
- JavaScript: `.js`, `.jsx`
- Config: `.json`, `.yml`, `.yaml`, `.toml`
- Documentation: `.md`, `.txt`
- Web: `.html`, `.css`
- Environment: `.env`, `.gitignore`, `.npmrc`

### Size Limits
- Maximum file size: 10MB (configurable)
- Total workspace size: Monitored but not enforced (set at OS level)

### Rate Limiting
- 2000 requests per hour per IP
- Prevents abuse

## File Operations Best Practices

### Creating Files
1. Always specify correct file extension
2. Validate content before sending
3. Use workspaceId to organize projects

### Updating Files
- Automatic backup created before update
- Backup stored as `filename.backup`
- Restore from backup if needed

### Validating TypeScript
- Requires `tsconfig.json` in workspace
- Runs `tsc --noEmit` for type checking
- Returns detailed error messages with line numbers

## Deployment

### Railway (Recommended)

```bash
railway up
```

Configure environment variables in Railway dashboard.

Ensure persistent volume is mounted for `STORAGE_PATH`.

### Docker

```bash
docker build -t crav-mcp-filesystem .
docker run -p 3003:3003 -v /data:/tmp/javari-builds --env-file .env crav-mcp-filesystem
```

## Monitoring

Check server health:
```bash
curl http://localhost:3003/health
```

## Error Handling

All endpoints return consistent error format:
```json
{
  "error": "Error description",
  "details": "Detailed message"
}
```

## Workspace Management

Workspaces are isolated directories for each project:
```
/tmp/javari-builds/
  ├── project-123/
  │   ├── src/
  │   ├── package.json
  │   └── ...
  ├── project-456/
  │   └── ...
```

Clean workspaces after deployment to save storage.

## Logs

- `combined.log`: All operations
- `error.log`: Errors only
- Console: Real-time colored output

## License

MIT - CR AudioViz AI
