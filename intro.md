### What does it do?

- Allows an IS-04 Receiver to express parametric constraints on the types of streams that it is capable of consuming

### Why does it matter?

- Controllers need to know whether a Receiver is capable of handling a specific Sender's stream before connecting the two

### How does it work?

- Establishes an open Capabilities register in the NMOS Parameter Registers that lists specifications for parametric constraints (such as width, height, frame rate, number of channels, etc.)
- Defines how a Receiver instantiates these Parameter Constraints to make up a list of acceptable Constraint Sets, within the IS-04 'caps' attribute
- Defines how Controllers evaluate whether an IS-04 Sender satisfies these constraints, based on the target parameters specified for each constraint (such as IS-04 Flow attributes and SDP format-specific parameters)
