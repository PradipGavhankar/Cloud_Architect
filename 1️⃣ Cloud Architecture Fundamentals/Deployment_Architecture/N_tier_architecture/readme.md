# 🚀 Cloud Architect Learning – N-Tier Architecture Design (AWS + Azure) using IaC Terraform

---

# 🌍 What is N-Tier Architecture

## 🟢 हिन्दी में आसान समझ

N-Tier Architecture का मतलब है application को अलग-अलग हिस्सों (layers) में बांटना ताकि हर layer का काम clear हो और system आसानी से manage और scale किया जा सके।

👉 Real Life Example (Shopping Mall):

- Customer → Front desk (Presentation Layer)
- Billing Counter → Application Logic
- Warehouse → Data Storage (Database)

👉 Cloud में भी ऐसा होता है:

- UI Layer (Frontend)
- Business Logic Layer (Backend)
- Database Layer (Data Storage)

हर layer अलग-अलग server या service पर चलता है।

---

## 🔵 English Explanation

N-Tier Architecture is a **layered design pattern** where applications are divided into multiple tiers:

- Presentation Tier (Frontend/UI)
- Application Tier (Business Logic/API)
- Data Tier (Database)

Each tier is independently scalable, deployable, and secure, making it ideal for **cloud-native production systems**.

---

# 🧠 Why N-Tier Architecture is Important in Cloud Architecture

### 🔹 Role in Production Systems
- Modular design
- Easy deployment and updates
- Fault isolation

### 🔹 Scalability Benefits
- Auto-scale frontend and backend independently
- Database scaling (Read replicas, sharding)

### 🔹 Security Benefits
- DB in private subnet
- Zero direct internet exposure
- Controlled access via API

### 🔹 Cost Optimization
- Scale only required tier
- Use serverless / containers
- Reduce idle resources

---

# 🏗 Architecture Diagram
            Users (Web/Mobile)
                    |
           DNS (Route53 / Azure DNS)
                    |
     CDN (CloudFront / Azure CDN)
                    |


---

### 🔍 Diagram Explanation

1. User hits domain → DNS resolves IP
2. CDN caches static content
3. Load balancer distributes traffic
4. Web tier handles UI
5. App tier processes logic
6. Database stores data securely

---

# ⚙ How It Works (Step by Step)

1. User opens website
2. DNS resolves to Load Balancer
3. CDN serves cached content
4. Load Balancer routes request
5. Web Tier handles frontend
6. Backend API processes request
7. DB stores/retrieves data
8. Response returned to user

---

# 💻 Practical Implementation (Terraform – AWS + Azure)

## 🧱 Code Example

```hcl
# AWS VPC create karna
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"  # network range define karna
}

# AWS Subnet create karna
resource "aws_subnet" "public" {
  vpc_id     = aws_vpc.main.id  # VPC attach karna
  cidr_block = "10.0.1.0/24"    # subnet range define
}

# AWS EC2 instance create karna
resource "aws_instance" "web" {
  ami           = "ami-123456"  # OS image select
  instance_type = "t2.micro"    # instance size
  subnet_id     = aws_subnet.public.id  # subnet assign
}

# AWS RDS database create karna
resource "aws_db_instance" "db" {
  engine            = "mysql"      # DB engine
  instance_class    = "db.t3.micro" # DB size
  allocated_storage = 20           # storage define
}

# Azure Resource Group banana
resource "azurerm_resource_group" "rg" {
  name     = "prod-rg"         # resource group ka naam
  location = "Central India"   # region define
}

# Azure Virtual Network banana
resource "azurerm_virtual_network" "vnet" {
  name                = "prod-vnet"   # VNet naam
  address_space       = ["10.1.0.0/16"] # network range
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
}

# Azure VM create karna
resource "azurerm_linux_virtual_machine" "vm" {
  name                = "web-vm"   # VM naam
  resource_group_name = azurerm_resource_group.rg.name
  location            = azurerm_resource_group.rg.location
  size                = "Standard_B1s" # VM size
}

