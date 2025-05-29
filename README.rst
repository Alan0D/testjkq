=======
aws-cli
=======

.. image:: https://github.com/aws/aws-cli/actions/workflows/run-tests.yml/badge.svg
   :target: https://github.com/aws/aws-cli/actions/workflows/run-tests.yml
   :alt: Build Status


This package provides a unified command line interface to Amazon Web Services.

The aws-cli package works on Python versions:

* 3.9.x
* 3.10.x
* 3.11.x
* 3.12.x
* 3.13.x

.. attention::
   We recommend that all customers regularly monitor the
   `Amazon Web Services Security Bulletins website`_ for any important security bulletins related to
   aws-cli.

 Jump to:

-  `Notices <#notices>`__
-  `Installation <#installation>`__
-  `API Keys and Credentials <#api-keys-and-credentials>`__
-  `Getting Started <#getting-started>`__
-  `Getting Help <#getting-help>`__
-  `More Resources <#more-resources>`__


-------
Notices
-------
On 2024-11-13, support for macOS version 10.15 was dropped.
To use up-to-date versions of AWS CLI v2, customers using macOS 10.15 or prior
should upgrade to macOS 11 or later. For more information, please see
this `blog post <https://aws.amazon.com/blogs/developer/macos-support-policy-updates-for-the-aws-cli-v2/>`__.


------------
Installation
------------

AWS CLI v2 can easily be installed on most standard platforms:

* `MacOS pkg installer <https://awscli.amazonaws.com/AWSCLIV2.pkg>`__

* `Linux x86-64 executable installer <https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip>`__

* `Linux arm64 (aarch64) executable installer <https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip>`__

* `Windows MSI installer <https://awscli.amazonaws.com/AWSCLIV2.msi>`__

You can find more detailed installation instructions `here <https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html>`__.

If you want to run the ``v2`` development branch of the CLI, see the
"CLI Dev Version" section below.


-----------------------
API Keys and Credentials
-----------------------

Secure management of your AWS credentials is crucial for maintaining the security of your AWS resources. The AWS CLI uses these credentials to authenticate your requests to AWS services.

**Types of Credentials**

1. **AWS Access Key ID and Secret Access Key**: The primary credentials used to authenticate with AWS services.
   
2. **Session Token**: Used with temporary credentials for additional security.

**Methods for Managing Credentials**

There are several ways to provide your credentials to the AWS CLI:

1. **Environment Variables** (recommended for temporary sessions):
   
   .. code-block:: bash
   
       $ export AWS_ACCESS_KEY_ID=<your_access_key>
       $ export AWS_SECRET_ACCESS_KEY=<your_secret_key>
       $ export AWS_SESSION_TOKEN=<your_session_token>  # if using temporary credentials
   
2. **Shared Credentials File** (recommended for local development):
   
   Create a file at ``~/.aws/credentials`` (Linux/Mac) or ``%UserProfile%\.aws\credentials`` (Windows):
   
   .. code-block:: ini
   
       [default]
       aws_access_key_id = <your_access_key>
       aws_secret_access_key = <your_secret_key>
       aws_session_token = <your_session_token>  # if using temporary credentials
       
       [project1]
       aws_access_key_id = <project1_access_key>
       aws_secret_access_key = <project1_secret_key>
   
   You can specify a different location for this file using:
   
   .. code-block:: bash
   
       $ export AWS_SHARED_CREDENTIALS_FILE=/path/to/credentials_file
   
3. **AWS Config File**:
   
   Create a file at ``~/.aws/config`` (Linux/Mac) or ``%UserProfile%\.aws\config`` (Windows):
   
   .. code-block:: ini
   
       [default]
       aws_access_key_id = <your_access_key>
       aws_secret_access_key = <your_secret_key>
       region = us-west-2
       
       [profile project1]
       aws_access_key_id = <project1_access_key>
       aws_secret_access_key = <project1_secret_key>
       region = us-east-1
   
   You can specify a different location for this file using:
   
   .. code-block:: bash
   
       $ export AWS_CONFIG_FILE=/path/to/config_file
   
4. **IAM Roles** (recommended for EC2 instances and production environments):
   
   When running on an EC2 instance, use IAM roles to automatically obtain credentials without storing them in files or environment variables.

