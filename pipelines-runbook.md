# Runbook
--------------------------------------------------------------------
## Setup
1. Install the OpenShift Pipelines Operator
--------------------------------------------------------------------
## Pipeline Triggers
1. Use the provided templates for each component needed to create Triggers in OCP Pipelines: `Event Listener`, `Trigger Template`, and `Trigger Binding`.
2. Update the provided template values to reflect your pipelines deployment and pipeline-name.
3. For the `Event Listener`: You can add additional specifications for the `Template` that can read JSON data from the BODY of your webhook.
    - Ex. GitHub will provide a body with a webhook (which calls OCP Pipelines' Event Listener)
    - The body provides details about the latest commit, ref/branch, user, and other details. These details can be loaded into `Pipeline Parameters` at this point using the `Event Listener` + `Trigger Template`
--------------------------------------------------------------------
## Creating the Webhook in External System
1. Create an `OpenShift Route` for the `Event Listener` created in the first step.
    - This Route URL will be used in the webhook on the external system as the destination to `POST` the webhook.
2. Add an integration / webhook / event / etc. (specific to each system)
    - Select the desired event in source control that tiggers the webhook - i.e. Pull Request Approved, Source Code Updated, etc.
    - Use the `Route URL` for the Pipeline `Event Listener` created in the first step.
3. Test! Once the trigger is fired from GitHub / Azure DevOps / etc. It will automatically:
    - POST trigger data to OpenShift using the `Event Listener` and `Route` URL
    - `Event Listener` will share any details / data with the `Trigger Template`, using that template to effectively create a new `PipelineRun`
    - `PipelineRun` can now be monitored for success in tasks / parameter evaluation.
4. Done!
--------------------------------------------------------------------
# GIT Credentials
u: justin
p: <secret>
url: https://dev.azure.com