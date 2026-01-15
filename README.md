# Worksheet One

## Java Generics

+ This worksheet examines your existing knowledge on Java Generics to reinforce 
the concepts from your previous studies.
+ It is essential that you commit regularly any changes to your source code 
(to the respective `GitHub Classroom` repository).
+ Where the questions make incremental changes to the code you **do not** need to keep 
separate versions of your code, as your commits will deal with that situation. 
+ Text-based questions should be answered inline by modifying this document.

You are provided with some source files under `src/`.

## Learning Goals

When you have completed this worksheet you should have achieved the following learning goals:

+ To understand the basic benefits of using generics.
+ To understand the difference between type invariance and covariance.
+ To understand when to use upper-bounded and lower-bounded wildcards.

You should be able to finish most of the exercises during the lab session given your prior knowledge of the topic.

## The Exercises

1. Add the following code snippet to the `Example1` class `main` method, 
creating two different storage objects with two different type specialisations:
  
   ```.java
   Storage<BankAccount> bankAccountStorage = new Storage<BankAccount>();
   Storage<String> stringStorage = new Storage<String>();
   ```
   + What are the reasons for using generics here?
   
     To be able to use the same treatment for 2 different objects
   
   + What are the benefits?
   
     The principle of write once and then implement multiple times! 
     And more specifically we can use the same implementation with the different types
     bankAccountStorage.empty()
     stringStorage.empty()

Add the following code to your `Example1` class:

   ```.java
   Object account = new BankAccount(2025);
   bankAccountStorage.setItem(account);

   Object account1 = bankAccountStorage.getItem();
   account1.deposit(15);
   ```
   + Compile and analyse the compiler output.
      + java: incompatible types: java.lang.Object cannot be converted to BankAccount
        The generic has explicitly set the type that bankAccountStorage can accept as
        BankAccount. But the reference type of account is Object and Java will not explicitly downcast
   
   + What is the cause of the problems reported by the compiler, if any?
   
     ** YOUR ANSWER HERE **
     + I'm not sure what i required here

Now replace:

   ```
   Object account = new BankAccount(2025);
   ```
   with
   ```
   BankAccount account = new BankAccount(2025);
   ```
   + How does this affect the compilation process?
   
     - java: cannot find symbol
       symbol:   method deposit(int)
       location: variable account1 of type java.lang.Object
     - It passed the previous line and raises and error for the method deposit
     
   
   + What is the problem, if any?

       - Deposit is not a method of ~~storage~~ Object
       - Fix by using `((BankAccount) account1).deposit(15);` instead of `account1.deposit(15);`
   
Now replace 

   ```.java
   Storage<BankAccount> bankAccountStorage = new Storage<BankAccount>();
   Storage<String> stringStorage = new Storage<String>();
   ```
with
   ```.java
   Storage<BankAccount> bankAccountStorage = new Storage<>();
   Storage<String> stringStorage = new Storage<>();
   ```

   + Does it still compile?

     Yes the empty brace <> infers the type from the generic reference type on the left

Finally, replace

   ```.java
   Storage<BankAccount> bankAccountStorage = new Storage<>();
   Storage<String> stringStorage = new Storage<>();
   ```
with
   ```.java
   var bankAccountStorage = new Storage<>();
   var stringStorage = new Storage<>();
   ```

   + Does it still compile? If not, explain why.

     Yes var in this case is inferring that Storage<>() uses Object 
        as it's bound type
     Due to the phrasing of the next question, I have replaced the previous casting to
     `BankAccount account1 = bankAccountStorage.getItem();`
     java: incompatible types: java.lang.Object cannot be converted to BankAccount

   + How would you fix the error?

     I need to be more specific here about the concrete type in `bankAccountStorage` to pair with 
      the use of inference in the reference 
      var bankAccountStorage = new Storage<BankAccount>();


2. In Java, arrays are **covariant**, which means that an array of type `T1[]` can be used in a context where 
an array of type `T2[]` is expected (in other words, `T1[]` is a subtype of `T2[]`) whenever `T1` is a subtype of `T2`. Consider class `Example2`: 
its method `test` is declared to accept an array of any objects, in particular, an array of `BankAccount` 
(as `BankAccount` is a subtype of `Object`). 
Inside method `test`, the parameter array `a` has type `Object[]` and so, we can store a new `BankAccount` in 
the parameter array. Compile and run the code to ensure it can be executed successfully.

Replace now

   ```.java
   BankAccount[] array = new BankAccount[2];
   array[0] = new BankAccount(2024);
   ```
with
   ```.java
   String[] array = new String[2];
   array[0] = "hi there";
   ```

+ Compile and run the code again. Does it still compile? Does it run? What is the output?

  ** YOUR ANSWER HERE **

3. Generics in Java, in contrast, are **invariant**, which means that the type parameters need to match exactly:
`C<T1>` is **not** a subtype of `C<T2>` when `T1` is a subtype of `T2` (unless, of course, `T1` is the same as `T2`).
Consider class `Example3`, which is very similar to `Example2` except that it uses the `Storage` generic class
instead of an array. Compile and run the code to ensure it can be executed successfully.

Replace now

   ```.java
   Storage<BankAccount> storage = new Storage<>();
   storage.setItem(new BankAccount(2024));
   ```
with
   ```.java
   Storage<String> storage = new Storage<>();
   storage.setItem("Happy 2025!");
   ```

+ Compile the code again. Does it still compile? Why not?

  ** YOUR ANSWER HERE **

4. Study the code in class `Example4`.

+ What is the type of variable `storage1`?

  ** YOUR ANSWER HERE **

+ Compile and run the code to make sure it can be executed successfully. What is the printed output?

  ** YOUR ANSWER HERE **

Add the following 3 lines to method `main`:

   ```.java
   var storage2 = new Storage<SavingsAccount>();
   storage2.setItem(new SavingsAccount(2025, 2));
   process(storage2);
   ```

+ What is the type of variable `storage2`?

  ** YOUR ANSWER HERE **

+ Compile the code. Does it compile? If not, what is the problem?

  ** YOUR ANSWER HERE **

+ Fix the compile-time error, without changing the contents of method `main` and without any code duplication. 
In other words, assume that users of your 
method `process` may want to invoke it with `Storage<BankAccount>`, `Storage<SavingsAccount>` or indeed with
any `Storage<T>` for a subtype `T` of `BankAccount`.

5. Study the code in class `Example5`.

+ What is the type of variable `storage1`? 

  ** YOUR ANSWER HERE **

+ Compile and run the code to make sure it can be executed successfully. What is the printed output?

  ** YOUR ANSWER HERE **

Add the following 4 lines to method `main`:

   ```.java
   var storage2 = new Storage<BankAccount>();
   store(storage2, account);

   var storage3 = new Storage<Object>();
   store(storage3, account);
   ```

+ What is the type of variable `storage2`? What is the type of variable `storage3`?

  ** YOUR ANSWER HERE **

+ Compile the code. Does it compile? If not, what is the problem?

  ** YOUR ANSWER HERE **

+ Fix the compile-time error, without changing the contents of method `main` and without any code duplication.
  In other words, assume that users of your
  method `store` may want to invoke it with `Storage<SavingsAccount>` or with
  any `Storage<T>` for a supertype `T` of `SavingsAccount` (in particular, with `Storage<BankAccount>` and `Storage<Object>`).

The last two exercises illustrate the PECS abbreviation: **P**roducer **e**xtends, **C**onsumer **s**uper.

------

