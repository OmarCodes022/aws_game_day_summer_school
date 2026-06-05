# AWS GameDay - Summer School

## AWS Environment

- Account: 381491876933 (GameDay account via WSParticipantRole)
- Region: us-east-1
- Credentials: temporary STS session tokens (expire periodically - user must refresh)

Verify with: `aws sts get-caller-identity`

## App URL

`http://WebServer-434843546.us-east-1.elb.amazonaws.com`

## Architecture

| Component | Resource | Details |
|-----------|----------|---------|
| ALB | WebServer | `WebServer-434843546.us-east-1.elb.amazonaws.com` |
| Target Group | WebServerTG | Port 8080, health check at `/healthcheck` |
| EC2 (x3) | WebServer ASG | t3.large, all 3 targets healthy |
| RDS | unicorn-prod-cluster-donotdelete | Aurora Postgres, port 5432 |
| RDS (unused) | unicorn-dev-cluster-unused | Aurora Postgres, port 5432 |
| S3 | unicorns-381491876933-us-east-1 | Unicorn images (happy.png, donkeith.png, llarry.png, puglynne.png) |
| Other RDS | unicorn-rentals-dbinstance | MySQL port 3306 (not used by app) |

## SSM Parameters

- `/webserver/db_endpoint` = `unicorn-prod-cluster-donotdelete.cluster-civ8i20sqb0e.us-east-1.rds.amazonaws.com`
- `/webserver/log_level` = `DEBUG`

## Key Endpoints

- `POST /unicorn` - returns 200 + `{"message":"Successfully hired unicorn","request_id":"...","signature":"..."}` (signed via secret_sauce_* binary on EC2)
- `GET /gallery?unicorn_name=happy` - returns PNG image from S3

## CloudWatch Logs

- App logs: `/aws/ec2/unicorn-webserver-logs`
- RDS prod logs: `/aws/rds/cluster/unicorn-prod-cluster-donotdelete/postgresql`

## ASGs

| ASG | Desired | Min | Max |
|-----|---------|-----|-----|
| WebServer | 3 | 0 | 3 |
| Bastion | 3 | 0 | 3 |
| CloudIde | 1 | 1 | 1 |
| Gitea | 1 | 1 | 1 |

## Other Instances (DO NOT MODIFY)

- Gitea Server: `i-0101222b57ea1cf8f` (32.199.136.126) - `Gitea-1583908568.us-east-1.elb.amazonaws.com`
- Cloud IDE: `i-0459d6ebe9e11e8dd` (44.204.175.228) - `CloudIde-1132501074.us-east-1.elb.amazonaws.com`

## Quests

- START HERE - Unicorn.Rentals App (submit ALB URL, get health checks green)
- Unicorn Birth Registry (Compute)
- Warm Up the Stables (Compute)
- Unicorn Catering (Serverless)
- Keep Up with the Llamas (Serverless)

## TCO Scoring

- Base points per request: 200 / TCO_score
- TCO starts at 200 - completing tasks reduces it, increasing points per request
- SLA: must respond within 2 seconds

## Workflow

- Always check existing resources before making changes
- Credentials expire - user must re-export before sessions
- Never modify Gitea or Cloud IDE instances
- WebServer ASG min is 0 - don't accidentally scale it to 0
