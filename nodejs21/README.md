## Stack yaml example
```yaml
version: '3.8'

services:
  node_app:
    image: ghcr.io/rxflex/swarm-package:nodejs21
    deploy:
      replicas: 1
      restart_policy:
        condition: on-failure
    environment:
      GIT_ADDRESS: "READCTED"
      BRANCH: "main"
      USER_UPLOAD: "0"
      AUTO_UPDATE: "1"
      NODE_PACKAGES: ""
      UNNODE_PACKAGES: ""
      USERNAME: "READCTED"
      ACCESS_TOKEN: "READCTED"
      MAIN_FILE: "bot.js"
      NODE_ARGS: ""
    volumes:
      - node_app_data:/mnt/server
    command: bash -c 'if [ "$$USER_UPLOAD" = "true" ] || [ "$$USER_UPLOAD" = "1" ]; then echo "assuming user knows what they are doing have a good day."; exit 0; fi && if [[ ! "$$GIT_ADDRESS" =~ \.git$$ ]]; then GIT_ADDRESS="$$GIT_ADDRESS.git"; fi && if [ -z "$$USERNAME" ] && [ -z "$$ACCESS_TOKEN" ]; then echo "using anon api call"; else GIT_ADDRESS="https://$$USERNAME:$$ACCESS_TOKEN@$$GIT_ADDRESS"; fi && if [ -z "$$BRANCH" ]; then git clone "$$GIT_ADDRESS" /mnt/server; else git clone --single-branch --branch "$$BRANCH" "$$GIT_ADDRESS" /mnt/server; fi && echo "Installing nodejs packages" && if [ ! -z "$$NODE_PACKAGES" ]; then npm install $$NODE_PACKAGES; fi && if [ -f /mnt/server/package.json ]; then npm install --production; fi && echo "install complete" && /usr/local/bin/node /mnt/server/"$$MAIN_FILE" $$NODE_ARGS'

volumes:
  node_app_data:
    driver: local
```