5. **AWS Single Sign-On (SSO)**:
   
   For organizations using AWS SSO, configure profiles in your config file:
   
   .. code-block:: ini
   
       [profile my-sso-profile]
       sso_start_url = https://my-sso-portal.awsapps.com/start
       sso_region = us-east-1
       sso_account_id = 123456789011
       sso_role_name = SSOReadOnlyRole
       region = us-west-2
       output = json

**Security Best Practices**

1. **Never hardcode credentials** in your application code or commit them to version control.
   
2. **Use temporary credentials** whenever possible instead of long-term access keys.
   
3. **Rotate access keys** regularly (every 90 days recommended).
   
4. **Use IAM roles** for applications running on AWS services like EC2, ECS, or Lambda.
   
5. **Apply the principle of least privilege** by granting only the permissions needed.
   
6. **Enable Multi-Factor Authentication (MFA)** for all users.
   
7. **Monitor credential usage** with AWS CloudTrail.

For more information on credential security, see the `AWS Security Best Practices <https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html>`__ and `IAM User Guide <https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html>`__.


------------
CLI Releases
------------

The release notes for the AWS CLI can be found `here <https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst>`__.


------------------
Command Completion
------------------

The aws-cli package includes a very useful command completion feature.
This feature is not automatically installed so you need to configure it manually.
To enable tab completion for bash either use the built-in command ``complete``::

    $ complete -C aws_completer aws

Or add ``bin/aws_bash_completer`` file under ``/etc/bash_completion.d``,
``/usr/local/etc/bash_completion.d`` or any other ``bash_completion.d`` location.

For tcsh::

    $ complete aws 'p/*/`aws_completer`/'

You should add this to your startup scripts to enable it for future sessions.

For zsh please refer to ``bin/aws_zsh_completer.sh``.  Source that file, e.g.
from your ``~/.zshrc``, and make sure you run ``compinit`` before::

    $ source bin/aws_zsh_completer.sh

For now the bash compatibility auto completion (``bashcompinit``) is used.
For further details please refer to the top of ``bin/aws_zsh_completer.sh``.

---------------
Getting Started
---------------

Before using aws-cli, you need to configure your AWS credentials. For detailed information about credential types and configuration methods, see the `API Keys and Credentials <#api-keys-and-credentials>`__ section above.

The quickest way to get started is to run the ``aws configure`` command::

    $ aws configure
    AWS Access Key ID: foo
    AWS Secret Access Key: bar
    Default region name [us-west-2]: us-west-2
    Default output format [None]: json

This command will prompt you for your credentials and store them in the shared credentials file located at ``~/.aws/credentials`` (or ``%UserProfile%\.aws\credentials`` on Windows).

Once you've configured your credentials, you can start using the AWS CLI to interact with AWS services. For example, to list your S3 buckets::

    $ aws s3 ls

For more information about configuration options, please refer to the
`AWS CLI Configuration Variables topic <http://docs.aws.amazon.com/cli/latest/topic/config-vars.html#cli-aws-help-config-vars>`_. You can access this topic
from the CLI as well by running ``aws help config-vars``.

----------------------------
Other Configurable Variables
----------------------------

In addition to credentials, a number of other variables can be
configured either with environment variables, configuration file
entries or both.  The following table documents these.

============================= =========== ============================= ================================= ==================================
Variable                      Option      Config Entry                  Environment Variable              Description
============================= =========== ============================= ================================= ==================================
profile                       --profile   profile                       AWS_PROFILE                       Default profile name
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
region                        --region    region                        AWS_DEFAULT_REGION                Default AWS Region
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
config_file                                                             AWS_CONFIG_FILE                   Alternate location of config
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
credentials_file                                                        AWS_SHARED_CREDENTIALS_FILE       Alternate location of credentials
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
output                        --output    output                        AWS_DEFAULT_OUTPUT                Default output style
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
ca_bundle                     --ca-bundle ca_bundle                     AWS_CA_BUNDLE                     CA Certificate Bundle
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
access_key                                aws_access_key_id             AWS_ACCESS_KEY_ID                 AWS Access Key
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
secret_key                                aws_secret_access_key         AWS_SECRET_ACCESS_KEY             AWS Secret Key
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
token                                     aws_session_token             AWS_SESSION_TOKEN                 AWS Token (temp credentials)
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
cli_timestamp_format                      cli_timestamp_format                                            Output format of timestamps
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
metadata_service_timeout                  metadata_service_timeout      AWS_METADATA_SERVICE_TIMEOUT      EC2 metadata timeout
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
metadata_service_num_attempts             metadata_service_num_attempts AWS_METADATA_SERVICE_NUM_ATTEMPTS EC2 metadata retry count
----------------------------- ----------- ----------------------------- --------------------------------- ----------------------------------
parameter_validation                      parameter_validation                                            Toggles local parameter validation
============================= =========== ============================= ================================= ==================================

