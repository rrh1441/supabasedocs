# Grafana not displaying data

Last edited: 2/4/2025

* * *

This guide is for identifying configuration mistakes in [self-hosted Supabase Grafana installations](https://supabase.com/docs/guides/monitoring-troubleshooting/metrics#deploying-supabase-grafana)

## Step 1: Ping your Grafana endpoint [\#](https://supabase.com/docs/guides/troubleshooting/grafana-not-displaying-data-sXJrMj\#step-1-ping-your-grafana-endpoint)

Use the below cURL command to make sure your metrics endpoint returns data:

`
curl https://<YOUR_PROJECT_REF>.supabase.co/customer/v1/privileged/metrics --user 'service_role:<SERVICE_ROLE_KEY>'
`

## Step 2: Set your Grafana Dashboard to auto-refresh in the top right corner [\#](https://supabase.com/docs/guides/troubleshooting/grafana-not-displaying-data-sXJrMj\#step-2-set-your-grafana-dashboard-to-auto-refresh-in-the-top-right-corner)

![388343266-ed4b8f38-e0cd-474e-bc1c-1ac6ae68e1aa](https://supabase.com/docs/img/troubleshooting/47998bed-0b77-433a-bfed-63222beb2aee.png)

## Step 3: Make sure your docker container has the default configurations [\#](https://supabase.com/docs/guides/troubleshooting/grafana-not-displaying-data-sXJrMj\#step-3-make-sure-your-docker-container-has-the-default-configurations)

Run the following command in the terminal:

`
docker ps -f name=supabase-grafana
`

The output should look something like this:

![image](https://supabase.com/docs/img/troubleshooting/6c284180-0ffd-432d-b86b-e9fbcfe23868.png)

Here it is in an easier to read format

`
- CONTAINER ID: < container id >
- IMAGE: supabase-grafana-supabase-grafana
- COMMAND: /entrypoint.sh
- CREATED: < time >
- STATUS: Up < unit of time > ago
- PORTS: 3000/tcp, 0.0.0.0:8000 → 8080/tcp
- NAMES: supabase-grafana-supabase-grafana-1
`

## Step 4: Enter the container [\#](https://supabase.com/docs/guides/troubleshooting/grafana-not-displaying-data-sXJrMj\#step-4-enter-the-container)

Try running the following terminal command:

`
docker exec -it <container id> bash
`

## Step 5: Check the environment variables for errors [\#](https://supabase.com/docs/guides/troubleshooting/grafana-not-displaying-data-sXJrMj\#step-5-check-the-environment-variables-for-errors)

Run the following in the docker container:

`
printenv | egrep 'GRAFANA_PASSWORD|SUPABASE_PROJECT_REF|SUPABASE_SERVICE_ROLE_KEY'
`

Ensure the values are correct by comparing them with those in the Dashboard. Users have previously encountered issues by accidentally omitting the last character of their strings, so a thorough check is essential.

## Step 6: Go to the root folder and check permissions on the `entrypoint.sh` file [\#](https://supabase.com/docs/guides/troubleshooting/grafana-not-displaying-data-sXJrMj\#step-6-go-to-the-root-folder-and-check-permissions-on-the-entrypointsh-file)

Run the following terminal commands:

`
cd /
ls -l | grep entrypoint.sh
`

`entrypoint.sh` should have the following permissions:

`
-rwxr-xr-x
`

If off, update the values

`
chmod +x entrypoint.sh
`

1. We use first-party cookies to improve our services. [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)



   [Learn more](https://supabase.com/privacy#8-cookies-and-similar-technologies-used-on-our-european-services)•Privacy settings





   AcceptOpt outPrivacy settings