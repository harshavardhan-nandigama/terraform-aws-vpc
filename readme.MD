# Terraform AWS VPC Module

This Terraform module is developed for setting up the **VPC architecture of the Roboshop project**. It is designed to be reusable, configurable, and production-ready for creating a complete VPC environment with public, private, and database subnets, route tables, NAT, and peering.

---

##  Features

- Creates a highly available VPC with configurable CIDR
- Public, private, and database subnets across two AZs
- Internet Gateway and public routing
- NAT Gateway with Elastic IP for private access to the internet
- Custom route tables for public, private, and database traffic
- Peering connection with default VPC for cross-communication

---

## 🔧 Input Variables

| Name         | Description                            | Type   | Required | Default        |
|--------------|----------------------------------------|--------|----------|----------------|
| `project`    | Name of the project                    | string | ✅ Yes   | N/A            |
| `environment`| Environment name (e.g. dev, prod)      | string | ✅ Yes   | N/A            |
| `cidr_block` | Custom CIDR block for the VPC          | string | ❌ No    | `"10.0.0.0/16"` |

---

## VPC Creation Steps

1. Create VPC
2. Create Internet Gateway (IGW)
3. Attach IGW to the VPC

###  Subnets

4. Create 2 Public Subnets  
5. Create 2 Private Subnets  
6. Create 2 Database Subnets  

### Route Tables

7. Create a **Public Route Table**  
8. Add a route to the internet via IGW  
9. Associate it with public subnets

10. Create a **Private Route Table**  
11. Associate it with private subnets

12. Create a **Database Route Table**  
13. Associate it with database subnets

### NAT & Elastic IP

14. Create an Elastic IP  
15. Create a NAT Gateway using the EIP

16. Add default route `0.0.0.0/0` in private and DB route tables via NAT

###  Associations

17. Associate public route table with public subnets  
18. Associate private route table with private subnets  
19. Associate database route table with database subnets

###  VPC Peering

20. Create a VPC peering connection with the **default VPC**  
21. Add a route to **default VPC CIDR** in Roboshop VPC route tables  
22. Add a route to **Roboshop VPC CIDR** in default VPC route table  

---

## 📁 Folder Structure

terraform-aws-vpc/
├── data.tf
├── locals.tf
├── peering.tf
├── readme.md
├── variables.tf
└── vpc.tf