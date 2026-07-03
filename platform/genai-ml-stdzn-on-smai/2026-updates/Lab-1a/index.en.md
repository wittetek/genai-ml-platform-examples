---
title : "Lab 1A: Setup Self-Provisioned Environment"
weight : 122
---

## Lab Overview

In this lab, you'll establish and configure a governed ML environment using Amazon SageMaker AI. You have provisioned the infrastructure using AWS CloudFormation for building and deploying your ML Project.

### Setup Infrastructure  


### Infrastructure Overview

Our CloudFormation template creates a production-ready ML governance environment with:

- **Security & Networking**: VPC with private subnets, VPC endpoints, and security groups
- **SageMaker Resources**: Domain, user profiles, private spaces, and MLflow tracking server
- **Data Governance**: S3 Access Grants and segregated storage buckets

**Viewing Your Infrastructure:**
1. Navigate to **CloudFormation Console** → **Stacks** → **sagemaker-domain-with-vpc**
2. Review the **Resources** and **Outputs** tabs

![CloudFormation Stack](/static/images/lab-1/sm-ai-gov-cloudformation.png)

3. Take the note of **GrantsBucketName** and **DataBucketName** from **Outputs** tabs. You will need these buckets later in labs. 

![CloudFormation Outputs](/static/images/lab-1/cfn-output.png)

### Lab 1 Workflow

This lab guides you through setting up a complete AI governance environment. Here's how the lab flows:

**🔍 Step 1: User Profiles** - Explore pre-configured user roles and permissions in the SageMaker console

**⚙️ Step 2: Authentication** - Verify IAM-based authentication setup for secure access

**🚀 Step 3: Enable Projects & JumpStart** - Configure SageMaker capabilities and create required IAM policies

**🔒 Step 4: VPC Security** - Review network isolation and security configurations

**📊 Step 5: Data Governance** - Test S3 Access Grants by running a Jupyter notebook as userA

**🏠 [Optional] Step 6: Private Spaces** - Create your personal development environment as userB


**Navigation Flow**: AWS Console → SageMaker → Domain Settings → User Profiles → JupyterLab → Data Testing

![SageMaker AI Domain Architecture](/static/images/lab-1/sm-domain-arch.jpg)

## 1. User Profiles

User profiles serve as the identity foundation within your SageMaker AI domain, enabling personalized access control and resource management.

**Steps:**
1. Navigate to **Amazon SageMaker AI** → **Domains** → **sagemaker-ai-gov-Domain**

![Domain User Profile](/static/images/lab-1/sm-ai-domain.png)

2. Select the **User profiles** tab

![Domain User Profile](/static/images/lab-1/userprofile.png)

3. Click on each of the users to see their details and locate the role:
   - **userA**: Assigned `sagemaker-ai-gov-usera-role`
   - **userB**: Assigned `sagemaker-ai-gov-userb-role`

![Domain User Profile](/static/images/lab-1/rolea.png)

Each user profile provides role-based access control with least privilege principles and complete audit trails.

## 2. Authentication Mode

SageMaker AI domains support IAM and Identity Center authentication modes. This workshop uses **IAM mode** authentication.

**Verification Steps:**
1. Navigate to **Amazon SageMaker AI** → **Domains** → **sagemaker-ai-gov-Domain**
2. Select **Domain settings** tab
3. Locate **Authentication method** field showing "IAM"

![Domain Authentication Mode](/static/images/lab-1/sm-domain-auth.png)

## 3. VPC Security

Your SageMaker domain operates in VPCOnly mode, providing enterprise-grade network security by isolating all ML workloads within a private network boundary.

**Verification Steps:**
1. Navigate to **Amazon SageMaker AI** → **Domains** → **sagemaker-ai-gov-Domain**
2. Select **Domain settings** → **Network** section
3. Observe VPCOnly mode, VPC ID, private subnets, and security groups

![VPC Configuration](/static/images/lab-1/studio-private-vpc.png)

4. Navigate to **VPC Console** → **Endpoints** to view configured VPC endpoints

![VPC Endpoints](/static/images/lab-1/vpc-endpoints.png)

## 4. [Optional] Data Governance with S3 Access Grants  

S3 Access Grants provide fine-grained access control with automatic enforcement of least-privilege principles.

**Access Control Configuration:**

| User Role | Accessible Prefixes | Access Level |
|-----------|-------------------|-------------|
| `sagemaker-ai-gov-usera-role` | `Product/*`, `UserA/*` | Read/Write |
| `sagemaker-ai-gov-userb-role` | `Product/*`, `UserB/*` | Read/Write |

**Testing Data Access:**
1. Navigate to **Amazon SageMaker AI** → **Domains** → **sagemaker-ai-gov-Domain** → **User profiles** and click **Open Studio** for **userA**

![UserA Studio](/static/images/lab-1/userAjupitor.png)

2. Open **JupyterLab**, click **run** for **usera-jl-space** and wait for notebook to be ready. 

![UserA notebook ](/static/images/lab-1/useranotebook.png)

3. Once ready click **Open**
4. Open `user-data-governance.ipynb` notebook

![UserA notebook ](/static/images/lab-1/notebook.png)

5. Run each cell and find place where you would be asked to place bucket name you noted from the CloudFormation Outputs tab (step 3 in Infrastructure Overview).

![Cell bucket ](/static/images/lab-1/cellbucket.png)

6. Test authorized access (UserA/abalone.csv) - you should have access.
7. Test unauthorized access (UserB/abalone.csv) - should fail

**Understanding Access Grants:**
1. Navigate to **S3 Console** → **Access Grants** → **View details**
2. Review grant records for each user role

![S3 Access Grants](/static/images/lab-1/s3-access-grants.png)

## 5. [Optional] Private Spaces

Private spaces provide dedicated, secure development environments for individual users.

**Creating Your Private Space:**
1. Navigate to **Amazon SageMaker AI** → **Domains** → **sagemaker-ai-gov-Domain** → **User profiles** → **userB** → **Open Studio**

![User Profile B](/static/images/lab-1/userprofileB.png)

2. Select **JupyterLab** → **Create JupyterLab Space**
3. Configure:
   - **Name**: `userB-private-space`
   - **Sharing**: `Private`
4. Click **Create space**


![Private Space Creation](/static/images/lab-1/studio-private-space.png)

5. Provide following configuration and click **Run Space**:
   - **Instance Type**: `ml.m5.2xlarge`
   - **Storage**: 50GB EBS volume


## Key Takeaways

You have successfully:

✅ **Infrastructure**: Deployed enterprise-grade ML infrastructure using CloudFormation  
✅ **Security**: Configured VPCOnly mode with private endpoints  
✅ **Identity**: Implemented role-based access control with user profiles  
✅ **Development**: Created isolated private spaces for development  
✅ **Governance**: Applied S3 Access Grants for data access control  
✅ **Projects**: Enabled SageMaker Projects and JumpStart capabilities  

## Next Steps

In Lab 2, you'll fine-tune Meta's Llama 3.2 3B model while leveraging the governance infrastructure:

- Model training with SageMaker Training Jobs
- Experiment tracking with MLflow
- Complete audit trails and lineage tracking
- Governance controls throughout the ML lifecycle