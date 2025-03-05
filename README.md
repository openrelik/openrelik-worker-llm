# OpenRelik worker for analysing files using an LLM provider

This worker supports the LLM providers defined in openrelik-ai-common.

This worker will take any files it can read as UTF-8 and run a prompt on it.

You prompt can reference the file contents explicitly by mentioning `$file`.

The worker expects two environment variables `LLM_PROVIDER` and `LLM_MODEL`.

You can also specify the optional `LLM_MAX_INPUT_TOKENS` to truncate the prompt.

You can tack this worker onto your docker-compose.yml with something like
```
  openrelik-worker-llm:
    container_name: openrelik-worker-llm
    build:
      context: ./openrelik-worker-llm
      dockerfile: Dockerfile
    restart: always
    environment:
      - OPENRELIK_PYDEBUG=1
      - OPENRELIK_PYDEBUG_PORT=5678
      - REDIS_URL=redis://openrelik-redis:6379
      - LLM_PROVIDER=ollama
      - LLM_MODEL=llama3
      - LLM_MAX_INPUT_TOKENS=1000000
      - OLLAMA_SERVER_URL=http://openrelik-ollama:11434
      - OLLAMA_DEFAULT_MODEL=llama3
    ports:
      - 5678:5678
    volumes:
      - ./data:/usr/share/openrelik/data
    command: "celery --app=src.app worker --task-events --concurrency=4 --loglevel=INFO -Q openrelik-worker-llm"
```

If you're using Ollama then you can tack on a container exposing TCP/11434 ie.
```
  openrelik-ollama:
    container_name: ollama
    image: ollama/ollama:latest
    ports:
      - 11434:11434
```

You will also need to load the llama3 model in the Ollama server instance ie.
```
docker compose exec openrelik-ollama /bin/bash
root@04456c12e6e8:/# ollama run llama3
pulling manifest 
pulling 6a0746a1ec1a... 100% ▕█████████████████████████████████████████████████████████████████████████████████▏ 4.7 GB                         
pulling 4fa551d4f938... 100% ▕█████████████████████████████████████████████████████████████████████████████████▏  12 KB                         
pulling 8ab4849b038c... 100% ▕█████████████████████████████████████████████████████████████████████████████████▏  254 B                         
pulling 577073ffcc6c... 100% ▕█████████████████████████████████████████████████████████████████████████████████▏  110 B                         
pulling 3f8eb4da87fa... 100% ▕█████████████████████████████████████████████████████████████████████████████████▏  485 B                         
verifying sha256 digest 
writing manifest 
success
```
