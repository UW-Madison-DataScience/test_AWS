# Step 2: Running Python Code with SageMaker Notebooks

Amazon SageMaker provides a managed environment to simplify the process of building, training, and deploying machine learning models. By using SageMaker, you can focus on model development without needing to manually provision resources or set up environments. Here, we'll guide you through setting up a Jupyter notebook instance, loading data, training a model, and performing optional hyperparameter tuning using the Titanic dataset in S3.

> **Note**: We’ll use SageMaker notebook instances directly (instead of SageMaker Studio) for easier instance monitoring across users and streamlined resource management.

## Using the Notebook as a Controller
In this setup, the notebook instance functions as a **controller** to manage more resource-intensive compute tasks. By selecting a minimal instance (e.g., `ml.t3.medium`) for the notebook, you can perform lightweight operations and leverage the **SageMaker Python SDK** to launch more powerful, scalable compute instances when needed for model training, batch processing, or hyperparameter tuning. This approach minimizes costs by keeping your controller instance lightweight while accessing the full power of SageMaker for demanding tasks.

## Summary of Key Steps
1. Navigate to SageMaker in AWS.
2. Create a Jupyter notebook instance as a controller.
3. Set up the Python environment within the notebook.
4. Load the Titanic dataset from S3.
5. Use SageMaker SDK to launch training and tuning jobs on powerful instances.
6. View and monitor training/tuning progress.

## Detailed Procedure

### 1. Navigate to SageMaker
- In the AWS Console, search for **SageMaker** and select **SageMaker - Build, Train, and Deploy Models**.
- Click **Set up for single user** (if prompted) and wait for the SageMaker domain to spin up.
- Under **S3 Resource Configurations**, select the S3 bucket you created earlier containing your dataset.

### 2. Create a New Notebook Instance
- In the SageMaker menu, go to **Notebooks > Notebook instances**, then click **Create notebook instance**.
- **Notebook Name**: Enter a name (e.g., `Titanic-ML-Notebook`).
- **Instance Type**: Start with a small instance type, such as `ml.t3.medium`. You can scale up later as needed for intensive tasks, which will be managed by launching separate training jobs from this notebook.
- **Permissions and Encryption**:
   - **IAM Role**: Choose an existing role or create a new one. The role should include the `AmazonSageMakerFullAccess` policy to enable access to AWS services like S3.
   - **Root Access**: Choose to enable or disable root access. If you’re comfortable with managing privileges, enabling root access allows for additional flexibility in package installation.
   - **Encryption Key** (Optional): Specify a KMS key for encrypting data at rest if needed. Otherwise, leave it blank.
- **Network (Optional)**: Networking settings are optional. Configure them if you’re working within a specific VPC or need network customization.
- **Git Repositories Configuration (Optional)**: Connect a GitHub repository to automatically clone it into your notebook. Note that larger repositories consume more disk space, so manage storage to minimize costs.
   - **Tips to Manage Storage**:
     - Use **S3** for large files or datasets instead of storing them in the repository.
     - Keep Git repositories small (code and small files only).
     - Monitor storage with the following command in a terminal to check disk usage:
       ```bash
       du -sh *
       ```
- **Tags (Optional)**: Adding tags helps track and organize resources for billing and management.
   - Example: `Key: Job, Value: Titanic-Analysis-Notebook`

Click **Create notebook instance**. It may take a few minutes for the instance to start. Once its status is **InService**, you can open the notebook instance and start coding.

### Managing Training and Tuning with the Controller Notebook
After setting up the controller notebook, use the **SageMaker Python SDK** within the notebook to launch compute-heavy tasks on more powerful instances as needed. Examples of tasks to launch include:

- **Training a Model**: Use the SDK to submit a training job, specifying a higher-powered instance (e.g., `ml.p2.xlarge` or `ml.m5.4xlarge`) based on your model’s resource requirements.
- **Hyperparameter Tuning**: Configure and launch tuning jobs, allowing SageMaker to automatically manage multiple powerful instances for optimal tuning.
- **Batch Processing**: Offload batch data processing tasks to a larger instance if needed.

This setup allows you to control costs by keeping the notebook instance minimal and only incurring costs for larger instances when they are actively training or tuning models.

For more details, refer to the [SageMaker Python SDK documentation](https://sagemaker.readthedocs.io/) for example code on launching and managing remote training jobs.
