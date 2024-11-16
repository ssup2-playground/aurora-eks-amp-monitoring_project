# aurora-eks-amp-monitoring

aurora-eks-amp-monitoring is a prototyping project how to monitor Aurora using EKS and AMP.

* [aws-terraform](https://github.com/ssup2-playground/aurora-eks-amp-monitoring_aws-terraform) : Terraform for Aurora Cluster, EKS Cluster and AMP

## Install

* Run terraform

```bash
# Get terraform code
$ git clone https://github.com/ssup2-playground/aurora-eks-amp-monitoring_aws-terraform.git && rm ./aurora-eks-amp-monitoring_aws-terraform/terraform.tf

# Run terraform
$ cd aurora-eks-amp-monitoring_aws-terraform
$ terraform init
$ terraform apply -target="module.irsa_adot_collector"
$ terraform apply -target="module.prometheus"
$ terraform apply
```

* Set grafana security group
  * Update `aurora-eks-amp-monitoring-grafana-sg` security group inbound rules from `127.0.0.1/32` to `your labtop IP/32`

* Access grafana

```bash
# Set kubectl
$ aws eks update-kubeconfig --name aurora-eks-amp-monitoring-eks

# Update grafana service to LoadBalancer Type
$ kubectl -n observability patch service grafana -p '{"spec": {"type": "LoadBalancer"}}'

# Get grafana url
$ echo http://$(kubectl -n observability get service grafana -o json | jq ".status.loadBalancer.ingress[0].hostname" -r)
```

* Login grafana
  * username : admin
  * password : `kubectl -n observability get secrets grafana -o jsonpath='{.data.admin-password}' | base64 --decode`

## Architecture

<img src="/images/architecture.png" width="700"/>

* A MySQLd exporter accesses an Aurora MySQL instance to collect metrics.
* A PostgreSQL exporter accesses an Aurora PostgreSQL instnace to collect metrics.
* ADOT collector collectes metrics from the exporter and stores them in AMP.
* For high availability, ADOT collector uses 2 replicas.
* Grafana collects and visualizes metrics from AMP and CloudWatch.

## Screenshots

* grafana-aurora-mysql

<img src="/images/grafana-aurora-mysql.png" width="800"/>

* grafana-aurora-postgresql

<img src="/images/grafana-aurora-postgresql.png" width="800"/>
