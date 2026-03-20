# 04 — Azure Cloud & DevOps Overview

> **Source:** Uday Academy — Azure DevOps Course (Lecture 1 & 2)  
> **Topics:** Cloud Computing, Servers, Azure, DevOps, Azure DevOps, Azure Portal, Resource Hierarchy

---

## 1. The Problem — Why Cloud Exists

### The Business Scenario
Ram (a business-minded person) wants to build an app like **Bookmyshow** — a ticket booking platform. He hires developers who build the web application. But now: **where does the app run?**

Initially it runs on **developer machines** — only accessible to those with physical access to those machines. This is called the **Developer's Machine Issue**.

### Solution: Server
A **server** is another computer (a supercomputer) with high specifications used to run applications and serve requests over the internet.

| | Normal Laptop | Server |
|---|---|---|
| RAM | 8–16 GB | 32 GB → 256 GB+ |
| Storage | 1 TB | 10 TB → 100 TB+ |
| Cost | ~₹50,000 | ~₹2,00,000+ |

When the app runs on a server connected to the internet, **anyone with internet can access it** — problem solved.

---

## 2. Problems with Owning a Server

| Problem | Details |
|---|---|
| **Capital Expenditure** | ~₹2 lakhs to purchase upfront |
| **Waiting Period** | 1–3 months for delivery (custom orders, often from abroad) |
| **Maintenance Cost** | ~₹10,000/month for electricity, internet, cooling/AC |
| **Updates & Upgrades** | Owner is responsible for security patches, OS updates |

---

## 3. What is Cloud?

> **Simple definition: Cloud = Someone else's computer taken on rent.**

Big companies like **Microsoft, Amazon, Google** saw these problems and bought servers in bulk (thousands/lakhs). They offer these servers for rent over the internet.

You need only:
- An active internet connection
- A laptop/device

### Cloud Computing
The **delivery of computing services** (servers, storage, databases, etc.) **over the internet**.

### Benefits of Cloud over Own Server

| Benefit | Explanation |
|---|---|
| **On-Demand** | Create a server in <1 minute vs waiting 1 month |
| **Scalability** | Scale up/down based on traffic (e.g., big movie release) |
| **Flexibility** | Change specs anytime — increase or decrease RAM/storage |
| **Cost Efficiency** | ~₹1,000/month on cloud vs ₹2.1 lakh+ to own a server |
| **No Maintenance** | Cloud provider handles cooling, electricity, updates |

---

## 4. Cloud Providers

The **Big 3:**

| Provider | Company | Notes |
|---|---|---|
| **Azure** | Microsoft | Great GUI portal, low-code, 80% of Fortune 500 companies use it |
| **AWS** | Amazon (Amazon Web Services) | Most popular globally |
| **GCP** | Google (Google Cloud Platform) | Strong in ML/AI workloads |

Others: Linode, Alibaba Cloud, etc.

### Why Azure in this course?
- **No Linux prerequisite** to get started (unlike AWS/GCP)
- **Low-code** — GUI portal makes it beginner-friendly
- Excellent **Microsoft product integration** (Outlook, Teams)
- **80% of Fortune 500** companies use Azure

> **Key insight:** Learn any cloud deeply and switching to another is easy. The concepts are the same; only the naming changes (e.g., Azure calls it "Virtual Machine", AWS calls it "EC2 instance").

---

## 5. What is Azure?

> **Azure is a cloud platform where one can create servers (and other resources).**

Access it at: **portal.azure.com**

### Getting Started
1. Go to portal.azure.com
2. Create a Microsoft account (Gmail works)
3. Add a credit card or Visa debit card

### Subscription Types

| Type | Details |
|---|---|
| **Free Trial** | 30 days, $200 (~₹16,000) worth of credits. Whichever runs out first (30 days OR $200) ends the trial |
| **Pay As You Go** | Postpaid model — use services, pay bill at month-end. Switch to this after free trial |

> **Analogy:** Free trial = Netflix free trial. Pay As You Go = Postpaid mobile plan (use first, pay later).

---

## 6. Azure Resource Hierarchy

Everything in Azure follows a strict hierarchy. You cannot create a resource without the entities above it being in place.

