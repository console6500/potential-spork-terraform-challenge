# 04_06_ci_cd_for_infrastructure_as_code


## Reccommended Reading
- [Running Terraform in Automation](https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform)

# Using the Exercise Files
## Add permissions to your service account; Create an S3 bucket
1. _For details on creating or updating a service account, see the [instructions in lesson 04_04](../04_04_create_a_service_account/README.md)._

    Add the following permission to the service account you will use for this exercise:

        AmazonS3FullAccess

1. Create an S3 bucket to use for storing Terraform state files.
    1. Go to the [S3 homepage](s3.console.aws.amazon.com).
    1. Select `Create bucket`.
    1. Give your bucket a name.  The bucket name must be globally unique and must not contain spaces or uppercase letters. [See rules for bucket naming](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucketnamingrules.html).
    1. Confirm the region for the bucket.  It should be the same region you will use to configure your service account in the repository.
    1. Keep all defaults and select `Create bucket` at the bottom of the form.

## Add service account credentials; Configure branch and environment protection rules
1. Create a new repo and add the exercise files for this lesson.
1. Move the workflow files into `.github/workflows`.
1. Configure the service account credentials.
    1. Select `Settings` -> `Secrets and variables` -> `Actions`.
    1. Select `New respository secret`.
    1. Create entries for the following using the values retrieved when you created the service account.
        - `AWS_ACCESS_KEY_ID`
        - `AWS_SECRET_ACCESS_KEY`
        - `AWS_ACCOUNT_NUMBER`
    1. Select the `Variables` tab.
    1. Select `New repository variable`.
    1. Create an entry for `AWS_REGION` using the same region as the bucket created in previous steps.

1. Create a Branch protection rule.
    1. Select `Settings` -> `Branch protection rules` -> `Add branch protection rule`.
    1. Under "Branch name pattern" enter: `main`.
    1. Under "Protect matching branches":
        - Select `Require a pull request before merging`.
        - Un-selecet `Require approvals`.  (This is because you can't approve your own merge requests.  As a result, you would need to overried the merge protection any way.)
        - Select `Require status checks to pass before merging`.
        - At the bottom of the page, select `Create`.
1. Edit the file [`variables.tf`](./variables.tf).
1. Find the `server_count` code block at the top of the file.

        variable "server_count" {
            type        = number
            default     = 3
            description = "The total number of VMs to create"
        }

    Change `default     = 3` -> `default     = 4`.

1. Select `Commit changes`.
1. Select `Create a new branch for this commit and start a pull request`.  Then select, `Propose changes`.
1. Select `Create pull request`.
1. Observe the checks and summaries from  GitHub Actions being written to the pull request.  Wait for the workflow to complete.
1. Select `Merge pull request` -> `Confirm merge`.
1. Go to the `Actions` tab.  Select the most recent running workflow.
1. Observe the pipeline's progress and note the updates to the workflow summary.
1. When prompted, select `Review deployments`.
1. Select `Production` -> `Approve and deploy`.
1. Observe the pipeline's progress an note the updates to the workflow summary.

# Remove the resources
1. Select the `Actions` tab.
1. Select the workflow `99-Destroy Resources`.
1. Next to "This workflow has a workflow_dispatch event trigger.", Select `Run workflow` -> `Run workflow`.
1. Select the running workflow. Observe the pipeline's progress and note the updates to the workflow summary.
1. When prompted, select `Review deployments`.
1. Select `Production` -> `Approve and deploy`.
1. Observe the pipeline's progress and note the updates to the workflow summary.

https://developer.hashicorp.com/terraform/tutorials/automation/automate-terraform
