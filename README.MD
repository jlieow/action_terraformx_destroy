Sample workflow to reference to this action

```
name: Perform Terraformx Destroy Action using Linux exe
on:
  push:
    tags:
      - 'dev/destroy/ec2/v[012].[0-9]+.[0-9]+' # Make sure this matches up with the dir set below
      - 'dev/destroy/all_terraform_root_in_repo/v[012].[0-9]+.[0-9]+'

jobs:
  provision_resources:
    runs-on: ubuntu-latest
    name: Perform Terraformx Destroy on Terraform Root EC2
    steps:

      - name: Checkout
        uses: actions/checkout@v3

      - name: Perform Terraformx Destroy
        id: terraformx_destroy
        uses: jlieow/terraformx_destroy@V1 # Uses an action in the repo terraformx_destroy owned by jlieow tagged as V1
        with:
          dir: ec2 # Remember to set the tag dir too
          aws_profiles: |
            [
              {
                "aws_access_key_id": ${{ toJSON(secrets.AWS_HUB_PROFILE_AWS_ACCESS_KEY_ID) }},
                "aws_secret_access_key": ${{ toJSON(secrets.AWS_HUB_PROFILE_AWS_SECRET_ACCESS_KEY) }},
                "profile_name": ${{ toJSON(secrets.AWS_HUB_PROFILE_NAME) }}
              },
              {
                "aws_access_key_id": ${{ toJSON(secrets.AWS_SPOKE_PROFILE_AWS_ACCESS_KEY_ID) }},
                "aws_secret_access_key": ${{ toJSON(secrets.AWS_SPOKE_PROFILE_AWS_SECRET_ACCESS_KEY) }},
                "profile_name": ${{ toJSON(secrets.AWS_SPOKE_PROFILE_NAME) }}
              }
            ]
```
