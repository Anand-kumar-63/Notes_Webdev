Solidity has specific types of variables based on where they're stored and what their purpose is.

#DataTypesinSolidity
- Solidity is a **statically typed** language, which means you must declare the data type of a variable when you define it. Data types are broadly classified into two categories:
### value types
- Variables of these types pass their own copy when used in assignments or function arguments.
- `uint`/`int`**: Unsigned and signed integers of various sizes (`uint8` to `uint256`).
- **`bool`**: Boolean values (`true` or `false`).
- **`address`**: Stores a 20-byte Ethereum address.
- **`bytes`**: Fixed-size byte arrays (`bytes1` to `bytes32`).
- **`enum`**: User-defined types for creating a set of named constants.
### Reference Types
These variables hold a "reference" or memory address to their data, rather than the data itself.
- **`string`**: A dynamic array of characters.
- **`array`**: A collection of data of the same type.
- **`struct`**: A custom, user-defined data structure.
- **`mapping`**: A key-value store, similar to a hash table.
### Visibility and Modifiers
Solidity also allows you to control who can access state variables using **visibility modifiers**.
- **`public`**: Accessible from anywhere, including other contracts.
- **`private`**: Only accessible within the contract where it's declared.
- **`internal`**: Accessible within the contract and by contracts that inherit from it.