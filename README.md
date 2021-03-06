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

# Chapter 2 Day 2 - Transactions and Scripts

1. Scripts are meant to only read the data present in the contact, changing data requires gas and can be only done in a transaction.
2. Using AuthAccount type, once can access data in their account. In order to perform a transaction one has to pay for it and sign, the signing information can also accessed using AuthAccount.
3. ```prepare``` phase to access the data in the users account and ```execute``` phase is where the transcation will get executed by calling the methods in smart contract. Note: Everything can be done in prepare phase but not vice versa.

4. Smart Contract 
```
pub contract HelloWorld {

    pub var myNumber: Int

    pub fun updateMyNumber (newNumber: Int) {
        self.myNumber = newNumber
    }

    init() {
        self.myNumber = 0
    }
}
```

Script
```
import HelloWorld from 0x01

pub fun main(): Int {
    return HelloWorld.myNumber
}
```

Transaction
```
import HelloWorld from 0x01

transaction(myNewNumber: Int) {
    prepare(signer: AuthAccount) {}

    execute {
      HelloWorld.updateMyNumber(newNumber: myNewNumber)
    }
}
```

# Chapter 2 Day 3 - Arrays, Dictionaries, and Optionals
1. In a script, initialize an array (that has length == 3) of your favourite people, represented as Strings, and log it.
```
pub fun main() {
    let favouritePeople: [String] = ["Fav1", "Fav2", "Fav3"]
    log(favouritePeople)
}
```
2.
```
pub fun main() {
    let usage: {String: UInt64} = {"Facebook": 5, "Instagram": 1, "Twitter": 4, "YouTube": 2, "Reddit": 0, "LinkedIn ": 3}
    log(usage)
}
```
3.  force unwrap operator ```!``` gets the actual value from an optional and fails the program if the value is nil
```
pub fun main() {
    var num: Int? = 1
    var unwrappedNum: Int = num! 

    var num2: Int? = nil
    var unwrappedName2: Int = num2! // fails the program
}
```

4.a The return type is declared as String but the function is returning a String optional i.e. String?

4.b When you access elements of a dictionary, it returns the value as an optional by default. Since return type is defined as just String, it is giving mismatched type error.

4.c This can be fixed in two ways, one is to use force unwarp operator (!) on the returned value and other is to change the return type to an optional type (String?)

# Chapter 2 Day 4 - Basic Structs

```
pub contract Authentication {

    pub var favouriteBooks: {Address: FavouriteBook}
    
    pub struct FavouriteBook {
        pub let bookName: String
        pub let authorName: String

        init(_bookName: String, _authorName: String) {
            self.bookName = _bookName
            self.authorName = _authorName
        }
    }

    pub fun addFavouriteBook(bookName: String, authorName: String, account: Address) {
        let newFavouriteBook = FavouriteBook(_bookName: bookName, _authorName: authorName)
        self.favouriteBooks[account] = newFavouriteBook
    }

    init() {
        self.favouriteBooks = {}
    }

}
```

```
import Authentication from 0x01

transaction(book: String, author: String, account: Address) {

    prepare(signer: AuthAccount) {}

    execute {
        Authentication.addFavouriteBook(bookName: book, authorName: author, account: account)
        log("We're done.")
    }
}
```

```
import Authentication from 0x01

pub fun main(address: Address): Authentication.FavouriteBook {
  return Authentication.favouriteBooks[address]!
}
```

# Chapter 3 Day 1 - Resources

1. Structs can be copied and overriden but not the resources. Structs can be created anywhere whereas resource can be created only in smart contract. Resource must be used to handle secure data like NFT's whereas Structs are containers for data which are used for general purpose.
2. Whenever there is a need to contain data in a very secure manner like giving someone an NFT which is worth millions.
3. ```create``` is the keyword to make a new resource.
4. No. Resource can be created only in smart contract.
5. The type is ```Jacob```
6. Resource return type definiton must be prefixed with @ i.e. ```@Jacob```. ```create``` keyword must be used to create a resource. Resource cannot be assigned, it must be moved using the operator ```<-```. Returning a resource should be prefixed with ```<-```
```
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob { 
        let myJacob <- create Jacob() 
        return <- myJacob 
    }
}
```

# Chapter 3 Day 2 - Resources in Dictionaries & Arrays

```
pub contract Test {

    pub let booksDictionary: @{String: Book}
    pub let booksArray: @[Book]

    pub resource Book {
        pub let id: String
        pub let name: String
        init(_id: String, _name: String) {
            self.id = _id
            self.name = _name
        }
    }

    pub fun addBookIntoDictionary(book: @Book) {
        self.booksDictionary[book.name] <-! book
    }

    pub fun addBookIntoArray(book: @Book) {
        self.booksArray.append(<- book)
    }

    pub fun removeBookFromDictionary(key: String): @Book {
        return <- self.booksDictionary.remove(key: key)!
    }

    pub fun removeBookFromArray(index: Int): @Book  {
        return <- self.booksArray.remove(at: index)
    }

    init() {
        self.booksDictionary <- {}
        self.booksArray <- []
    }
}
```

# Chapter 3 Day 3 - References

```
pub contract Test {

    pub var dictionaryOfBooks: @{String: Book}

    pub resource Book {
        pub let name: String
        init(_name: String) {
            self.name = _name
        }
    }

    pub fun getReference(key: String): &Book? {
        return &self.dictionaryOfBooks[key] as &Book?
    }

    init() {
        self.dictionaryOfBooks <- {
            "Horror": <- create Book(_name: "Conjuring"), 
            "Action": <- create Book(_name: "JohnWick")
        }
    }
}
```

```
import Test from 0x01

pub fun main(): String {
  let ref = Test.getReference(key: "Horror")
  return ref!.name // returns "Conjuring"
}
```

For the scenarios where developers wants to operate on the data inside resource, ref will be useful since there is no need to move the resources in that case.

# Chapter 3 Day 4 - Resource/Struct Interfaces
1. To define finite set of requirements and to limit the access to the resource variables and methods.
2. 
```
pub contract Stuff {

    pub resource interface IBook {
      pub var name: String
    }

    pub resource Book: IBook {
      pub var name: String
      pub var author: String

      pub fun updateAuthor(newAuthor: String) {
        self.author = newAuthor
      }

      init() {
        self.name = "Spongebob"
        self.author = "authorrr"
      }
    }

    pub fun noInterface() {
      let book: @Book <- create Book()
      book.updateAuthor(newAuthor: "pradeep")
      log(book.author) // 5

      destroy book
    }

    pub fun yesInterface() {
      let book: @Book{IBook} <- create Book()
      book.updateAuthor(newAuthor: "pradeep") // ERROR: `member of restricted type is not accessible: updateAuthor`
      log(book.author)
      destroy book
    }
}
```
3. ```favouriteFruit``` must be declared in the struct as well and ```ITest``` interface should define ```changeGreeting`` method
