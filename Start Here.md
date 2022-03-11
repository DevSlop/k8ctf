**Background**
This workshop was hosted at the 2021 Diana Initiative CTF and was developed by the OWASP Devslop project.  It is intended to be a lightweight, low-cost Kubernetes setup, that you can run in your own AWS account. Its format is a little different than normal CTF's. The goal is to introduce you gently to Kubernetes concepts (learning exercises) and then to break the security of something (breaker exercises) and finally to fix the thing you just broke (builder exercises).  We hope that this format helps you learn the introductory concepts well enough to understand why you are able to violate security and then solve the underlying security problem.

Big shout out to the Kops team. Without Kops this would have made individualized Kubnernetes clusters near impossible to be both low cost and light weight.


## Build Kubernetes Environment
#### [Before you begin](https://kubernetes.io/docs/setup/production-environment/tools/kops/#before-you-begin)

1.   You must have [kubectl](https://kubernetes.io/docs/tasks/tools/) installed.
2.  You must [install](https://github.com/kubernetes/kops#installing) `kops` on a 64-bit (AMD64 and Intel 64) device architecture.
3. You must have an [AWS account](https://docs.aws.amazon.com/polly/latest/dg/setting-up.html), generate [IAM keys](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html#access-keys-and-secret-access-keys) and [configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration) them. The IAM user will need [adequate permissions](https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md#setup-iam-user).


#### Step 1
##### Goal: Install `kops` 

> What is Kops?

`kops` is an automated provisioning system:
-   Fully automated installation
-   Uses DNS to identify clusters
-   Self-healing: everything runs in Auto-Scaling Groups
-   Multiple OS support (Debian, Ubuntu 16.04 supported, CentOS & RHEL, Amazon Linux and CoreOS) - see the [images.md](https://github.com/kubernetes/kops/blob/master/docs/operations/images.md)
-   High-Availability support - see the [high_availability.md](https://github.com/kubernetes/kops/blob/master/docs/operations/high_availability.md)
-   Can directly provision, or generate terraform manifests - see the [terraform.md](https://github.com/kubernetes/kops/blob/master/docs/terraform.md)

##### Installation[](https://kubernetes.io/docs/setup/production-environment/tools/kops/#installation)

Download kops from the [releases page](https://github.com/kubernetes/kops/releases):

**or** 

Download the latest release with the command:

```shell
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-darwin-amd64
```

```

Make the kops binary executable.

```shell
chmod +x kops-darwin-amd64
```

Move the kops binary in to your PATH.

```shell
sudo mv kops-darwin-amd64 /usr/local/bin/kops
```

You can also install kops using [Homebrew](https://brew.sh/).

```shell
brew update && brew install kops
```

Validate this by running
```
kops version
```

##### Step 2: Create AWS Profile & Install AWS CLI
In order to build our cluster, we'll need to interact with AWS using its CLI.

a. Create a [Free Tier account](https://aws.amazon.com/free/), or use an existing one.
b. Create an [IAM user](https://console.aws.amazon.com/iamv2/home?#/users). You will need to navigate to the IAM section of the AWS Console and select “Users” from the Left side menu then select “Add User”, as pictured below.
![[Pasted image 20220307192609.png]]
c. On the following screen, enter a username for the account and give the account programmatic access, which will generate keys for the user. We’ll be using these keys to configure a profile in the AWS command line interface. The image below shows the author creating a user named “k8_ctf” and giving the user programmatic access.
![[Screen Shot 2022-03-07 at 7.29.56 PM.png]]
d. Next we need to assign permissions to our user "k8_ctf" 

 [According](https://github.com/kubernetes/kops/blob/master/docs/getting_started/aws.md) to `kops`  our user will require the following IAM permissions to function properly:
```
AmazonEC2FullAccess
AmazonRoute53FullAccess
AmazonS3FullAccess
IAMFullAccess
AmazonVPCFullAccess
AmazonSQSFullAccess
AmazonEventBridgeFullAccess
```

you can search through the AWS permission list and select what you need.
![[Screen Shot 2022-03-07 at 7.39.40 PM.png]]

e. Click through tags and you should be at the review screen with this:
![[../images/Screen Shot 2022-03-07 at 7.43.31 PM.png]]

f. You should now have an AWS Access Key and Secret Key

![[Screen Shot 2022-03-07 at 7.44.48 PM.png]]

g. Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

Verify its working
```
aws --version
```

h. We are going to create a new AWS CLI profile with the user (keys) we just created. The CLI lets you have many profiles so you dont have to worry about re-exporting the Access/Secret Keys every time you change users.
Below we are creating a profile called "k8_ctf"
```
 aws configure --profile k8_ctf
```

once you enter in the access key, secret key, leave region and output format default run this command to verify its working. it just Lists all s3 buckets. 

```
 aws s3 ls --profile k8_ctf
```

If you dont want to add the `--profile` on each request, you can export `AWS_PROFILE`: 
you need to do this for `kops` to work properly.

```
export AWS_PROFILE=k8_ctf
```

##### Step 3: Create an S3 bucket to store your clusters state[](https://kubernetes.io/docs/setup/production-environment/tools/kops/#3-5-create-an-s3-bucket-to-store-your-clusters-state)
###### Goal: Create s3 bucket

kops lets you manage your clusters even after installation. To do this, it must keep track of the clusters that you have created, along with their configuration, the keys they are using etc. This information is stored in an S3 bucket. S3 permissions are used to control access to the bucket.

Multiple clusters can use the same S3 bucket, and you can share an S3 bucket between your colleagues that administer the same clusters - this is much easier than passing around kubecfg files. But anyone with access to the S3 bucket will have administrative access to all your clusters.

With our new AWS Profile, lets create an s3 bucket via the CLI
    
a. Create the S3 bucket using `aws s3 mb s3://[YOUR_BUCKET_NAME]`
> Note: You wil need to create your OWN unique bucket name. 
    
b. Since we are going to use this to store our cluster state,  you need to export this variable. 
`export KOPS_STATE_STORE=s3://[YOUR_BUCKET_NAME]` 
> Note: 
    



 #### Step 4: Deploy the cluster.

Head into the prep directory in the Github repo.

We will be building the cluster using the `build_cluster.yaml` file. It will configure 2 t2.small AWS instances and configure the networking to access the API server.

Open the `build_cluster.yaml`. On line 15, edit the Bucket name to the bucket you just created.


 ```
kops create -f build_cluster.yaml

Created cluster/devslopk8ctf.k8s.local
Created instancegroup/master-us-east-1a
Created instancegroup/nodes-us-east-1a

To deploy these resources, run: kops update cluster --name devslopk8ctf.k8s.local --yes
 ```

Follow the next command:
```
kops update cluster --name devslopk8ctf.k8s.local --yes
```

It should then prompt you with this message: 

> SSH public key must be specified when running with AWS (create with `kops create secret --name devslopk8ctf.k8s.local sshpublickey admin -i ~/.ssh/id_rsa.pub`)

Create the actual cluster (same command)

```
kops update cluster --name devslopk8ctf.k8s.local --yes
```

Now you should see lots of activity in the console. Kops is creating a kubernetes cluster for you on 2 EC2 hosts.

you can run the command below to validate the cluster is functional.
```
kops validate cluster
```

you should see this:
```
Using cluster from kubectl context: devslopk8ctf.k8s.local

Validating cluster devslopk8ctf.k8s.local

INSTANCE GROUPS
NAME                    ROLE    MACHINETYPE     MIN     MAX     SUBNETS
master-us-east-1a       Master  t2.small        1       1       us-east-1a
nodes-us-east-1a        Node    t2.small        1       1       us-east-1a

NODE STATUS
NAME                            ROLE    READY
ip-172-20-48-231.ec2.internal   node    True
ip-172-20-56-192.ec2.internal   master  True
```

we have our cluster running!

Last thing, we need to install `kubectl` and log into to our cluster.
To install, go here: https://kubernetes.io/docs/tasks/tools/

Once you are done, we need to pull down the admin credentials for our cluster 
```
kops update cluster devslopk8ctf.k8s.local --yes --admin
```

then type in
```
kubectl get nodes
```

you should see this
```
NAME                            STATUS   ROLES                  AGE     VERSION
ip-172-20-48-231.ec2.internal   Ready    node                   114s    v1.20.7
ip-172-20-56-192.ec2.internal   Ready    control-plane,master   3m19s   v1.20.7

```



> Important Note

>To delete your cluster, its very simple and will tear down everything it created. Make sure you do this to avoid any unnecessary charges!!!
```
kops delete -f file.yaml
```



##### Step 5: Run a few Post-Build installs.

a. `kubectl apply -f ns.yaml`
b. `kubectl apply -f mongo.yaml`
c. `kubectl apply -f hubble-ui.yaml`
d. `kubectl apply -f v1.16.x.yaml`
