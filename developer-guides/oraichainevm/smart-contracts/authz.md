# Authz

Authz Precompiles contract is a gate-way that allowed Solidity contracts can interact with Cosmos SDK Authz module. By this, user now can grants permission to execute a specific message to differents address.

## Solidity Interfaces

The Authz solidity interfaces include the following transactions:

* grant

Grant method allow to register grant in Authz module

```
function grant(
        address granter,
        address grantee,
        string memory denom
    ) external view returns (uint256 amount);
```

* setGrant

SetGrant method allow to grant a message in Authz module

```
function setGrant(
        address grantee,
        string memory denom,
        uint256 amount
    ) external returns (bool success);
```

* execGrant

ExecGrant method allowed caller to execute grant message

```
function execGrant(
        address granter,
        address recipient,
        string memory denom,
        uint256 amount
    ) external returns (bool success);
```
