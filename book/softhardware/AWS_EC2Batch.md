# AWS EC2 Batch

Here's a short tutorial on using Amazon EC2 Batch with Fargate Spot and containers to perform a job that involves writing and reading from Amazon S3:

The below steps require [setting up the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html) as well as the [jq tool](https://jqlang.github.io/jq/download/) (optional).


## 1. Create role

AWS batch requires an IAM role to be created *for running the jobs*. This can be done from the IAM webconsole.

Create a role using the following options:

- Trusted Entity Type: AWS Service
- Use Case: Elastic Container Service
    - Elastic Container Service Task

On the next page, search for and add:
- AmazonECSTaskExecutionRolePolicy
- AmazonS3FullAccess

Once the role is created, one more permission is needed:
- Go to: Permissions tab --> Add Permissions --> Create inline policy
- Search for "batch"
- Click on **Batch**
- Select Read / Describe Jobs
- Click Next
- Add a poolicy name, e.g. "Describe_Batch_Jobs"
- Click Create Policy

Finally, go to the S3 bucket where you'll be writing the results of the jobs.
Open the Permissions tab and add a statement to the bucket policy granting full access to the role you
just created:

```json
		{
			"Sid": "Statement3",
			"Principal": {
			    "AWS": "arn:...your job role ARN."
			},
			"Effect": "Allow",
			"Action": "s3:*",
			"Resource": "arn:...your bucket ARN."
		}
```

Note that the job role ARN will be in the format of `arn:aws:iam::<YOUR_ACCOUNT_ID>:role/<JOB_ROLE_NAME>`. The bucket ARN will be in the format of `arn:aws:s3:::<YOUR_S3_BUCKET>`.


## 2. Create a Compute Environment

You'll need two pieces of information to create the compute environment. The list of subnets in your VPC and the default security group ID. You can use the following commands to retrieve them:

```
aws ec2 describe-subnets  | jq ".Subnets[] | .SubnetId"
```
```
aws ec2 describe-security-groups --filters "Name=group-name,Values=default" | jq ".SecurityGroups[0].GroupId"
```

We design the computing environment in a YAML file

```yaml
computeEnvironmentName: ''  # [REQUIRED] The name for your compute environment.
type: MANAGED
state: ENABLED
computeResources: # Details about the compute resources managed by the compute environment.
  type: FARGATE
  maxvCpus: 256 # [REQUIRED] The maximum number of Amazon EC2 vCPUs that a compute environment can reach.
  subnets: # [REQUIRED] The VPC subnets where the compute resources are launched.
  - ''
  securityGroupIds: # [REQUIRED] The Amazon EC2 security groups that are associated with instances launched in the compute environment.
  - ''
```
Use this values to update the missing fields in `compute_environment.yaml` and the run:

```
aws batch create-compute-environment --no-cli-pager --cli-input-yaml file://compute_environment.yaml
```

Make a note of the compute environment ARN to use in the next step.

## 3. Create a Job queue

Add the compute environment and a name to `job_queue.yaml` as the folling file:

```yaml
jobQueueName: ''  # [REQUIRED] The name of the job queue.
state: ENABLED
priority: 0
computeEnvironmentOrder: # [REQUIRED] The set of compute environments mapped to a job queue and their order relative to each other.
- order: 0  # [REQUIRED] The order of the compute environment.
  computeEnvironment: '' # [REQUIRED] The Amazon Resource Name (ARN) of the compute environment.
```
 and then run:

```
aws batch create-job-queue --no-cli-pager --cli-input-yaml file://job_queue.yaml
```

## 4. Create a Job Definition

Update the `jobRoleArn` and `executionRoleArn` fields in the `job_definition.yaml` file with the ARN of the role created in the first step:
```yaml
jobDefinitionName: ''  # [REQUIRED] The name of the job definition to register.
type: container
platformCapabilities:
- FARGATE
containerProperties:
  image: 'ghcr.io/noisepy/noisepy'
  command:
  - '--help'
  jobRoleArn: ''
  executionRoleArn: ''
  resourceRequirements: # The type and amount of resources to assign to a container.
  - value: '16'
    type: VCPU
  - value: '32768'
    type: MEMORY
  networkConfiguration: # The network configuration for jobs that are running on Fargate resources.
    assignPublicIp: ENABLED  # Indicates whether the job has a public IP address. Valid values are: ENABLED, DISABLED.
  ephemeralStorage: # The amount of ephemeral storage to allocate for the task.
    sizeInGiB: 21  # [REQUIRED] The total amount, in GiB, of ephemeral storage to set for the task.
retryStrategy: # The retry strategy to use for failed jobs that are submitted with this job definition.
  attempts: 1  # The number of times to move a job to the RUNNABLE status.
propagateTags: true # Specifies whether to propagate the tags from the job or job definition to the corresponding Amazon ECS task.
timeout: # The timeout configuration for jobs that are submitted with this job definition, after which Batch terminates your jobs if they have not finished.
  attemptDurationSeconds: 36000  # The job timeout time (in seconds) that's measured from the job attempt's startedAt timestamp.
```


Add a name for the `jobDefinition`. Finally, run:

```
aws batch register-job-definition --no-cli-pager --cli-input-yaml file://job_definition.yaml
```

## 4. Submit a job

We will take the example of noisepy. Update `job_cc.yaml` with the names of your `jobQueue` and `jobDefinition` created in the last steps:
```yaml
jobName: 'noisepy-cross-correlate'
jobQueue: ''
jobDefinition: '' # [REQUIRED] The job definition used by this job.
# Uncomment to run a job across multiple nodes. The days in the time range will be split across the nodes.
# arrayProperties:
#   size: 16  # number of nodes
containerOverrides: # An object with various properties that override the defaults for the job definition that specify the name of a container in the specified job definition and the overrides it should receive.
  resourceRequirements:
  - value: '90112' # CC requires more memory
    type: MEMORY
  command: # The command to send to the container that overrides the default command from the Docker image or the job definition.
  - cross_correlate
  - --raw_data_path=s3://scedc-pds/continuous_waveforms/
  - --xml_path=s3://scedc-pds/FDSNstationXML/CI/
  - --ccf_path=s3://<YOUR_S3_BUCKET>/<CC_PATH>
  - --config=s3://<YOUR_S3_BUCKET>/<CONFIG_PATH>/config.yaml
timeout:
  attemptDurationSeconds: 36000  # 10 hrs
```


Then update the S3 bucket paths to the locations you want to use for the output and your `config.yaml` file.

```
aws batch submit-job --no-cli-pager --cli-input-yaml file://job_cc.yaml --job-name "<your job name>"
```

## Submit a Stacking job

Update `job_stack.yaml` with the names of your `jobQueue` and `jobDefinition` created in the last steps. 
```yaml
jobName: 'noisepy-stack'
jobQueue: ''
jobDefinition: '' # [REQUIRED] The job definition used by this job.
# Uncomment to run a job across multiple nodes. The station pairs to be stacked will be split across the nodes.
# arrayProperties:
#   size: 16  # number of nodes
containerOverrides: # An object with various properties that override the defaults for the job definition that specify the name of a container in the specified job definition and the overrides it should receive.
  resourceRequirements:
  - value: '32768'
    type: MEMORY
  command: # The command to send to the container that overrides the default command from the Docker image or the job definition.
  - stack
  - --ccf_path=s3://<YOUR_S3_BUCKET>/<CC_PATH>
  - --stack_path=s3://<YOUR_S3_BUCKET>/<STACK_PATH>
timeout:
  attemptDurationSeconds: 7200  # 2 hrs
```


Then update the S3 bucket paths
to the locations you want to use for your input CCFs (e.g. the output of the previous CC run), and the stack output. By default, NoisePy will look for a config file in the `--ccf_path` location to use the same configuration for stacking that was used for cross-correlation.

```
aws batch submit-job --no-cli-pager --cli-input-yaml file://job_stack.yaml --job-name "<your job name>"
```
