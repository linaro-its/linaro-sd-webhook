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
