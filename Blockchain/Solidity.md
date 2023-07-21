# Solidity
    This document is intended to give a clear cut view on best practices that needs to be followed during Solidity programming.
    
## Table of Contents:

* [Pragma Version](#pragma-version)
* [Import Files](#import-files)
* [Storage,Memory,CallData](#storage-memory-calldata)
* [Types](#types)
  + [Boolean](#boolean)
  + [Integer](#integer)
  + [String](#string)
  + [Address](#address)
    - [balance](#balance)
    - [transfer and send](#transfer-and-send)
    - [call, delegatecall](#call)
  + [Array](#array)
  + [Fixed byte arrays](#fixed-byte-arrays)
  + [Dynamic byte arrays](#dynamic-byte-arrays)
  + [Enum](#enum)
  + [Struct](#struct)
  + [Mapping](#mapping)
  + [Control Structures](#control-structures)
* [Functions](#functions)
  + [Structure](#structure)
  + [Access modifiers](#access-modifiers)
  + [Parameters](#parameters)
    - [Input parameters](#input-parameters)
    - [Output parameters](#output-parameters)
  + [Constructor](#constructor)
  + [Function Calls](#function-calls)
    - [Internal Function Calls](#internal-function-calls)
    - [External Function Calls](#external-function-calls)
    - [Named Calls](#named-calls)
    - [Unnamed function parameters](#unnamed-function-parameters)
  + [Function type](#function-type)
  + [Function Modifier](#function-modifier)
  + [View or Constant Functions](#view-or-constant-functions)
  + [Pure Functions](#pure-functions)
  + [Payable Functions](#payable-functions)
  + [Fallback Function](#fallback-function)
* [Contracts](#contracts)
  + [Creating contracts using `new`](#creating-contracts-using--new-)
  + [Contract Inheritance](#contract-inheritance)
    - [Multiple inheritance](#multiple-inheritance)
    - [Constructor of base class](#constructor-of-base-class)
  + [Abstract Contracts](#abstract-contracts)
* [Interface](#interface)
* [Events](#events)
* [Library](#library)
* [Using - For](#using---for)
* [Error Handling](#error-handling)
* [Global variables](#global-variables)
  + [Block variables](#block-variables)
  + [Transaction variables](#transaction-variables)
  + [Mathematical and Cryptographic Functions](#mathematical-and-cryptographic-functions)
  + [Contract Related](#contract-related)
* [Security Tips](#security-tips) 
  + [External Calls](#external-calls)
  + [Using access modifiers](#using-access-modifiers)
  + [Make use of function mofidiers](#using-function-modifiers)
  + [Using upgradable contracts via Proxy](#using-proxy-contracts)
* [Final Tips](#final-tips)

## Pragma Version

Using ```pragma solidity ^0.8.0``` will compile version >=0.8.0 and <0.9.0
So instead of mentioning ```pragma solidity >=0.8.0 <0.9.0``` we can simply use ```pragma solidity ^0.8.0```

Also ^0.8.0 - versions are more suitable for compiling.
^0.8.9 - preferred version for compiling, for higher version other than this solidity official documentation study is required.

## Import Files

```import "filename";``` directly imports the file.
```import * as symbolName from "filename";``` or ```import "filename" as symbolName;``` importing a file with a new name assigned.
```import {symbol1 as alias, symbol2} from "filename";``` This method allows you to import specific symbols (functions, variables, etc.) from the file and assign them to a variable with a custom name (alias)

## Storage, Memory, Calldata

- Storage
Storage is persistent and permanent storage of Ethereum and ethereum based blockchains.
State variables are declared outside of functions and are stored in the contract's storage.
Changes to state variables are expensive in terms of gas because they require writing data to the blockchain.

- Memory
Memory is temporary and primarily used for storing function arguments and local variables within function execution.
Data in memory is not persisted across different function calls and is cleared after the function execution completes

- Calldata
calldata is a special data location that contains the function arguments and data sent to the contract when a function is called.
It is a read-only area where the contract can access the input data, but it cannot modify it.
calldata is used when interacting with external contracts, especially when making external function calls.

eg:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CombinedExample {
    uint256 public myNumber; // State variable stored in the contract's storage

    function setNumber(uint256 newValue) public {
        myNumber = newValue; // This writes the new value to the contract's storage.
    }

    function getNumber() public view returns (uint256) {
        return myNumber; // This reads the value from the contract's storage.
    }

    function addNumbers(uint256 a, uint256 b) public pure returns (uint256) {
        uint256 result = a + b; // Variables "a", "b", and "result" are stored in memory during function execution.
        return result; // The result is returned without storing it in the contract's storage.
    }

    function myFunction(uint256 a, string calldata b) external {
        // Function arguments "a" and "b" are stored in calldata.
        // Calldata is used to access the function arguments within the function.
    }
}
```


## Types

### Boolean
      
```true``` or ```false``` are the two values for a boolean datatype.
Operators: ```!``` (logical negation), ```&&``` (AND) ```||``` (OR)
Comparisons: ```==``` (equality) , ```!=``` (inequality)

### Integer

Unsigned : `uint8 | uint16 | uint32 | uint64 | uint128 | uint256(uint)`
Signed   : `int8  | int16  | int32  | int64  | int128  | int256(int) `

Operators:

- Comparisons: `<=`, `<`, `==`, `!=`, `>=` and `>`
- Bit operators: `&`, `|`, `^` (bitwise exclusive or) and `~` (bitwise negation)
- Arithmetic operators: `+`, `-`, unary `-`, unary `+`, `*`, `/`, `%`, `**` (exponentiation), `<<` (left shift) and `>>` (right shift)  

NOTE: ```uint``` and ```uint256``` both denote the same. So use ```uint``` or ```int``` if you are intending to use 256 bits integer. Only specify the bit explicity if you are not using 256.

### String

- string datatype is used as ```string userName = "John Wick"```

- When logical comparision is done between strings ```keccak256(abi.encode(str))``` will make the comparision easy. In solidity strings cannot be compared directly like we do in Javascript using ==.

eg:
```solidity
contract CompareStrings{

function compareStrings(string _str1, string _str2) public returns (bool){
    if(keccak256(abi.encode(_str1)) == keccak256(abi.encode(_str2)) )
        return true;
    else
        return false;
}
}
```

### Address

`address`: Holds an Ethereum address (20 byte value).
`address payable` : Same as address, but includes additional methods `transfer` and `send`

Operators:

- Comparisons: `<=`, `<`, `==`, `!=`, `>=` and `>`

Methods:

#### balance

- `<address>.balance (uint256)`: balance of the Address in Wei format (1*((10)**18))

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract BalanceChecker {
    function getBalance(address account) public view returns (uint) {
        return account.balance;
    }
}
```

The above example contract provides a function that accepts an address as a parameter and returns uint256 value. The line
```account.balance``` will give the balance of the native token of the Blockchain held in the address. (eg: ETH balance if the contract is in ethereum network). The result will be returned in WEI format. To convert WEI to actual number we must multiply 
it with the result with 10**18.

#### transfer and send

- `<address>.transfer(uint256 amount)`: send given amount of Wei to Address, throws on failure
- `<address>.send(uint256 amount) returns (bool)`: send given amount of Wei to Address, returns false on failure

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract EtherTransferExample {
    function sendEther(address payable receiver, uint256 amount) public {
        // Using transfer function
        receiver.transfer(amount);
    }

    function sendEtherWithStatus(address payable receiver, uint256 amount) public returns (bool) {
        // Using send function and returning the status
        return receiver.send(amount);
    }
}
```

Both transfer and send functions are used to send currency from one address to another. The difference is send will return boolean
```true``` or ```false``` based on transaction success or failure.

NOTE: send is useful when you want to check the status of the transfer and handle failures explicitly.

However, in most cases, it's recommended to use transfer as it provides a safer way to handle Ether transfers by automatically reverting the transaction on failure.

#### call, delegatecall

- `<address>.call(...) returns (bool)`: issue low-level CALL, returns false on failure

    When using call, the called contract's state variables remain unchanged, and the called contract cannot access the calling contract's storage or modify its state.

    NOTE: There are some security considerations while using call such as the risk of reentrancy attacks, so careful coding practices are required to ensure the safety of the contract.

- `<address>.delegatecall(...) returns (bool)`: issue low-level DELEGATECALL, returns false on failure

    When a contract calls another contract with delegatecall, the called contract's code is executed in the context of the calling contract.

    This means that the called contract can access and modify the calling contract's storage and state variables.
    This feature is often used in proxy contracts and contract upgradeability patterns.
    Caution is needed when using delegatecall as it can lead to unintended consequences if not carefully managed, and it also introduces potential security risks

```solidity
contract A {
  uint value;
  address public sender;
  address a = address(0); // address of contract B
  function makeDelegateCall(uint _value) public {
    a.delegatecall(
        abi.encodePacked(bytes4(keccak256("setValue(uint)")), _value)
    ); // Value of A is modified
  }
}

contract B {
  uint value;
  address public sender;
  function setValue(uint _value) public {
    value = _value;
    sender = msg.sender; // msg.sender is preserved in delegatecall. It was not available in callcode.
  }
}
```

> gas() option is available for call, callcode and delegatecall. value() option is not supported for delegatecall.

NOTE: callcode is an deprecated method since solidity compiler 0.5.0.

### Array

Arrays can be dynamic or have a fixed size.

```solidity
uint[] dynamicSizeArray; // an uint256 array with no fixed size

uint[7] fixedSizeArray; // an uint256 array with size 7
```

### Fixed byte arrays

`bytes1(byte)`, `bytes2`, `bytes3`, ..., `bytes32`.

Operators:

Comparisons: `<=`, `<`, `==`, `!=`, `>=`, `>` (evaluate to bool)
Bit operators: `&`, `|`, `^` (bitwise exclusive or), `~` (bitwise negation), `<<` (left shift), `>>` (right shift)
Index access: If x is of type bytesI, then x[k] for 0 <= k < I returns the k th byte (read-only).

Members

- `.length` : read-only (```arrayVariableName.length``` will return the size of the array)

### Dynamic byte arrays

`bytes`: Dynamically-sized `byte` array. It is similar to `byte[]`, but it is packed tightly in calldata. Not a value-type!

`string`: Dynamically-sized UTF-8-encoded string. It is equal to `bytes` but does not allow length or index access. Not a value-type!

### Enum

Enum works just like in every other language.

```solidity
enum ActionChoices { 
  GoLeft, 
  GoRight, 
  GoStraight, 
  SitStill 
}

ActionChoices choice = ActionChoices.GoStraight;
```

### Struct

New types can be declared using struct.

Use PascalCase: Start the name of the struct with an uppercase letter, and use uppercase for the first letter of each word in the name. For example: MyStruct, UserInfo, TokenData
Instead of declaring variables singularly use struct to control large data in structured manner.


```solidity
struct UserInfo {
    address user_wallet_address;
    uint age;
    string username;
    mapping(string=>bool) taskNameAndIsCompletedStatus;
}

Funder funders;
```

As shown in above example we can use struct datatype to store large data. Even array and mappings can be stored inside a struct.

### Mapping

Declared as `mapping(_KeyType => _ValueType)`

Mappings can be seen as **hash tables** which are virtually initialized such that every possible key exists and is mapped to a value.

**key** can be almost any type except for a mapping, a dynamically sized array, a contract, an enum, or a struct. **value** can actually be any type, including mappings.

```solidity
mapping(string=>uint) nameToage; // a mapping to map every person name with their age.
mapping(string=>(mapping(string=>uint))) nameToMailIDToPasscode; // a nested mapping. Person name is key to a mapping which has emailID as a key to the passcode.
```

NOTE: It is best to use mappings instead of Array as mapping is more gas effective.

eg: 

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract PersonInfo {
    mapping(string => uint) public nameToAge;
    mapping(string => mapping(string => uint)) public nameToMailIDToPasscode;

    function addPerson(string memory name, uint age) public {
        nameToAge[name] = age;
    }

    function addEmailID(string memory name, string memory emailID, uint passcode) public {
        nameToMailIDToPasscode[name][emailID] = passcode;
    }

    function getAge(string memory name) public view returns (uint) {
        return nameToAge[name];
    }

    function getPasscode(string memory name, string memory emailID) public view returns (uint) {
        return nameToMailIDToPasscode[name][emailID];
    }
}
```

## Control Structures

Most of the control structures from JavaScript are available in Solidity except for `switch` and `goto`. 

- `if` `else`
- `while`
- `do`
- `for`
- `break`
- `continue`
- `return`
- `? :`

## Functions

 NOTE: Keep the function clean as possible.
	   keep the calculation as seperate "internal" function whereas use this as callback to the main function.
	   keep important logic as internal function, so @dev can verify and check what is happening under the hood.

### Structure

`function (<parameter types>) {internal|external|public|private} [pure|constant|view|payable] [returns (<return types>)]`

### Access modifiers

- ```public``` - Accessible from this contract, inherited contracts and externally
- ```private``` - Accessible only from this contract
- ```internal``` - Accessible only from this contract and contracts inheriting from it
- ```external``` - Cannot be accessed internally, only externally. Recommended to reduce gas. Access internally with `this.f`.

### Parameters

#### Input parameters

Parameters are declared just like variables and are `memory` variables.

```solidity
function f(uint _a, uint _b) {}
```

#### Output parameters

Output parameters are declared after the `returns` keyword

```solidity
function f(uint _a, uint _b) returns (uint _sum) {
   _sum = _a + _b;
}
```

Output can also be specified using `return` statement. In that case, we can omit parameter name `returns (uint)`.

Multiple return types are possible with `return (v0, v1, ..., vn)`.

### Constructor

Function that is executed during contract deployment. Defined using the `constructor` keyword. 
The logic within constructor is executed only once which is when the contract is deployed. The constructor parameter must be
passed via the contract deployment script (Refer Hardhat documentation) while deploying a contract.

```solidity
contract C {
   address owner;
   uint status;
   constructor(uint _status) {
       owner = msg.sender;
       status = _status;
   }
}
```

### Function Calls

#### Internal Function Calls

Functions of the current contract can be called directly (internally - via jumps) and also recursively

```solidity
contract C {
    function funA() returns (uint) { 
       return 5; 
    }
    
    function FunB(uint _a) returns (uint ret) { 
       return funA() + _a; 
    }
}
```

#### External Function Calls

`this.g(8);` and `c.g(2);` (where c is a contract instance) are also valid function calls, but, the function will be called “externally”, via a message call.

> `.gas()` and `.value()` can also be used with external function calls.

#### Named Calls

Function call arguments can also be given by name in any order as below.

```solidity
function f(uint a, uint b) {  }

function g() {
    f({b: 1, a: 2});
}
```

#### Unnamed function parameters

Parameters will be present on the stack, but are not accessible.

```solidity
function f(uint a, uint) returns (uint) {
    return a;
}
```

### Function type

Pass function as a parameter to another function. Similar to `callbacks` and `delegates`

```solidity
pragma solidity ^0.4.18;

contract Oracle {
  struct Request {
    bytes data;
    function(bytes memory) external callback;
  }
  Request[] requests;
  event NewRequest(uint);
  function query(bytes data, function(bytes memory) external callback) {
    requests.push(Request(data, callback));
    NewRequest(requests.length - 1);
  }
  function reply(uint requestID, bytes response) {
    // Here goes the check that the reply comes from a trusted source
    requests[requestID].callback(response);
  }
}

contract OracleUser {
  Oracle constant oracle = Oracle(0x1234567); // known contract
  function buySomething() {
    oracle.query("USD", this.oracleResponse);
  }
  function oracleResponse(bytes response) {
    require(msg.sender == address(oracle));
  }
}
```
### Function Modifier

Modifiers can automatically check a condition prior to executing the function.

```solidity
modifier onlyOwner {
    require(msg.sender == owner);
    _;
}

function close() onlyOwner {
    selfdestruct(owner);
}
```

### View or Constant Functions

Functions can be declared `view` or `constant` in which case they promise not to modify the state, but can read from them.

```solidity
function f(uint a) view returns (uint) {
    return a * b; // where b is a storage variable
}
```

> The compiler does not enforce yet that a `view` method is not modifying state.

### Pure Functions

Functions can be declared `pure` in which case they promise not to read from or modify the state.

```solidity
function f(uint a) pure returns (uint) {
    return a * 42;
}
```

### Payable Functions

Functions that receive `Ether` are marked as `payable` function.

### Fallback Function

A contract can have exactly one **unnamed function**. This function cannot have arguments and cannot return anything. It is executed on a call to the contract if none of the other functions match the given function identifier (or if no data was supplied at all).

```solidity
function() {
  // Do something
}
```

## Contracts

### Creating contracts using `new`

Contracts can be created from another contract using `new` keyword. The source of the contract has to be known in advance.

```solidity
contract A {
    function add(uint _a, uint _b) returns (uint) {
        return _a + _b;
    }
}

contract C {
    address a;
    function f(uint _a) {
        a = new A();
    }
}
```

### Contract Inheritance

Solidity supports multiple inheritance and polymorphism.

```solidity
contract owned {
    function owned() { owner = msg.sender; }
    address owner;
}

contract mortal is owned {
    function kill() {
        if (msg.sender == owner) selfdestruct(owner);
    }
}

contract final is mortal {
    function kill() { 
        super.kill(); // Calls kill() of mortal.
    }
}
```

#### Multiple inheritance

```solidity
contract A {}
contract B {}
contract C is A, B {}
```

#### Constructor of base class

```solidity
contract A {
    uint a;
    constructor(uint _a) { a = _a; }
}

contract B is A(1) {
    constructor(uint _b) A(_b) {
    }
}
```

### Abstract Contracts

- One of the most preferred method to use within contract to perfrom active functions rather than importing the whole contract.
- An abstract contract is a contract that cannot be instantiated directly. It serves as a blueprint for other contracts and defines the structure and interface that derived contracts must implement.
- Abstract contracts contain function declarations without any implementation.

example abstract contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

abstract contract PaymentProcessor {
    function pay(uint amount) external virtual;
    function getBalance() external view virtual returns (uint);
}
```

example contract that uses the abstract contract:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyPaymentProcessor is PaymentProcessor {
    uint private balance;

    function pay(uint amount) external override {
        // Custom payment logic here
        balance -= amount;
    }

    function getBalance() external view override returns (uint) {
        return balance;
    }
}
```
## Interface

`Interfaces` are similar to abstract contracts, but they have restrictions:

- Cannot have any functions implemented.
- Cannot inherit other contracts or interfaces.
- Cannot define constructor.
- Cannot define variables.
- Cannot define structs.
- Cannot define enums.

```solidity
pragma solidity ^0.4.11;

interface Token {
    function transfer(address recipient, uint amount);
}
```
## Events

Events allow the convenient usage of the EVM logging facilities, which in turn can be used to “call” JavaScript callbacks in the user interface of a dapp, which listen for these events.

- Up to three parameters can receive the attribute indexed, which will cause the respective arguments to be searched for.

- The events that are emitted can be viewed in the Blockexplorer of the Blockchain where the contract is deployed.

> All non-indexed arguments will be stored in the data part of the log.

```solidity
pragma solidity ^0.4.0;

contract ClientReceipt {
    event Deposit(
        address indexed _from,
        bytes32 indexed _id,
        uint _value
    );

    function deposit(bytes32 _id) payable {
        emit Deposit(msg.sender, _id, msg.value);
    }
}
```

NOTE: Use events whenever we are we want our Dapp (Decentralised app) to perform certain actions once an event is emitted from the 
contract. (eg: Once Deposit event is emitted from contract our Dapp will display a Deposited successfully message with the
sender wallet address and amount he deposited)

## Library

Libraries are similar to contracts, but they are deployed only once at a specific address, and their code is used with [`delegatecall`](#delegatecall). 

By using libraries, we can easily reuse the common functionality across multiple contracts
Can be imported in contracts to reduce code duplication and gas costs, can be shared across other contracts.

Math Library:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

library MathLibrary {
    function add(uint256 a, uint256 b) external pure returns (uint256) {
        return a + b;
    }
}
```

ContractUsingLibrary:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ContractUsingLibrary {
    address public mathLibraryAddress;

    constructor(address _mathLibraryAddress) {
        mathLibraryAddress = _mathLibraryAddress;
    }

    function useLibraryAdd(uint256 a, uint256 b) external returns (bool, bytes memory) {
        // Prepare the data for the delegatecall
        bytes memory data = abi.encodeWithSignature("add(uint256,uint256)", a, b);

        // Perform the delegatecall
        (bool success, bytes memory result) = mathLibraryAddress.delegatecall(data);

        // Check the delegatecall result
        return (success, result);
    }
}
```
## Using - For

`using A for B;` can be used to attach library functions to any type.

```solidity
library arithmatic {
    function add(uint _a, uint _b) returns (uint) {
        return _a + _b;
    }
}

contract C {
    using arithmatic for uint;
    
    uint sum;
    function f(uint _a) {
        sum = _a.add(3);
    }
}
```

## Error Handling

- `assert(bool condition)`: throws if the condition is not met - to be used for internal errors.
- `require(bool condition)`: throws if the condition is not met - to be used for errors in inputs or external components.
- `revert()`: abort execution and revert state changes

```solidity
 function sendHalf(address addr) payable returns (uint balance) {
    require(msg.value % 2 == 0,"Only Allow Even Numbers"); // Only allow even numbers else throw the revert message
    uint balanceBeforeTransfer = this.balance;
    addr.transfer(msg.value / 2);
    assert(this.balance == balanceBeforeTransfer - msg.value / 2);
    return this.balance;
}
```

It is best to use:
 a. Use if conditions instead of require.
 b. use custom error method instead of require.

```solidity

 error NotEvenNumber(uint value);

 function sendHalf(address addr) payable returns (uint balance) {
    if(msg.value % 2 != 0)
        revert NotEventNumber(value: msg.value);  // Only allow even numbers else throw the revert message
    uint balanceBeforeTransfer = this.balance;
    addr.transfer(msg.value / 2);
    assert(this.balance == balanceBeforeTransfer - msg.value / 2);
    return this.balance;
}
```


## Global variables

### Block variables

- `block.blockhash(uint blockNumber) returns (bytes32)`: hash of the given block - only works for the 256 most recent blocks excluding current
- `block.coinbase (address)`: current block miner’s address
- `block.difficulty (uint)`: current block difficulty
- `block.gaslimit (uint)`: current block gaslimit
- `block.number (uint)`: current block number
- `block.timestamp (uint)`: current block timestamp as seconds since unix epoch
- `now (uint)`: current block timestamp (alias for `block.timestamp`)

### Transaction variables

- `msg.data (bytes)`: complete calldata
- `msg.gas (uint)`: remaining gas
- `msg.sender (address)`: sender of the message (current call)
- `msg.sig (bytes4)`: first four bytes of the calldata (i.e. function identifier)
- `msg.value (uint)`: number of wei sent with the message
- `tx.gasprice (uint)`: gas price of the transaction
- `tx.origin (address)`: sender of the transaction (full call chain)

### Mathematical and Cryptographic Functions

- `addmod(uint x, uint y, uint k) returns (uint)`:
   compute (x + y) % k where the addition is performed with arbitrary precision and does not wrap around at 2**256.
- `mulmod(uint x, uint y, uint k) returns (uint)`:
   compute (x * y) % k where the multiplication is performed with arbitrary precision and does not wrap around at 2**256.
- `keccak256(...) returns (bytes32)`:
   compute the Ethereum-SHA-3 (Keccak-256) hash of the (tightly packed) arguments. Can be used while comparing string equality along with abi.encodePacked()
- `sha256(...) returns (bytes32)`:
   compute the SHA-256 hash of the (tightly packed) arguments
- `sha3(...) returns (bytes32)`:
   alias to keccak256
- `ripemd160(...) returns (bytes20)`:
   compute RIPEMD-160 hash of the (tightly packed) arguments
- `ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address)`:
   recover the address associated with the public key from elliptic curve signature or return zero on error (example usage)
   
### Contract Related
- `this (current contract’s type)`: the current contract, explicitly convertible to Address
- `selfdestruct(address recipient)`: destroy the current contract, sending its funds to the given Address
- `suicide(address recipient)`: alias to selfdestruct. Soon to be deprecated.

## Security Tips

### External calls

Avoid using external calls whenever possible. Calls to untrusted code can lead to security flaw. When using external contract calls, assume that unsafe code might execute. Even if the contract is not malicious, malicious code can be executed by any contracts it calls.

- Use External calls at the bottom of the function as it will forward all the gas to the funtion.

- Use `<address>.call.gas(gasAmount)()` to limit sending gas to external calls.

### Using access modifiers

Use access modifiers explicitly. Using `external` for external only function will reduce gas.

```
// BAD
function externalFunction() {
    // Do Something
}

// GOOD
uint public status;
function externalFunction() external {
    // Do Something
}
```

### Make use of function modifiers

Use modifiers to restrict access to functions. Avoid unauthorized access for all functions. You should use `msg.sender` over `tx.origin` for authorization.

```
// BAD
function selfdestruct() {
    selfdestruct(<address>);
}

// GOOD
modifier onlyOwner() {
  require(msg.sender == <address of owner>);
  _
}
function selfdestruct() onlyOwner {
    selfdestruct(<address>);
}
```

### Upgrading contracts

- Using upgradable contracts with a proxy setup is advised. This way we can keep upgrading our contract without losing the funds that we stored. Use `delegatecall` in a proxy contract for upgrading contracts overtime.

- Here we have a proxy contract that holds the funds and other data. The proxy contract holds the address of the implementation contract which has the functional logic.  When we upgrade the logic all we need to do is replace the implementation contract address with new contract address in the proxy contract.

## Final Tips

- Ethereum will be updated very frequently until a stable build. So expect new best practices and security consideration every now and then.
- Your code will have bugs and be prepared to handle those. Use techniques such as pausing the contract and limiting usage.
- Keep your contract simple and Modularize.
- Make use of libraries so that you can reuse and update code.
- Try to use already available tools and libraries and update them frquently.
- Never try to store and implement everything in solidity. Use it where decentralization is needed.