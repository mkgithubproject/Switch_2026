With **VPC Peering**, AWS creates a private network connection between two VPCs.

Suppose:

### VPC-A

```text
CIDR: 10.0.0.0/16
EC2: 10.0.1.10
```

### VPC-B

```text
CIDR: 172.31.0.0/16
EC2: 172.31.1.20
```

After creating a VPC Peering connection, **nothing works yet** until you update the route tables.

---

## Step 1: Create Peering Connection

```text
VPC-A <------ Peering ------> VPC-B
```

AWS creates a peering connection ID.

---

## Step 2: Update Route Tables

### Route Table of VPC-A

```text
Destination      Target
10.0.0.0/16      Local
172.31.0.0/16    Peering Connection
```

Meaning:

> If traffic is destined for `172.31.x.x`, send it through the peering connection.

---

### Route Table of VPC-B

```text
Destination      Target
172.31.0.0/16    Local
10.0.0.0/16      Peering Connection
```

Meaning:

> If traffic is destined for `10.x.x.x`, send it through the peering connection.

---

## Step 3: Security Groups / NACLs

Allow the traffic.

For example, on the EC2 in VPC-B:

```text
Inbound:
TCP 22
Source: 10.0.0.0/16
```

Now an EC2 in VPC-A can SSH into VPC-B.

---

## How Traffic Flows

```text
EC2-A (10.0.1.10)
        |
        v
Route Table A
        |
        v
Peering Connection
        |
        v
Route Table B
        |
        v
EC2-B (172.31.1.20)
```

Notice:

* No Internet Gateway
* No NAT Gateway
* No public IP required

Traffic stays entirely on AWS's private backbone network.

---

## Important Restriction

The CIDR ranges **must not overlap**.

❌ Invalid:

```text
VPC-A: 10.0.0.0/16
VPC-B: 10.0.0.0/16
```

AWS will not allow peering because routing becomes ambiguous.

✅ Valid:

```text
VPC-A: 10.0.0.0/16
VPC-B: 172.31.0.0/16
```

---

## Interview Question: How can one VPC access another through VPC Peering?

**Answer:**

1. Create a VPC Peering connection between the two VPCs.
2. Add routes in both VPC route tables pointing the other VPC's CIDR to the peering connection.
3. Configure security groups and NACLs to allow the traffic.
4. Instances communicate using their **private IP addresses** over AWS's private network.
