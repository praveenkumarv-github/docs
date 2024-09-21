# POC on Private Connection Between Terraform Enterprise and Databricks Workspace

I’ve observed an issue with Terraform Enterprise (TF server) being unable to access data inside the Databricks workspace after we disabled public access. As expected, all HTTP connections within the Databricks workspace must now route through a private link (PE).
 
![image](https://github.com/user-attachments/assets/8451ba93-72f1-46d7-8c9b-26e42fabca7c)

To address this, I believe we should enable a private connection between the Terraform server and Databricks via a private link to maintain secure communication. 
Alternatively, should we reach out to Databricks support to explore other possibilities or solutions for this issue?
