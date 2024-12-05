## **Budgeting and Expense Tracking (bext) File Format Specification**

### **Overview**

This file format is designed to support **budgeting and expense tracking**, allowing users to record income and expenses, categorized by different criteria such as categories, people, accounts, and payment methods. The format is human-readable and flexible, enabling varied ordering of fields while ensuring that all necessary data points are captured. The format can be processed by a custom parser to calculate totals by income, expense, category, person, account, and payment method.

### **File Extension**
The file extension for this format should be `.bext` (Budget and Expense Tracking).

### **Field Delimiters**
- **`+`**: Used to denote an income entry.
- **`-`**: Used to denote an expense entry.
- **`@`**: Used to specify the person associated with the transaction (e.g., who is making or receiving the payment).
- **`#`**: Used to specify the category of the transaction (e.g., salary, groceries, utilities).
- **`$`**: Used to specify the budget allocation for a specific account, category or person.
- **`[]`**: Denotes a timestamp for the entry in the format `YY/MM/DD HH:MM`
- **`~`**: Denotes the account from which the transaction was made (e.g., Bank, Wallet, Cash).
- **`?`**: Indicates remarks or additional notes about the transaction.
- **`:`**: Denotes the payment method used (e.g., cash, card, wallet, QR).
- **`;`**: to separate multiple entries within specific fields (`@`, `#`, `~`).
- **`>`**: to specify subcategories within (`~` and `#`)

### **General Structure**

Each line in the file represents a **single transaction** or record. The first character of the line determines the type of the record (`+`, `-`, or `$`) followed by amount and other fields. Other fields (`@`, `#`, `[]`, `~`, `:` and `?`) in the record can appear in any order. All records should be separated by a newline.

### **Field Specifications**

1. **Income and Expense Entries (`+` and `-`)**:
   - **`+`**: Indicates an income entry (positive amount).
   - **`-`**: Indicates an expense entry (negative amount).
   - **Example**: `+ 2000` represents an income of 2000 units of currency.
   - **Example**: `- 150` represents an expense of 150 units of currency.

2. **Person (`@`)**:
   - The **`@`** symbol specifies a person associated with the transaction. If multiple persons are involved, they can be separated by semicolons.
   - **Example**: `@Bhupal` refers to the person "Bhupal".
   - **Multiple persons**: `@Bhupal;Partner` refers to two people involved in the transaction.

3. **Category (`#`)**:
   - The **`#`** symbol is used to define the category of the transaction. Multiple categories can be specified by separating them with semicolon.
   - **Example**: `#Salary` refers to the category of "Salary".
   - **Multiple categories**: `#Salary;Bonus` refers to two categories, "Salary" and "Bonus".

4. **Budget (`$`)**:
   - The **`$`** symbol is used to define the budget allocated for a category or person.
   - **Example**: `$ 10000 ~Bank ?Initial deposit :bank` defines a budget allocation of 5000 units for a account, category or person.

5. **Timestamp (`[]`)**:
   - Timestamps should be enclosed in square brackets and follow the format `MM/DD/YYYY HH:mm`.
   - **Example**: `[12/01/2024 10:00]` represents a timestamp indicating the date and time of the transaction.

6. **Account (`~`)**:
   - The **`~`** symbol is used to specify the account associated with the transaction (e.g., bank account, wallet, etc.).
   - **Example**: `~Bank` refers to the "Bank" account.
   - **Multiple accounts**: `~Bank;Wallet` refers to two accounts involved in the transaction.

7. **Remarks (`?`)**:
   - The **`?`** symbol is used to specify additional remarks or notes about the transaction.
   - **Example**: `?Rent payment for November` provides additional information about the transaction.

8. **Payment Method (`:`)**:
   - The **`:`** symbol denotes the payment method used for the transaction.
   - Valid payment methods include: `cash`, `card`, `qr`, `wallet`, `bank`.
   - **Example**: `:cash` indicates that the payment method used is "cash".
   - **If not specified, the payment method defaults to "Other"**.


### **Field Rules**

1. **Required Fields**:
   - The **amount** is always required (can be positive or negative depending on whether it's income or expense).
   - **Timestamp** is optional but recommended for tracking when the transaction occurred.
   - **Payment Method** is optional; if not specified, it defaults to `"Other"`.

2. **Multiple Values**:
   - Multiple values for fields such as **persons**, **categories**, and **accounts** can be separated by semicolon.
   - **Example**: `@Bhupal;Partner` indicates two persons involved in the transaction.
   - **Example**: `#Salary;Bonus` indicates two categories for the same transaction.

3. **Flexibility**:
   - The order of fields within each record can vary, allowing users to specify the fields in any order. For example:
     - Valid: `+2000 @Bhupal #Salary ~Bank [12/01/2024 10:00] :card ?Salary for November`
     - Valid: `[12/01/2024 10:00] +2000 @Bhupal ~Bank #Salary :card ?Salary for November`
   
4. **Defaults**:
   - If the **payment method** is not specified, it is automatically set to `"Other"`.
   - If a **person** or **account** is not specified, it defaults to `"Other"`.

5. **Escape Character**:
   - Use the backslash (`\`) to escape any special characters in the remarks field, for example:
     - `?This is a rent payment for December, \#includes bonus` to escape the hash symbol.

### **Usage Example**

The following example shows a complete record in the file format:

```text
+ 7500 @Bhupal #Salary;Bonus ~Bank [12/01/2024 10:00] :cheque ?Salary for December, includes bonus
```

This represents an income of 5000 units, with Bhupal as the persons, categorized under Salary and Bonus, from the Bank account, using the cheque payment method, with the remark indicating that the salary includes a bonus.

### **Example Records**

Below are 10 example records (`sample.bext`) that show how different fields can be used in the format. The fields can appear in any order.

```
$ 10000 ~Bank ?Initial deposit :bank [2024/12/01 10:00]
+ 7000 @Bhupal #Salary [12/01 10:00] ~Bank ?Monthly salary deposit :cash
- 500 #Groceries ~Wallet ?Grocery shopping @Family :card
+ 1000 @Bhupal;Bhumika [12/01 10:00] ~Bank ?Multiple persons and account specified #Test :qr
+ 1000 @Partner #Consulting [12/02 09:00] ?Payment for consulting services :wallet
- 2000 @Bhupal #Rent;Bhada ~Bank ?December rent payment :card
+ 500 @Dad #Gift ~Wallet ?Birthday gift for Partner :qr
- 70 @Family #Utilities ~Cash ?Electricity bill payment :cash
+ 1000 @Partner #Investment [2024/12/05 15:00] ~Bank ?Stock market investment :bank
- 200 @Bhupal #Dining;Food ~Cash ?Dinner at restaurant :cash
```

### **Parsing the files**

- PHP Parser [BEXT-PHP](https://github.com/bhu1st/bext-php) 

### **Future**

This file format provides a flexible and readable way to record and track budget, expenses, and income across multiple categories, persons, accounts, and payment methods. The ability to specify fields in any order and the use of special symbols for categorization and tracking ensures that the format is versatile for personal and business use. The .bext aims for simplicity, compactness and encourages plain-text budget and expense tracking.
