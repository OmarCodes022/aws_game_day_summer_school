# AWS GameDay - Summer School

## Context

AWS GameDay event for the Unicorn.Rentals App. Team 64, account accessed via CLI credentials.

## AWS Environment

- Region: check with `aws configure get region`
- Always run `aws sts get-caller-identity` first to confirm correct account
- GameDay resources: WebServer ALB, Auto Scaling Group, EC2 (Python Flask), RDS Aurora Postgres, S3

## Key Endpoints

- POST /unicorn - must return 200 with signed payload (uses secret_sauce_* binary), under 2s SLA
- GET /gallery?unicorn_name=happy - gallery endpoint

## Quests

- START HERE - Unicorn.Rentals App (setup, submit ALB URL)
- Unicorn Birth Registry (Compute)
- Warm Up the Stables (Compute)
- Unicorn Catering (Serverless)
- Keep Up with the Llamas (Serverless)

## Workflow

1. Always check existing AWS resources before making changes
2. Use `aws` CLI for all infrastructure changes
3. Send tasks one at a time and verify completion before moving on
