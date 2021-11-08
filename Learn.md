# Data Locations - Storage, Memory and Calldata

The amount of gas you will use during a transaction depends on the data location you use in your smart contract.
Hence, using appropriate location matters for having an optimized code that uses a minimum amount of gas.

Variables are declared as either `storage`, `memory` or `calldata` to explicitly specify the location of the data.

## storage location

- Here variable is a state variable i.e. stored on blockchain.
- It is permanent, persistant data and can be accessed into all functions within the contract.
- It is quite expensive compared to other data locations.

Here `arr`, `map` and `myStructs` are all state variables. Code it all up in the editor.

```
    uint[] public arr;
    mapping(uint => address) map;
    struct MyStruct {
        uint foo;
    }
    mapping(uint => MyStruct) myStructs;
```

They can be accessed by all functions within the contract.
Here, we are accessing state variable `myStructs[1]` in `_f()` function and changing its `foo` value.

```
    function f() public returns (uint) {
        // call _f with state variables
        _f(arr, map, myStructs[1]);

        // get a struct from a mapping
        MyStruct storage myStruct = myStructs[1];

        return myStruct.foo;
    }

    function _f(
        uint[] storage _arr,
        mapping(uint => address) storage _map,
        MyStruct storage _myStruct
    ) internal {
        _myStruct.foo = 1;
    }
```

Hit `Run` to check if you have written the code correctly. The output of `f()` should be `1` which is `foo`'s value.

## memory location

- Here variable is in memory and it exists while a function is being called.
- It is temporary data and cheaper than the storage location.
- Think of it as a RAM of each individual function.

```
    function f() public returns (uint) {
        // create a struct in memory
        MyStruct memory myMemStruct = MyStruct(0);
    }
```

You can even return `memory` variables from a function.

```
    function g(uint[] memory _arr) public returns (uint[] memory) {
        _arr[0] = 1;
        return _arr;
    }
```

Hit `Run` to check if you have written the code correctly. The output of `g()` should be `[1]` which is `_arr`'s value.

## calldata location

- Special data location that contains function arguments, only available for `external` functions.
- It is non-modifiable and non-persistant data location.

```
    function h(uint[] calldata _arr) external {
        // do something with calldata array
    }
```
