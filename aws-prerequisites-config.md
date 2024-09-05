```markdown
# AWS CLI Configuration Steps

### Prerequisites (IAM Developer User)
1. Go to [IAM Console](https://us-east-1.console.aws.amazon.com/iam/home#/home) to create access and secret keys.

   **Steps:**
   - IAM Dashboard -> IAM resources -> Users -> Your 'user_name' -> Click
   - Summary -> Access key -> Create -> Choose AWS CLI -> [ ] Agree to Terms & Conditions -> Next
   - Copy both Access Key & Secret Key. **Keep them safe.**

2. Configure AWS CLI by typing the following command in your terminal:

   ```bash
   aws configure
   ```

   You'll be prompted to enter the following:

   ```bash
   AWS Access Key ID [None]: xxx55EJKDSXDZJxxxx
   AWS Secret Access Key [None]: xxxxxxxq8HgIeD6QI4xGWXI/b4dZuN6xxxx
   Default region name [None]: xx-south-x
   Default output format [None]: json
   ```

   After configuring these settings, the AWS CLI will use this information by default when executing commands.
   You can change these configurations later by running `aws configure` again or by manually editing the `~/.aws/credentials` and `~/.aws/config` files.

3. To verify the configuration, run:

   ```bash
   aws sts get-caller-identity
   ```

   The output should look something like this:

   ```json
   {
       "UserId": "xxxxx",
       "Account": "xxxx",
       "Arn": "arn:aws:iam::xxxx:user/xxxx"
   }
   ```
```