^^^^^^^^
Examples
^^^^^^^^

If you get tired of specifying a ``--region`` option on the command line
all of the time, you can specify a default region to use whenever no
explicit ``--region`` option is included using the ``region`` variable.
To specify this using an environment variable::

    $ export AWS_DEFAULT_REGION=us-west-2

To include it in your config file::

    [default]
    aws_access_key_id=<default access key>
    aws_secret_access_key=<default secret key>
    region=us-west-1

Similarly, the ``profile`` variable can be used to specify which profile to use
if one is not explicitly specified on the command line via the
``--profile`` option.  To set this via environment variable::

    $ export AWS_PROFILE=testing

The ``profile`` variable can not be specified in the configuration file
since it would have to be associated with a profile and would defeat the
purpose.

^^^^^^^^^^^^^^^^^^^
Further Information
^^^^^^^^^^^^^^^^^^^

For more information about configuration options, please refer the
`AWS CLI Configuration Variables topic <http://docs.aws.amazon.com/cli/latest/topic/config-vars.html#cli-aws-help-config-vars>`_. You can access this topic
from the CLI as well by running ``aws help config-vars``.


----------------------------------------
Accessing Services With Global Endpoints
----------------------------------------

Some services, such as *AWS Identity and Access Management* (IAM)
have a single, global endpoint rather than different endpoints for
each region.

To make access to these services simpler, aws-cli will automatically
use the global endpoint unless you explicitly supply a region (using
the ``--region`` option) or a profile (using the ``--profile`` option).
Therefore, the following::

    $ aws iam list-users

will automatically use the global endpoint for the IAM service
regardless of the value of the ``AWS_DEFAULT_REGION`` environment
variable or the ``region`` variable specified in your profile.

--------------------
JSON Parameter Input
--------------------

Many options that need to be provided are simple string or numeric
values.  However, some operations require JSON data structures
as input parameters either on the command line or in files.

For example, consider the command to authorize access to an EC2
security group.  In this case, we will add ingress access to port 22
for all IP addresses::

    $ aws ec2 authorize-security-group-ingress --group-name MySecurityGroup \
      --ip-permissions '{"FromPort":22,"ToPort":22,"IpProtocol":"tcp","IpRanges":[{"CidrIp": "0.0.0.0/0"}]}'

--------------------------
File-based Parameter Input
--------------------------

Some parameter values are so large or so complex that it would be easier
to place the parameter value in a file and refer to that file rather than
entering the value directly on the command line.

Let's use the ``authorize-security-group-ingress`` command shown above.
Rather than provide the value of the ``--ip-permissions`` parameter directly
in the command, you could first store the values in a file.  Let's call
the file ``ip_perms.json``::

    {"FromPort":22,
     "ToPort":22,
     "IpProtocol":"tcp",
     "IpRanges":[{"CidrIp":"0.0.0.0/0"}]}

Then, we could make the same call as above like this::

    $ aws ec2 authorize-security-group-ingress --group-name MySecurityGroup \
        --ip-permissions file://ip_perms.json

The ``file://`` prefix on the parameter value signals that the parameter value
is actually a reference to a file that contains the actual parameter value.
aws-cli will open the file, read the value and use that value as the
parameter value.

This is also useful when the parameter is really referring to file-based
data.  For example, the ``--user-data`` option of the ``aws ec2 run-instances``
command or the ``--public-key-material`` parameter of the
``aws ec2 import-key-pair`` command.

