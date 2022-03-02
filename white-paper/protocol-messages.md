# Protocol Messages

### Cosmos SDK Messages[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#cosmos-sdk-messages)

Since the Oraichain blockchain is built from the Cosmos network, it also supports all message types of Cosmos SDK so that they can be delivered to all nodes for execution and querying. For simplicity, all message types follow the same protocol, which are described on the [Cosmos SDK documentation](https://docs.cosmos.network/master/building-modules/messages-and-queries.html)

### Oraichain Messages[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#oraichain-messages)

All Oraichain messages use the same protocol as other Cosmos SDK message types. To support validators running Oracle Scripts, AI Data Source, and AI Testcase, there are other message types implemented on Oraichain as follows:

#### Oraichain Specific Messages[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#oraichain-specific-messages)

Apart from the messages that stems from the Cosmos SDK, Oraichain also supports a number of messages native to its a ai data oracle system. These messages' specification is presented below.

**MsgCreateAIDataSource#**

Deploys and registers a new AI Data Source to Oraichain. Once registered, the Data Source is assigned a Data Source prefix with its unique name as an identifier which can be used to refer to it forever.

| Parameter   | Type             | Description                                                                          |
| ----------- | ---------------- | ------------------------------------------------------------------------------------ |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the ai Data Source |
| Name        | `string`         | The human-readable string name for this Data Source                                  |
| Description | `string`         | The description of this Data Source                                                  |
| Code        | `[]byte`         | The source code in bytes of this Data Source                                         |
| Fees        | `string`         | The transaction fee required to run this Data Source. Eg: 5000orai                   |

message type: set\_datasource

#### MsgEditAIDataSource[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgeditaidatasource)

Edits an existing Data Source given the unique identifier

| Parameter   | Type             | Description                                                                                                                                              |
| ----------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OldName     | `string`         | The current unique identifier that the Data Source has                                                                                                   |
| NewName     | `string`         | The new unique identifier and also the new name that the Data Source is assigned to. If it is identical to the OldName, then the identifier is unchanged |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the ai Data Source                                                                     |
| Description | `string`         | The description of this Data Source                                                                                                                      |
| Code        | `[]byte`         | The source code in bytes of this Data Source                                                                                                             |
| Fees        | `string`         | The transaction fee required to run this Data Source. Eg: 5000orai                                                                                       |

message type: edit\_datasource

#### QueryResAIDataSource[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresaidatasource)

When querying, the user only needs to type the name of the Data Source, and the system will return three attributes below in JSON format.

| Parameter   | Type             | Description                                                                          |
| ----------- | ---------------- | ------------------------------------------------------------------------------------ |
| Name        | `string`         | The human-readable string name for this Data Source                                  |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the ai Data Source |
| Code        | `string`         | The source code in string of this Data Source                                        |
| Description | `string`         | The description of this Data Source                                                  |

query type: datasource

#### QueryResAIDataSources[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresaidatasources)

A list of AI Data Sources with its size will be shown after querying.

| Parameter          | Type                     | Description                                                                   |
| ------------------ | ------------------------ | ----------------------------------------------------------------------------- |
| QueryResAIDSources | `[]QueryResAIDataSource` | The list of the AI Data Sources with attributes of the `QueryResAIDataSource` |
| Count              | `int`                    | The size of the Data Source list                                              |

query type: datasources

#### QueryResAIDataSourceNames[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresaidatasourcenames)

A list of AI Data Source unique identifiers will be shown.

| Parameter                 | Type       | Description                                          |
| ------------------------- | ---------- | ---------------------------------------------------- |
| QueryResAIDataSourceNames | `[]string` | The list of the AI Data Source identifiers in string |

query type: dnames

#### MsgCreateOracleScript[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgcreateoraclescript)

Deploys a new Oracle Script to Oraichain's network. Once registered, the script is assigned a unique Oracle Script prefix + name identifier which can be used to refer to it forever.

| Parameter   | Type             | Description                                                                         |
| ----------- | ---------------- | ----------------------------------------------------------------------------------- |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the Oracle Script |
| Name        | `string`         | The human-readable string name for this Data Source                                 |
| Description | `string`         | The description of this Oracle Script                                               |
| Code        | `[]byte`         | The source code in bytes of this Oracle Script                                      |
| DataSources | `[]string`       | The Data Source identifiers that this Oracle Script is going to use                 |
| TestCases   | `[]string`       | The Test Cases identifiers that this Oracle Script is going to use                  |

message type: set\_oscript

#### MsgEditOracleScript[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgeditoraclescript)

Edits an existing Oracle Script given the unique identifier which is the name. The sender must be the owner of the Oracle Script for the transaction to succeed.

| Parameter   | Type             | Description                                                                                                                                                |
| ----------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OldName     | `string`         | The current unique identifier that the Oracle Script has                                                                                                   |
| NewName     | `string`         | The new unique identifier and also the new name that the Oracle Script is assigned to. If it is identical to the OldName, then the identifier is unchanged |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the Oracle Script                                                                        |
| Description | `string`         | The description of this Oracle Script                                                                                                                      |
| Code        | `[]byte`         | The source code in bytes of this Oracle Script                                                                                                             |
| DataSources | `[]string`       | The Data Source identifiers that this Oracle Script is going to use                                                                                        |
| TestCases   | `[]string`       | The Test Cases identifiers that this Oracle Script is going to use                                                                                         |

message type: edit\_oscript

#### QueryResOracleScript[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresoraclescript)

When querying, the user only needs to type the name of the Oracle Script, and the system will return three attributes below in JSON format.

| Parameter   | Type             | Description                                                                         |
| ----------- | ---------------- | ----------------------------------------------------------------------------------- |
| Name        | `string`         | The human-readable string name for this Oracle Script                               |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the Oracle Script |
| Code        | `string`         | The source code in string of this Oracle Script                                     |
| Description | `string`         | The description of this Oracle Script                                               |
| MinimumFees | `sdk.Coins`      | The minimum fees required to run this Oracle Script                                 |
| DSources    | `[]string`       | The list of AI Data Source identifiers that this Oracle Script uses                 |
| TCases      | `[]string`       | The list of Test Case identifiers that this Oracle Script uses                      |

query type: oscript

#### QueryResOracleScripts[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresoraclescripts)

A list of Oracle Scripts with its size will be shown after querying.

| Parameter        | Type                     | Description                                                                  |
| ---------------- | ------------------------ | ---------------------------------------------------------------------------- |
| QueryResOScripts | `[]QueryResOracleScript` | The list of the Oracle Scripts with attributes of the `QueryResOracleScript` |
| Count            | `int`                    | The size of the Oracle Script list                                           |

query type: oscripts

#### QueryResOracleScriptNames[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresoraclescriptnames)

A list of AI Data Source unique identifiers will be shown.

| Parameter                 | Type       | Description                                         |
| ------------------------- | ---------- | --------------------------------------------------- |
| QueryResOracleScriptNames | `[]string` | The list of the Oracle Script identifiers in string |

query type: onames

#### MsgCreateTestCase[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgcreatetestcase)

| Parameter   | Type             | Description                                                                     |
| ----------- | ---------------- | ------------------------------------------------------------------------------- |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the Test Case |
| Name        | `string`         | The human-readable string name for this Test Case                               |
| Description | `string`         | The description of this Test Case                                               |
| Code        | `[]byte`         | The source code in bytes of this Test Case                                      |
| Fees        | `string`         | The transaction fee required to run this Test Case. Eg: 5000orai                |

message type: set\_test\_case

#### MsgEditTestCase[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgedittestcase)

| Parameter   | Type             | Description                                                                                                                                             |
| ----------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OldName     | `string`         | The current unique identifier that the Test Case has                                                                                                    |
| NewName     | `string`         | The new unique identifier and also the new name that the Test Case is assigned to. If it is identical to the OldName, then the identifier is unchanged. |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the ai Test Case                                                                      |
| Description | `string`         | The description of this Test Case                                                                                                                       |
| Code        | `[]byte`         | The source code in bytes of this Test Case                                                                                                              |
| Fees        | `string`         | The transaction fee required to run this Test Case. Eg: 5000orai                                                                                        |

message type: edit\_test\_case

#### QueryResTestCase[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryrestestcase)

When querying, the user only needs to type the name of the Data Source, and the system will return three attributes below in JSON format.

| Parameter   | Type             | Description                                                                     |
| ----------- | ---------------- | ------------------------------------------------------------------------------- |
| Name        | `string`         | The human-readable string name for this Test Case                               |
| Owner       | `sdk.AccAddress` | The address of the entity who will be responsible for maintaining the Test Case |
| Code        | `string`         | The source code in string of this Test Case                                     |
| Description | `string`         | The description of this Test Case                                               |

query type: testcase

#### QueryResTestCases[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryrestestcases)

A list of Test Cases with its size will be shown after querying.

| Parameter         | Type                 | Description                                                          |
| ----------------- | -------------------- | -------------------------------------------------------------------- |
| QueryResTestCases | `[]QueryResTestCase` | The list of the Test Cases with attributes of the `QueryResTestCase` |
| Count             | `int`                | The size of the Test Case list                                       |

query type: testcases

#### QueryResTestCaseNames[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryrestestcasenames)

A list of Test Case unique identifiers will be shown.

| Parameter             | Type       | Description                                     |
| --------------------- | ---------- | ----------------------------------------------- |
| QueryResTestCaseNames | `[]string` | The list of the Test Case identifiers in string |

query type: tcnames

#### MsgSetAIRequest[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgsetairequest)

Requests a new data based on an existing Oracle Script. A data request will be assigned a unique identifier with an AI request prefix once the transaction is confirmed. After sufficient validators report successfully. The results of the data requests will be written and stored permanently on Oraichain for future uses.

| Parameter        | Type              | Description                                                                                                                                                                                                                                |
| ---------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RequestID        | `string`          | the unique identifier of this oracle request, as specified by the client This same unique ID will be sent back to the requester with the oracle response. This field is filled automatically, so the user does not need to care about this |
| OracleScriptName | `string`          | The unique name identifier of the Oracle Script                                                                                                                                                                                            |
| Creator          | `sdk.AccAddress`  | The address of the message's sender or creator of this request                                                                                                                                                                             |
| ValidatorCount   | `int64`           | The number of validators that are needed to execute the request. The current default value is 1 for testing                                                                                                                                |
| Fees             | `string`          | The transaction fee required to run this AI Request. Eg: 5000orai                                                                                                                                                                          |
| Input            | `json.RawMessage` | User's input for the AI Request, can be anything                                                                                                                                                                                           |
| ExpectedOutput   | `json.RawMessage` | User's expected output for the AI Request, can be anything                                                                                                                                                                                 |

message type: set\_ai\_request

#### QueryResAIRequest[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresairequest)

When querying, the user only needs to type the id of the AI request, and the system will return a few attributes below in JSON format.

| Parameter        | Type               | Description                                                                                                                                                                                                                                |
| ---------------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| RequestID        | `string`           | the unique identifier of this oracle request, as specified by the client This same unique ID will be sent back to the requester with the oracle response. This field is filled automatically, so the user does not need to care about this |
| OracleScriptName | `string`           | The unique name identifier of the Oracle Script                                                                                                                                                                                            |
| Creator          | `sdk.AccAddress`   | The address of the message's sender or creator of this request                                                                                                                                                                             |
| Validators       | `[]sdk.ValAddress` | The list validator addresses that participate in the request                                                                                                                                                                               |
| BlockHeight      | `int64`            | The block height that contains the transaction creating the AI request                                                                                                                                                                     |
| AIDataSources    | `[]AIDataSource`   | The list of Data Sources that will be called                                                                                                                                                                                               |
| TestCases        | `[]TestCase`       | The list of Test Cases that will be called                                                                                                                                                                                                 |
| Fees             | `string`           | The transaction fee required to run this request. Eg: 20000orai                                                                                                                                                                            |

query type: aireq

#### QueryResAIRequestIDs[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresairequestids)

A list of AI Request unique identifiers will be shown.

| Parameter            | Type       | Description                                      |
| -------------------- | ---------- | ------------------------------------------------ |
| QueryResAIRequestIDs | `[]string` | The list of the AI Request identifiers in string |

query type: aireqs

#### QueryResFullRequest[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#queryresfullrequest)

When querying, the user only needs to type the id of the AI request, and the system will return a few attributes below in JSON format.

| Parameter | Type              | Description                                                                                                                                                             |
| --------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AIRequest | `AIRequest`       | The AIRequest object that is stored on Oraichain which shows all the information about an AI request                                                                    |
| Reports   | `[]Report`        | The list of reports created by the validators after executing the Test Cases and Data Sources                                                                           |
| Result    | `AIRequestResult` | A wrapper attribute for the request results. Behind the scenes, it includes a request ID, status of the request result as well as a list of results from the validators |

query type: fullreq

#### MsgAddReporter[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgaddreporter)

MsgAddReporter is a message for adding a new reporter for a validator. Each validator needs to have a reporter to send report transactions to the Oraichain network.

| Parameter | Type              | Description                                                         |
| --------- | ----------------- | ------------------------------------------------------------------- |
| Validator | `sdk.ValAddress`  | The validator that wishes to add a new reporter. This is the signer |
| Reporter  | `sdk.AccAddress`  | The address to be added as a reporter to the validator. Sources     |
| Adder     | `AIRequestResult` | The address responsible for adding the reporter                     |

message type: add\_reporter

#### MsgRemoveReporter[#](https://docs.orai.io/docs/WhitePaper/ProtocolMessages#msgremovereporter)

MsgRemoveReporter is a message for removing an existing validator's reporter.

| Parameter | Type              | Description                                                         |
| --------- | ----------------- | ------------------------------------------------------------------- |
| Validator | `sdk.ValAddress`  | The validator that wishes to add a new reporter. This is the signer |
| Reporter  | `sdk.AccAddress`  | The address to be added as a reporter to the validator. Sources     |
| Remover   | `AIRequestResult` | The address responsible for removing the reporter                   |

message type: remove\_reporter
