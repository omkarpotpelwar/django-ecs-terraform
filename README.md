# Deploying Django to AWS ECS with Terraform

1. Install Terraform

1. Sign up for an AWS account

1. Create an ECR repository, `django-app`.

1. Fork/Clone

1. Build the Django Docker image and push them up to ECR:

    ```sh
    $ cd app
    $ docker build -t <AWS_ACCOUNT_ID>.dkr.ecr.us-west-1.amazonaws.com/django-app:latest .
    $ docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-west-1.amazonaws.com/django-app:latest
    $ cd ..
    ```

1. Update the variables in *terraform/variables.tf*.

1. Set the following environment variables, init Terraform, create the infrastructure:

    ```sh
    $ cd terraform
    $ export AWS_ACCESS_KEY_ID="YOUR_AWS_ACCESS_KEY_ID"
    $ export AWS_SECRET_ACCESS_KEY="YOUR_AWS_SECRET_ACCESS_KEY"

    $ terraform init
    $ terraform apply
    $ cd ..
    ```

1. Terraform will output an ALB domain.
1. Open the EC2 instances overview page in AWS. Use `ssh ec2-user@<ip>` to
   connect to the instances until you find one for which `docker ps` contains
   the Django container. Run
   `docker exec -it <container ID> python manage.py migrate`.

1. Now you can open `http://your.domain.com/admin`. Note that `https://` won't work.

1. You can also run the following script to bump the Task Definition and update the Service:

    ```sh
    $ cd deploy
    $ python update-ecs.py --cluster=production-cluster --service=production-service
    ```
1. Note: If you get any SSH key error, create an SSH key on your machine using the following commands:
    
    ```sh
    $ cd ~ && mkdir .ssh && cd .ssh
    $ ssh-keygen 
    ```
