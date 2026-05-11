Terraform Drift Detection & Auto-Remediation
Automated infrastructure drift detection using GitHub Actions, Terraform, and AWS.

Terraform AWS

📖 Overview
Detects and automatically fixes AWS infrastructure drift. Deploys VPC, ALB, ASG, and S3 with continuous monitoring.

Drift Detection and Remediation Using Terraform and GitHub Actions
<img width="1506" height="545" alt="image" src="https://github.com/user-attachments/assets/c03f64da-3159-4be0-930b-f677ffaeb4a6" />

<img width="1259" height="777" alt="image" src="https://github.com/user-attachments/assets/4f5b9314-7af6-4be0-a64a-56a663ac7b7a" />

Key Features: ⏰ Scheduled checks (every minute) • 🔄 Auto-remediation • 🌍 Multi-environment • 📊 Notifications

🚀 Quick Start
# 1. Clone & setup backend
git clone <repository-url>
./scripts/setup-backend.sh my-terraform-state-bucket us-east-1

Create the dev branch and move it 
<img width="1406" height="174" alt="image" src="https://github.com/user-attachments/assets/0a8aec7b-3cc7-4d74-ac23-a9638d05fb69" />
<img width="945" height="150" alt="image" src="https://github.com/user-attachments/assets/c5c6e5de-fdf2-4a9a-9f9e-41c4b7e07ce7" />
<img width="1548" height="594" alt="image" src="https://github.com/user-attachments/assets/843f4b3a-1043-43e3-94ba-909faf943018" />
# 2. Update backend-dev.hcl / backend-prod.hcl
# 3. Add GitHub Secrets: AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, SLACK_WEBHOOK_URL (optional)
# 4. Deploy
terraform init -backend-config="backend-dev.hcl"
terraform apply

📁 Project Structure
terraform-drift-detection/
├── .github/workflows/drift_detection.yml
├── scripts/ (setup-backend.sh, user_data.sh)
├── main.tf, vpc.tf, security_groups.tf, alb.tf, asg.tf, s3.tf
├── variables.tf, outputs.tf, backend.tf
└── backend-dev.hcl, backend-prod.hcl

🔄 How It Works
GitHub Actions triggers every minute
Terraform plan compares actual vs. desired state
If drift detected (exit code 2): auto-apply fixes
Create/close GitHub issues and Slack notifications

Testing the drift detection:
To test this we have to manually update something from the aws cloud services which we have created
Here I’m updating tag details of aws load balancer service

# To update it click on manage tags
<img width="2533" height="1188" alt="image" src="https://github.com/user-attachments/assets/b2bcaf01-b463-400d-b9f1-41e206dda89a" />

I have updated managed by tag manually at cloud
<img width="2533" height="1188" alt="image" src="https://github.com/user-attachments/assets/64701717-e962-4f25-9efb-8a92b03f2182" />

<img width="2409" height="859" alt="image" src="https://github.com/user-attachments/assets/7098d062-1050-47bf-b15d-79dcbe838f65" />

As we have that drift detection workflow continuously pulling between desired state and actual state
After every one minute as per the cron job. It should detect if there is any difference between desired state and actual state. 
It will find that someone had made the changed manually from aws cloud not from terraform console through commands so it should revert the changes

This will make sure it will not make the accidental changes to be happened to save it from drift detection. 
Only the code in the GitHub repository will act as single source of truth

# checking the logs:

<img width="1784" height="1079" alt="image" src="https://github.com/user-attachments/assets/4b0b2bf1-79a0-4e9e-a433-471816d31d57" />
<img width="1011" height="1069" alt="image" src="https://github.com/user-attachments/assets/dcdb31b5-52ca-42c8-b9d6-70150934899b" />

# we can check the slack channel notification as we got a drift:

<img width="1802" height="1055" alt="image" src="https://github.com/user-attachments/assets/decd6912-359f-40a4-8719-c24e65505aa4" />

# Workflow detects and fixes automatically
🔧 Configuration
Change detection frequency in .github/workflows/drift_detection.yml:

After the drift detection was completed the changes we have made for managed by tags were reverted back to original

<img width="2303" height="645" alt="image" src="https://github.com/user-attachments/assets/0b86a2cd-a654-4523-a83b-789464a23b70" />

<img width="2548" height="1226" alt="image" src="https://github.com/user-attachments/assets/4a7d047c-1eac-40d3-b066-9fd4171bfeb2" />

create the main branch and check out to main and merge the dev branch coce to main:
<img width="1355" height="479" alt="image" src="https://github.com/user-attachments/assets/c25d6057-3809-4084-b28d-6951a3219caf" />

<img width="2013" height="841" alt="image" src="https://github.com/user-attachments/assets/aebf8aa2-fb0d-4349-80c7-bf05bc4a6f0f" />


As I have not made any changed so it was exited with zero

<img width="2172" height="1233" alt="image" src="https://github.com/user-attachments/assets/3d57d579-f75b-4f92-9531-f01d31cdd9d1" />

<img width="850" height="282" alt="image" src="https://github.com/user-attachments/assets/4af091a2-4929-4038-9712-62f248e7a90c" />

<img width="1940" height="979" alt="image" src="https://github.com/user-attachments/assets/5a207388-cabe-426c-a64e-5038869d0c4a" />

<img width="2048" height="692" alt="image" src="https://github.com/user-attachments/assets/7a2597da-43e5-4072-9d72-21b900809cda" />

<img width="1104" height="673" alt="image" src="https://github.com/user-attachments/assets/2aa040b1-20a7-44cb-a7e9-44dffc598f26" />

schedule:
  - cron: "*/5 * * * *"  # Every 5 minutes

# To destroy the cloud services:

its a good practice to destroy the aws servies to save cost:

<img width="2510" height="1145" alt="image" src="https://github.com/user-attachments/assets/5fb83e72-82e1-42df-8683-2f8ccbd5964b" />

<img width="1841" height="796" alt="image" src="https://github.com/user-attachments/assets/f273fac0-0b57-41a5-a587-f4927ed12f29" />

<img width="2136" height="1194" alt="image" src="https://github.com/user-attachments/assets/54461f16-418c-4194-a564-0db0250b2f6c" />

<img width="1675" height="951" alt="image" src="https://github.com/user-attachments/assets/d71e53c3-4b4d-4d95-9d58-40adb1d2bd56" />

# Final CICD flow through github actions: 

<img width="2261" height="1123" alt="image" src="https://github.com/user-attachments/assets/97d279b7-91b9-4da7-b191-ec723d1c5875" />




🐛 Troubleshooting
Issue	Fix
Workflow not running	Enable workflow in Actions tab
Apply fails	Check AWS credentials & IAM permissions
State lock error	terraform force-unlock <LOCK_ID>

🧹 Cleanup
terraform destroy -auto-approve

