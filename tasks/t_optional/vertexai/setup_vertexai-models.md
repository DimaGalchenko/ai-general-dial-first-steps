# Set up Google VertexAI Models

You can use different models from [Google VertexAI](https://console.cloud.google.com/vertex-ai/publishers/google/model-garden)

Documentation for [adapter-dial-vertexai](https://github.com/epam/ai-dial-adapter-vertexai)

## GPT OpenAI Models

1. Create API Key in [Google AI Studio](https://aistudio.google.com/app/api-keys)
2. Open the core config.json
3. Add such config to `models`:
    ```json
        "gemini-2.5-flash": {
          "displayName": "Gemini 2.5 Flash",
          "type": "chat",
          "endpoint": "http://adapter-dial-vertexai:5000/openai/deployments/gemini-2.5-flash/chat/completions",
          "iconUrl": "http://localhost:3001/Gemini-Pro-Vision.svg",
          "upstreams": [
            {
              "key": "${API_KEY}"
            }
          ]
        },
    ```
   Replace `${API_KEY}` with generated API key
4. Open root `docker-compose.yml` and add such service:
    ```yaml
      adapter-dial-vertexai:
        image: epam/ai-dial-adapter-vertexai:development
        #platform: linux/amd64
        environment:
          DIAL_URL: "http://core:8080"
          LOG_LEVEL: "DEBUG"
    ```
5. Restart DIAL Core service
6. Test it in DIAL Chat

---

## AFTER ALL THE TASKS DONE - DON'T FORGET TO REMOVE API KEYs FROM core/config.json
