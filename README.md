# Week 2 Physics In-Class Assignment

Physical AI, World Models, and Games — CS 6983.

## Project

This Unity scene contains a floor, an actor, a target, a payload, a goal zone, a camera, and a directional light. The actor applies force toward the target and can push the payload through physical contact.

- **Unity Editor:** 6000.3.24f1 (Unity 6.3 LTS)
- **Template:** Universal 3D
- **Scene:** `Assets/Scenes/SampleScene.unity`

## Run the scene

1. Clone or download this repository.
2. Add the repository's root folder to Unity Hub and open it with Unity 6000.3.24f1.
3. Allow Unity to import the assets and resolve the packages.
4. Open `Assets/Scenes/SampleScene.unity`.
5. Press Play to observe the actor moving toward the target and colliding with the payload.

The saved scene uses the baseline Actor Linear Damping value of **0**. To reproduce the modified run, stop Play mode, select **Actor**, set **Rigidbody → Linear Damping** to **2**, and run the scene again.

## Script implementation

- `InClassActorController.cs` applies force in the normalized horizontal direction toward the target, using `moveStrength` and `ForceMode.Force` in `FixedUpdate`.
- `InClassGoalZone.cs` checks the entering collider's tag against `Payload`. On the first qualifying entry, it marks success, calculates the elapsed game time, and invokes `PayloadSucceeded` with that time.
- The success guard prevents duplicate success notifications within a run. The scene has no success-event listener or on-screen success display; the script exposes `HasSucceeded` and `TimeToSuccess` for other code to inspect.

## Experiment: Actor Linear Damping

The experiment changed the **Actor's Linear Damping from 0 to 2**. The other physics settings were kept the same for the comparison.

| Setting | Baseline | Modified run |
| --- | ---: | ---: |
| Actor Linear Damping | 0 | 2 |
| Actor Mass | 1 | 1 |
| Actor Angular Damping | 0.05 | 0.05 |
| Actor Move Strength | 20 | 20 |
| Payload Mass | 1 | 1 |
| Payload Linear Damping | 0 | 0 |
| Payload Angular Damping | 0.05 | 0.05 |

**Baseline observation:** The boxes collided and tumbled. The moving box returned after the collision and settled after a few tumbles.

**Observation after the change:** The tumbling stopped when the Actor's Linear Damping was increased to 2.

**Interpretation:** Linear damping reduces translational velocity. A plausible explanation is that the additional damping reduced motion during the interaction, making the collisions less likely to cause tumbling. Angular damping, which directly damps rotation, was not changed. This explanation is an interpretation of the qualitative observation rather than a measurement of impact speed or torque.

No numerical time-to-success result was recorded for this comparison.
