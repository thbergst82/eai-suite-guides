# 1. Hands-on lab using the AMD AI Workbench

This guide walks participants through using the **AMD Enterprise AI Suite** on a pre-provisioned environment. The environment has already been installed — you will begin directly with AI model deployment on AMD AI Workbench.

In this hands-on lab session we will complete the following tasks:

- How to deploy an AI Model (AIM) and use the AMD AI Workbench interface to interact with it
- How to create and access a Jupyter Lab workspace within AMD AI Workbench
- How to connect to the deployed AIM from the AI workspace via the OpenAI-compatible API
- Experiment with the deployed AIM and AI workspace to test model capabilities

## Accessing the Environment

Your lab URL and credentials are provided in the **course handout** distributed at the start of the session.

1. Navigate to the AIWB URL.

2. Log in with the EAI Suite credentials. Ensure you are working within the project you created in the previous section.

------------------------------------------------------------------------

## Deploy an AI Model (AIM)

![AI Workbench — AIM catalog](../images/04-workbench/model-catalog-name-rm.png)

1. Navigate to the **Models** tab to access the AIM catalog
2. Select **GPT OSS 20B** model in the AIM catalog (you need to scroll down on the page)
3. Select **Deploy**. Use the default settings.

![Deploy AIM panel with Performance metric dropdown](../images/04-workbench/03-deploy-config-panel.png)

4. Click **Deploy**. A confirmation message will appear indicating the workload has started.

<!-- SCREENSHOT: Deployment confirmation notification or toast message -->

------------------------------------------------------------------------

## Monitor the deployment
While the model downloads and initializes, you can monitor its deployment status:

1. Navigate to the Dasboard page

2. Observe the status of your new workload

3. Wait for the status to transition from Pending to Running.
    - The deployment typically takes around 3-5 minutes for the GPT-OSS 20B model depending on network speed and cluster load.
    - You can also check the progress from the logs (click on the three dots on the workload, and select **View logs**)
    - **Note**: Hit refresh on the browser to update the status in case the model is still pending after 5 minutes.

## Interact with the model via the chat interface

Once your model is successfully deployed, you can interact with it through the chat Interface. The Chat page is ideal for exploratory testing, prompt engineering, and receiving immediate feedback from your deployed models.

To interact with your model, follow these steps:

1. On the Dashboard page, select the 3 dots icon on your model row in the Worloads table

2. Select Chat with model, or open the Chat page directly from the main navigation menu and then select your model

3. Enter your prompt in the chat box and submit it to the model. For example, you can submit a prompt like, *“Create a 3-day travel itinerary for a family visiting Dublin, focusing on historical sites and kid-friendly activities. Organize the output by day, time, and location.”*

4. Review the response in the chat window and refine your prompt or parameters as needed to achieve the desired result (see Figure below)

![Chat with the model directly in the Workbench interface.](../images/04-workbench/workbench_chat.png)

The Chat interface also includes a **Compare** mode. This feature sends the same prompt to two models (or the same model with different settings) and displays their responses side‑by‑side, making it easy to evaluate differences in responses, accuracy, tone, and reasoning. Typical use cases include comparing a base model against its fine-tuned version or testing how different system prompts and generation parameters affect a single model's behavior.

Let's demonstrate this feature by comparing our deployed model against itself, but with a modified system prompt:

1. Click the **Compare** toggle at the top of the Chat page to activate the dual-panel view
2. In the selection box ("Select model"), choose your deployed model
3. For the model on the right, open its settings panel and change the **System Prompt** to the following:
    - *"You are a helpful AI model that answers in a very cheerful tone with long answers"*
4. Enter the prompt *"Create a 3-day travel itinerary for a family visiting Dublin, focusing on historical sites and kid-friendly activities. Organize the output by day, time, and location."* and submit it
5. Observe how the two models provide different responses with one delivering a standard factual answer and the other adopting a cheerful persona

## Interact with the model via Jupyter Lab workspace
From the Workspace page, you can launch pre-configured development workspaces to accelerate experimentation. For example, JupyterLab and VS Code workspaces enable users to harness the power of the cluster with zero configuration on their local machines.

