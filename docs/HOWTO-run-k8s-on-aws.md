HOWTO run k8s on AWS
====================

1. Install via console

   [Ref](https://docs.aws.amazon.com/eks/latest/userguide/create-public-private-vpc.html)   

   - Create stack 
   - failed with `An error occured while attempting to create the stack` and nothing more!

1. Install AWS CLI

   ```
   pip3 install --user awscli
   ``` 

   Check installation:

   ```
   ~/.local/bin/aws
   ```

2. Install AWS IAM authenticator

   ```
   curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.14.6/2019-08-22/bin/linux/amd64/aws-iam-authenticator
   chmod +x ./aws-iam-authenticator
   mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$HOME/bin:$PATH
   PATH=$HOME/bin:$PATH' >> ~/.bashrc

   ```

2. If you don't have an AWS user yet
   [Ref](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html)

3. Configure cli with access token
   [Ref](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)

   ```
   aws configure
   ```

   NOTE Stores credentials in `~/.aws/credentials`

4. Install Terraform
   [Ref](https://learn.hashicorp.com/terraform/getting-started/build)

   Note Terraform relies on AWS credentials above

   ```
   curl https://releases.hashicorp.com/terraform/0.12.16/terraform_0.12.16_linux_amd64.zip
   ```



4. Terraform example: 
   [Ref](https://learn.hashicorp.com/terraform/getting-started/build)

   first thing we need is an AWS AMI id...

4. Look up AMI id
   [Ref](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/finding-an-ami.html#finding-an-ami-aws-cli)

   ```
   
   ```

4. Create service role (pre-req for cluster)
   
   Created eksServiceRole which has ARN: arn:aws:iam::379413463976:role/eksServiceRole

5. Now need 2 subnets and a security group

   [Ref](https://docs.aws.amazon.com/eks/latest/userguide/network_reqs.html)

   I'm not sure what other workloads would call for but public subnet to expose services and private for worker nodes makes the most sense to me.


      

5. Create cluster using CLI
   [Ref](https://docs.aws.amazon.com/cli/latest/reference/eks/create-cluster.html)

aws eks create-cluster --name prod \
--role-arn arn:aws:iam::012345678910:role/eks-service-role-AWSServiceRoleForAmazonEKS-J7ONKE3BQ4PI \
--resources-vpc-config subnetIds=subnet-6782e71e,subnet-e7e761ac,securityGroupIds=sg-6979fe18

6. 

