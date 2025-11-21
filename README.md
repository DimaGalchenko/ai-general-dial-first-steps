# Practice with DIAL Core, Chat, Themes and Adapters setup

![dial](https://dialx.ai/platform-rev9.svg)

## Configuration Files
- **[core/config.json](core/config.json)** - Main DIAL configuration with applications, models, and API keys
- **[settings/settings.json](settings/settings.json)** - Core server settings and security configuration
- **[docker-compose.yml](docker-compose.yml)** - Service orchestration and networking

## AFTER ALL THE TASKS DONE - DON'T FORGET TO REMOVE API KEYs FROM core/config.json

## Learning Path
1. **[T1](tasks/t1)** - Basic DIAL Chat setup
2. **[T2](tasks/t2)** - **OPTIONAL**, First Echo application in container
3. **[T3](tasks/t3)** - Local development workflow
4. **[T4](tasks/t4)** - Models and adapter integration
5. **[T5](tasks/t5)** - Application with streaming
6. **[T5Optional](tasks/t_optional)** - Additional configurations of different models

## Environment Requirements
- Docker and Docker Compose
- Python 3.11+ for local development

## How to hide API keys:

Importunately, there is no option to fetch them from env variables, but you can hide them in project:

1. Create `keys.json` in [core config folder](core). `keys.json` is added to the [.gitignore](.gitignore) and will be ignored by git
2. Segregate [core config](core/config.json):
    Original config:
    ```json
    {
      "routes": {},
      "applications": {},
      "models": {
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
      },
      "keys": {
        "dial_api_key": {
          "project": "TEST-PROJECT",
          "role": "default"
        }
      },
      "roles": {
        "default": {
          "limits": {}
        }
      }
    }
    ```
    Updated [core config](core/config.json):
    ```json
    {
      "routes": {},
      "applications": {},
      "models": {
        "gpt-4o": {
          "displayName": "GPT 4o",
          "overrideName": "gpt-4o",
          "endpoint": "http://adapter-dial-openai:5000/openai/deployments/gpt-4o/chat/completions",
          "iconUrl": "http://localhost:3001/gpt4.svg",
          "type": "chat"
        }
      },
      "keys": {
        "dial_api_key": {
          "project": "TEST-PROJECT",
          "role": "default"
        }
      },
      "roles": {
        "default": {
          "limits": {}
        }
      }
    }
    ```
    Moved `upstreams` to [keys config](core/keys.json):
    ```json
    {
      "models": {
        "gpt-4o": {
          "upstreams": [
            {
              "endpoint": "https://api.openai.com/v1/chat/completions",
              "key": "${OPENAI_API_KEY}"
            }
          ]
        }
      }
    }
    ```
3. Update `core` service env variable adn provide path to `keys.json` config:
`'aidial.config.files': '["/opt/config/config.json", "/opt/config/keys.json"]'`
4. Delete `core` container and start it from scratch 