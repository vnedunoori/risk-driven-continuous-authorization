# Conceptual Architecture: Risk-Driven Continuous Authorization

This conceptual architecture illustrates the flow of an action-level authorization decision.

1. A user initiates a sensitive action involving regulated data.
2. Contextual risk signals are evaluated at action time.
3. Authorization policy is evaluated against action sensitivity, data class, and risk tier.
4. A decision outcome is produced (allow, challenge, deny).
5. Audit-verifiable evidence is generated without logging regulated data.

This architecture is illustrative and does not prescribe deployment topology or technology choices.


<img width="1259" height="470" alt="A-Lite" src="https://github.com/user-attachments/assets/30e7b353-b6b9-43bf-8d59-085ebb92c7b9" />
