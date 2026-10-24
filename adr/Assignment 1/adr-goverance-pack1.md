
## 5. Architecture Decision Records (ADRs)

### ADR-01: External LLM API vs. Self-Hosted LLM

**Decision:** Use an external LLM API.

**Options & Trade-offs:**
- **External API:** Easier integration and maintenance, but introduces provider dependency, usage costs, and privacy considerations.
- **Self-hosted LLM:** Greater infrastructure control, but requires additional hardware, deployment, and maintenance.

**Rationale:** An external API is simpler for this project. Only necessary, authorized data should be shared with the provider, with sensitive information excluded.

### ADR-02: Batch vs. Real-Time Processing

**Decision:** Run campaign generation as a nightly batch.

**Options & Trade-offs:**
- **Batch:** Reduces repeated processing and can lower costs, but offers may be less timely.
- **Real-time:** Enables immediate responses to customer activity, but increases processing frequency and operational costs.

**Rationale:** The system analyzes accumulated purchase history to identify relevant products and generate offers, so nightly processing is sufficient for the initial design.

### ADR-03: Relational Database + Vector Search

**Decision:** Use a relational database as the primary data store, alongside vector search for product similarity.

**Options & Trade-offs:**
- **Relational database only:** Handles structured relationships and business rules effectively, but offers limited semantic similarity search.
- **Vector search only:** Supports semantic similarity, but is unsuitable as the primary store for structured business relationships and constraints.
- **Hybrid approach:** Combines structured queries with semantic product similarity, but adds storage and synchronization complexity.

**Rationale:** Customers, orders, products, sellers, and campaign criteria require relational storage. Vector search helps identify potentially similar products using names and descriptions, supporting campaign generation and the identification of potentially relevant exclusions. Deterministic rules remain responsible for enforcing seller constraints and campaign eligibility.