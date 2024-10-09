# Project Deployment Using AWS CloudFormation

This project demonstrates how to launch and configure the infrastructure using a `.yaml` template file with **CloudFormation** on **AWS**. Follow the diagram and instructions to build the entire project infrastructure.

## Prerequisites

1. **Key Pair**: Before launching the project, create a key pair that will be needed for EC2 connections.
    - Name the key pair `mykey`, or use any name of your choice and update the `.yaml` file accordingly with the chosen name.

## Deployment Steps

1. **Launch the CloudFormation Template**: Use the provided `.yaml` file with CloudFormation to build the infrastructure as outlined in the project's architecture diagram.

## Testing the Setup

1. **Ping the Bastion Host**:
    - From your terminal, ping the **Bastion Host (Public Instance)** to verify connection:

      ```bash
      ping <ip-address-of-bastion-host>
      ```

2. **Transfer the Key File**:
    - Once you confirm the connection, transfer the key file from your local machine to the Bastion Host:

      ```bash
      scp -i <keyfile> <file-to-transfer> <user>@<bastion-host-ip>:/path/to/save/file
      ```

    - Example:

      ```bash
      scp -i mykey.pem mykey.pem ec2-user@52.235.87.25:/home/ec2-user
      ```

3. **Connect to the Bastion Host**:
    - After transferring the key file, SSH into the **Bastion Host**:

      ```bash
      ssh -i mykey.pem ec2-user@52.235.87.25
      ```

4. **Verify the Key File**:
    - Confirm that the key file is present on the Bastion Host by listing the directory contents:

      ```bash
      ls
      ```

5. **Ping the Private Instance (VPC A)**:
    - Now, ping the private instance in **VPC A**:

      ```bash
      ping <ip-address-of-private-instance>
      ```

6. **SSH into the Private Instance**:
    - Once you've confirmed the connection, SSH into the private instance from the Bastion Host:

      ```bash
      ssh -i mykey.pem ec2-user@10.0.15.6
      ```

7. **Success**:
    - You have successfully connected from **VPC B** (Bastion Host Instance) to **VPC A** (Private Instance).

## Additional Tips

- **Key File Permissions**: Don't forget to change the permissions of the key file before using it for SSH connections:

    ```bash
    chmod 400 mykey.pem
    ```

- **Note**: The IP addresses used in the examples are fictitious. Replace them with the actual IPs assigned to your instances.

---

By following these steps, you'll have a fully operational infrastructure as defined by the CloudFormation template.
