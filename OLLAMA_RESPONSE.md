## Battle Loop (Declare -> Commit -> Resolve -> Recover)

### 1. Declare
- **Objective**: Players choose their actions and targets for the next turn.
- **Input**: Player selects skills, units, and target enemies.
- **Commitment Type**: Instant (players decide immediately)
- **Disruption Impact**: No commitment; decisions can be altered up to the Commit phase.

### 2. Commit
- **Objective**: Players confirm their actions before they execute.
- **Input**: Player confirms their selected actions.
- **Commitment Type**: Charged (actions are committed but not yet executed)
- **Disruption Impact**: Actions cannot be changed; units commit their chosen skills and movements.

### 3. Resolve
- **Objective**: The game resolves all actions simultaneously, applying effects to units.
- **Input**: None (Automated resolution based on committed actions)
- **Commitment Type**: Instant (actions are executed immediately upon confirmation)
- **Disruption Impact**: Confirmed actions resolve; defeated units are removed from combat.

### 4. Recover
- **Objective**: Units recover health and energy, and the battle loop begins anew.
- **Input**: None (Automated recovery based on unit statuses)
- **Commitment Type**: Instant (units recover without further input)
- **Disruption Impact**: Defeated units can be resurrected with skills or items, restoring combat readiness.

### Summary
The 4-phase battle loop in this JRPG proof of concept follows a structured process of declaring actions, committing them, resolving their effects, and recovering. Each phase has specific commitment types (Instant, Charged) to ensure player decisions are handled predictably and the game remains engaging. Disruption rules allow for adjustments up to the Commit phase but lock commitments once confirmed to maintain momentum in combat.
