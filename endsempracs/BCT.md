
# BLOCKCHAIN TECHNOLOGY – PRACTICALS

## Practical 3 – Bank Account Using Solidity

### Aim
To implement a smart contract using Solidity that allows users to deposit Ether, withdraw Ether, and check their account balance.

### Problem Statement
Develop a smart contract named `BankAccount` that allows users to deposit Ether into their account, withdraw Ether from their account, and check their current balance. The contract should ensure that deposits are greater than zero and withdrawals do not exceed the available balance.

### Software Required
- Remix IDE
- Web Browser
- Solidity Compiler (0.8.20 or compatible version)

### Program: BankAccount.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BankAccount {
    mapping(address => uint256) private balances;

    event Deposit(address indexed account, uint256 amount);
    event Withdraw(address indexed account, uint256 amount);

    function deposit() public payable {
        require(msg.value > 0, "Deposit amount must be greater than 0");
        balances[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    function withdraw(uint256 amount) public {
        require(amount > 0, "Withdraw amount must be greater than 0");
        require(balances[msg.sender] >= amount, "Insufficient balance");

        balances[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);

        emit Withdraw(msg.sender, amount);
    }

    function getBalance() public view returns (uint256) {
        return balances[msg.sender];
    }
}
```

### Explanation

**1. SPDX License Identifier**
```solidity
// SPDX-License-Identifier: MIT
```
Specifies the open-source license used for the program.

**2. Solidity Version**
```solidity
pragma solidity ^0.8.20;
```
Specifies the compatible Solidity compiler version.

**3. Contract Declaration**
```solidity
contract BankAccount {
```
Declares a smart contract named BankAccount.

**4. Mapping**
```solidity
mapping(address => uint256) private balances;
```
- Stores data in key-value pairs.
- Address represents the user's wallet address.
- uint256 represents the account balance in Wei.
- private restricts direct external access to the mapping.

**5. Events**
```solidity
event Deposit(address indexed account, uint256 amount);
event Withdraw(address indexed account, uint256 amount);
```
Events record deposit and withdrawal activities in blockchain transaction logs.

**6. Deposit Function**
```solidity
function deposit() public payable {
    require(msg.value > 0, "Deposit amount must be greater than 0");
    balances[msg.sender] += msg.value;
    emit Deposit(msg.sender, msg.value);
}
```
- payable allows the function to receive Ether.
- msg.value represents the Ether sent in Wei.
- require() checks that the deposit is greater than zero.
- msg.sender identifies the calling wallet address.
- The amount is added to the user's balance.
- emit generates a Deposit event.

**7. Withdraw Function**
```solidity
function withdraw(uint256 amount) public {
    require(amount > 0, "Withdraw amount must be greater than 0");
    require(balances[msg.sender] >= amount, "Insufficient balance");

    balances[msg.sender] -= amount;
    payable(msg.sender).transfer(amount);

    emit Withdraw(msg.sender, amount);
}
```
- Accepts the withdrawal amount.
- Checks that the amount is greater than zero.
- Checks whether sufficient balance is available.
- Deducts the amount from the user's balance.
- Transfers Ether to the user's wallet.
- Emits a Withdraw event.

**8. Get Balance Function**
```solidity
function getBalance() public view returns (uint256) {
    return balances[msg.sender];
}
```
Returns the balance of the calling user in Wei. The view keyword indicates that the function does not modify contract state.

### Execution Steps

1. Open https://remix.ethereum.org/
2. Create a file named `BankAccount.sol`.
3. Paste the Solidity code.
4. Open the Solidity Compiler tab.
5. Select compiler version 0.8.20 or compatible.
6. Click Compile BankAccount.sol.
7. Open Deploy & Run Transactions.
8. Select Environment as Remix VM.
9. Select BankAccount and click Deploy.
10. Expand the deployed contract to access its functions.

### Testing and Expected Output

**Test 1: Deposit Ether**

- Enter 1 in the Value field.
- Select Ether as the unit.
- Click deposit.

Expected result:
```text
Deposit successful
```

**Test 2: Check Balance**

Click getBalance.

Expected output:
```text
1000000000000000000
```

**Test 3: Withdraw Ether**

Call withdraw with:
```text
500000000000000000
```

Expected balance:
```text
500000000000000000
```

**Test 4: Insufficient Balance**

Call withdraw with:
```text
2000000000000000000
```

Expected output:
```text
Insufficient balance
```

### Result
The BankAccount smart contract was successfully implemented, compiled, deployed, and tested using Remix IDE. Ether deposit, withdrawal, and balance checking operations were performed successfully.

---

# Practical 4 – Student Data Management Using Solidity

### Aim
To implement a smart contract using Solidity to store, retrieve, and manage student information using structures and arrays. Also demonstrate fallback and receive functions.

### Problem Statement
Develop a smart contract named `StudentData` that stores student information including student ID, name, age, and course. The contract should allow adding student records, retrieving student details using an index, and checking the total number of students. It should also demonstrate fallback and receive functions.

### Software Required
- Remix IDE
- Web Browser
- Solidity Compiler (0.8.20 or compatible version)

### Program: StudentData.sol

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract StudentData {
    struct Student {
        uint256 id;
        string name;
        uint8 age;
        string course;
    }

    Student[] private students;

    event StudentAdded(uint256 id, string name, string course);
    event FallbackCalled(address sender, uint value, string message);

    function addStudent(uint256 _id, string memory _name, uint8 _age, string memory _course) public {
        students.push(Student(_id, _name, _age, _course));
        emit StudentAdded(_id, _name, _course);
    }

    function getStudent(uint256 index) public view returns (uint256, string memory, uint8, string memory) {
        require(index < students.length, "Invalid index");
        Student memory s = students[index];
        return (s.id, s.name, s.age, s.course);
    }

    function getTotalStudents() public view returns (uint256) {
        return students.length;
    }

    fallback() external payable {
        emit FallbackCalled(msg.sender, msg.value, "Fallback function triggered!");
    }

    receive() external payable {
        emit FallbackCalled(msg.sender, msg.value, "Receive function triggered!");
    }
}
```

### Explanation

**1. SPDX License and Solidity Version**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;
```
Specifies the license and compiler version.

**2. Contract Declaration**
```solidity
contract StudentData {
```
Declares a smart contract named StudentData.

**3. Structure (struct)**
```solidity
struct Student {
    uint256 id;
    string name;
    uint8 age;
    string course;
}
```
- struct is a user-defined data type.
- Groups multiple variables of different data types.
- Represents an individual student record.

**4. Dynamic Array**
```solidity
Student[] private students;
```
Creates a dynamic array to store multiple Student structures.

**5. Events**
```solidity
event StudentAdded(uint256 id, string name, string course);
event FallbackCalled(address sender, uint value, string message);
```
Events record student additions and fallback/receive function execution in transaction logs.

**6. Add Student Function**
```solidity
function addStudent(uint256 _id, string memory _name, uint8 _age, string memory _course) public {
    students.push(Student(_id, _name, _age, _course));
    emit StudentAdded(_id, _name, _course);
}
```
- Accepts student details.
- Creates a Student structure.
- push() adds the record to the dynamic array.
- memory stores temporary string data.
- Emits StudentAdded event.

**7. Get Student Function**
```solidity
function getStudent(uint256 index) public view returns (uint256, string memory, uint8, string memory) {
    require(index < students.length, "Invalid index");
    Student memory s = students[index];
    return (s.id, s.name, s.age, s.course);
}
```
- Retrieves student details using an array index.
- require() checks whether the index is valid.
- Returns ID, name, age, and course.

**8. Get Total Students Function**
```solidity
function getTotalStudents() public view returns (uint256) {
    return students.length;
}
```
Returns the total number of student records stored in the array.

**9. Fallback Function**
```solidity
fallback() external payable {
    emit FallbackCalled(msg.sender, msg.value, "Fallback function triggered!");
}
```
- Executes when a call does not match any existing function.
- Can receive Ether because it is payable.
- Emits a FallbackCalled event.

**10. Receive Function**
```solidity
receive() external payable {
    emit FallbackCalled(msg.sender, msg.value, "Receive function triggered!");
}
```
- Executes when Ether is sent directly to the contract with empty calldata.
- Must be declared external payable.
- Emits an event when triggered.

### Execution Steps

1. Open https://remix.ethereum.org/
2. Create a file named `StudentData.sol`.
3. Paste the Solidity code.
4. Open Solidity Compiler.
5. Select compiler version 0.8.20 or compatible.
6. Click Compile StudentData.sol.
7. Open Deploy & Run Transactions.
8. Select Environment as Remix VM.
9. Select StudentData and click Deploy.
10. Expand the deployed contract to access the functions.

### Testing and Expected Output

**Test 1: Add First Student**

Call:
```text
addStudent(101, "Rahul", 20, "Computer Engineering")
```

Expected:
```text
Student Added Successfully
```

**Test 2: Add Second Student**

Call:
```text
addStudent(102, "Priya", 21, "Information Technology")
```

Expected:
```text
Student Added Successfully
```

**Test 3: Get First Student**

Call:
```text
getStudent(0)
```

Expected output:
```text
101
Rahul
20
Computer Engineering
```

**Test 4: Get Second Student**

Call:
```text
getStudent(1)
```

Expected output:
```text
102
Priya
21
Information Technology
```

**Test 5: Get Total Students**

Call:
```text
getTotalStudents()
```

Expected output:
```text
2
```

**Test 6: Invalid Index**

Call:
```text
getStudent(5)
```

Expected output:
```text
Invalid index
```

**Test 7: Receive Function**

- Send Ether directly to the deployed contract address.
- Leave calldata empty.
- The receive function executes.

Expected event:
```text
Receive function triggered!
```

**Test 8: Fallback Function**

- Send a transaction to the contract address with unmatched non-empty calldata.
- For example, use `0x12345678`.

Expected event:
```text
Fallback function triggered!
```

### Result
The StudentData smart contract was successfully implemented, compiled, deployed, and tested using Remix IDE. Student records were added and retrieved, the total number of students was checked, and fallback and receive functions were demonstrated.

---

# Important Solidity Concepts

| Concept | Description |
|---|---|
| Blockchain | Distributed digital ledger |
| Smart Contract | Program deployed on a blockchain |
| Solidity | Programming language for smart contracts |
| Ethereum | Blockchain supporting smart contracts |
| Ether | Native cryptocurrency of Ethereum |
| Wei | Smallest unit of Ether |
| Mapping | Key-value data structure |
| Struct | User-defined data structure |
| Array | Collection of elements |
| Event | Records transaction information |
| Gas | Computational cost of blockchain operations |
| Remix IDE | Solidity development environment |
| Deploy | Publishing a contract to a blockchain |
| Transaction | Operation submitted to blockchain |

## Ether and Wei Conversion

1 ETH = 10^18 Wei

| Ether | Wei |
|---|---:|
| 1 ETH | 1000000000000000000 |
| 0.5 ETH | 500000000000000000 |
| 0.1 ETH | 100000000000000000 |
| 0.01 ETH | 10000000000000000 |

---

# Viva Questions and Answers

## General Blockchain Questions

**Q1. What is blockchain?**

Blockchain is a decentralized and distributed digital ledger that stores transactions in blocks connected using cryptography.

**Q2. What is a smart contract?**

A smart contract is a self-executing program deployed on a blockchain.

**Q3. What is Solidity?**

Solidity is a high-level programming language used to develop smart contracts.

**Q4. What is Remix IDE?**

Remix IDE is a browser-based environment used to write, compile, deploy, and test Solidity programs.

**Q5. What is Ethereum?**

Ethereum is a decentralized blockchain platform that supports smart contracts and decentralized applications.

**Q6. What is gas?**

Gas measures the computational resources required to execute operations on Ethereum.

**Q7. What is Ether?**

Ether (ETH) is the native cryptocurrency of Ethereum.

## BankAccount Practical Questions

**Q8. What is mapping in Solidity?**

Mapping is a key-value data structure used to store and retrieve information.

**Q9. What is msg.sender?**

It represents the address of the account calling the current function.

**Q10. What is msg.value?**

It represents the amount of Ether sent with a transaction, measured in Wei.

**Q11. Why do we use payable?**

The payable keyword allows a function or address to receive Ether.

**Q12. What is require()?**

It checks a condition and reverts the transaction if the condition is false.

**Q13. What is an event?**

An event records information in blockchain transaction logs.

**Q14. Difference between view and pure?**

- view can read contract state but cannot modify it.
- pure cannot read or modify contract state.

**Q15. Why is uint256 used?**

It is an unsigned 256-bit integer used to store non-negative integer values.

**Q16. What is transfer()?**

It transfers Ether from a contract to an address and reverts if the transfer fails.

**Q17. What happens if someone withdraws more than their balance?**

The require condition fails and the transaction is reverted.

## StudentData Practical Questions

**Q18. What is a struct?**

A struct is a user-defined data type used to group variables of different data types.

**Q19. What is an array?**

An array is a collection of elements stored under one variable name.

**Q20. What is a dynamic array?**

A dynamic array is an array whose length can change during execution.

**Q21. What is memory in Solidity?**

Memory is temporary data storage that exists during function execution.

**Q22. Difference between storage and memory?**

- Storage represents persistent contract data.
- Memory represents temporary data.

**Q23. What is fallback()?**

Fallback is a special function that executes when a call does not match any existing function.

**Q24. What is receive()?**

Receive is a special function that executes when Ether is sent with empty calldata.

**Q25. Difference between fallback and receive?**

Receive handles empty-calldata Ether transfers, while fallback handles unmatched function calls.

**Q26. Why do we use students.length?**

It returns the total number of elements in the dynamic array.

**Q27. Why does array indexing start from zero?**

Solidity uses zero-based indexing, so the first element is at index zero.

**Q28. What is uint8?**

It is an unsigned 8-bit integer that stores values from 0 to 255.

**Q29. What happens if an invalid student index is entered?**

The require condition fails and the transaction is reverted.

**Q30. Can a smart contract store student information permanently?**

Yes. Data stored in contract state persists on the blockchain.

---

# Quick Revision Before Practical Examination

## Practical 3 – BankAccount

1. Create a mapping for account balances.
2. Create Deposit and Withdraw events.
3. Write a payable deposit function.
4. Write a withdrawal function using require().
5. Write a getBalance function.
6. Compile and deploy the contract.
7. Test deposit, withdrawal, and balance checking.

## Practical 4 – StudentData

1. Create a Student structure.
2. Create a dynamic array.
3. Write addStudent().
4. Write getStudent().
5. Write getTotalStudents().
6. Implement fallback().
7. Implement receive().
8. Compile and deploy the contract.
9. Test student records and both special functions.

---

# Important Notes

- Both programs are written in Solidity.
- Use Remix IDE to execute them.
- Always compile before deploying.
- Use Remix VM for local testing.
- BankAccount uses Ether and Wei.
- StudentData uses structures and dynamic arrays.
- receive() handles empty-calldata Ether transfers.
- fallback() handles unmatched calls.
- View functions read state without modifying it.
- Remix VM is a local testing environment, not the Ethereum mainnet.
