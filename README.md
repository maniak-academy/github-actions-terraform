# **Setting up Centralized Terraform State with GitHub Actions**

The aim of this guide is to establish a centralized location for Terraform state files, which will then be automatically applied via GitHub Actions whenever you push changes to your GitHub repository.

## **Prerequisites**
- An active AWS account.
- Git installed on your local machine.
- A GitHub account.
- A GitHub repository where your Terraform code will reside.
- [Terraform](https://www.terraform.io/downloads.html) binary installed.

## **Setup Guide**

### **1. Backend Bucket Creation**

The initial phase involves creating an S3 bucket on AWS to serve as the backend for your Terraform state files.

**Steps:**

1. **Clone the Repository:**  
   Use the following command to clone the repository to your local machine:

```
git clone git@github.com:malconip/terraform-tfstate.git
```

2. **Set AWS Credentials Locally:**  
Ensure you have your AWS credentials set up in your environment, so Terraform can interact with AWS services.
```
export AWS_ACCESS_KEY_ID=<your-access-key-id>
export AWS_SECRET_ACCESS_KEY=<your-secret-access-key>
```

3. **Initialize Terraform:**  
Navigate to the cloned repository's directory and run:

```
terraform init
```


4. **Update Bucket Name:**  
Edit the `main.tf` file. Find the `bucket` properties in both the backend configuration and the S3 resource blocks. Update them with your desired S3 bucket name.

5. **Apply Terraform Configuration:**  
Run the following command and confirm by typing `yes` when prompted:

```
terraform apply
```


### **2. Setting Up Terraform with GitHub Actions**

Once the backend is set up, you'll configure GitHub Actions to automatically apply your Terraform configurations whenever there's a change.

**Steps:**

6. **Enable Backend Configuration:**  
Go back to the `main.tf` file and uncomment (remove the `#` symbols) the backend configuration section.

7. **Re-initialize Terraform:**  
As you've made changes to the backend configuration, you'll need to re-initialize Terraform:

```
terraform init
```

When prompted, type `yes` to confirm the move of your state to the new backend location.

8. **Store AWS Credentials in GitHub:**  
For GitHub Actions to interact with AWS on your behalf, it needs your AWS credentials. Store them as secrets in your GitHub repository:

- Navigate to your repository on GitHub.
- Go to the 'Settings' tab.
- Under the left sidebar, click on 'Secrets'.
- Click the 'New repository secret' button.
- Add both `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` with their respective values.

9. **Commit Changes:**  
Commit any changes made to your Terraform files:

```
git add .
git commit -m "First commit with backend configuration"
```

10. **Push to GitHub:**  
Push your changes to the remote GitHub repository:

```
git push
```


---

After completing these steps, your Terraform configurations will be set up to use a centralized state stored in an AWS S3 bucket. Additionally, using GitHub Actions, every push you make to this repository will trigger Terraform actions (e.g., `terraform apply`) based on your configurations. Ensure that you set up a `.github/workflows/` directory in your repository with the necessary GitHub Actions configurations for Terraform.



