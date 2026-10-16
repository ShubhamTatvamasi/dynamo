# dynamo

Run `pre-deployment-check` before deploying dynamo:
```bash
https://github.com/ai-dynamo/dynamo/blob/main/deploy/pre-deployment/pre-deployment-check.sh
```

Add helm repo:
```bash
helm repo add ai-dynamo https://helm.ngc.nvidia.com/nvidia/ai-dynamo
```

Install dynamo-platform:
```bash
helm upgrade -i dynamo-platform ai-dynamo/dynamo-platform \
  --namespace dynamo-system \
  --create-namespace
```

---

Port-forward your service:
```bash
kubectl -n dynamo port-forward svc/qwen35-frontend 8000:8000
```

Check the list of models:
```bash
curl -s http://localhost:8000/v1/models | jq
```

Send your message:
```bash
curl -s http://localhost:8000/v1/chat/completions \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "RedHatAI/Qwen3.5-9B-FP8-dynamic",
    "messages": [
      {
        "role": "user",
        "content": "Hi, How are you?"
      }
    ],
    "max_tokens": 256,
    "temperature": 0.2
  }' | jq -r '.choices[0].message.content'
```

