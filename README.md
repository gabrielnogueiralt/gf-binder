# ERC20 Grammar Documentation

This documentation aims to demonstrate the functions, parameters, returns, possible words and phrases accepted by the grammar that aims to assist smart contract developers in creating **post conditions** for ERC20 token functions.
These post condition notations become very useful for testing the functions, passing through the **solc-verify** framework, link to the framework: https://github.com/SRI-CSL/solidity/blob/0.7/SOLC-VERIFY-README.md

# gf-binder

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gabrielnogueiralt/gf-binder/master)

## Playing with GF notebooks

Just press the icon above! Note that your changes or any new notebooks you create _will not be persistent_. That is, if you close your tab and go back later, all changes will be lost!

# Grammars

Initially, three grammars were created, one abstract (ERC20) and two concrete, one supporting entries in the English language (ERC20Eng) and the other for formal language (ERC20Formal).
To access the grammars, access the notebook at the following path: notebooks/ERC20Grammar.ipynb

# Supported Functions

- **totalSupply**
  - function signature: function totalSupply() external view returns (uint256);
  - post condition:
    - ERC20Eng: "the resulting total supply of tokens should be equal to the total supply of tokens"
    - ERC20Formal: "supply == \_totalSupply"
- **balanceOf**
  - function signature: function balanceOf(address who) external view returns (uint256);
  - post condition:
    - ERC20Eng: "the resulting balance of the specified address should be equal to the balance of the specified address"
    - ERC20Formal: "\_balances[owner] == balance"
- **allowance**
  - function signature: function allowance(address owner, address spender) external view returns (uint256);
  - post condition:
    - ERC20Eng: "the resulting allowance of the specified address should be equal to the allowance of the specified address"
    - ERC20Formal: "\_allowed[owner][spender] == remaining"
- **transfer**
  - function signature: function transfer(address to, uint256 value) external returns (bool);
  - post conditions:
    - ERC20Eng: "the sender new balance (msg.sender) should be equal to their old balance (msg.sender) minus the transferred value if the sender address (msg.sender) is not equal to the recipient address or the sender new balance (msg.sender) should be equal to their old balance (msg.sender) if the sender address (msg.sender) is equal to the recipient address and the transfer is successful or the transfer is not successful"
    - ERC20Formal: "( ( \_balances[msg.sender] == **verifier_old_uint (\_balances[msg.sender] ) - value && msg.sender != to ) || ( \_balances[msg.sender] == **verifier_old_uint ( \_balances[msg.sender]) && msg.sender == to ) && success ) || !success"
    - ERC20Eng: "the recipient new balance should be equal to their old balance (msg.sender) plus the transferred value if the sender address (msg.sender) is not equal to the recipient address or the recipient new balance should be equal to their old balance (msg.sender) if the sender address (msg.sender) is equal to the recipient address and the transfer is successful or the transfer is not successful"
    - ERC20Formal: "( ( \_balances[to] == **verifier_old_uint ( \_balances[to] ) + value && msg.sender != to ) || ( \_balances[to] == **verifier_old_uint ( \_balances[to] ) && msg.sender == to ) && success ) || !success"
- **approve**
  - function signature: function approve(address spender, uint256 value) external returns (bool);
  - post condition:
    - ERC20Eng: "the spender allowance (msg.sender)(spender) should be equal to the specified value and the operation is successful or the spender allowance (msg.sender)(spender) should be equal to the previous value if the operation is not successful"
    - ERC20Formal: "(\_allowed[msg.sender][spender] == value && success) || ( \_allowed[msg.sender ][ spender] == \_\_verifier_old_uint ( \_allowed[msg.sender ][ spender] ) && !success )"
- **transferFrom**
  - function signature: function transferFrom(address from, address to, uint256 value) external returns (bool);
  - post conditions:
    - ERC20Eng: "the sender new balance (from) should be equal to their old balance (from) minus the transferred value if the sender address (from) is not equal to the recipient address or the sender new balance (from) should be equal to their old balance (from) if the sender address (from) is equal to the recipient address and the transfer is successful or the transfer is not successful"
    - ERC20Formal: "( (\_balances[from] == **verifier_old_uint (\_balances[from] ) - value && from != to) || (\_balances[from] == **verifier_old_uint (\_balances[from] ) && from == to ) && success ) || !success"
    - ERC20Eng: "the recipient new balance should be equal to their old balance (to) plus the transferred value if the sender address (from) is not equal to the recipient address or the recipient new balance should be equal to their old balance (from) if the sender address (from) is equal to the recipient address and the transfer is successful or the transfer is not successful"
    - ERC20Formal: "( ( \_balances[to] == **verifier_old_uint( \_balances[to] ) + value && from != to ) || (\_balances[to] == **verifier_old_uint( \_balances[to] ) && from == to ) && success ) || !success"
    - ERC20Eng: "the spender allowance (from)(msg.sender) should be equal to the previous value (from)(msg.sender) minus the transferred value and the operation is successful or the spender allowance (from)(msg.sender) should be equal to the previous value (from)(msg.sender) and the operation is not successful or the spender allowance address (from) is equal to the sender address (msg.sender)"
    - ERC20Formal: "(\_allowed[from][msg.sender] == **verifier_old_uint (\_allowed[from][msg.sender] ) - value && success) || (\_allowed[from][msg.sender] == **verifier_old_uint (\_allowed[from][msg.sender] ) && !success) || from == msg.sender"
    - ERC20Eng: "the spender allowance (from)(msg.sender) should be less than or equal to the previous value (from)(msg.sender) or the spender allowance (from) is not equal to the sender address (msg.sender)"
    - ERC20Formal: "\_allowed[from][msg.sender] <= \_\_verifier_old_uint (\_allowed[from][msg.sender] ) || from == msg.sender"

