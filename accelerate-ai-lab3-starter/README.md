# Lab 3: Prototype to Production - Starter Files

Welcome to Lab 3! This is the starter repository where you'll build a production-ready ADK agent step by step.

## 🚀 What You'll Build

In this lab, you'll focus on critical deployment aspects:

1. **Deploy Gemma to Cloud Run with GPU** - Set up a high-performance Gemma model backend
2. **Integrate the Gemma deployment with an ADK agent** - Connect your agent to the GPU-accelerated model
3. **Test with ADK Web interface** - Validate your conversational agent works correctly
4. **Perform load testing** - Observe how both Cloud Run instances auto-scale under load

## 📁 Starter Structure

```
accelerate-ai-lab3-starter/
├── README.md                    # This file
├── ollama-backend/              # Ollama backend (separate deployment)
│   └── Dockerfile               # Backend container (TODO: implement)
└── adk-agent/                   # ADK agent (separate deployment)
    ├── pyproject.toml           # Python dependencies (complete)
    ├── env.template             # Environment template (complete)
    ├── server.py                # FastAPI server (TODO: implement)
    ├── Dockerfile               # Container config (TODO: implement)
    ├── elasticity_test.py       # Elasticity testing (TODO: implement)
    └── production_agent/        # Agent implementation
        ├── __init__.py          # Package init (complete)
        └── agent.py             # Agent logic (TODO: implement)
```

## 🎯 Files to Complete

You'll need to implement the following files by following the codelab instructions:

**Ollama Backend:**

- 🚧 `ollama-backend/Dockerfile` - Ollama container

**ADK Agent:**

- ✅ `adk-agent/pyproject.toml` - Dependencies (already complete)
- ✅ `adk-agent/env.template` - Environment template (already complete)
- 🚧 `adk-agent/production_agent/agent.py` - ADK agent implementation
- 🚧 `adk-agent/server.py` - FastAPI server with endpoints
- 🚧 `adk-agent/Dockerfile` - Container configuration
- 🚧 `adk-agent/elasticity_test.py` - Elasticity testing script

## 📚 Getting Started

1. Follow the codelab instructions to implement each TODO section
2. Copy and paste the provided code snippets
3. Deploy Gemma backend to Cloud Run with GPU
4. Deploy ADK agent and test with elasticity testing

## 🔗 Resources

- [Complete Solution](https://github.com/amitkmaraj/accelerate-ai-lab3-complete)
- [Google ADK Documentation](https://cloud.google.com/agent-development-kit)
- [Cloud Run Documentation](https://cloud.google.com/run/docs)

Happy coding! 🎉

# Deployment GPU

'''
gcloud run deploy ollama-gemma3-270m-gpu \
  --source . \
  --region us-central1 \
  --concurrency 7 \
  --cpu 8 \
  --set-env-vars OLLAMA_NUM_PARALLEL=4 \
  --gpu 1 \
  --gpu-type nvidia-l4 \
  --max-instances 1 \
  --memory 16Gi \
  --allow-unauthenticated \
  --no-cpu-throttling \
  --no-gpu-zonal-redundancy \
  --timeout 600 \
  --labels dev-tutorial=codelab-agent-gpu
'''

# Deployment Agent

'''
export PROJECT_ID=$(gcloud config get-value project)

gcloud run deploy production-adk-agent \
   --source . \
   --region us-central1 \
   --allow-unauthenticated \
   --memory 4Gi \
   --cpu 2 \
   --max-instances 1 \
   --concurrency 50 \
   --timeout 300 \
   --set-env-vars GOOGLE_CLOUD_PROJECT=$PROJECT_ID \
   --set-env-vars GOOGLE_CLOUD_LOCATION=us-central1 \
   --set-env-vars GEMMA_MODEL_NAME=gemma3:270m \
   --set-env-vars OLLAMA_API_BASE=$OLLAMA_URL \
   --labels dev-tutorial=codelab-agent-gpu
'''

# Run the Elasticity Test

