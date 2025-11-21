# Set up Claude and Bedrock models

Documentation for [adapter-dial-bedrock](https://github.com/epam/ai-dial-adapter-bedrock)

## Anthropic

1. Open the core config.json
2. Add such config to `models`:
    ```json
        "claude-sonnet-4": {
          "displayName": "Sonnet 4",
          "endpoint": "http://adapter-dial-bedrock:5000/openai/deployments/claude-sonnet-4-20250514/chat/completions",
          "type": "chat",
          "upstreams": [
            {
              "key": "${ANTHROPIC_API_KEY}"
            }
          ]
        }
    ```
   Replace `${ANTHROPIC_API_KEY}` with your [Anthropic API Key](https://console.anthropic.com/settings/keys)
3. Open root `docker-compose.yml` and add such service:
    ```yaml
      adapter-dial-bedrock:
        image: epam/ai-dial-adapter-bedrock:development
        platform: linux/amd64
        environment:
          COMPATIBILITY_MAPPING: '{"claude-sonnet-4-20250514": "anthropic.claude-sonnet-4-20250514-v1:0"}'
          DIAL_URL: "http://core:8080"
          LOG_LEVEL: "DEBUG"
    ```
   About `COMPATIBILITY_MAPPING` you can read [here](https://github.com/khshanovskyi/ai-dial-adapter-bedrock/tree/development?tab=readme-ov-file#compatibility-mode)
4. Start service and restart DIAL Core service
5. Test it

---

## Bedrock

Add Claude Sonnet 3.7 from [AWS Bedrock](https://aws.amazon.com/bedrock/). You need to generate [AWS Access Key in IAM](https://repost.aws/knowledge-center/create-access-key)

1. Open the core config.json
2. Add such config to `models`:
    ```json
        "us.anthropic.claude-3-7-sonnet-20250219-v1:0": {
          "displayName": "Sonnet 3.7 Bedrock",
          "endpoint": "http://adapter-dial-bedrock:5000/openai/deployments/us.anthropic.claude-3-7-sonnet-20250219-v1:0/chat/completions",
          "type": "chat",
          "upstreams": [
            {
              "extraData": {
                "region": "us-east-1",
                "aws_access_key_id": "${AWS_ACCESS_KEY_ID_STARTS_WITH_AKIA}",
                "aws_secret_access_key": "${AWS_SECRET_ACCESS_KEY}"
              }
            }
          ]
        }
    ```
3. Replace `${AWS_ACCESS_KEY_ID_STARTS_WITH_AKIA}` and `${AWS_SECRET_ACCESS_KEY}` with AWS Keys
4. Restart DIAL Core service
5. Test it

---

## AFTER ALL THE TASKS DONE - DON'T FORGET TO REMOVE API KEYs FROM core/config.json