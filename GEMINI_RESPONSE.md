# JRPG Battle Loop Specification

This document defines the core 4-phase battle loop for a JRPG proof of concept, including commitment types and disruption rules, as per the provided constraints.

## 1. Battle Loop Phases

The battle proceeds in a cyclical sequence of four distinct phases: Declare, Commit, Resolve, and Recover.

### 1.1. Declare Phase
*   **Purpose:** Units (player characters and AI enemies) select their actions for the current turn.
*   **Process:**
    *   Each active unit evaluates its current state, available resources (e.g., MP, action points), and tactical situation.
    *   Units choose a single action (e.g., Attack, Skill, Item, Defend, Flee).
    *   For player characters, this involves player input. For AI, this involves AI decision-making.
*   **Commitment Type Interaction:**
    *   **Instant Skills:** Chosen and immediately prepared for commitment.
    *   **Charged Skills:** Chosen, initiating a "charging" state for the unit. The skill is declared but its execution is deferred.
    *   **Channeled Skills:** Chosen, initiating a "channeling" state for the unit. The skill is declared but its continuous effect begins in the Resolve phase.
*   **Transition:** Once all active units have declared an action, the loop proceeds to the Commit Phase.

### 1.2. Commit Phase
*   **Purpose:** All declared actions are locked in, and their commitment type determines their readiness for resolution.
*   **Process:**
    *   All actions declared in the previous phase are now committed. This means they cannot be changed or cancelled by the unit itself (unless a specific skill or effect allows it).
    *   The game system registers each committed action along with its associated unit, target(s), and commitment type.
*   **Commitment Type Definitions:**
    *   **Instant Commitment:**
        *   **Description:** Skills that execute quickly, typically within the current Resolve phase. They require minimal preparation.
        *   **Characteristics:** High priority in execution order, less susceptible to pre-execution disruption due to their immediate nature.
        *   **Example:** A basic attack, a quick healing spell, a defensive guard.
    *   **Charged Commitment:**
        *   **Description:** Skills that require a preparation turn before execution. The unit commits to the skill but does not execute it until a subsequent Resolve phase.
        *   **Characteristics:** The unit enters a "charging" state for one or more turns. During this charging period, the unit is vulnerable to disruption. If successfully charged, the skill resolves with its full potency in a later Resolve phase.
        *   **Example:** A powerful spell that takes a turn to "charge up," a physical attack that requires "winding up."
    *   **Channeled Commitment:**
        *   **Description:** Skills that require continuous focus or effort over multiple turns to maintain an effect. The unit commits to the skill, and its effect persists or repeats as long as channeling is maintained.
        *   **Characteristics:** The unit enters a "channeling" state. The effect may occur at the start/end of each Resolve phase while channeling, or be continuous. Highly vulnerable to disruption, which can interrupt the channel prematurely.
        *   **Example:** A continuous healing aura, a sustained damage-over-time spell, a defensive stance that reduces incoming damage over turns.
*   **Transition:** Once all actions are committed, the loop proceeds to the Resolve Phase.

### 1.3. Resolve Phase
*   **Purpose:** All committed actions are executed, and their effects are applied to the battle state.
*   **Process:**
    *   Actions are executed in an order determined by factors such as unit speed, skill priority, and commitment type.
    *   **Execution Order (Example Hierarchy):**
        1.  High-priority Instant skills (e.g., counter-attacks, interrupts).
        2.  Standard Instant skills (e.g., basic attacks, most spells).
        3.  Resolution of Charged skills (from previous turns that completed charging).
        4.  Effects of Channeled skills (per tick/turn).
        5.  Other turn-based effects (e.g., environmental hazards).
    *   Each action's effect is calculated, taking into account unit stats, target defenses, and any active status effects or disruptions.
*   **Disruption Rules:**
    *   **Disruption Degrades Output:** If a unit is afflicted by a disruption status effect (e.g., Stun, Silence, Fear, Weaken, Blind) *at the moment their skill is meant to resolve*, the output of their committed skill is degraded.
        *   **Examples of Degradation:**
            *   **Damage/Healing Skills:** Reduced potency (e.g., 50% less damage/healing).
            *   **Status Effects:** Reduced chance to apply, shorter duration, or reduced potency of the applied effect.
            *   **Charged Skills:** If disrupted during the charging turn, the charge might be lost, or the resolved skill's output is degraded.
            *   **Channeled Skills:** If disrupted during channeling, the channel might be interrupted prematurely, or the effect per tick is degraded.
    *   **Committed Skills Resolve Unless a Unit is Defeated:** A committed skill will always attempt to resolve, even if the unit is heavily disrupted, *unless* the unit is defeated (HP drops to 0 or below) *before* their skill's execution window in the current Resolve phase.
        *   If a unit is defeated, any committed skill they had for the current Resolve phase (or any ongoing Charged/Channeled skill) immediately fails and does not apply its effects.
        *   If a unit is defeated *after* their skill has resolved, the skill's effects still apply.
*   **Transition:** Once all actions for the turn have resolved, the loop proceeds to the Recover Phase.

### 1.4. Recover Phase
*   **Purpose:** End-of-turn maintenance, status effect updates, and resource regeneration.
*   **Process:**
    *   **Status Effect Management:** Durations of temporary buffs, debuffs, and other status effects (e.g., Poison, Regeneration, Stun) are decremented. Effects that expire are removed.
    *   **Resource Regeneration:** Units may regenerate a portion of their resources (e.g., MP, special gauges) based on their stats or active effects.
    *   **Ongoing Effects:** Any continuous effects not handled in Resolve (e.g., passive healing, environmental damage) are applied.
    *   **Charged/Channeled State Updates:** Units that were charging or channeling update their state. If a Charged skill's charge period is complete, it's marked for resolution in the next Resolve phase. If a Channeled skill's duration is complete, the channel ends.
    *   **Battle End Check:** The game checks if victory or defeat conditions have been met (e.g., all enemies defeated, all player characters defeated).
*   **Transition:** If the battle is not over, the loop returns to the Declare Phase for the next turn.

## 2. Disruption Mechanics (Summary)

*   **Definition:** Disruption refers to any effect that negatively impacts a unit's ability to perform actions effectively. This includes, but is not limited to, status effects like Stun, Silence, Fear, Weaken, Blind, or specific abilities that interfere with skill execution.
*   **Impact on Output:** When a unit is disrupted, the effectiveness of their committed skills is reduced. This is a degradation, not an outright cancellation (unless specified by a particular disruption effect, or if the unit is defeated).
*   **Unit Defeat vs. Disruption:**
    *   **Defeat:** An absolute failure. If a unit is defeated before their skill resolves, the skill fails entirely.
    *   **Disruption:** A partial failure or reduction in effectiveness. The skill still attempts to resolve, but its impact is lessened.
