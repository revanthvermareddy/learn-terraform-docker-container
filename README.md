# Learn-Terraform-Docker-Container

This is a sample project for learning terraform and its basics using by doing locally using docker as the provider.

### Requirements:

- Docker
- Docker Desktop

### Key learnings:

#### To manage Terraform State:

1. The application state is contained within a file called `terraform.tfstate`.

2. Do not directly modify the `terraform.tfstate` file. If we ever need to modify the state of the terraform do it by changing the `main.tf` file and then runnign `terraform apply`.

3. When working in a team, every team member needs to work on the latest terraform state file i.e, `terraform.tfstate`. Hence we need to always set up a shared remote storage for the state file.
    
    Some of the popular choices for storing the state files are:

    - AWS S3
    - Terraform Cloud (by Hashicorp)
    - Azure Blog Storage
    - Google Cloud Storage

    and we can configure the terraform to the remote file as the state file storage location.

    Note: Never save TF state files in git, they can contain sensitive information in plain text format;

4. To prevent 2 users from concurrently modifying the application state we have to use state locking to prevent the conflicts and to prevent the state file corruption. So we lock the state file until the writing of state file is completed, this prevents concurrent writes to the state file.

    Ex: AWS S3 supports state locking and consistency checking via Dynamo DB.

    Note: Not all storage backends support this.

5. Always backup your state file in case of data loss or data corruption, etc. We can do this by enable versioning on the storage backend like S3 and we can rollback to the prev state.

6. Use one dedicated state file per environment (like DEV, STAGING and PROD) and each state file will have its own storage backend with locking and versioning enabled. Use workspace for multiple environments.

#### To manage Terraform Code and Apply Infrastructure Changes (IaC - GIT ops):

7. Host the TF scripts in github.

    - provides collaboration
    - provides versioning

8. Review the TF code and run automated CI tests just like application code. Use merge request to integrate the code changes.

9. Apply TF changes only through CD pipeline. This streamlines the process of updating the infrastructure.

#### TF Code:

- Use for_each instead of count if it's possible
- Use modules for code reuse (DRY)
- - Use pre-commit hooks to do basic Terraform fmt, linting before commiting changes
