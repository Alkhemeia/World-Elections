# World-Elections
A anonymous, transparent &amp; anti-fraud election system

### Setting your Action ID

The action ID (also called "external nullifier") makes sure that the proof your contract receives was generated for it. We recommend generating Action IDs on the [Developer Portal](https://developer.worldcoin.org) (more on [Action IDs](https://docs.worldcoin.org/id/anonymous-actions)).

> Note: Make sure you're passing the correct Action ID when initializing the JS widget! The generated proof will be invalid otherwise.

### Setting your signal

The signal adds an additional layer of protection to the World ID ZKP, it makes sure that the input provided to the contract is the one the person who generated the proof intended (more on [signals](https://docs.worldcoin.org/advanced/on-chain)). By default this contract expects an address (`receiver`), but you can update it to be any arbitrary string.

To update the signal, you should change the `input` on the `abi.encodePacked(input).hashToField()` line. You should provide the exact same string when initializing the JS widget, to make sure the proof includes them.

> Note: The `hashToField` part is really important, as validation will fail otherwise even with the right parameters. Make sure to include it!

### About nullifiers

_Nullifiers_ are what enforces uniqueness in World ID. You can generate multiple proofs for a given signal and action ID, but they will all have the same nullifier. Note how, in the `verifyAndExecute` function we first check if the given nullifier has already been used (and revert if so), then mark it as used after the proof is verified.

If your use-case doesn't require uniqueness, you can use them as "anonymous identifiers", linking users between different signals (for example, allowing them to change which address they've verified in a social network). To do this, update the `nullifierHashes` mapping to point to some sort of identifier instead of a boolean. See [this project](https://github.com/m1guelpf/lens-humancheck/blob/main/src/HumanCheck.sol) as an example.