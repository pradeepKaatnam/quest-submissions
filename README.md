# quest-submissions

# Chapter 1 - Day 1

1. Block chain is a decentralized shared database which can be accessed by everyone in a secure way. Data on the block chain can be modified using smart contracts. Any change in the chain must be approved by more than half of the nodes in the network.
2. Smart contracts are simply programs stored on a blockchain that run when predetermined conditions are met. 
3. A script is to view the data in the block chain and is free of cost whereas a transaction is used to alter the data in the chain with some cost associated to it.

# Chapter 1 - Day 2 - The Flow Blockchain & Cadence
**What are the 5 Cadence Programming Language Pillars?**    

*Safety & Security:* Strong typing support in Cadence makes its safer & secure for the developers while writing contracts.

*Developer experience:* Making debugging process easier by providing detailed error messages.

*Resource oriented programming:* Need to understand further.

*Clarity:* Code written should be easily read and should make sense for the other developers as well.

*Approachability:* Cadence was built in such a way that it is very similar to popular programming languages which makes it easy to learn if one has prior programming language.

**why could the 5 Pillars be useful?**

*Safety & Security:* This is needed since the code with security loop holes can be easily attacked and results in larger issues.

*Developer experience:* Easy debugging experience is needed to identify any issue in a timely fashion.

*Clarity:* This is the common problem where developers keeps on changing, a well written code with comments and proper naming conventions will make easy for the other developers to understand.

*Approachability:* Spending less time to adapt to a new language is always appreciated. With that more developers will be willing to learn.

# Chapter 2 - Day 1 - Our First Smart Contract

Smart Contract 
```
pub contract JacobTucker {
  
  pub let is: String

  init() {
    self.is = "the best"
  }  

  pub fun readIs(): String {
    return self.is
  }
}
```

Script
```
import JacobTucker from 0x03

pub fun main(): String {
  return JacobTucker.readIs()
}
```
![image](https://user-images.githubusercontent.com/12966342/173198095-eeee4d39-f6f5-48bb-8786-4a3314c1f496.png)

