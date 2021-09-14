# GKE Cluster Creation for KubeFlow Pipeline Creation #

Files Included 
   1.	Terraform.tvars – Configurable file where in all the environment variables which are to be reused in the other scripts are listed out here . Settings like project_id , project_name are done here .
   2.	Gke.tf – Contains the details of kubernetes cluster configuration like
Project_Id , VPC, Subnet configuration , Number of nodes , OAuth permissions ,Machine type, Auto-scaling enablement .
   3.	Vpc.tf – ip_cidr range could be mentioned here 

Once all the files are ready , the provisioning can be triggered by using the Terraform commands init,plan ,apply and destroy using a terminal 
Another way of triggering the provisioning of resources are by enabling a trigger from the Cloud Build service , where in triggers can be configured. Sample cloudbuild.yaml is as below.

```yaml
steps:
  - name: gcr.io/$PROJECT-ID/terraform
    id: terraform-init
    args:
      - init
  - name: gcr.io/$PROJECT-ID/terraform
    id: terraform-plan
    args:
      - plan
  - name: gcr.io/$PROJECT-ID/terraform
    id: terraform-apply
    args: ['apply','-auto-approve']
```
Create a trigger using the Cloud Build option on GCP to enable the CI/CD .

## Configuration

You can configure the file with the following input variables:

| Name                      | Description                        | Default                                                               |
| ------------------------- | ---------------------------------- | --------------------------------------------------------------------- |
| `name`                    | The name of your GKE Cluster       | `gke-cluster`                                                         |
| `location`                | Region to deploy GKE               | `us-central1`                                                         |
| `node_count`              | Number of nodes                    | `3`                                                                   |
| `machine_type `           | Worker Node GCP instance type      | `c2-standard-4`                                                       |

>terraform.tfvars file can be used for the configuration , the parameters created in this file are replaced in the corresponding scripts.
