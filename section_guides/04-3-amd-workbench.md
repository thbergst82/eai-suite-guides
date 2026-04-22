# 1. AMD AI Workbench

AMD AI Workbench is the end-user interface for deploying and interacting with AI models. 

1. Navigate to the AIWB URL.

2. Log in with the EAI Suite credentials. Ensure you are working within the project you created in the previous section.

<!-- SCREENSHOT: AMD AI Workbench landing page after login, showing the main navigation -->

------------------------------------------------------------------------

## Deploy an AI Model (AIM)

> **Note:** You are in the **AMD AI Workbench** interface for this section. Ensure you have selected the correct project before proceeding.

![AI Workbench — AIM catalog](../images/04-workbench/model-catalog-name-rm.png)

1. Navigate to the **Models** tab to access the AIM catalog
2. Select **GPT OSS 20B** model and click the **three-dot menu (⋮)** in the bottom-right corner of the model card
3. Select **Deploy**

![Model card with Deploy option in three-dot menu](../images/04-workbench/model-catalog-name-rm.png)

4. Configure the **Deployment Settings**:

   - **Performance metric** — Select the optimization target from the dropdown:

     | Option | When to use |
     |--------|-------------|
     | **Latency** | When minimizing response time per request is the priority |
     | **Throughput** | When maximizing sustained requests/second is the priority |

   - **Unoptimized deployment** — Toggle **Allow** only when deploying to hardware the AIM is not specifically optimized for. Leave this off for standard deployments.

   <!-- TODO: Specify which performance metric to select for this HOL exercise -->

<!-- SCREENSHOT: Deployment config panel showing Performance metric dropdown open with Latency and Throughput options -->

![Deploy AIM panel with Performance metric dropdown](../images/04-workbench/03-deploy-config-panel.png)

6. Click **Deploy**. A confirmation message will appear indicating the workload has started.

<!-- SCREENSHOT: Deployment confirmation notification or toast message -->

7. **Wait for the model to become ready.** Navigate to **Workloads** to monitor deployment status. The model is ready when its status shows **Running**.

   > Deployment typically takes <!-- TODO: fill in approximate time, e.g., "3–5 minutes" --> depending on model size and cluster load.

<!-- SCREENSHOT: Workloads list view showing the deployed model with "Running" status -->

------------------------------------------------------------------------

## Monitor the deployment
While the model downloads and initializes, you can monitor its deployment status:

1. Navigate to either the Deployed Models tab (on the Models page) or the Dasboard page

2. Observe the status of your new workload

3. Wait for the status to transition from Pending to Running

## Interact with the model via the chat interface

Once your model is successfully deployed, you can interact with it through the chat Interface. The Chat page is ideal for exploratory testing, prompt engineering, and receiving immediate feedback from your deployed models.

To interact with your model, follow these steps:

1. Navigate to the Deployed Models tab, pick your model and click Chat with model, or open the Chat page directly from the main navigation menu and then select your model

2. Enter your prompt in the chat box and submit it to the model

3. Experiment with the model’s output by adjusting generation parameters (see Figure 5 below). Access these controls, such as temperature, by clicking the settings toggle in the upper-right corner.

4. Review the response in the chat window and refine your prompt or parameters as needed to achieve the desired result

For example, you can submit a simple prompt like, “Can you create an itinerary for my visit to Paris during 7 days” and then observe how modifying the generation parameters affects the detail and style of the answer.

Output:

## Interact with the model via Jupyter Lab workspace
From the Workspace page, you can launch pre-configured development workspaces to accelerate experimentation. For example, JupyterLab and VS Code workspaces enable users to harness the power of the cluster with zero configuration on their local machines.

### Deploy your Jupyter Lab workspace
Navigate to the Workspaces page, where you will find a catalog of available workspaces:

1. Locate the Jupyter Lab card and click View and deploy. This will open the deployment configuration view where you can customize your workspace before deployment (See Figure 7).

2. Configure the following settings:

    * Workload name: Give your workspace a unique name

    * Container image: Keep the default image. The workspace will automatically pull and run the image upon deployment.

    * Customize resource allocation: Allocate hardware resources using the provided sliders. Please use the following configuration:

      * GPU: 0

      * CPU: 8 cores

      * RAM: 32 GB

3. Once you have finalized the configuration, launch the environment by clicking Quick deploy

As with AIM deployments, you can monitor the status of your workspace on the Dashboard page. It will show Pending while the resources are being provisioned.

### Launch the workspace
Once the workspace is ready, the deployment overlay will display a Launch button. Click it to open your workspace.

For the next steps, you will need to create a Jupyter Notebook:

Click New file from the File menu

Choose the Jupyter notebook file type or create a new file and save it with the file extension “.ipynb”

If you want to save it, make sure it’s saved in your persistent storage directory

From this point forward, all code provided in this blog should be executed within this notebook.

### Retrieve connection details
To connect to your deployed model, you first need to retrieve its unique API endpoint:

1. Navigate to the Dashboard page

2. Select the deployed model and click on the 3 dots

3. Select **Connect to model**

This will open a dialog window displaying the essential connection details, specifically the External URL (for connections outside the platform) and the Internal URL (for connections inside the platform, such as a workspace). See Figure 9 for reference.

The window also provides sample code for querying the model in cURL, Python and Javascript format. We will use the Python snippet to connect from our Jupyter Lan notebook:

1. Since our notebook is running inside the platform, select the Internal URL

2. Choose the Python tab to view the corresponding Python code snippet

3. Click the Copy icon in the top-right corner and paste the code into a new cell in your Jupyter notebook

Finally, modify the sample code to send a more specific prompt. Locate the line that defines the user message and update it as shown below:

“content”: “Hello!” to “content”: “What is the capital of Sweden?”

The request should look like this:
```
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

### Run your request
Since we are using an internal connection, an API key is not required for authentication. You can now execute the code cell containing the Python script. You need to install the ```requests``` library before running the code:

```!pip install requests```

When you run the notebook for the first time, you may be prompted to select a kernel for your notebook. If prompted, install or choose the appropriate Python environment. Once the kernel is active, the notebook will execute the code and display the results.

If the connection is successful, you should receive an answer like the one below. The exact phrasing may vary slightly with each execution, which is expected behavior for large language models:

```
The capital of Sweden is Stockholm. It is located on the eastern coast of the country, where Lake Mälaren meets the Baltic Sea. Stockholm is known for its beautiful archipelago, historic sites like the Royal Palace, and vibrant culture.
```
### Monitor inference endpoint
In this example, we are connecting to the model to verify the setup; however, if this workload was running in production—serving one or multiple products—monitoring the inference endpoint logs and metrics would be essential for maintaining reliability, detecting regressions, and planning capacity.

To monitor your inference endpoint, open the Dashboard page, select the workload (use the three dots on the far right) and choose “Open details”.

The details’ view lets you inspect and review inference metrics over time such as time‑to‑first‑token, request count, tokens generated and other indicators. It also shows workload metadata such as resource utilization, AIM build/version, and configuration settings.

**Next:** Proceed to [Blueprints](./05-4-blueprints.md) to deploy a solution blueprint.
