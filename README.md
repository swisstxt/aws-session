# aws-session

Create temporary AWS sessions to use MFA with CLI tools.

## Motiviation

When enforcing multi-factor authentication on AWS IAM user accounts, sending API
calls directly will no longer work. Standard tools and libraries only have
limited support for creating temporary sessions.

The official [AWS documentation](https://aws.amazon.com/premiumsupport/knowledge-center/authenticate-mfa-cli/)
describes commands to create user sessions using the AWS CLI, but these are
not automated and require manually copying tokens back and forth.

## Usage

`aws-session` is a shell script that asks for a security token and returns
temporary session credentials to be used in the current shell.

The script can be called through `eval` to automatically set these temporary
credentials in the shell environment:

```shell
eval $(aws-session)
```

This will query the current AWS profile (or the default profile) for a list
of MFA devices, pick the first one, then ask for a security token. Type in
this security token, and your current shell will have a valid MFA-authenticated
AWS session.

When the session expires, you have to manually recreate the token. Unset the
created environment variables to do this, then execute `aws-session` again:

```shell
unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
eval $(aws-session)
```

## Legal

aws-session is Copyright © 2020 SWISS TXT AG and may be used under the terms
of the Simplified BSD License. See the LICENSE file for details.