### Deploy your Jupyter Lab workspace
In this lab we will deploy a Jupyter Lab workspace. Navigate to the Workspaces page, where you will find a catalog of available workspaces:

1. Locate the Jupyter Lab card and click View and deploy. This will open the deployment configuration view where you can customize your workspace before deployment (See Figure 7).

2. Configure the following settings:

    * Workload name: Give your workspace a unique name

    * Container image: Keep the default image. The workspace will automatically pull and run the image upon deployment.

    * Select **Customize resource allocation** to deploy without GPUs. Specify the following values:

      * GPU: 0

      * CPU: 8 cores

      * RAM: 32 GB

![Customize resource allocation](../images/04-workbench/workspace-deploy-custom-resource-allocation.png)


3. Once you have finalized the configuration, launch the environment by clicking Quick deploy

As with AIM deployments, you can monitor the status of your workspace on the Dashboard page. It will show Pending while the resources are being provisioned.

### Launch the workspace
Once the workspace is ready, the deployment overlay will display a Launch button on the Workspaces page. Click it to launch and open your workspace.

### Retrieve connection details
To connect to your deployed model, you first need to retrieve its unique API endpoint:

1. Navigate to the Dashboard page

2. Select the deployed model (gpt-oss-20B) and click on the 3 dots

3. Select **Connect to model**

This will open a dialog window displaying the essential connection details, specifically the External URL (for connections outside the platform) and the Internal URL (for connections inside the platform, such as a workspace). See Figure below for reference.

![Model connection details](../images/04-workbench/workbench_aim_connect.png)

The window also provides sample code for querying the model in cURL, Python and Javascript format. We will use the Python snippet to connect from our Jupyter Lan notebook:

1. Since our notebook is running inside the platform, under the **CODE SNIPPET** section, click the **Use internal URL** toggle

2. Select the Python tab to view the corresponding Python code snippet

3. Click the Copy icon in the top-right corner and paste the code into a new cell in your Jupyter notebook

### Run your request
You need to install the ```requests``` library before running the code. Please copy the following line as the first row in the cell:

```!pip install requests```

Finally, modify the sample code to send a more specific prompt. Locate the line that defines the user message and update it as shown below:

```“content”: “Hello!”``` to ```“content”: “What is the capital of Sweden?”```

The request should look like this:
```python
!pip install requests
import requests

url = "YOUR_INTERNAL_URL"
headers = {
    "Authorization": "Bearer UPDATE_YOUR_API_KEY_HERE",
    "Content-Type": "application/json"
}
data = {
    "model": "mistralai/Mistral-Small-3.2-24B-Instruct-2506",
    "messages": [
        {"role": "user", "content": "What is the capital of Sweden?"}
    ],
    "stream": False
}

response = requests.post(url, headers=headers, json=data)
result = response.json()
print(result["choices"][0]["message"]["content"])
```



If the connection is successful, you should receive an answer like the one below. The exact phrasing may vary slightly with each execution, which is expected behavior for large language models:

```
The capital of Sweden is Stockholm.
```
### Monitor inference endpoint
In this example, we are connecting to the model to verify the setup; however, if this workload was running in production—serving one or multiple products—monitoring the inference endpoint logs and metrics would be essential for maintaining reliability, detecting regressions, and planning capacity.

To monitor your inference endpoint, open the Dashboard page, select the workload (use the three dots on the far right) and choose “Open details”.

The details’ view lets you inspect and review inference metrics over time such as time‑to‑first‑token, request count, tokens generated and other indicators. It also shows workload metadata such as resource utilization, AIM build/version, and configuration settings.

![Monitor the status of your workload](../images/04-workbench/workbench_aim_monitor_details.png)

## Experimentation within the workspace

Now that you’ve successfully connected to the model, it’s time to **experiment and explore practical use cases**. You might want to:

- Integrate the model into your custom applications
- Benchmark the model’s performance on specific tasks
- Build an agent
- Implement a Retrieval-Augmented Generation (RAG) pipeline
- Apply the model to your own data for tasks like sentiment analysis, summarization, or classification

