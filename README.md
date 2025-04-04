# update-credentials-without-mnemonic

A number of individuals lost access to their mnemonic before it was possible to update validator withdrawal credentials. As a result, these individuals are now faced with a situation where their original deposit and consensus rewards are permanently locked. Even though there were ample warnings about the importance of the mnemonic, many agree that the users should not be considered at fault due to the complexity of the process.

This proposal suggests employing a mechanism similar to [CLWP](https://github.com/benjaminchodroff/ConsensusLayerWithdrawalProtection) to verify validator ownership through a signature from the deposit address and validator keystore. After creating a curated list and allowing an extended period for verification, the credentials could potentially be updated through a hard fork.

If you are one such individual please follow the instructions below to claim your validator with the desired withdrawal address.

If you have any questions or concerns, please reach out to use at the [EthStaker Discord](https://discord.com/invite/ethstaker).

*Note:* This is only a proposal and no assurances or guarantees are made. But we are dedicated to doing all we can to help those impacted by this issue.


## Step 1: Create Keystore signature

- Download the latest version of [ethstaker-deposit-cli](https://github.com/eth-educators/ethstaker-deposit-cli/tags), at least v0.1.3
- Follow instructions on how to run the [generate-bls-to-execution-change-keystore command](https://deposit-cli.ethstaker.cc/generate_bls_to_execution_change_keystore.html)
```
./deposit generate-bls-to-execution-change-keystore --keystore=PATH_TO_FILE
```
- This will output a `bls_to_execution_change_keystore_transaction-*-*.json` file in the `bls_to_execution_changes_keystore` directory
- You will need the `signature` value from the created `bls_to_execution_change_keystore_transaction-*-*.json` for step 3


## Step 2: Create Deposit signature

<div>
  <strong>⚠️ Alert:</strong> If you're unable to create a signature for your deposit address, for instance, if you used a centralized exchange, you'll need to undergo a manual proof of ownership process for that address. Please still submit a pull request with the keystore signature and you will be contacted for further steps
</div>

- Go to [Etherscan Verified Signatures](https://etherscan.io/verifiedSignatures)
- Click on `Sign Message`
- Make sure to select the account you originally deposited from
- Provide a version similar to `Signature message format` described below for the message. Please note that the file must not contain any spaces (or other formatting) as this will result in a different signature.
- You will need the `Signature Hash` value for step 3


## Step 3: Create signature json file

- After creating both signatures, create a json file using the validator index as the name such as `5426.json`
- Put this file in the `signatures` folder
- Review the `Signature file format` section below for the contents of the file
- Use the `signature` created in step 1 for the `keystore_signature`
- Use the returned `Signature Hash` from step 2 for the `deposit_signature`.
- Create a pull request after which the signatures will be automatically verified


### Keystore file format
```
{
  "message": {
    "to_execution_address": "${to_execution_address}",
    "validator_index": ${validator_index}
  },
  "signature": "${keystore_signature}"
}
```

As an example:

```
{
    "message": {
      "to_execution_address": "0xcd60a5f152724480c3a95e4ff4daceef4074854d",
      "validator_index": 1
    },
    "signature": "0xa1ec43ffefc87d8a55749d98bcac87f2cb8e2969fdf12131eb171ffd49b46046c1c7ca1e22d6da7074d6303780aa17cc0af875536170bda53029450e4ad21e16a3a9b69d540a1e35313d637010c1ab3b3e43511162721c78e7fb382d682ad622"
}
```


### Signature message format
<div>
  <strong>⚠️ Alert:</strong> Ensure that the message does not contain any spaces or other formatting like newlines
</div>

```
{"to_execution_address":"${to_execution_address}","validator_index":${validator_index}}
```

As an example:

```
{"to_execution_address":"0xcd60A5f152724480c3a95E4Ff4dacEEf4074854d","validator_index":1}
```


### Signature file format
```
{
  "to_execution_address": "${to_execution_address}",
  "validator_index": ${validator_index},
  "deposit_signature": "${deposit_signature}",
  "keystore_signature": "${keystore_signature}"
}
```

As an example:

```
{
  "to_execution_address": "0xcd60A5f152724480c3a95E4Ff4dacEEf4074854d",
  "validator_index": 1,
  "deposit_signature": "0x5c60eeb5e2f03c4e740c646f356e2940321e559e83339b6b344d9a40f8dbe520552dd51470dc30f805520bbb59a54dded024b6fdbde1ec7dcc08909e4233f5b41c",
  "keystore_signature": "0xa1ec43ffefc87d8a55749d98bcac87f2cb8e2969fdf12131eb171ffd49b46046c1c7ca1e22d6da7074d6303780aa17cc0af875536170bda53029450e4ad21e16a3a9b69d540a1e35313d637010c1ab3b3e43511162721c78e7fb382d682ad622"
}
```
