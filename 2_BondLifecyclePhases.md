# Bond Lifecycle Phases


With an understanding of all of the ecosystem roles, it is next important to understand each of the phases in the bond lifecycle. Within each phase are a different set of rules and allowable actions that agents can take. There are also certain conditions that must be met in order to move from one phase to another.

In this document we will provide general descriptions of the phases in order to provide the full context of the activities that occur in the ecosystem, however the *Inputs* and *Next State Outputs* are only intended to define the specific system inputs and outputs that affect the bonding curve system state.

From a technical perspective, the bonding curve system can be described as a finite state machine, with multiple finite phases or states, inputs and outputs in and out of each state, and state transitions from one state to another in response to an input.

> Note: For simplicity and consistency with modelling and simulation explainers, this document adopts a pseudo-code or protocol-like language.

![bond lifecycle phases diagram](artifacts/BondLifecyclePhasesV1.png)

---

## Phase 0: Pre-initialization 
The pre-initialization phase is meant to represent all of the activities that occur prior to launch of the bond. This includes planning, setup, agreements, expectations, obligations, decisions around bond parameters, initial conditions, payment conditions, roles and rights of stakeholders, etc. During the pre-initialization phase, the bond is set up with an associated wallet and repository and all bond participants are identified with digital IDs, public keys, and credentials. All investments including funding as well as resources, materials, and equipment are agreed upon. At the end of this phase, the bond should be for all intents and purposes ready to launch.

### Transitions into Pre-initialization Phase
Since this is the first state of the bond lifecycle, there are no state transitions into this phase.

### Inputs
N/A

### Outputs to Next State
Below is a brief list of outputs passed from the Pre-initialization Phase to the Initialization Phase.

1. Agreed upon ProjectTime Threshold<a href="glossary.md#note29" id="note29ref"><sup>29</sup></a>
2. Agreed upon Alpha Threshold<a href="glossary.md#note33" id="note33ref"><sup>33</sup></a>
3. Agreed upon Reserve Funds<a href="glossary.md#note34" id="note34ref"><sup>34</sup></a> Raised
4. Agreed upon Project Operating Funds Raised<a href="glossary.md#note44" id="note44ref"><sup>44</sup></a>
5. Agreed upon Supply Tokens<a href="glossary.md#note35" id="note35ref"><sup>35</sup></a> to Mint
6. Agreed upon Maximum Reserve Ratio<a href="glossary.md#note38" id="note38ref"><sup>38</sup></a>
7. Agreed upon Minimum Reserve Ratio
8. Agreed upon Fund Fee<a href="glossary.md#note45" id="note45ref"><sup>45</sup></a>
9. Agreed upon Outcomes Payer's Commitment<a href="glossary.md#note40" id="note40ref"><sup>40</sup></a>

### Transitions out of Pre-initialization Phase
All planning and setup completed and with the bond ready to launch will move the system from the Pre-initialization Phase into the Initialization Phase.

<br>

---

## Phase 1: Initialization
During the Initialization Phase, the bond is launched. All of the parameters, roles and rights, thresholds, conditions, etc. are set. All funds as agreed upon are deposited into appropriate accounts. Funds are divided with a portion of funds going into the Project Operating Funds for use on the project and a portion going into the Reserve Funds to provide liquidity to the bonding curve. Initialization is essentially the instantiation of everything that was agreed upon in pre-initialization.

### Transitions into Initialization Phase
All planning and setup completed and with the bond Ready to Launch will move the system from the Pre-initialization Phase into the Initialization Phase.

### Inputs
1. Agreed upon ProjectTime Threshold
2. Agreed upon Alpha Threshold
3. Agreed upon Reserve Funds Raised
4. Agreed upon Project Operating Funds Raised
5. Agreed upon Supply Tokens to Mint
6. Agreed upon Maximum Reserve Ratio
7. Agreed upon Minimum Reserve Ratio
8. Agreed upon Fund Fee
9. Agreed upon Outcomes Payer's Commitment

### Outputs to Next State
1. ProjectTime Threshold
2. Alpha Threshold
3. Reserve Funds Raised
4. Project Operating Funds Raised
5. Supply Tokens to Mint
6. Maximum Reserve Ratio
7. Minimum Reserve Ratio
8. Fund Fee
9. Outcomes Payer's Commitment

### Transitions out of Initilization Phase
Initialization Threshold Triggering Event: The bond is launched and as long as funds are placed in the appropriate accounts as agreed upon, the funding threshold is met and the system will move from the Initialization Phase to the Execution Phase. If the funding threshold is not met, the system will move from the Initialization Phase to the Settlement Phase.

