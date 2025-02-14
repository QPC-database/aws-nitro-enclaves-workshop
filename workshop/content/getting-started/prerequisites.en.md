---
title: "Prerequisites and environment setup"
weight: 30
---


You will be using AWS Cloud9 IDE as development environment to streamline the process. Now that you have it created, we can proceed to install the prerequisites we'll use throughout the workshop. These prerequisites will include:

* [Disable AWS managed temporary credentials](#disable-aws-managed-temporary-credentials)
* [Install and configure Nitro Enclaves CLI and tools](#install-and-configure-nitro-enclaves-cli-and-tools)
* [Clone the workshop repository](#clone-the-workshop-repository)

### Disable AWS managed temporary credentials

1. With your AWS Cloud9 the environment open, in the AWS Cloud9 IDE, on the menu bar choose the **AWS Cloud9 icon, Preferences**.

1. On the **Preferences** tab, in the navigation pane, choose **AWS Settings, Credentials**.

1. Use **AWS managed temporary credentials** to turn AWS managed temporary credentials off.


### Install and configure Nitro Enclaves CLI and tools
{{% notice note %}}
You do not have to be in any specific folder as by default CLI tool will be installed in `build/install`. Learn more in [Documentation](https://github.com/aws/aws-nitro-enclaves-cli)
{{% /notice %}}

1. Install the Nitro Enclaves CLI. This will enable you to run enclaves.
    ```sh
    $ sudo amazon-linux-extras install aws-nitro-enclaves-cli -y
    ```

1. Install the Nitro Enclaves development tools. This will enable you to build enclaves.
    ```sh
    $ sudo yum install aws-nitro-enclaves-cli-devel -y
    ```

1. Configure user permissions.
    ```sh
    $ sudo usermod -aG ne $USER
    $ sudo usermod -aG docker $USER
    ```

1. Verify that the Nitro Enclaves CLI installed correctly.
    ```sh
    $ nitro-cli --version
    ```
    The **output** should look similar to
    <pre>Nitro CLI &lt;VERSION&gt;</pre>

1. Start and enable the Nitro Enclaves allocator service.
    ```sh
    $ sudo systemctl start nitro-enclaves-allocator.service && sudo systemctl enable nitro-enclaves-allocator.service
    ```

1. Start and enable the Docker service.
    ```sh
    $ sudo systemctl start docker && sudo systemctl enable docker
    ```

1. Reboot the instance. At this point, we would need to log out and log back in to ensure that the `usermod` command that appended user to groups takes effect. One option could be to start a new shell with something like `exec sudo su --login $USER` and another could be by preparing the `groupadd` and `usermod` commands before the current shell login. However, since Cloud9 handles reboots gracefully and returns the shell to a similar state, we'll restart the instance.
    ```sh
    $ sudo shutdown -r now
    ```
    This takes a moment (about 30 seconds) to complete.
    
    <!--
        {{% notice note %}}
    While we would only need to log out and log back in to ensure that the `usermod` command that appended our `ec2-user` user to the groups takes effect, Cloud9 handles reboots gracefully and returns the shell to a similar state. An alternative option could be to start a new shell with something like `exec sudo su --login $USER`.
        {{% /notice %}}
    -->

1. Verify that `ec2-user` is a member of both the `docker` and `ne` groups.
    ```sh
    $ id $USER
    ```

    The **output** should look similar to
    <pre>uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user),4(adm),10(wheel),190(systemd-journal),991(docker),1001(ne)</pre>

{{% notice tip %}}
For additional details, see [Documentation](https://docs.aws.amazon.com/enclaves/latest/user/nitro-enclave-cli-install.html).
{{% /notice %}}

### Clone the workshop repository

1. Use `git clone` download a copy of this workshop's repository. This repo includes the content used in subsequent modules.
    ```sh
    $ git clone https://github.com/aws-samples/aws-nitro-enclaves-workshop.git
    ```

---
#### Proceed to the [My First Enclave](../my-first-enclave.html) module to continue the workshop.