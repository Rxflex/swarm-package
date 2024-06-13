## Stack yaml example
```yaml
version: '3.8'

services:
  python_app:
    image: ghcr.io/rxflex/swarm-package:python38
    deploy:
      replicas: 1
      restart_policy:
        condition: on-failure
    environment:
      GIT_ADDRESS: "READCTED"
      BRANCH: "main"
      USER_UPLOAD: "0"
      AUTO_UPDATE: "1"
      PY_FILE: "bot.py"
      PY_PACKAGES: ""
      USERNAME: "READCTED"
      ACCESS_TOKEN: "READCTED"
      REQUIREMENTS_FILE: "requirements.txt"
    volumes:
      - python_app_data:/mnt/server
    command: bash -c 'if [ "$$USER_UPLOAD" = "true" ] || [ "$$USER_UPLOAD" = "1" ]; then echo "assuming user knows what they are doing have a good day."; exit 0; fi && if [[ ! "$$GIT_ADDRESS" =~ \.git$$ ]]; then GIT_ADDRESS="$$GIT_ADDRESS.git"; fi && if [ -z "$$USERNAME" ] && [ -z "$$ACCESS_TOKEN" ]; then echo "using anon api call"; else GIT_ADDRESS="https://$$USERNAME:$$ACCESS_TOKEN@$$GIT_ADDRESS"; fi && if [ -z "$$BRANCH" ]; then git clone "$$GIT_ADDRESS" /mnt/server; else git clone --single-branch --branch "$$BRANCH" "$$GIT_ADDRESS" /mnt/server; fi && if [ -f /mnt/server/"$$REQUIREMENTS_FILE" ]; then pip install -U --prefix .local -r /mnt/server/"$$REQUIREMENTS_FILE"; fi && /usr/local/bin/python /mnt/server/"$$PY_FILE"'

volumes:
  python_app_data:
    driver: local
```