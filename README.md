# terraform-s3-static-website

Part 1

Using terraform, create a S3 bucket to serve a static website through a cloudfront distribution. Create an index.html file that prints "Hello World!" and upload it to the root of the S3 bucket. The S3 bucket should have a restrictive bucket policy that only allows the Cloudfront distribution to read objects present in it. Also create an error page that prints "Page not available". You can choose to write the terraform code from scratch or reuse the public terraform modules available for these resources.

SOLUTION:


Terraform for hosting a secure static website hosted on AWS using S3.

The terraform will set up the following components:

- S3 bucket for www.yourdomain.com which will host your website files.
- S3 bucket for yourdomain.com which redirects to www.yourdomain.com.
- SSL certificate for yourdomain.com.
- Cloudfront distribution for www S3 bucket.
- Cloudfront distribution for S3 bucket redirect.
- Route 53 records for:
  - root
  - www

## Using this repository

### Create your S3 bucket to store your state files.

Create a new S3 bucket manually to store your state files. This should have the following policy set:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::1234567890:root"
            },
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::yourdomain-terraform"
        },
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::1234567890:root"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::yourdomain-terraform/*"
        }
    ]
}
```


## Commands to run

- `terraform init` - To initialise the project and download any required packages.
- `terraform plan` - To see what has changed compared to your current configuration.
- `terrform apply` - To apply your changes.

Note however, that DNS validation requires the domain nameservers to already be pointing to AWS. You won't actually know the nameservers until after the NS route 53 record has been created. Alternatively, you can follow the validation instructions from the ACM page for your domain and apply to where your nameservers are currently hosted. 
DNS validation can take 30 minutes or more during which the terraform script will still be running.


