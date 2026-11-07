# AWS S3 and IAM access-control lab

I compared Amazon S3 access as an IAM user and as assumed-role sessions in a preconfigured AWS training environment. The purpose was to understand why creating a bucket does not grant object-write access, and how a bucket policy can authorize a role.

## Environment and method

The lab supplied `devuser`, `DeveloperGroup`, IAM roles, and three S3 buckets. I used the AWS Management Console to attempt operations, switch roles, and inspect policies. The evidence spans several training sessions; it is not a single uninterrupted test run. Account identifiers and generated bucket names are removed from the publication copies.

## Recorded results

| Principal and test | Result | Evidence |
| --- | --- | --- |
| `devuser`: create a bucket | Allowed | Written observation and original capture reviewed |
| `devuser`: upload a local file | Denied | [Upload result](assets/02-user-upload-denied.png); [group policy](assets/01-user-group-policy.png) |
| `devuser`: read an object from bucket1 | Denied | Original XML error capture reviewed |
| `BucketsAccessRole`: download from bucket1 | Allowed | [Completed download](assets/03-role-object-download.png) |
| `BucketsAccessRole`: upload to bucket2 | Allowed | [Upload result](assets/05-role-bucket2-upload.png); [earlier bucket2 policy](assets/04-bucket2-policy.png) |
| `OtherBucketAccessRole`: upload to bucket3 | One file succeeded | [Upload result](assets/07-other-role-bucket3-upload.png); [bucket3 policy](assets/06-bucket3-role-policy.png) |

The key finding was that identity policies alone did not describe every access path. The reviewed bucket2 policy granted object access to the role. The bucket3 policy named a different role, matching the principal shown in the later successful upload.

## Report and limits

[Full report](report.pdf)

The report distinguishes screenshots from written observations and explains the separate capture sessions. It does not claim a complete permissions audit, verified CloudTrail history, or successful completion of an unseen grading workflow. The lab's identities, roles, buckets, and policies were supplied by the training environment.

Technical references: [IAM policy evaluation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) and [S3 API permissions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-with-s3-policy-actions.html).
