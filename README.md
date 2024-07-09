# Amazon Bedrock Studio Bootstrapper

## Overview

This bootstrapper creates the required permissions boundary and roles for Amazon Bedrock Studio, simplifying the [creation of a Bedrock Studio workspace](https://docs.aws.amazon.com/bedrock/latest/userguide/administer-create-workspace.html#administer-creating-roles?trk=2483aad2-15a6-4b7a-a1c5-189851586b67&sc_channel=el).

By default, the bootstrapper creates a provisioning role, a service role, and a set of permissions boundaries.

Optionally, you can enable the creation of a KMS key and an OpenSearch encryption policy by providing a custom configuration.

## ⚠️ Usage disclaimer

This script interacts with services that may incur charges to your AWS account. For more information, see [AWS Pricing](https://aws.amazon.com/pricing/?trk=2483aad2-15a6-4b7a-a1c5-189851586b67&sc_channel=el).

Additionally, it might theoretically modify or delete existing AWS resources. As a matter of due diligence, do the following:

- Be aware of the resources that it creates or deletes.
- Be aware of the costs that might be charged to your account as a result.
- Back up your important data.

## Run the script

### Prerequisites

* You must have an AWS account, and have your default credentials and AWS Region configured as described in the [AWS Tools and SDKs Reference Guide](https://docs.aws.amazon.com/credref/latest/refdocs/creds-config-files.html?trk=2483aad2-15a6-4b7a-a1c5-189851586b67&sc_channel=el).
* You must have an [AWS IAM Identity Center set up](https://docs.aws.amazon.com/bedrock/latest/userguide/administer-create-workspace.html#administer-create-workspace-configure-identity-center?trk=2483aad2-15a6-4b7a-a1c5-189851586b67&sc_channel=el) in the same AWS Region as your Bedrock Studio workspace.
* [Python 3.12.0 or later](https://www.python.org/)

### Install packages

Depending on how you have Python installed and on your operating system, the commands to install and run might vary slightly. For example, on Windows, you might have to use `py` in place of `python`.

This repository contains a `requirements.txt` file that defines the packages needed to run the bootstrapper. To install the required packages, first create a virtual environment by running the following:

```shell
python -m venv .venv
```

This creates a virtual environment folder named `.venv`. Each virtual environment
contains an independent set of Python packages. Activate the virtual environment by
running one of the following:

```shell
.venv\Scripts\activate # Windows
source .venv/bin/activate # Linux, macOS, or Unix
```

Install the packages for the bootstrapper by running the following:

```shell
python -m pip install -r requirements.txt
```

This installs all the packages listed in the `requirements.txt` file in the current
folder.

### Customize the bootstrapper script

1. Open the script `bedrock_studio_bootstrapper.py` in a text editor.

2. Locate the `customize_and_run()` function (around line 27).

3. By default, the script runs with a default configuration. If you want to use this default setup, you don't need to make any changes.

4. To customize the bootstrapper, you can uncomment and modify the `custom_configuration` object. Here's how you can customize different aspects:

   a. Change role names:
      ```python
      custom_configuration = BootstrapConfiguration(
          provisioning_role_name="CustomProvisioningRoleName",
          service_role_name="CustomServiceRoleName"
      )
      ```

   b. Enable KMS key creation with a custom alias:
      ```python
      custom_configuration = BootstrapConfiguration(
          kms_config=KmsConfiguration(
              enabled=True,
              key_alias="CustomKmsKeyAlias"
          )
      )
      ```

   c. Enable OpenSearch encryption policy with a custom domain ID:
      ```python
      custom_configuration = BootstrapConfiguration(
          opensearch_config=OpenSearchConfiguration(
              enabled=True,
              domain_id="1234567"  # Replace with the first seven digits of your OpenSearch domain ID
          )
      )
      ```

   You can combine these configurations as needed.

5. After setting up your custom configuration, uncomment the following lines:
   ```python
   custom_bootstrapper = BedrockStudioBootstrapper(
       region=region,
       config=custom_configuration
   )
   custom_bootstrapper.run()

6. Comment out or remove the default bootstrapper lines:
```python
default_bootstrapper = BedrockStudioBootstrapper(region=region)
default_bootstrapper.run()
```

7. Save your changes to the script.

By following these steps, you can easily customize the bootstrapper to fit your specific needs before running it. If you don't need any customization, you can run the script as-is to use the default configuration.

### Run the bootstrapper

Once all prerequisites are in place and you have customized the bootstrapper, it can be run from the command line:

```shell
python bedrock_studio_bootstrapper.py
```

## Additional resources

- [Amazon Bedrock Studio User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/br-studio.html?trk=2483aad2-15a6-4b7a-a1c5-189851586b67&sc_channel=el)
- [Creating an Amazon Bedrock Studio Workspace](https://docs.aws.amazon.com/bedrock/latest/userguide/administer-create-workspace.html?trk=2483aad2-15a6-4b7a-a1c5-189851586b67&sc_channel=el)

# Copyright and license

All content in this repository, unless otherwise stated, is Copyright © Amazon Web Services, Inc. or its affiliates. All rights reserved.

Except where otherwise noted, all examples in this collection are licensed under the [Apache license, version 2.0](https://www.apache.org/licenses/LICENSE-2.0) (the "License"). The full license text is provided in the `LICENSE` file accompanying this repository.
