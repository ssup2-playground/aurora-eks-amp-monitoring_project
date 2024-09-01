# aurora-eks-amp-monitoring

aurora-eks-amp-monitoring is a prototyping project how to monitor Aurora using EKS and AMP.

* [aws-terraform](https://github.com/ssup2-playground/aurora-eks-amp-monitoring_aws-terraform) : Terraform for Aurora Cluster, EKS Cluster and AMP

## Architecture

<img src="/images/architecture.png" width="700"/>

* A MySQLd exporter accesses an Aurora MySQL instance to collect metrics.
* A PostgreSQL exporter accesses an Aurora PostgreSQL instnace to collect metrics.
* ADOT collector collectes metrics from the exporter and stores them in AMP.
* For high availability, ADOT collector uses 2 replicas.
* Grafana collects and visualizes metrics from AMP.

