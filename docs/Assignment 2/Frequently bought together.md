# Frequently Bought Together at Checkout
See the [Frequently Bought Together Architecture Diagram](../../diagrams/Assignment%202/Frequently%20bought%20together.drawio.png)

## 1. Use Case and Business Objective
The system recommends complementary products frequently purchased together with the products currently in a customer's cart. The objective is to improve cross-selling opportunities and increase the average order value.

## 2. Background Ingestion and Batch Processing
- The Operational Database stores products, carts, and orders.
- CDC captures changes to Orders and Order Items.
- Kafka transports the captured database events.
- Airflow orchestrates a daily processing workflow consisting of:
  - ETL and data transformation to filter completed orders and prepare product baskets.
  - Market Basket Analysis (e.g., Apriori) to identify frequently purchased product associations.
- The resulting product associations are stored in the Feature Store for online retrieval.

## 3. Real-Time Recommendation Flow
- The customer proceeds to checkout through the frontend.
- The request passes through the API Gateway to the E-commerce Service.
- The E-commerce Service retrieves the customer's cart and sends its product IDs to the Frequently Bought Together Recommendation Engine.
- The engine retrieves precomputed product associations from the Feature Store, combines recommendations, removes duplicates and products already in the cart, and returns the recommended products.
- The E-commerce Service returns the recommendations to the frontend for display.

## 4. Architectural Options and Decision

**Option A – Precomputed Recommendations (Selected)**

Product associations are calculated periodically through batch processing and stored in the Feature Store. At checkout, the Recommendation Engine retrieves the precomputed associations.

**Option B – Runtime Calculation**

Product associations are calculated dynamically during checkout using historical order data.

Option A was selected because it offers:
- **Latency:** Lower checkout latency since recommendations are already calculated.
- **Cost:** Less computational overhead during peak traffic because calculations happen offline.
- **Operational DB Load:** Reduces the need for expensive analytical queries against the transactional database.

The main trade-off is that recommendations may be up to one day old, depending on the batch schedule.

## 5. Over-Engineering Check

The following components from the high-level architecture are not required for this use case:

- **CDP:** Customer-level profiles and personalization are not required; recommendations are based on product purchase associations.
- **Vector DB:** Semantic similarity and embedding-based search are outside the current scope.
- **Flink:** Daily batch processing is sufficient; continuous stream processing is unnecessary.
- **WhatsApp:** Recommendations are displayed directly during checkout, so no external messaging channel is needed.

## 6. Missing Components

No additional architectural components are required. ETL and Market Basket Analysis are processing jobs orchestrated by Airflow, rather than separate systems.