# SignalFx-SmartAgent-Tomcat9-JMX-Terraform
Cloning this git repository will allow you to deploy a GCP instance (Ubuntu 18.04.5 LTS) with Tomcat9, SignalFx SmartAgent configured to monitor the Tomcat JVM and send metrics to SFx

# Assumptions

* You have a GCP account
* You have terraform installed and working
* You have an active Splunk SignalFx account

# Adding permissions for terraform to create resources on GCP

Log into your GCP account > Choose a project from the dropdown or Create a new project > Give your project or name or accept the default > Create.

Now you need to create a service account. In the GCP dashboard > IAM & admin > Service accounts > CREATE SERVICE ACCOUNT > Service Account Name [terraform-sa] > Create > Select a role > Project > Editor > Continue > Create Key > Key Type [json] > Create.

Download your project key and place it in the secrets/ directory. Then change: credentials = file("secrets/My_Project_7847-b6797c6771e2.json") to your project json filename in the main.tf file.

# Deploying with terraform

You can add your token before deploying through terraform (recommended) by changing "YOUR_SFX_ACCESS_TOKEN" to your token key in the scripts/installSignalFxSmartAgent.txt file.

Change YOUR_SFX_ACCESS_TOKEN to your access token in secrets/token

Change YOUR_USER to your username.
 metadata = {
   ssh-keys = "YOUR_USER:${file("~/.ssh/id_rsa.pub")}"
 }

$ terraform init

$ terraform apply

Answer 'yes' to deploy.

# Checking your JVM metrics are showing up in SignalFx

Create a chart and enter the metric gauge.jvm.threads.count and the dimension host:YOUR_HOST_NAME. You should see the a line showing your JVM datapoints in SignalFx.

---

# Cleaning up when you are done'

When you are done, you can destroy your deployment using the following command.

$ terraform destroy

Answer 'yes' to destroy the environment.
