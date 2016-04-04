# Lab66 Server Backups

This is the script we use to backup our servers to S3.

## Requirements

* MySQL command line tools (e.g. `mysql`)
* [s3cmd](http://s3tools.org/s3cmd)

## Download

Download using the following commands:

    wget https://github.com/lab66/server-backups/archive/master.tar.gz
    tar zxvf master.tar.gz
    rm master.tar.gz
    cd server-backups-master

## Installation

1. Copy `.env.example` to `.env`

2. Then `nano .env` and enter your server details

3. Copy `.s3cmd.example` to `.s3cmd`

4. Then `nano .s3cmd` and change your `access_key` and `secret_key` (and further details if you wish)

5. Run `./setup` to setup a cron or run a test backup

6. Alternatively, run `./backup` directly

## Amazon S3 Setup

It is recommended that you create a IAM user per server/backup area.

1. Log into the Amazon AWS Console

2. Click `Your Name` > `Security Credentials`

3. Go into `Users` and add a new user (we used `servername-server`)

4. Record the Access and Secret keys, as these will be used in the `.env` config

5. Go into `Policies` and create a new policy.
Use the following:
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AllowListingOfUserFolder",
                "Action": [
                    "s3:ListBucket"
                ],
                "Effect": "Allow",
                "Resource": [
                    "arn:aws:s3:::BUCKET"
                ],
                "Condition": {
                    "StringLike": {
                        "s3:prefix": [
                            "FOLDER"
                        ]
                    }
                }
            },
            {
                "Effect": "Allow",
                "Action": [
                    "s3:ListBucket",
                    "s3:PutObject",
                    "s3:GetObject",
                    "s3:GetObjectVersion",
                    "s3:DeleteObject",
                    "s3:DeleteObjectVersion"
                ],
                "Resource": "arn:aws:s3:::BUCKET/FOLDER/*"
            }
        ]
    }
This will allow the user to access only the `FOLDER` path in `BUCKET`, this means we can use 1 bucket for multiple server's backups keeping them secure.

6. Attach this "Customer Managed" policy to the User created previously.

7. Rinse and repeat for each server backup.

Note: To test, use `s3cmd --config=".s3cfg" ls s3://BUCKET/FOLDER`. If you see Permission denied this has not worked.