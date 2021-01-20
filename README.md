# linaro-sd-webhook

This repo exists to make it easier to deploy the Service Desk webhook framework, coupled with Linaro's handlers, to AWS Lambda by using Zappa.

## Prerequisites

The `sd-webhook-framework` and `sd-webhook-handlers` repositories must exist at the same level as this repository.

## Using this repo

1. Add a `configuration.jsonc` file, based off the sample configuration file included in the `sd-webhook-framework` repo.
2. `./copy-repos.sh`     - copy the files from the two repos into a tree structure that can then be used by Zappa.
3. `pipenv install`      - initialise the virtual environment and install the required production packages.
4. `pipenv shell`        - start the virtual environment.
5. `python build_cfs.py` - initialise the custom field file.
6. `pip3 install zappa`  - install the Zappa tool.
7. `zappa init`          - initialise Zappa.
8. `zappa deploy dev`    - deploy to a `dev` stage.

## Zappa settings

Running `zappa init` creates the file `zappa_settings.json` based on the answers you provide. For an optimal deployment, and for double-checking that the base settings are correct, here is a sample configuration. Some sections have been replaced with `<COMMENTS>` as these are project specific.

The complete `"extra_permissions"` section can be removed if you are not using Vault authentication to retrieve secrets (i.e. all of the required passwords are specified in `configuration.jsonc`).

```JSON
{
    "dev": {
        "app_function": "app.APP",
        "aws_region": "<YOUR PREFERRED REGION>",
        "profile_name": "<AWS PROFILE NAME>",
        "project_name": "<YOUR PREFERRED PROJECT NAME>",
        "runtime": "python3.8",
        "s3_bucket": "<ZAPPA BUCKET NAME>",
        "keep_warm": false,
        "exclude": [
            ".gitignore",
            ".vscode",
            "boto3*",
            "botocore*",
            "build_cfs.py",
            "copy-repos.sh",
            "Pipfile*",
            "README.md"
        ],
        "extra_permissions": [
            {
                "Effect": "Allow",
                "Action": [
                    "sts:AssumeRole"
                ],
                "Resource": [
                    "<VAULT ROLE IF USING VAULT AUTHENTICATION>"
                ]
            }
        ],
        "log_level": "INFO"
    }
}
```