--------------
Command Output
--------------

The default output for commands is currently JSON.  You can use the
``--query`` option to extract the output elements from this JSON document.
For more information on the expression language used for the ``--query``
argument, you can read the
`JMESPath Tutorial <http://jmespath.org/tutorial.html>`__.

^^^^^^^^
Examples
^^^^^^^^

Get a list of IAM user names::

    $ aws iam list-users --query Users[].UserName

Get a list of key names and their sizes in an S3 bucket::

    $ aws s3api list-objects --bucket b --query Contents[].[Key,Size]

Get a list of all EC2 instances and include their Instance ID, State Name,
and their Name (if they've been tagged with a Name)::

    $ aws ec2 describe-instances --query \
      'Reservations[].Instances[].[InstanceId,State.Name,Tags[?Key==`Name`] | [0].Value]'


You may also find the `jq <http://stedolan.github.com/jq/>`_ tool useful in
processing the JSON output for other uses.

There is also an ASCII table format available.  You can select this style with
the ``--output table`` option or you can make this style your default output
style via environment variable or config file entry as described above.
Try adding ``--output table`` to the above commands.


---------------
CLI Dev Version
---------------

If you are just interested in using the latest released version of the AWS CLI,
please see the Installation_ section above.  This section is for anyone who
wants to install the development version of the CLI.  You normally would not
need to do this unless:

* You are developing a feature for the CLI and plan on submitting a Pull
  Request.
* You want to test the latest changes of the CLI before they make it into an
  official release.

The latest changes to the CLI are in the ``v2`` branch on github.  This is
**NOT** the default branch when you clone the git repository, so you'll need
to make sure you ``git checkout v2``.

If you just want to install a snapshot of the latest development version of
the CLI, you can use the ``requirements-dev.txt`` file included in this repo.
This file points to the development version of our dependencies::

    $ cd <path_to_awscli> && git checkout v2
    $ pip install -r requirements-dev.txt
    $ pip install -e .

Verify that the AWS CLI is correctly installed. Note that the word ``source`` should appear in the output::

    $ aws --version
    aws-cli/2.2.30 Python/3.8.11 Darwin/20.4.0 source/x86_64 prompt/off

Generate the autocompletion index::

    $ ./scripts/gen-ac-index --include-builtin-index

Verify the autocompletion index is generated by entering auto-prompt mode::

    $ aws --cli-auto-prompt

------------
Getting Help
------------
The best way to interact with our team is through GitHub. You can `open
an issue <https://github.com/aws/aws-cli/issues/new/choose>`__ and
choose from one of our templates for guidance, bug reports, or feature
requests.

You may find help from the community on `Stack
Overflow <https://stackoverflow.com/>`__ with the tag
`aws-cli <https://stackoverflow.com/questions/tagged/aws-cli>`__ or on
the `AWS Discussion Forum for
CLI <https://forums.aws.amazon.com/forum.jspa?forumID=150>`__. If you
have a support plan with `AWS Premium
Support <https://aws.amazon.com/premiumsupport>`__, you can also create
a new support case.

Please check for open similar
`issues <https://github.com/aws/aws-cli/issues/>`__ before opening
another one.

The AWS CLI implements AWS service APIs. For general issues regarding
the services or their limitations, you may find the `Amazon Web Services
Discussion Forums <https://forums.aws.amazon.com/>`__ helpful.


--------------
More Resources
--------------

-  `Changelog <https://github.com/aws/aws-cli/blob/v2/CHANGELOG.rst>`__
-  `AWS CLI
   Documentation <https://docs.aws.amazon.com/cli/index.html>`__
-  `AWS CLI User
   Guide <https://docs.aws.amazon.com/cli/latest/userguide/>`__
-  `AWS CLI Command
   Reference <https://awscli.amazonaws.com/v2/documentation/api/latest/reference/index.html>`__
-  `Amazon Web Services Discussion
   Forums <https://forums.aws.amazon.com/>`__
-  `AWS Support <https://console.aws.amazon.com/support/home#/>`__


.. _`Amazon Web Services Security Bulletins website`: https://aws.amazon.com/security/security-bulletins
.. _`download the tarball`: https://pypi.org/project/awscli/