To demonstrate how you can experiment with your deployed models within the workspace, we will implement a **simple RAG pipeline** using **ChromaDB**. The goal is to enable the model to answer questions about custom data, in this case fictional products that were not part of its original training set.

Please continue to use the same Jupyter Lab notebook as before.

### RAG

A general RAG pipeline consists of four steps:

1. Submit – The user sends a query to the system
2. Retrieve – A vector store retrieves relevant chunks of information (context) based on the query
3. Augment – The retrieved context is combined with the query to form a new, more detailed prompt
4. Generate – The model uses the augmented prompt to produce a context-aware answer

We will now walk through a simplified implementation of these steps without detailed explanations. If you are keen to learn more about RAG, it’s highly recommended to check out our previous blogs:

- [Retrieval Augmented Generation (RAG) with vLLM, LangChain and Chroma](https://rocm.blogs.amd.com/artificial-intelligence/rag-pipeline-vllm/README.html)
- [HPC-Agent-RAG](https://rocm.blogs.amd.com/artificial-intelligence/hpc-agent-rag/README.html)
- [Retrieval Augmented Generation (RAG) using LlamaIndex](https://rocm.blogs.amd.com/artificial-intelligence/rag-llamaindex/README.html#retrieval-augmented-generation-rag-using-llamaindex)

### Dependencies

First, install the required dependencies inside your notebook:

```python
!pip install chromadb
```

Next, import the necessary libraries.

```python
import requests
import chromadb
```

### Submit a query

We will now create a reusable function to query our deployed model. This function will build upon the code from the previous section but will also incorporate a system prompt to guide the model's behavior:

```python
def query_model(system_prompt, user_query):
    """
    Query the model using the provided user question and system prompt.
    """

    url = "YOUR_INTERNAL_URL"
    headers = {
        "Authorization": "Bearer UPDATE_YOUR_API_KEY_HERE",
        "Content-Type": "application/json"
    }
    data = {
        "model": "openai/gpt-oss-20b",
        "messages": [
            {"role": "user", "content": user_query},
            {"role": "system", "content": system_prompt}  # new system prompt
        ],
        "stream": False
    }

    response = requests.post(url, headers=headers, json=data)
    result = response.json()

    return result["choices"][0]["message"]["content"]
```

Now, let's test the function by querying the model about a fictional product with ID 456. This will help demonstrate how the model responds when queried about topics or facts it has not encountered before, which is a crucial first step in understanding the need for RAG.

Execute the following code to test the function:

```python
# Prepare the system and user prompts
system_prompt = "You are a helpful assistant. Answer the user's question. If you don't know the answer, say you don't know."
user_query = "What are the 2024 sales numbers for Product ID 456?"

# Execute the query and print the result
query_model(system_prompt, user_query)
```

As expected, the model correctly identifies that it does not have access to this information and avoids inventing an answer (a behavior known as "hallucination"). The response should be similar to the following:

```text
I don’t have access to real-time sales data or specific databases, so I can’t provide the 2024 sales numbers for Product ID 456. To get this information, you might need to check your company’s internal sales reports, inventory management system, or contact the relevant department (e.g., sales, finance, or operations). If you have access to a CRM or ERP system, you could also look it up there.
```

### Create a vector database

To enable the model to answer questions about our fictional products, we must provide it with the relevant information. This is accomplished by augmenting the prompt with context retrieved from a knowledge base. For this purpose, we will store our fictional product data in a vector database using [Chroma](https://docs.trychroma.com/docs/overview/introduction) (an open‑source AI application database). The process is as follows:

1. Each text document is converted into a vector representation using an embedding model
2. When a user query is made, the system compares the query vector to the vectors in the database using a similarity measure, such as cosine similarity, to identify relevant information
3. The extracted context is then provided to the language model along with the original query, enabling it to generate a context-aware response

Let's create the Chroma database. By setting `chroma_data_path` to a local directory, Chroma will persist its data there. After executing the code below, a new `chroma_db_storage` folder will be created in your workspace's persistent storage (or in the folder where your notebook is saved):

```python
chroma_data_path = "chroma_db_storage" # Local path for ChromaDB
collection_name = "simple_rag"

client = chromadb.PersistentClient(path=chroma_data_path)
collection = client.get_or_create_collection(name=collection_name)

print(f"ChromaDB client initialized. Collection '{collection_name}' ready.")
print(f"Items in collection: {collection.count()}")
```

```text
ChromaDB client initialized. Collection 'simple_rag' ready.
Items in collection: 0
```

### Add example documents to chroma

Now, let's populate our vector database with some sample data. In a real-world application, you would typically source and chunk content from your organization’s knowledge bases, such as internal documents or other repositories. However, since this is a simple example, we will use a few simple text strings representing product information.

Add the following documents to your Chroma by running this code in a new cell:

```python
# Add three dummy chunks to the existing ChromaDB collection 
docs = [
"Product ID 123 - Sales numbers 2024: SEK 300m",
"Product ID 456 - Sales numbers 2024: SEK 503m",
"Product ID 789 - Sales numbers 2024: SEK 102m",
] 

collection.add( 
ids=["prod-123", "prod-456", "prod-789"], 
documents=docs 
) 

print(f"Items in collection after insert: {collection.count()}") 
```

```text
Items in collection after insert: 3
```

The next step in our RAG pipeline is to retrieve relevant information based on a user's query. This step uses semantic search to find the chunks that are most closely related to your question. These chunks are then added to the prompt. The helper function below encapsulates this retrieval logic, querying the ChromaDB collection and returning the most relevant document:

```python
def query_chroma(prompt, n_results=1):
    """
    Query the ChromaDB collection and return the top n_results documents
    joined into a single context string.
    """
    results = collection.query(
        query_texts=[prompt],
        n_results=n_results,
        include=["documents"],
    )

    retrieved_docs_text = []
    docs = results.get("documents", [[]])[0] if isinstance(results, dict) else []
    for i, doc_text in enumerate(docs):
        retrieved_docs_text.append(doc_text)

    return "\n\n".join(retrieved_docs_text)
```

Let's test our retrieval function with a sample query to see it in action:

```python
query_chroma("Sales figures for product 456")
```

The function should successfully find and return the relevant document from our database:

```text
Product ID 456 - Sales numbers 2024: SEK 503m
```

Feel free to experiment by adding more documents to your collection or by trying different queries to see how the retrieval works.

### Augment and generate

We will now combine the retrieval and generation steps to complete our RAG pipeline. The following function orchestrates this process by first querying the vector database for relevant context and then using that context to build an augmented prompt.

Notice that the system prompt is updated to explicitly instruct the model to use the provided context when formulating its answer.

```python
def augment_and_generate(user_question, n_results=1):
    """
    Retrieve context from ChromaDB, build a system prompt containing that context,
    and query the model with the augmented prompt.
    """
    # 1) Retrieve context from ChromaDB
    context = query_chroma(user_question, n_results=n_results)

    # 2) Build a system prompt with the retrieved context
    system_prompt = (
        "You are a helpful assistant. Use the provided context to answer the user's question."
        "If the answer is not in the context, say you don't know.\n\n"
        f"Context:\n{context if context else '[No relevant context retrieved]'}"
    )

    # 3) Query the model with the augmented prompt
    return query_model(system_prompt=system_prompt, user_query=user_question)
```

Let’s ask the same question as before. This time, our `augment_and_generate` function will automatically retrieve the necessary context before querying the model:

```python
answer = augment_and_generate("What are the 2024 sales numbers for Product ID 456?", n_results=1)
print("Model answer:\n", answer)
```

The answer is now accurate, as the model has received the domain specific information that it was not trained on:

```text
Model answer:
The 2024 sales numbers for Product ID 456 are SEK 503 million
```
## Summary

In this hands-on lab session, you have completed the practical steps for deploying and managing AI workloads using AMD AI Workbench. We began by deploying an AMD Inference Microservice (AIM) for optimized inference, verifying its functionality directly through the Chat UI. Following this, we deployed a workspace to facilitate hands-on experimentation with the AIM inference endpoint within the platform.
