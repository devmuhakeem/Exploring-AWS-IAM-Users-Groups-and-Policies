# Exploring AWS IAM Users, Groups, and Policies

A hands-on AWS lab where I explored IAM users, groups, and policies, then tested them by logging in as three different users to confirm each had exactly the access their role required — no more, no less.

## Scenario
A company was onboarding three new staff members with different job functions: S3 support, EC2 support, and EC2 administration. My job was to assign each user to the right group and then verify, by actually logging in as them, that the permissions were correctly scoped.

## What I did

### 1. Explored the existing IAM setup
Reviewed three pre-created users (`user-1`, `user-2`, `user-3`) with no group memberships or permissions yet, and three groups, each carrying a different policy:
- **S3-Support** — AmazonS3ReadOnlyAccess (a managed policy)
- **EC2-Support** — AmazonEC2ReadOnlyAccess (a managed policy)
- **EC2-Admin** — a custom inline policy allowing view, start, and stop actions on EC2 instances

Learned the distinction between managed policies (reusable, centrally updated) and inline policies (one-off, tied to a single user or group).

### 2. Assigned users to the correct groups
- `user-1` → S3-Support (read-only S3 access)
- `user-2` → EC2-Support (read-only EC2 access)
- `user-3` → EC2-Admin (view, start, stop EC2 instances)

### 3. Verified each user's actual access by logging in as them
Used the IAM sign-in URL in a private browser window to log in as each user individually and test their real permissions:
- **user-1** could view S3 bucket contents, but got an authorization error trying to access EC2
- **user-2** could view EC2 instances but got an authorization error trying to stop one, and saw an empty bucket list in S3
- **user-3** could successfully stop the running EC2 instance, confirming admin-level access worked as intended

## Key takeaways
- Testing IAM policies by actually logging in as the affected user is far more convincing than reading the policy JSON — the "You are not authorized" errors are the real proof the boundary works
- Managed policies vs. inline policies is a real architectural choice: managed policies scale cleanly across many groups, inline policies suit one-off, non-reusable permission sets
- Read-only roles that can view but not modify a resource are a clean, simple example of least privilege in action — support staff who need visibility without change authority

## Tools
AWS IAM, Amazon EC2, Amazon S3

---
*Completed as an AWS Cloud Technical Essentials hands-on lab.*
