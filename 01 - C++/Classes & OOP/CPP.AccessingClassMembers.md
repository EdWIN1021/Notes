```cpp
Account my_account;

my_account.balance;
my_account.deposit(100.00);
```

```cpp
Account* my_account = new Account();

(*my_account).balance;
(*my_account).deposit(100.00);

my_account->balance;
my_account->deposit(100.00);
```