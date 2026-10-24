# Marketing Automation with AI & Agents

## 1. Pain Points

- Creating and managing marketing campaigns manually takes time and effort.
- Identifying relevant products and offers for customers requires analyzing purchase history.
- Campaigns may violate seller-defined criteria or contain inappropriate information.
- Customer data must be protected, and sensitive information must not be exposed.
- AI usage and campaign delivery costs need to be controlled.

## 2. Business Goals

- **G1 — Trust & Compliance:** Protect customer data and respect applicable privacy requirements.
- **G2 — Seller Growth:** Promote sellers' products to increase sales and profitability.
- **G3 — Marketing Automation:** Automate campaign creation and delivery.
- **G4 — Engagement & Cost Efficiency:** Improve customer engagement while controlling costs.

## 3. Scope

### In Scope
- Sellers and managers define campaign criteria through structured settings and optional free-text guidelines.
- Select individual products or groups of products for promotion.
- Retrieve relevant product information and customer purchase history.
- Use AI to generate personalized offers and campaigns.
- Validate campaigns against privacy rules, customer eligibility, seller guidelines, and budget limits.
- Automatically deliver campaigns only when all mandatory guardrails pass.
- Block campaigns when any mandatory guardrail fails.

### Out of Scope
- General-purpose chatbots or conversational agents.
- AI overriding seller-defined criteria or mandatory guardrails.
- Delivering campaigns that fail validation.
- Human approval of every campaign as a required step.

## 4. Architecture Principles

| Principle | Description | Business Goal |
|---|---|---|
| **Privacy by Design** | Use only authorized customer data and never expose sensitive information. | G1 — Trust & Compliance |
| **Guardrails Before Delivery** | Every campaign must pass mandatory validation. If any check fails, delivery is blocked. | G1 — Trust & Compliance; G3 — Marketing Automation |
| **Seller-Controlled Automation** | Campaigns must follow seller-defined criteria, including product selection and other restrictions. | G2 — Seller Growth |
| **Cost-Aware Automation** | AI usage and campaign delivery costs must remain within user-defined limits. | G4 — Engagement & Cost Efficiency |

## 5. Architecture Decision Records (ADRs)

See the [Architecture Decision Records](../../adr/Assignment%201/adr-goverance-pack1.md)
for the options, trade-offs, and rationale behind our architectural decisions.

## 6. Architecture Review Board (ARB) Checklist

- [ ] **Security:** Verify authentication, authorization, and access controls.
- [ ] **Privacy (PDPL):** Ensure only authorized customer data is used and sensitive information is not exposed.
- [ ] **Evaluations:** Validate campaign quality and ensure generated output follows the required structure.
- [ ] **Cost:** Ensure AI usage and campaign delivery remain within seller-defined limits.
- [ ] **Human Oversight:** Ensure sellers define campaign criteria and failed validations block delivery.

## 7. Automated Guardrails

Campaign generation uses two validation stages:

1. **Pre-generation validation:** Filter products based on seller-defined criteria and exclude ineligible products before campaign generation.
2. **Post-generation validation:** Validate the generated campaign's structure, content, privacy requirements, and seller constraints. If validation fails, the campaign is blocked from delivery.

## 8. Architecture Diagram

See the [Architecture Diagram](../../diagrams/Assignment%201/goverance-pack1.png) for the system architecture.