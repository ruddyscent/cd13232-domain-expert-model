# Domain Expert Model with Amazon SageMaker

This project uses Amazon SageMaker AI and JumpStart to evaluate, fine-tune, deploy, and test a Meta Llama 2 7B language model for domain-specific text generation.

The workflow is split into two notebooks:

- `model_evaluation.ipynb`
  - Deploys the base Llama 2 7B model.
  - Sends domain-specific prompts to evaluate the base model before fine-tuning.

- `model_fine_tuning.ipynb`
  - Fine-tunes Llama 2 7B on a selected domain dataset.
  - Deploys the fine-tuned model.
  - Runs the same type of prompt evaluation against the fine-tuned endpoint.

## Repository Contents

- `financial_dataset.txt`: Finance domain sample data.
- `medical_dataset.txt`: Medical domain sample data.
- `it_dataset.txt`: IT domain sample data.
- `model_evaluation.ipynb`: Base model evaluation notebook.
- `model_fine_tuning.ipynb`: Fine-tuning and fine-tuned model evaluation notebook.
- `udacity_gen_ai_aws_project_report.docx`: Project report template/document.
- `udacity_gen_ai_aws_project_report.pdf`: Project report export.

## SageMaker Runtime

Recommended notebook kernel:

```text
conda_pytorch_p310
```

The notebook instance runs the SageMaker SDK and launches SageMaker Training Jobs or Endpoints. It does not run the Llama 2 7B model locally.

Recommended notebook instance:

```text
ml.t3.large or ml.m5.large
```

The Training Job and Endpoint still need a GPU instance. Keep the Llama 2 7B training and deployment instance type as:

```python
instance_type="ml.g5.2xlarge"
```

Use at least 20GB of EBS storage, preferably 30GB, to leave enough room for package installation.

## Running the Notebooks

Run each notebook in SageMaker JupyterLab with this sequence:

1. Select the `conda_pytorch_p310` kernel.
2. Run the setup/install cell.
3. Restart the kernel when prompted.
4. Select `conda_pytorch_p310` again.
5. Run the notebook from top to bottom.
6. Capture the required output for the project report.
7. Run the cleanup cell.
8. Confirm in the SageMaker console that the endpoint was deleted.

## Fine-Tuning Domain

The fine-tuning notebook uses a `domain` variable to choose the training dataset and evaluation prompt.

```python
domain = "finance"
```

Supported values:

```python
domain = "finance"
domain = "medical"
domain = "it"
```

Choose the domain that matches the dataset and report section you are working on.

## Expected Progress Logs

The following SageMaker logs are normal while a model is being deployed:

```text
INFO:sagemaker:Creating model ...
INFO:sagemaker:Creating endpoint-config ...
INFO:sagemaker:Creating endpoint ...
```

Creating a Llama 2 7B endpoint can take more than 10 minutes. Waiting up to around 20 minutes is usually normal. If there is no change after 30 minutes, check the endpoint status in the SageMaker console.

## Troubleshooting

If a notebook fails during deployment, check the printed `FailureReason` first.

Common causes:

- `ResourceLimitExceeded` or `quota`: insufficient `ml.g5.2xlarge` Training/Endpoint quota.
- `AccessDenied`: missing permissions on the SageMaker execution role.
- `ValidationException`: EULA, input data, model version, or instance type configuration issue.
- `Capacity`: temporary capacity shortage in the selected region or availability zone.

## Cost Warning

Training Jobs and Endpoints incur costs separately from the notebook instance. Endpoints continue to accrue cost until deleted, so always run the cleanup cell after finishing and confirm deletion in the SageMaker console.
