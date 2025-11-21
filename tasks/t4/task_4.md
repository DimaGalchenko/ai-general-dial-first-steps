# Time to add models

1. Open the `/core/config.json` (in root) and add configs for echo app:
    - Into `models`:
        ```
            "gpt-4o": {
              "displayName": "GPT 4o",
              "overrideName": "gpt-4o",
              "endpoint": "http://adapter-dial-openai:5000/openai/deployments/gpt-4o/chat/completions",
              "iconUrl": "http://localhost:3001/gpt4.svg",
              "type": "chat",
              "upstreams": [
                {
                  "endpoint": "https://api.openai.com/v1/chat/completions",
                  "key": "${OPENAI_API_KEY}"
                }
              ]
           }
        ```
    > In `upstreams` we need to configure `endpoint` and `key` (OPENAI_API_KEY).
2. Replace `${OPENAI_API_KEY}` with your OpenAI API Key
3. Run docker-compose (root docker compose)
     ```bash
     docker compose stop && docker compose up -d --build
     ```
4. Open in browser [local dial chat](http://localhost:3000/marketplace) and test GPT 4o model
    ```text
    Hi, what can you?
    ```
   <img src="_screenshots/error_adapter.png">

    > **IT SHOULD FAIL!** We need to add `adapter-dial-openai` to work with it!

5. Add `adapter-dial-openai` service to docker compose (in root):
   For Mac/Linux uncomment `platform: linux/amd64`
    ```yaml
      adapter-dial-openai:
        image: epam/ai-dial-adapter-openai:development
        #platform: linux/amd64 
        environment:
          DIAL_URL: "http://core:8080"
          LOG_LEVEL: "INFO"
    ```

6. Run docker-compose (root docker compose)
      ```bash
      docker compose stop && docker compose up -d --build
      ```
7. Check that adapter-dial is up:
    ```bash
    docker compose ps -a 
    ```
8.  Open in browser [local dial chat](http://localhost:3000/marketplace) and test again GPT 4o model

    <img src="_screenshots/with_adapter.png">

9.  Let's test `limits` config (limits set the limit of tokes that user can use for one API Key):
    > "day": "1000000"
    
    > "minute": "256000"
    
    > "week": "1256000"
    
    > "month": "11256000"

   - Update limits for `gpt-4o` (100 tokens per minute are allowed, if we hit the limit with the next request we will get an error):
    ```
    "gpt-4o": {
        "minute": "100"
    }
    ```
    <img src="_screenshots/limit.png">
    <img src="_screenshots/hit_limit.png">

10. **Don't forget to remove `limits` for `gpt-4o` after testing!**