<br>

---
## Phase 2: Execution
The execution phase is the phase where active bond trading occurs resulting in updates to the reserve, supply, available project operating funds, and price. Claims and disputes are submitted as the project progresses. Alpha is continually updated to reflect the project's likelihood of success.

### Transitions into Execution Phase
1. Initialization Threshold Triggering Event from Initialization Phase (sets the initial state)
2. Dispute Resolution Return Event to Execution from Dispute Pause Phase
3. A Not Complete Event from the Settlement Consideration Pause Phase

### Initial State
All values are at time t = 0:
1. Reserve Funds raised
2. Project Operating Funds Raised
3. Supply Tokens Minted
4. Alpha
5. Reserve Ratio
6. Price

### Inputs
1. Reserve Funds to be Deposited
2. Supply Tokens to be Burned
3. Dispute Submission
4. Settlement Consideration Claim

### Next State Outputs
1. Amount of Supply Tokens Minted
2. Amount of Reserve Funds Withdrawn
3. Amount of Funds to Project
4. Price Update
5. Alpha Update
6. Reserve Ratio Update
7. Project Attestion Token Ratio

### Transitions out of Execution Phase
1. A Dispute Triggering Event will move from the Execution Phase into the Dispute Pause Phase
2. A Propose Completion Event will move from the Execution Phase into the Settlement Consideration Pause Phase

<br>

---
## Phase 3: Dispute Pause
In the Dispute Pause Phase, disputes issued against the bond will undergo external resolution. During this state, all system activity except those directly involved in dispute resolution is paused.

### Transitions into Dispute Pause Phase
1. A Dispute Triggering event moves the system from the Execution Phase to the Dispute Pause Phase

### Inputs
1. Dispute Submission 
2. Funding Threshold
3. Dispute External Resolution

### Next State Outputs
1. Dispute Verdict

### Transitions out of Dispute Pause Phase
If Dispute Verdict results in a Return to Execution, the system will move from the Dispute Pause Phase back to the Execution Phase. (Example: This is the most typical case where a dispute gets issued, Execution is paused, dispute gets resolved and then returns to Execution.)

If Dispute Verdict results in a Resolved to Completion, the system will move from the Dispute Pause Phase to the Settlement Consideration Pause Phase. (Example: Massive fraud, attack, etc. is discovered, the bond state is unrecoverable and thus cannot not return to Execution.)

<br>

---
## Phase 4: Settlement Consideration Pause 
In this phase, the bond is evaluated against the Settlement Conditions<a href="glossary.md#note42" id="note42ref"><sup>42</sup></a> for success or failure. Similar to the Dispute Pause Phase, all system activity other than settlement consideration activity is paused during this phase. A detailed view of the Settlement process is shown [here](artifacts/SettlementConsiderationPhase.png).

### Transitions into Settlement Consideration Pause Phase
1. A Propose Completion Event moves the system from the Execution Phase to the Settlement Consideration Pause Phase.
2. If Dispute Verdict results in a Resolved to Completion, the system moves from the Dispute Pause Phase to the Settlement Consideration Phase Phase.

### Inputs
1. Settlement Claim
2. Dispute Verdict
3. Settlement External Resolution

### Next State Outputs
1. Settlement Verdict

### Transitions out of Settlement Consideration Pause Phase
1. A Completion event moves the system from the Settlement Consideration Pause Phase to the Settlement Phase
2. A Not Complete event moves the system from the Settlement Consideration Pause Phase back into the Execution Phase.

<br>

---
## Phase 5: Settlement
During the Settlement Phase, bond tokens are converted into a compatible form and are distributed to the participants in the bond along with any outcomes payments based on the project's success.

### Transitions into Settlement Phase
1. A Completion event moves the system from the Settlement Consideration Pause Phase to the Settlement Phase
2. If the funding threshold is not met, the system will move from the Initialization Phase to the Settlement Phase.

### Inputs
1. Bond to External Currency Conversion
2. Payments to be Redeeemed
3. Outcomes Payees
4. Outcomes Payments 

### Next State Outputs
1. Bond Payment Distribution
2. Outcomes Payment Distribution

### Transitions out of Settlement Phase
This is the last phase of the system, thus there are no outward state transitions. 

<br>

## **Continue reading Chapter 3: [Engineering Requirements](3_EngineeringRequirements.ipynb)**
