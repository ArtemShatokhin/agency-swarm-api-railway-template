# Agency Swarm Railway Deployment Template

This repo demonstrates how to deploy any Agency Swarm agency as a FastAPI application in a Docker container on Railway.

## Prerequisites
* Fully tested agency
* Docker installed on your system
* Python 3.12

## Setup Instructions
1. **Configure environment variables:** 
   - For local testing: Copy `.env.example` to `.env` and add your environment variables
   - For Railway: Configure environment variables in Railway Dashboard under Variables section

2. **Add requirements:** Add your extra requirements to the requirements.txt file.

3. **Add your Agency:**
   Drag-and-drop your agency into the /src directory and import it according to the example in the `main.py`:
```python
from ExampleAgency.agency import agency
```

4. **Set your APP_TOKEN:**
   In `src/main.py`, replace `YOUR_APP_TOKEN` with a secure token. This will be used for API authentication.

5. **Test your Agency:**
   Run `python agency.py` to test your agency. This will also save your agent settings in the settings.json file. Put this file in the /src directory. This step is necessary to avoid creating new assistants on every application start.

6. **Build and run Docker container:**
   Make sure you have created your .env file first.
   Then build and run with docker compose:
   ```bash
   docker compose up --build
   ```

   Alternative Docker commands for direct container management:
   ```bash
   docker build -t agency-swarm .
   docker run -p 8000:8000 -e OPENAI_API_KEY=your_key_here agency-swarm
   ```

7. **Test API:**
```bash
# macOS/Linux
curl -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <YOUR_APP_TOKEN>" \
  -d '{"message": "What is the capital of France?"}' \
  <YOUR_DEPLOYMENT_URL>/api/agency
```

```bash
# Windows PowerShell
curl -X POST `
  -H "Content-Type: application/json" `
  -H "Authorization: Bearer <YOUR_APP_TOKEN>" `
  -d "{\"message\": \"What is the capital of France?\"}" `
  <YOUR_DEPLOYMENT_URL>/api/agency
```

8. **Access the interfaces:**
   - Gradio UI: `<YOUR_DEPLOYMENT_URL>/demo-gradio` (local: http://localhost:8000/demo-gradio)
   - API Documentation: `<YOUR_DEPLOYMENT_URL>/docs` (local: http://localhost:8000/docs)

## API Documentation

### `POST /api/agency`

Request body:
```json
{
  "message": "What is the capital of France?",
  "attachments": [
    {
      "file_id": "file-123",
      "tools": [
        { "type": "file_search" },
        { "type": "code_interpreter" }
      ]
    }
  ]
}
```

* `message`: The message to send to the agent.
* `attachments` (optional): A list of files attached to the message, and the tools they should be added to. See [OpenAI Docs](https://platform.openai.com/docs/api-reference/messages/createMessage#messages-createmessage-attachments).

Response:
```json
{
  "response": "Paris"
}
```

### Authentication

All API requests require a Bearer token in the Authorization header:
```
Authorization: Bearer <YOUR_APP_TOKEN>
```

## Gradio Interface Features
- Dark/Light mode support
- File upload capabilities
- Support for multiple agents
- Real-time streaming responses
- Code interpreter and file search tool integration
