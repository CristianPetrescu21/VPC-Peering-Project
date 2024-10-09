# **Master Level Instructions (Project 4)**

## Step 1: Create 2 VPCs

### **VPC-1**

1. Open the Amazon VPC console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).
2. In VPC settings, select "VPC only."
3. In **Name**, provide the name `VPC-A`.
4. In **IPv4 CIDR block**, select "IPv4 CIDR manual input" and enter `10.0.0.0/16`.
5. In tags:
    - **Key** = `Name`
    - **Value** = `VPC-A`
6. Leave all options as default and create the VPC.

### **VPC-2**

1. Open the Amazon VPC console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).
2. In VPC settings, select "VPC only."
3. In **Name**, provide the name `VPC-B`.
4. In **IPv4 CIDR block**, select "IPv4 CIDR manual input" and enter `20.0.0.0/16`.
5. In tags:
    - **Key** = `Name`
    - **Value** = `VPC-B`
6. Leave all options as default and create the VPC.

---

## Step 2: Create Subnets

### **Subnet-1**

1. Go to **Create Subnet**.
2. Select VPC (`VPC-A`).
3. In **Subnet settings**, provide the subnet name `my-subnet-VPC-A-private`.
4. Select the Availability Zone.
5. Enter **IPv4 CIDR block** as `10.0.0.0/24`.
6. Create the subnet.

### **Subnet-2**

1. Go to **Create Subnet**.
2. Select VPC (`VPC-B`).
3. In **Subnet settings**, provide the subnet name `my-subnet-VPC-B-public`.
4. Select the Availability Zone.
5. Enter **IPv4 CIDR block** as `20.0.0.0/24`.
6. Create the subnet.

---

## Step 3: Create an Internet Gateway and Attach to VPC-B

1. Create an Internet Gateway.
2. Attach the Internet Gateway to `VPC-B`.

---

## Step 4: Create Route Tables

### **Route Table in VPC-A**

1. Go to VPC and select **Create Route Table**.
2. Provide a name (e.g., `VPC-A-PrivateRoute`).
3. Select `VPC-A` in the VPC option.
4. Go to **Subnet Association** and attach the subnet created in Subnet-1.
5. Create the route table.

### **Route Table in VPC-B**

1. Go to VPC and select **Create Route Table**.
2. Provide a name (e.g., `VPC-B-PublicRoute`).
3. Select `VPC-B` in the VPC option.
4. Create the route table.
5. Select the route table created in the above step.
6. Go to **Routes** and add routes:
    - **Destination**: `0.0.0.0/0`
    - **Target**: Internet Gateway
7. Go to **Subnet Association**:
    - Click **Edit Subnet Association** and select the public subnet created in Subnet-2.
8. Save the changes.

---

## Step 5: Create Security Groups

### **Security Group in VPC-A**

1. Open the Amazon VPC console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).
2. In the navigation pane, choose **Security Groups**.
3. Choose **Create Security Group**.
4. Enter the **Security Group Name** as `SSHAccess`.
5. Enter a description.
6. Select `VPC-A`.
7. In **Inbound rules**, open the SSH port.
8. In tags:
    - **Key** = `Name`
    - **Value** = `SSHAccess`
9. Create the security group.

### **Security Group in VPC-B**

1. Open the Amazon VPC console at [https://console.aws.amazon.com/vpc/](https://console.aws.amazon.com/vpc/).
2. In the navigation pane, choose **Security Groups**.
3. Choose **Create Security Group**.
4. Enter the **Security Group Name** as `SSHAccess`.
5. Enter a description.
6. Select `VPC-B`.
7. In **Inbound rules**, open the SSH port.
8. In tags:
    - **Key** = `Name`
    - **Value** = `SSHAccess`
9. Create the security group.

---

## Step 6: Launch EC2 Instances

### **EC2 Instance in VPC-A**

1. Launch one EC2 instance in the private subnet (created in Subnet-1) in `VPC-A`.
2. Select the AMI.
3. In **Instance type**, select `t2.micro`.
4. In **Configure Instance Details**, select `VPC-A` in the network.
5. In **Subnet**, select the private subnet (created in Subnet-1).
6. Auto-assign public IP: Select **Disable**.
7. In **Add Tags**, give a name to your EC2 instance.
8. Attach the security group created earlier.
9. Select the Keypair.
10. Launch the instance.

### **EC2 Bastion Instance in VPC-B**

1. Launch one EC2 instance in the public subnet (created in Subnet-2) in `VPC-B`.
2. Select the AMI.
3. In **Instance type**, select `t2.micro`.
4. In **Configure Instance Details**, select `VPC-B` in the network.
5. In **Subnet**, select the public subnet (created in Subnet-2).
6. Auto-assign public IP: Select **Enable**.
7. In **Add Tags**, give a name to your EC2 instance.
8. Attach the security group created earlier.
9. Select the Keypair.
10. Launch the instance.

---

## Step 7: VPC Peering

1. Create a VPC Peering connection between `VPC-A` and `VPC-B`.
2. Go to the VPC console and select **Peering Connections**.
3. Choose **Create Peering Connection**.
4. Provide a name and select both Requestor and Accepter VPC.
5. Create the Peering connection.
6. Once the Peering connection is created successfully, the accepter must accept the peering request:
    - Go to **VPC Peering** > **Actions** > **Accept Request**.

---

## Step 8: Update Route Tables

### **Update Route Table in VPC-A**

1. Go to the route table created in `VPC-A`.
2. Go to **Routes** and add a new route:
    - **Destination**: CIDR range of `VPC-B`
    - **Target**: Peering Connection
3. Save the changes.

### **Update Route Table in VPC-B**

1. Go to the route table created in `VPC-B`.
2. Go to **Routes** and add a new route:
    - **Destination**: CIDR range of `VPC-A`
    - **Target**: Peering Connection
3. In **Destination**, also add:
    - **0.0.0.0/0**
    - **Target**: Internet Gateway
4. Save the changes.

---

## Step 9: Testing

To test ping connectivity, open the ICMP port in the Security Group.
