# Solidity

Solidity is an object-oriented, high-level language for implementing smart contracts. Smart contracts are programs which govern the behavior of accounts within the Ethereum state.

Solidity is a  [curly-bracket language](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type#Curly-bracket_languages)  designed to target the Ethereum Virtual Machine (EVM). It is influenced by C++, Python and JavaScript. You can find more details about which languages Solidity has been inspired by in the  [language influences](https://docs.soliditylang.org/en/v0.8.13/language-influences.html)  section.

Solidity is statically typed, supports inheritance, libraries and complex user-defined types among other features.

With Solidity you can create contracts for uses such as voting, crowdfunding, blind auctions, and multi-signature wallets.

When deploying contracts, you should use the latest released version of Solidity. Apart from exceptional cases, only the latest version receives  [security fixes](https://github.com/ethereum/solidity/security/policy#supported-versions). Furthermore, breaking changes as well as new features are introduced regularly. We currently use a 0.y.z version number  [to indicate this fast pace of change](https://semver.org/#spec-item-4).


## Contracts

 #### Version Pragma
 ##### All solidity source code should start with a "version pragma" — a declaration of the version of the Solidity compiler this code should use. This is to prevent issues with future compiler versions potentially introducing changes that would break your code.

 1. **Version of Solidity compiler should use**
 2. **<= inclusive**
 3. **< exclusive**
 
 i.e.  

    pragma solidity >=0.5.0 <0.6.0; (**<---version**)
    
	contract HelloWorld { 
	 
	}

## State Variables & Integers

#### Variables
##### **_State variables_** are permanently stored in contract storage. This means they're written to the Ethereum blockchain. Think of them like writing to a DB.

#### Unsigned Integers:  `uint`

The  `uint`  data type is an unsigned integer, meaning  **its value must be non-negative**. There's also an  `int`  data type for signed integers.

> Note: In Solidity,  `uint`  is actually an alias for  `uint256`, a 256-bit unsigned integer. You can declare uints with less bits —  `uint8`,  `uint16`,  `uint32`, etc.. But in general you want to simply use  `uint`  except in specific cases, which we'll talk about in later lessons.



## Math Operations

-   Addition:  `x + y`
-   Subtraction:  `x - y`,
-   Multiplication:  `x * y`
-   Division:  `x / y`
-   Modulus / remainder:  `x % y`  _(for example,  `13 % 5`  is  `3`, because if you divide 5 into 13, 3 is the remainder)_

Solidity also supports an  **_exponential operator_**  (i.e. "x to the power of y", x^y):
-   Exponent / To the power of :  `x ** y`
```
uint x = 5 ** 2; // equal to 5^2 = 25
```

## Structs

Sometimes you need a more complex data type. For this, Solidity provides  **_structs_**:

```
** struct example below **

struct Person {
  uint age;
  string name;
}
```
##### example code:
```
pragma  solidity  >=0.5.0  <0.6.0;

  contract ZombieFactory {
	uint dnaDigits =  16;
	uint dnaModulus =  10 ** dnaDigits;
		
		struct Zombie {
			string name;
			uint dna;
		}
	}
```

Structs allow you to create more complicated data types that have multiple properties.

## Arrays

 There are two types of arrays in Solidity: **_fixed_** arrays and **_dynamic_** arrays:

#### Public Arrays
> You can declare an array as  `public`, and Solidity will automatically create a  **_getter_**  method for it. The syntax looks like:

```
Person[] public people;

```

> Other contracts would then be able to read from, but not write to, this array. So this is a useful pattern for storing public data in your contract.

##### example code:
```
pragma  solidity  >=0.5.0  <0.6.0;

  contract ZombieFactory {
	uint dnaDigits =  16;
	uint dnaModulus =  10 ** dnaDigits;
		
		struct Zombie {
			string name;
			uint dna;
		}

		Zombie[] public zombies;
	}
```

## Function Declarations

##### function example code:
```
function createZombie(string memory _name, uint _dna) {

}
```

##### example code:
```
pragma  solidity  >=0.5.0  <0.6.0;

  contract ZombieFactory {
	uint dnaDigits =  16;
	uint dnaModulus =  10 ** dnaDigits;
		
		struct Zombie {
			string name;
			uint dna;
		}

		Zombie[] public zombies;
		
		function createZombie(string memory _name, uint _dna) {

		}
	}
```
> Note: It's convention (but not required) to start function parameter variable names with an underscore (`_`) in order to differentiate them from global variables. We'll use that convention throughout our tutorial.


## Working With Structs and Arrays


##### example code:
```
pragma  solidity  >=0.5.0  <0.6.0;

  contract ZombieFactory {
	uint dnaDigits =  16;
	uint dnaModulus =  10 ** dnaDigits;
		
		struct Zombie {
			string name;
			uint dna;
		}

		Zombie[] public zombies;
		
		function createZombie(string memory _name, uint _dna) {
			zombies.push(Zombie(_name, _dna));
		}
	}
```
> This example creates a new `Zombie`, and adds it to the `zombies` array. The `name` and `dna` for the new Zombie should come from the `createZombie` function arguments.

> ***Note:*** that `array.push()` adds something to the **end** of the array, so the elements are in the order we added them.

## Private / Public Functions

In Solidity, functions are `public` by default.
This means anyone (or any other contract) can call your contract's function and execute its code.

> Obviously this isn't always desirable, and can make your contract vulnerable to attacks. Thus it's good practice to mark your functions as `private` by default, and then only make `public` the functions you want to expose to the world.

##### example code:
```
pragma  solidity  >=0.5.0  <0.6.0;

  contract ZombieFactory {
	uint dnaDigits =  16;
	uint dnaModulus =  10 ** dnaDigits;
		
		struct Zombie {
			string name;
			uint dna;
		}

		Zombie[] public zombies;
		
		function _createZombie(string memory _name, uint _dna) private {
			zombies.push(Zombie(_name, _dna));
		}
	}
```
> In this example, to change the `createZombie` function from public to private, we use the keyword *private* after the function name. Also, since it's convention, we add an underscore at the beginning of the function.


## More on Functions

#### return values:
In Solidity, the function declaration contains the type of the return value (i.e. `string`)

#### function modifiers:
There are 2 types of function modifiers in Solidity, `view` and `pure`.

- a function declared as `view` means that it's only viewing the data but not modifying it.
- a function declared as `pure` means that the function is not even accessing data within the app. Example code below...

```
function _multiply(uint a, uint b) private pure returns (uint) {
  return a * b;
}
```

This function doesn't even read from the state of the app — its return value depends only on its function parameters. So in this case we would declare the function as  **_pure_**.

##### example code following the lesson:
```
pragma  solidity  >=0.5.0  <0.6.0;

  contract ZombieFactory {
	uint dnaDigits =  16;
	uint dnaModulus =  10 ** dnaDigits;
		
		struct Zombie {
			string name;
			uint dna;
		}

		Zombie[] public zombies;
		
		function _createZombie(string memory _name, uint _dna) private {
			zombies.push(Zombie(_name, _dna));
		}

		function _generateRandomDna(string memory _str) private view returns (uint) {
		
		}
	}
```

> In this example, we create a *private* function called `_generateRandomDna`. It takes one parameter named `_str` (a `string` and we set the data location to `memory`), and returns a `uint`

##  Keccak256 and Typecasting
#### What is `keccak256`?:

We want our  `_generateRandomDna`  function to return a (semi) random  `uint`. How can we accomplish this?
Ethereum has the hash function  `keccak256`  built in. A hash function basically maps an input into a random 256-bit hexadecimal number.
> Also important, `keccak256` expects a single parameter of type `bytes`. This means that we have to "pack" any parameters before calling `keccak256`:

#### Typecasting:
##### example:
```
uint8 a = 5;
uint b = 6;
// throws an error because a * b returns a uint, not uint8:
uint8 c = a * b;
// we have to typecast b as a uint8 to make it work:
uint8 c = a * uint8(b);
```
In the above,  `a * b`  returns a  `uint`, but we were trying to store it as a  `uint8`, which could cause potential problems. By casting it as a  `uint8`, it works and the compiler won't throw an error.

##### example code generating a pseudo-random hexadecimal:
```
pragma  solidity  >=0.5.0  <0.6.0;

  contract ZombieFactory {
	uint dnaDigits =  16;
	uint dnaModulus =  10 ** dnaDigits;
		
		struct Zombie {
			string name;
			uint dna;
		}

		Zombie[] public zombies;
		
		function _createZombie(string memory _name, uint _dna) private {
			zombies.push(Zombie(_name, _dna));
		}

		function _generateRandomDna(string memory _str) private view returns (uint) {
			uint rand = uint(keccak256(abi.encodePacked(_str)));
			return rand % dnaModulus;
		}
	}
```
> The first line of code takes the  `keccak256`  hash of  `abi.encodePacked(_str)`  to generate a pseudo-random hexadecimal, typecasts it as a  `uint`, and finally stores the result in a  `uint`  called  `rand`.
> 
>  Since we want our DNA to be 16 digits long, we return the value `rand % dnaModulus`.

## Putting everything together

##### creating a public function to generate a zombie with a random name and DNA:
```
pragma solidity  >=0.5.0 <0.6.0;

contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string memory _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    } 

    function _generateRandomDna(string memory _str) private view returns (uint) {
        uint rand = uint(keccak256(abi.encodePacked(_str)));
        return rand % dnaModulus;
    }

    function createRandomZombie(string memory _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}
```

## Events

**_Events_** are a way for your contract to communicate that something happened on the blockchain to your app front-end, which can be 'listening' for certain events and take action when they happen.

##### creating an event to allow the FE to know a new zombie with a random name and DNA was generated:
```
pragma solidity  >=0.5.0 <0.6.0;

contract ZombieFactory {

	event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    function _createZombie(string memory _name, uint _dna) private {
        uint id = zombies.push(Zombie(_name, _dna)) - 1;
        emit NewZombie(id, _name, _dna);
    } 

    function _generateRandomDna(string memory _str) private view returns (uint) {
        uint rand = uint(keccak256(abi.encodePacked(_str)));
        return rand % dnaModulus;
    }

    function createRandomZombie(string memory _name) public {
        uint randDna = _generateRandomDna(_name);
        _createZombie(_name, randDna);
    }

}
```
> We created an **_event_** called `NewZombie` and modified the `_createZombie` function to fire the `NewZombie` event after the new Zombie to our `zombies` array.

Your app front-end could then listen for the event. A javascript implementation would look something like:

```
YourContract.IntegersAdded(function(error, result) {
  // do something with result
})
```
