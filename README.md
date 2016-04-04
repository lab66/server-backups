# Lab66 Server Backups

This is the script we use to backup our servers to S3.

## Installation

1. Copy `.env.example` to `.env`

2. Then `nano .env` and enter your server details

3. Copy `.s3cmd.example` to `.s3cmd`

4. Then `nano .s3cmd` and change your `access_key` and `secret_key` (and further details if you wish)

5. Run `./setup` to setup a cron or run a test backup

6. Alternatively, run `./backup` directly

