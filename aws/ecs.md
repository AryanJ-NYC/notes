---
description: 'https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html'
---

# ECS

## ECS CLI Configuration

Changes saved to $HOME/.ecs

### Create a Cluster Configuration

```bash
ecs-cli configure --cluster ec2-tutorial --default-launch-type EC2 --config-name ec2-tutorial --region us-west-2
```

### Create a Profile

```bash
ecs-cli configure profile --access-key AWS_ACCESS_KEY_ID --secret-key AWS_SECRET_ACCESS_KEY --profile-name ec2-tutorial-profile
```



