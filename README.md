# The actual most basic OpenRelic worker yet.

You need an Ollama container that this worker can connect to on TCP/11434 ie.
```
  openrelik-ollama:
    container_name: ollama
    image: ollama/ollama:latest
    ports:
      - 11434:11434
```

You will also need to load the llama3 model in the Ollama instance ie.
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
