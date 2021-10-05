# Smart-Contracts-with-Solidity

## Assignment 20

***This README.md file was create from the homework directive which contained the explanation of how each of the contracts works and what the motivation for each of the contracts is. The accompanied screenshots were provided to illustrate the functionality (e.g. how I sent transactions, how the transferred amount is then distributed by each of the contracts, and how the timelock functionality can be tested with the fastforward function).***

# Unit 20 - "Looks like we've made our first contract!"

![contract](https://image.shutterstock.com/z/stock-photo-two-hands-handshake-polygonal-low-poly-hud-illustration-smart-contract-agreement-blockchain-and-1161295627.jpg)

## Background

Your new startup has created its own Ethereum-compatible blockchain to help connect financial institutions, and the team wants to build smart contracts to automate some company finances to make everyone's lives easier, increase transparency, and to make accounting and auditing practically automatic.

Fortunately, you've been learning how to program smart contracts with Solidity! What you will be doing this assignment is creating 3 `ProfitSplitter` contracts. These contracts will do several things:

* Pay your associate-level employees quickly and easily.

* Distribute profits to different tiers of employees.

* Distribute company shares for employees in a "deferred equity incentive plan" automatically.

### Starting your project

Navigate to the [Remix IDE](https://remix.ethereum.org) and create a new contract called `AssociateProfitSplitter.sol` using the starter code for Level One above.

While developing and testing your contract, use the [Ganache](https://www.trufflesuite.com/ganache) development chain, and point MetaMask to `localhost:8545`, or replace the port with what you have set in your workspace.

### Level One: The `AssociateProfitSplitter` Contract

* **Level One** is an `AssociateProfitSplitter` contract. This will accept ether into the contract, and divide it evenly among associate-level employees. This will allow the human resources department to pay employees quickly and efficiently.

At the top of your contract, you will need to define the following `public` variables:

* `employee_one` — The `address` of the first employee. Make sure to set this to `payable`.

* `employee_two` — Another `address payable` that represents the second employee.

* `employee_three` — The third `address payable` that represents the third employee.

Create a constructor function that accepts:

* `address payable _one`

* `address payable _two`

* `address payable _three`

Within the constructor, set the employee addresses to equal the parameter values. This will allow you to avoid hardcoding the employee addresses.

Next, create the following functions:

* `balance` — This function should be set to `public view returns(uint)`, and must return the contract's current balance. Since we should always be sending ether to the beneficiaries, this function should always return `0`. If it does not, the `deposit` function is not handling the remainders properly and should be fixed. This will serve as a test function of sorts.

* `deposit` — This function should be set to `public payable` check, ensuring that only the owner can call the function.

  * In this function, perform the following steps:

    * Set a `uint amount` to equal `msg.value / 3;` in order to calculate the split value of the ether.

    * Transfer the `amount` to `employee_one`.

    * Repeat the steps for `employee_two` and `employee_three`.

    * Since `uint` only contains positive whole numbers, and Solidity does not fully support float/decimals, we must deal with a potential remainder at the end of this function, since `amount` will discard the remainder during division.

    * We may either have `1` or `2` wei left over, so transfer the `msg.value - amount * 3` back to `msg.sender`. This will re-multiply the `amount` by 3, then subtract it from the `msg.value` to account for any leftover wei, and send it back to human resources.

* Create a fallback function using `function() external payable`, and call the `deposit` function from within it. This will ensure that the logic in `deposit` executes if ether is sent directly to the contract. This is important to prevent ether from being locked in the contract, since we don't have a `withdraw` function in this use case.

#### The `AssociateProfitSplitter` Contract Coded 
![Picture1](Images/Level_1_Code.PNG)

#### Test the contract


In the `Deploy` tab in Remix, deploy the contract to your local Ganache chain by connecting to `Injected Web3` and ensuring MetaMask is pointed to `localhost:8545`.

You will need to fill in the constructor parameters with your designated `employee` addresses.

Test the `deposit` function by sending various values. Keep an eye on the `employee` balances as you send different amounts of ether to the contract and ensure the logic is executing properly.

First you need to set the value equal to 0 before deploying in order for there to be nore error messages. 

![Picture2](Images/Remix_Contract_Deployment.PNG)

![Picture3](Images/Remix_MetaMask_Contract_Deployment.PNG)

Then you test it using different values. In this scenario I chose a value of 6 and chagned the type to Ether.

![Picture4](Images/Test_Deposit.PNG)

![Picture5](Images/Confirm.PNG)


