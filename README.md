# Specialized Training Resources

## Overview
This repository contains files that are used for instructor-led training courses as part of the Cloud Learning Services Specialized Training program. These files are used in classroom demos, hands-on lab activities, and for supplemental discussions around course topics. Files are organized by course.

## Contributions
Please see [CONTRIBUTING.md](https://github.com/GoogleCloudPlatform/specialized-training-content/blob/main/CONTRIBUTING.md) for more details on the contribution workflow.

## Disclaimer
This is not an officially supported Google product. Usage of Google Cloud products will incur charges. Learn more about pricing [here](https://cloud.google.com/pricing).

## Licensing
All the code in this repo is licensed under the Apache License, Version 2.0 (the "License"). You may obtain a copy of the License [here](https://www.apache.org/licenses/LICENSE-2.0).

*Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License*

## Step

### Task 1. Set up your development environment and review the agent

```
git clone https://github.com/alvinrach/specialized-training-content.git
cd specialized-training-content
```

```
cd courses/build_production_ready_agents/ch2_lab/
uv venv
source .venv/bin/activate
uv pip install -r requirements.txt
```

```
cloudshell workspace .
```

### Task 2: Launch your servers

```
touch .env
edit .env
```

Paste to .env
```
APP_NAME="adk_agent_app"
GOOGLE_CLOUD_PROJECT="qwiklabs-gcp-02-b0e7308008c3"
GOOGLE_CLOUD_LOCATION="global"
AGENT_RUNTIME_LOCATION="us-central1"
SESSION_SERVICE_PROVIDER="in_memory"
MEMORY_SERVICE_PROVIDER="in_memory"
REASONING_ENGINE_APP_NAME="reasoning_engine_app"
DATABASE_URL="postgresql+asyncpg://adk:qwiklabs-gcp-02-b0e7308008c3-pass@localhost:5432/adk_sessions"

GOOGLE_GENAI_USE_VERTEXAI=TRUE
```

```
python sessions_server.py
```

in another terminal
```
cd ~/specialized-training-content/courses/build_production_ready_agents/ch2_lab/
python client_server.py
```

open server & then client

### Task 3. Test the InMemorySessionService implementation
```
Hello
```
To the chat

```
Please tell me about Google Cloud Run
```

```
Actually, I learn better if things are laid out like this:

1. Starting with a simple example scenario
2. Showing how to apply the concept to that scenario
3. Explaining why it works
4. Including a diagram
5. Defining any specialized terms
6. Availability

I learn best by seeing concepts applied to realistic situations.

Can you please re-address the question with this feedback in mind?
```

```
Tell me about BigQuery
```

See, it follows. But open new session, then
```
Tell me about Google Cloud Storage
```

It fails to follow

Another thing, try restart
```
python sessions_server.py
```

And
```
Please explain TPUs
```

It will
![alt text](image.png)

Session data has lost. Next we'll learn how to make it saved.

### Task 4. Update the application to use the VertexAiSessionService implementation

Open sessions_server.py and watch this:

![alt text](image-1.png)

Add third terminal

```
cd ~/specialized-training-content/courses/build_production_ready_agents/ch2_lab/scripts
export GOOGLE_CLOUD_PROJECT=qwiklabs-gcp-01-f270072861b4
uv venv
source .venv/bin/activate
uv pip install google-cloud-aiplatform==1.139.0 google-adk==1.26.0
python setup_agentruntime.py
```

And on STUDENT TASK: Add the VertexSessionService implementation, append before logging
```
    from google.adk.sessions import VertexAiSessionService
    session_service = VertexAiSessionService(project=GOOGLE_CLOUD_PROJECT, location=AGENT_RUNTIME_LOCATION)
    APP_NAME = os.getenv("REASONING_ENGINE_APP_NAME", "reasoning_engine_app")  
```

Use this .env

| **Variable**                 | **Old Value**          | **New Value**                                       |
| ---------------------------- | ---------------------- | --------------------------------------------------- |
| `SESSION_SERVICE_PROVIDER`   | `in_memory`            | `vertex`                                            |
| `REASONING_ENGINE_APP_NAME`  | `reasoning_engine_app` | *value copied from the Agent Runtime script output* |


Restart python sessions_server.py

```
Tell me about GKE
```

Restart again python sessions_server.py, but dont refresh the web page (why? this is only to provide session, not database)

```
Can you give me an example scenario and how it would be used?
```

### Task 5. Test memory with the InMemoryMemoryService implementation

Use this .env

| **Variable**                 | **Old Value**          | **New Value**                                       |
| ---------------------------- | ---------------------- | --------------------------------------------------- |
| `SESSION_SERVICE_PROVIDER`   | `vertex`            | `in_memory`                                            |
| `REASONING_ENGINE_APP_NAME`  | `reasoning_engine_app` | *value copied from the Agent Runtime script output* |


```
python memory_server.py
```

```
I learn best if things are laid out like this:

1. Starting with a simple example scenario
2. Showing how to apply the concept to that scenario
3. Explaining why it works
4. Including a diagram
5. Defining any specialized terms
6. Availability

I prefer seeing concepts applied to realistic situations.
```

```
Tell me about the Gemini model and API
```

now open new session on the chat and

```
Tell me about VPC Networks
```

Now its on the same format, it remembers the previous session

### Task 6: Update server to use VertexAiMemoryBankService implementation

Use this .env

| **Variable**                 | **Old Value**          | **New Value**                                       |
| ---------------------------- | ---------------------- | --------------------------------------------------- |
| `MEMORY_SERVICE_PROVIDER`   | `in_memory`            | `vertex`                                            |
| `REASONING_ENGINE_APP_NAME`  | `reasoning_engine_app` | *value copied from the Agent Runtime script output* |

start memory_server.py

```
I learn best if things are laid out like this:

1. Starting with a simple example scenario
2. Showing how to apply the concept to that scenario
3. Explaining why it works
4. Including a diagram
5. Defining any specialized terms
6. Availability

I prefer seeing concepts applied to realistic situations.
```

restart memory_server.py

```
Please teach me about Colab Enterprise
```