## Supported Tokens

Here is the catalog of possible articles, verbs, boolean operators, variables, and arithmetic operators:

|                      | tokens                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Articles             | `"the"` `"their"`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Verbs                | `"should be"` `"must be"` `"is"`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Boolean operators    | Equals: `"should be equal to"` `"must be equal to"` `"is equal to"` `"should be the same as"` `"must be the same as"` Not Equals: `"is not equal to"` `"should be different from"` `"must be different from"` `"must not be equal to"` `"should not be equal to"` `"should not be the same as"` `"must not be the same as"` Less Than: `"is less than"` `"should be less than"` `"must be less than"` Greater Than: `"is greater than"` `"should be greater than"` `"must be greater than"` Less Than Or Equals: `"is less than or equal to"` `"should be less than or equal to"` `"must be less than or equal to"` Greater Than Or Equals: `"is greater than or equal to"` `"should be greater than or equal to"` `"must be greater than or equal to"` Others: `"and"` `"if"` `"or"` |
| Arithmetic Operators | `"plus"` `"minus"`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| Variables            | `"resulting total supply of tokens"` `"total supply of tokens"` `"resulting balance of the specified address"` `"balance of the specified address"` `"resulting allowance of the specified address"` `"allowance of the specified address"` `"sender address (msg.sender)"` `"sender address (from)"` `"recipient address"` `"sender new balance (msg.sender)"` `"sender new balance (from)"` `"recipient new balance"` `"old balance (msg.sender)"` `"old balance (from)"` `"old balance (to)"` `"transferred value"` `"spender allowance (msg.sender)(spender)"` `"spender allowance (from)"` `"spender allowance (from)(msg.sender)"` `"spender allowance address (from)"` `"specified value"` `"previous value"` `"previous value (from)(msg.sender)"`                            |

## Making your own shared notebooks

1. Make a fork of this repo
2. Clone your forked repo to your local machine
3. Install docker if you have not already (https://docs.docker.com/engine/install/)
4. Run the script `./run_locally.sh`
5. Copy and paste the link that the script outputs into your browser (it will be something like: http://127.0.0.1:8888/?token=XXXXXX)
6. Create any notebooks you want to share in the `notebooks/` folder (only stuff created in that folder will be mirrored back to your git repository, all other folders "live" only inside your docker container.)
7. Commit any notebooks you created to the git repository and push to GitHub.
8. Go to https://mybinder.org/ and enter the link to your forked GitHub repository.
9. (Optional) Change this readme and the link on the top logo badge so people can go directly to your notebooks from your GitHub repo.

## GF shell command reference

The examples are from the grammar in `notebooks/Example.ipynb`.

### parse

Short form `p`. Example:  
`p -lang=Eng "John loves Mary"`

### linearize

Short form `l`. Examples:  
`l love john mary`  
Linearizes the tree `love john mary` to all languages.

`p -lang=Eng "John loves Mary" | l`  
Parses the English sentence "John loves Mary", linearizes to all languages.

`p -lang=Eng "John loves Mary" | l -lang=Ger`  
Parses the English sentence "John loves Mary", linearizes to German.

`p -lang=Eng "John loves Mary" | l -treebank`  
Parses the English sentence "John loves Mary", linearizes to all languages in a _treebank_ format:

```haskell
Example: love john mary
ExampleEng: "John loves Mary"
ExampleGer: "Johann liebt Maria"
```

### visualize_tree

Short form `vt`. Example:  
`p -lang=Eng "John loves Mary" | vt`

Visualises the abstract syntax tree for "John loves Mary". In the Jupyter notebook, you need to prefix this command with **view**:

**view** `p -lang=Eng "John loves Mary" | vt`

### visualize_parse

Short form `vp`. Example:  
`p -lang=Eng "John loves Mary" | vp`

Visualises the English parse tree for "John loves Mary". In the Jupyter notebook, you need to prefix this command with **view**:

**view** `p -lang=Eng "John loves Mary" | vt`

### generate_random

Short form `gr`. Generates a random tree. Examples:

`gr -number=5 | l`  
Generates 5 random trees (of start category), linearizes to all languages.

`gr -cat=NP`  
Geneerates a random tree of category NP.

### generate_trees

Short form `gt`. Generates all trees of the grammar (up to certain depth).

Examples:

`gt -cat=NP | l`  
Generates all trees of type NP, linearizes them in all languages.

`gt | l -lang=Eng`  
Generates all trees (of start category), linearizes them in English.
