# Getting Started

# CloudRun
Google Cloud Run

# Lab [URL](https://goo.gle/aaiwcr-2)

# Run the deployment command
'''
uvx --from google-adk \
adk deploy cloud_run \
  --project=$PROJECT_ID \
  --region=europe-west1 \
  --service_name=zoo-tour-guide \
  --with_ui \
  . \
  -- \
  --labels=dev-tutorial=codelab-adk
'''
