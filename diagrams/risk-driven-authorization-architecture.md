** Diagram TODO 

# Conceptual Architecture: Risk-Driven Continuous Authorization

This conceptual architecture illustrates the flow of an action-level authorization decision.

1. A user initiates a sensitive action involving regulated data.
2. Contextual risk signals are evaluated at action time.
3. Authorization policy is evaluated against action sensitivity, data class, and risk tier.
4. A decision outcome is produced (allow, challenge, deny).
5. Audit-verifiable evidence is generated without logging regulated data.

This architecture is illustrative and does not prescribe deployment topology or technology choices.
