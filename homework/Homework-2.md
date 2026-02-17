# ECS160-HW2

#### _(Start date: ) Feb 17, 2026_
#### _(Due date: ) March 6, 2026_

### Total points: 7

## Problem: Moderation and Hashtagging of Social Media Posts

_Learning objectives_
1. Software architecture
   - Pipeline architecture using microservices
   - Microservice architecture
   - Inter-service communication using gRPC
2. Libraries and Frameworks:
   - FastAPI (for microservices)
   - gRPC (for inter-service communication)
   - LLM integration using Google Gemini

_Necessary background knowledge_
1. Python decorators (See [Python Decorators](https://www.w3schools.com/python/python_decorators.asp))
2. HTTP GET and POST methods. (See [HTTP Methods](https://www.w3schools.com/tags/ref_httpmethods.asp))
3. Basic networking concepts such as IP address, ports, etc.
4. Protocol Buffers (protobuf) basics

_Problem Statement_

You are provided with an `input.csv` file that consists of thousands of social media posts from Bluesky [here](). First, parse these social media posts into Python classes using any python library
of your choice. Then, you will design a pipeline of microservices that consists of the two microservices described below. Each microservice will take the contents of a single message as input and process it,
depending on the functionality of the microservice.

- **Microservice 1:** A moderation service that checks the contents of the post against a list of "banned words" listed at the end of this assignment. The moderation service should return `FAILED` if the post content fails the moderation. If it succeeds, it should forward the request to the hashtagging microservice via gRPC and will ultimately return the results of the second microservice to the client.
- **Microservice 2:** A hashtagging service that will analyze the contents of the post and tag the post with a hashtag, like `#vacation` and `#happy`. You will invoke Google's [Gemini](https://ai.google.dev/) model for the analysis (more later).

You will execute the pipeline on the top-10 most-liked top-level posts in `input.csv`. 
The output of the pipeline will either be `FAILED` or the hashtag. If the LLM refuses to generate a hashtag for some reason, you can tag it with a default tag such as `#bskypost`. In case any post fails the moderation, display it as `[DELETED]`. For other posts append the hashtag to the post content. 

## Implementing a Microservice

We will use [FastAPI](https://fastapi.tiangolo.com/) as our microservices framework. FastAPI uses Python decorators (similar to Java annotations) to define REST API endpoints.

Please clone the handout repository at https://github.com/davsec-teaching/ECS160_HW2_HANDOUT.

### Setting up your environment

Create a virtual environment and install all the dependencies in the environment. Make sure to add a `requirements.txt` file that contains the imported libraries:

```bash
python3 -m venv venv
source venv/bin/activate
```

### Writing a FastAPI microservice

A FastAPI microservice consists of an application instance and route handlers defined with decorators. Here is a sample moderation service:

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class ModerateRequest(BaseModel):
    post_content: str

@app.post("/moderate")
def moderate(request: ModerateRequest):
    # Moderation logic here
    return {"result": "PASSED"}
```

The `@app.post("/moderate")` decorator is analogous to Spring Boot's `@PostMapping("/moderate")`. The `ModerateRequest` class uses Pydantic to automatically parse and validate the JSON request body, similar to Spring Boot's `@RequestBody`.

Run the microservice with:

```bash
uvicorn moderation_service:app --port 8001
```

On a successful launch, you should see log messages similar to:

```
INFO:     Started server process [12345]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8001 (Press CTRL+C to quit)
```

You can test it with `curl`:

```bash
curl -X POST http://localhost:8001/moderate -H "Content-Type: application/json" -d '{"post_content": "Hello, this is a sample post!"}'
```

Remember, a microservice is an independent piece of software. Each microservice should be developed as an individual Python project (separate directory).

## Chaining Microservices with gRPC

While the client communicates with the moderation service over HTTP (REST), the moderation service will communicate with the hashtagging service using [gRPC](https://grpc.io/). As discussed in class, 
gRPC is a high-performance RPC framework that uses Protocol Buffers (protobuf) for message serialization, and is commonly used for inter-service communication in microservice architectures.

As covered in lecture, you will need to:

- The handout provides a `hashtagging.proto` file that defines the gRPC service contract and message types for the hashtagging service.
- Use `grpcio-tools` to generate the Python stubs from your `.proto` file.
- Implement the hashtagging service as a gRPC server using the generated servicer base class.
- Call the hashtagging gRPC service from the moderation service using the generated client stub.
- The moderation microservice should invoke the hashtagging microservice via gRPC only if the moderation passes.

Refer to the [official gRPC Python documentation](https://grpc.io/docs/languages/python/) for detailed guidance.

## LLM Integration with Gemini

We will use Google's Gemini model to generate hashtags for social media posts. You will have access to Gemini through Google Cloud. Please log in to your Google Cloud account and enable the Gemini (or Vertex AI) service and generate a key.


Install the Google Gen AI Python SDK:

```bash
pip install google-genai
```

Set your API key as an environment variable:

```bash
export GOOGLE_API_KEY="your-api-key-here"
```

Use the `gemini-2.0-flash` model from within the hashtagging microservice to generate hashtags. Sample code:

```python
from google import genai
import os

client = genai.Client(api_key=os.environ["GOOGLE_API_KEY"])

def generate_hashtag(post_content: str) -> str:
    response = client.models.generate_content(
        model="gemini-2.0-flash",
        contents=f"... Your prompt goes here...",
    )
    return response.text.strip()
```

Refer to the [Gemini API documentation](https://ai.google.dev/gemini-api/docs) for more details.

## Unit Tests

Write unit tests for your microservices using `pytest`. The unit tests for the moderation microservice should **not** assume that the hashtagging gRPC service is up and running. Similarly, the unit tests for the hashtagging microservice should **not** assume that the Gemini API is available. Use mocking (e.g., `unittest.mock`) to isolate each service in your tests.

## List of Banned Words
1. illegal
2. fraud
3. scam
4. exploit
5. dox
6. swatting
7. hack
8. crypto
9. bots

## Grading Rubric
- Moderation microservice correctly filters posts: 1 point
- Hashtagging microservice generates hashtags via Gemini: 1 point
- gRPC proto definition and code generation: 1 point
- gRPC communication between microservices works correctly: 1 point
- Pipeline produces correct output for top-10 most-liked posts and their replies: 1 point
- Unit tests with proper mocking: 1 point
- Code quality and project organization: 1 point

## Submission

Your final submission will consist of three components - two microservices and one main application that invokes the pipeline. Please zip these together into a single Zip file and submit on Gradescope. The Gradescope link will be updated soon.

1. Please add a README.md which clearly discloses any AI usage.
2. Note that the Academic Integrity and AI Policy specified in the course page apply to all homeworks.
