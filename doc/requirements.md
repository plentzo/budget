# Accounts

1. An account can vary by type:
    1. Finance
        1. Checking
        2. Savings
        3. Money Market
        4. Investment (Retirement)
    2. Utility
        1. Electric - Tracks usage (amount/cost)
        2. Gas - Tracks usage (amount/cost)
        3. Water - Tracks usage (amount/cost)
    3. Other
        1. Car - Tracks fuel (amount/cost)
2. Utility and Other accounts can link to individual transactions to help associate spending with usage

## Accounts API

### All Accounts

 - accountId: Unique across all accounts
 - name: The name of the account

### Finance Accounts

 - type: checking | savings | moneymarket | investment

### Utility Accounts

 - type: electric | gas | water

### Utility Accounts

 - type: car



# Transactions

1. Transactions track all financial throughput in the system
2. Transactions are tied to an individual account
3. Transactions fall into a few broad categories:
    1. Income - Money flowing in
    2. Expense - Money flowing out
    3. Balance - Sets the current balance for an account. Used for initial balance and any balance corrections after that.

## Transactions API

### All Transactions

 - transactionId: Unique across all transactions, accounts for user
 - accountId: The transaction's account
 - date: The date of the transaction
 - note: Optional. A note about the transaction.

### Income/Expense Transactions

 - categoryId: The transaction's category
 - amount: The numerical amount; negative for **expense**, positive for **income**

### Balance Transactions

 - balance: The new balance for the account



# Transaction Categories

1. Categories are used to sort transactions in the budget
2. Category names are treated like a hierarchy. E.g. 'Home / Mortgage' is displayed as 'Mortgage' underneath a parent category of 'Home'
3. Categories can be marked as **recurring**. Transactions will not be automatically added if they're in recurring categories, but more fields will be carried over when a new transaction of the same category is added.

## Transaction Categories API

### All Transaction Categories

- categoryId: Unique across all categories, accounts for user
- name: The name of the category (include /s for hierarchy)
- recurring: `true` if all transactions in this category are considered recurring expenses



# Budgets

1. A budget assigns expected expenses per transaction category
2. Every month the budget "starts over":
    1. A new copy of the budget is made.
        1. Any changes to the budget impact this budget and future budgets.
        2. All prior month budgets are set in stone.
    2. All **non-rollover** remaining amounts revert to `$0`
    3. All **rollover** remaining amounts are carried over:
        1. If the previous month was under budget by `$x`, keep this month's budget at `$0` but don't start using it until we reach `$x` in the category.
        2. If the previous month was over budget by `$x`, immediately apply it to this month's starting budget (it starts at `$x`).
3. Expenses can be budgeted **monthly**, **bi-monthly**, **quarterly**, or **yearly**
    1. Monthly expenses are simple; they reset every month
    2. Bi-monthly expenses are tracked as `total/2` per month. Leftovers roll over. They assume you pay $`x` every two months, so we should budget $`x/2` every month.
    3. Quarterly expenses are tracked as `total/3` per month. Leftovers roll over. They assume you pay $`x` every three months, so we should budget $`x/3` every month.
    4. Yearly expenses are tracked as `total/12` per month. Leftovers roll over. They assume you pay $`x` every year, so we should budget $`x/12` every month.

## Budgets API




# Utilities



# Summary Information

## "What If" Scenarios

 1. The user can make a temporary budget (copying an existing one) to tweak and play with the numbers.
 2. The temporary budget can be "applied" to a previous month.
 3. The user can make temporary transactions to see how it impacts the budget.