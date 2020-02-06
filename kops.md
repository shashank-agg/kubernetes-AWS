We can also use Kops (https://github.com/kubernetes/kops) to create Kubernetes clusters. You specify your cluster configuration (number of kubernetes masters, nodes, instance-type etc.) using command line or yaml file(s), and Kops creates the cluster on the specified cloud platform (AWS, GCE etc.)

## Kops with AWS

1. Create an S3 bucket. Kops uses this internally to store and maintain cluster state.

```
aws s3api create-bucket \
    --bucket <unique-bucket-name> \
    --region us-east-1

aws s3api put-bucket-versioning --bucket <unique-bucket-name>  --versioning-configuration Status=Enabled
```

2. Some env vars
```
export NAME=kubecluster.k8s.local
export KOPS_STATE_STORE=s3://<unique-bucket-name>
```

3. Create the cluster
```
kops create cluster --zones us-west-2a --name ${NAME} --ssh-public-key <key-pair-downloaded-from-AWS> 
```