```
Microsoft Azure (Owner)
    └── Tenant (You — identified by Tenant ID)
            └── Subscription (Billing entity)
                    └── Resource Group (Organizer — like rooms in a flat)
                            └── Resource / Service (e.g., Server, Storage Account)
```

### Real-Life Analogy: Apartment Building

| Real Life | Azure Equivalent |
|---|---|
| Building owner | Microsoft/Azure |
| Tenant (renter) | You (Azure user) |
| Tenant ID | Unique ID per tenant |
| Subscription | Payment plan (holds billing details) |
| Flat/Room | Resource Group |
| Things in room (TV, fridge) | Resources/Services |

### Key Rules
- **Tenant ID** — Unique per Azure account. Created automatically when you sign up.
- **Subscription** — Holds billing/payment details. One tenant can have multiple subscriptions.
- **Resource Group** — Name must be **unique within a subscription**. Used to organize related resources.
- **Resource** — The actual service (server, storage account, etc.). Requires subscription + resource group to exist first.

### Resource Group Naming Rules
- Alphanumeric characters only
- Underscores `_`, hyphens `-`, parentheses `()` allowed
- No special characters like `@`, `#`, `$`

---

## 7. What is DevOps?

### The Software Delivery Problem (Without DevOps)

Software delivery follows stages: **Plan → Code → Test → Deploy → Monitor**

| Phase | Who Works | Who is Idle |
|---|---|---|
| Month 1: Development | Developers | QA Testers |
| Month 2: Testing | QA Testers | Developers |

**Problems with this serial approach:**
1. **Slow delivery** — 2 months minimum to take app to end user
2. **Poor quality** — Testing 5000 scenarios all at once = high chance of missing bugs
3. **Resource waste** — Teams sitting idle alternately

### DevOps Solution

> **DevOps = Development + Operations** — a set of principles and practices to deliver software **faster** with **higher quality**.

**How?**
- Plan in short **increments** (1–2 week sprints), not all at once
- Create **environments** so testing happens **continuously** alongside development
- **Infinite loop** — Plan → Code → Build → Test → Deploy → Monitor → repeat

**Result:**
- ↓ Time to delivery (time to market)
- ↑ Quality (bugs caught faster with continuous testing)

### The DevOps Infinite Loop (∞)
```
Plan → Code → Build → Test → Deploy → Monitor
  ←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←←
```

---

## 8. What is Azure DevOps?

> **Azure DevOps = A platform (one-stop solution) to implement DevOps principles.**

| Stage | Azure DevOps Service |
|---|---|
| Planning | **Azure Boards** — work items, sprints, Kanban boards |
| Code Storage | **Azure Repos** — Git repositories |
| Build & CI | **Azure Pipelines** — automated build pipelines |
| Testing | **Azure Test Plans** — test cases, scenarios |
| Deployment | **Azure Pipelines** (also handles release) |
| Monitoring | **Azure App Insights** (Azure service, but used with DevOps) |

> Azure DevOps is a **complete package** — no need for external tools like Jira, GitHub, Jenkins (though they can integrate).

### Azure vs Azure DevOps — The Key Difference

| | Azure | Azure DevOps |
|---|---|---|
| **What is it?** | Cloud platform | DevOps platform |
| **Purpose** | Create & manage servers/infrastructure | Store code, build pipelines, manage delivery |
| **URL** | portal.azure.com | dev.azure.com |

---

## 9. Course Roadmap (Uday Academy)

1. Azure Cloud fundamentals + Networking
2. PowerShell scripting (automate Azure tasks)
3. ARM Templates (Infrastructure as Code)
4. **Terraform** (IaC — must-know for DevOps interviews)
5. Azure DevOps — Boards, Repos, YAML Pipelines
6. Jenkins (open source CI/CD)
7. Docker + Kubernetes

---

## 📝 Key Takeaways

- Cloud = Rented server from someone else (Microsoft/Amazon/Google)
- Azure = Microsoft's cloud platform (create servers via portal.azure.com)
- Azure hierarchy: **Tenant → Subscription → Resource Group → Resource**
- DevOps = Principles to deliver software **faster + with higher quality**
- Azure DevOps = Platform to implement DevOps (Boards, Repos, Pipelines, Test Plans)
- Azure ≠ Azure DevOps (different purposes, easy to confuse)
