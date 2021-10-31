# Data Locations - Storage, Memory and Calldata

The amount of gas you will use during a transaction depends on the data location you use in your smart contract.
Hence, using appropriate location matters for having an optimized code that uses a minimum amount of gas.

Variables are declared as either `storage`, `memory` or `calldata` to explicitly specify the location of the data.

- `storage`
  - variable is a state variable i.e. stored on blockchain
  - it is permanent, persistant data and can be accessed into all functions within the contract
  - it is quite expensive compared to other data locations
- `memory`
  - variable is in memory and it exists while a function is being called
  - it is temporary data and cheaper than the storage location
  - think of it as a RAM of each individual function
- `calldata`
  - special data location that contains function arguments, only available for `external` functions
  - it is non-modifiable and non-persistant data location

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.3;

contract DataLocations {
    uint[] public arr;
    mapping(uint => address) map;
    struct MyStruct {
        uint foo;
    }
    mapping(uint => MyStruct) myStructs;

    function f() public {
        // call _f with state variables
        _f(arr, map, myStructs[1]);

        // get a struct from a mapping
        MyStruct storage myStruct = myStructs[1];
        // create a struct in memory
        MyStruct memory myMemStruct = MyStruct(0);
    }

    function _f(
        uint[] storage _arr,
        mapping(uint => address) storage _map,
        MyStruct storage _myStruct
    ) internal {
        // do something with storage variables
    }

    // You can return memory variables
    function g(uint[] memory _arr) public returns (uint[] memory) {
        // do something with memory array
    }

    function h(uint[] calldata _arr) external {
        // do something with calldata array
    }
}
```