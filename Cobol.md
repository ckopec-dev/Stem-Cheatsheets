
# 📘 COBOL Comprehensive Tutorial

*(From Zero to Enterprise-Ready)*

---

# 1. What is COBOL?

COBOL is a high-level programming language created in 1959, designed for **business, financial, and administrative systems**.

It is still heavily used today in:

* Banking & finance systems
* Insurance systems
* Government systems
* Airline and reservation systems
* Large enterprise batch processing

Modern COBOL runs on:

* IBM mainframes (z/OS)
* Linux / Windows (GnuCOBOL, Micro Focus, Fujitsu)
* Cloud environments

---

# 2. COBOL Program Structure

Every COBOL program is divided into **four divisions**:

```cobol
IDENTIFICATION DIVISION.
ENVIRONMENT DIVISION.
DATA DIVISION.
PROCEDURE DIVISION.
```

### Example Skeleton

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. HELLO.

ENVIRONMENT DIVISION.

DATA DIVISION.

PROCEDURE DIVISION.
    DISPLAY "Hello, world!".
    STOP RUN.
```

---

# 3. Installing COBOL (Modern Setup)

The most popular open-source compiler:

### ✅ GnuCOBOL

#### Windows:

* [https://gnucobol.sourceforge.io](https://gnucobol.sourceforge.io)
* or via MSYS2 / Chocolatey

#### Linux:

```bash
sudo apt install gnucobol
```

#### Mac:

```bash
brew install gnu-cobol
```

Compile & run:

```bash
cobc -x hello.cob
./hello
```

---

# 4. COBOL Syntax Basics

### Case-insensitive

```cobol
display "Hello".
DISPLAY "Hello".
```

### Period ends statements

```cobol
DISPLAY "Done".
```

### Free format (modern COBOL)

```cobol
*> This is a comment
```

---

# 5. Data Types and Variables

All variables go in the **DATA DIVISION**.

```cobol
DATA DIVISION.
WORKING-STORAGE SECTION.

01 CUSTOMER-NAME     PIC X(30).
01 CUSTOMER-AGE      PIC 9(3).
01 ACCOUNT-BALANCE   PIC 9(7)V99.
01 IS-ACTIVE         PIC X VALUE 'Y'.
```

### Picture (PIC) clause

| PIC | Meaning         |
| --- | --------------- |
| X   | character       |
| 9   | numeric         |
| V   | implied decimal |
| A   | alphabetic      |

Examples:

```cobol
PIC X(20)       *> 20 chars
PIC 9(5)        *> 00000–99999
PIC 9(5)V99     *> 99999.99
PIC S9(7)V99    *> signed decimal
```

---

# 6. Display and Accept (I/O)

```cobol
DISPLAY "Enter your name: ".
ACCEPT CUSTOMER-NAME.

DISPLAY "Hello " CUSTOMER-NAME.
```

---

# 7. Arithmetic

```cobol
COMPUTE ACCOUNT-BALANCE = 1000.50 + 250.25.
ADD 100 TO ACCOUNT-BALANCE.
SUBTRACT 50 FROM ACCOUNT-BALANCE.
MULTIPLY ACCOUNT-BALANCE BY 1.05.
DIVIDE ACCOUNT-BALANCE BY 2.
```

---

# 8. Conditional Logic

### IF / ELSE

```cobol
IF ACCOUNT-BALANCE > 0
    DISPLAY "Positive balance"
ELSE
    DISPLAY "Overdrawn"
END-IF.
```

### Comparison operators

```cobol
=   >   <   >=   <=   NOT =
```

---

# 9. Loops

### PERFORM UNTIL

```cobol
PERFORM UNTIL CUSTOMER-AGE > 99
    ADD 1 TO CUSTOMER-AGE
END-PERFORM.
```

### PERFORM VARYING (for-loop)

```cobol
PERFORM VARYING I FROM 1 BY 1 UNTIL I > 10
    DISPLAY I
END-PERFORM.
```

---

# 10. Paragraphs and Sections

```cobol
PROCEDURE DIVISION.

MAIN-PARA.
    PERFORM GREET.
    STOP RUN.

GREET.
    DISPLAY "Welcome to COBOL".
```

---

# 11. Working with Files

### File declaration

```cobol
ENVIRONMENT DIVISION.
INPUT-OUTPUT SECTION.
FILE-CONTROL.
    SELECT CUSTOMER-FILE ASSIGN TO "customers.dat"
    ORGANIZATION IS LINE SEQUENTIAL.
```

### File structure

```cobol
DATA DIVISION.
FILE SECTION.
FD CUSTOMER-FILE.
01 CUSTOMER-RECORD.
   05 CUST-ID        PIC 9(5).
   05 CUST-NAME      PIC X(20).
   05 CUST-BALANCE   PIC 9(7)V99.
```

### Reading

```cobol
OPEN INPUT CUSTOMER-FILE.

READ CUSTOMER-FILE
    AT END DISPLAY "End of file"
END-READ.

CLOSE CUSTOMER-FILE.
```

### Writing

```cobol
OPEN OUTPUT CUSTOMER-FILE.
WRITE CUSTOMER-RECORD.
CLOSE CUSTOMER-FILE.
```

---

# 12. Database Access (Embedded SQL)

```cobol
EXEC SQL
   SELECT NAME, BALANCE
   INTO :CUST-NAME, :ACCOUNT-BALANCE
   FROM CUSTOMERS
   WHERE ID = :CUST-ID
END-EXEC.
```

Used heavily with:

* DB2
* Oracle
* SQL Server

---

# 13. Arrays (Tables)

```cobol
01 TRANSACTIONS.
   05 TRANS OCCURS 12 TIMES.
      10 TRANS-AMOUNT PIC S9(7)V99.
```

Access:

```cobol
MOVE 1 TO I.
DISPLAY TRANS-AMOUNT(I).
```

---

# 14. String Handling

```cobol
STRING "Hello " CUSTOMER-NAME
   INTO FULL-MESSAGE
END-STRING.

UNSTRING FULL-MESSAGE
   DELIMITED BY SPACE
   INTO PART1 PART2
END-UNSTRING.
```

---

# 15. Modular Programming

### Calling programs

```cobol
CALL "TAXCALC" USING PRICE, TAX.
```

### Subprogram

```cobol
IDENTIFICATION DIVISION.
PROGRAM-ID. TAXCALC.

DATA DIVISION.
LINKAGE SECTION.
01 PRICE PIC 9(5)V99.
01 TAX   PIC 9(5)V99.

PROCEDURE DIVISION USING PRICE TAX.
    COMPUTE TAX = PRICE * 0.07.
    EXIT PROGRAM.
```

---

# 16. Error Handling

```cobol
READ CUSTOMER-FILE
    AT END MOVE 'Y' TO EOF-FLAG
    NOT AT END DISPLAY CUSTOMER-RECORD
END-READ.
```

File status:

```cobol
SELECT CUSTOMER-FILE ASSIGN TO "cust.dat"
   FILE STATUS IS WS-FILE-STATUS.
```

---

# 17. Batch Processing Pattern

```cobol
PERFORM UNTIL EOF = 'Y'
    READ INPUT-FILE
        AT END MOVE 'Y' TO EOF
        NOT AT END PERFORM PROCESS-RECORD
    END-READ
END-PERFORM.
```

This is the backbone of enterprise COBOL.

---

# 18. COBOL OOP (Modern COBOL)

```cobol
CLASS-ID. Account.

WORKING-STORAGE SECTION.
01 BALANCE PIC 9(7)V99.

METHOD-ID. Deposit.
PROCEDURE DIVISION USING AMOUNT.
    ADD AMOUNT TO BALANCE.
END METHOD.

END CLASS Account.
```

Supports:

* Classes
* Methods
* Inheritance
* Encapsulation

---

# 19. Interfacing with C / Java

COBOL can:

* Call C shared libraries
* Expose services
* Run under JVM (Micro Focus Visual COBOL)

Example:

```cobol
CALL "printf" USING BY VALUE "Hello from COBOL".
```

---

# 20. Building a Real Program (Mini Banking App)

Features:

* File storage
* Menu
* Deposits
* Withdrawals

```cobol
DISPLAY "1. Deposit".
DISPLAY "2. Withdraw".
DISPLAY "3. Exit".
ACCEPT USER-CHOICE.

EVALUATE USER-CHOICE
   WHEN 1 PERFORM DO-DEPOSIT
   WHEN 2 PERFORM DO-WITHDRAW
   WHEN 3 STOP RUN
   WHEN OTHER DISPLAY "Invalid"
END-EVALUATE.
```

---

# 21. COBOL Best Practices

* Use meaningful names
* Modularize heavily
* Always track FILE STATUS
* Use copybooks
* Document record layouts
* Avoid deeply nested logic
* Use batch patterns

---

# 22. COBOL in the Modern World

COBOL now integrates with:

* REST APIs
* Kafka & MQ
* Cloud batch processing
* Containers
* Web services

COBOL systems today handle:

* 95% of credit card transactions
* billions of lines of production code
* massive financial backends

---

# 23. Learning Path

### Beginner

* Syntax
* File I/O
* Batch loops
* Record structures

### Intermediate

* SQL
* Copybooks
* Modular programs
* Error handling

### Advanced

* Performance tuning
* OOP COBOL
* Mainframe JCL
* Online systems (CICS, IMS)
* API integration

---

# 24. Tools & Ecosystem

* GnuCOBOL
* IBM Enterprise COBOL
* Micro Focus Visual COBOL
* VS Code COBOL extensions
* DB2 / Oracle / SQL Server
* JCL (job control)
* CICS (transaction processing)

---

# 25. Why COBOL Still Matters

* Extreme reliability
* Handles massive data volumes
* Optimized for business logic
* Legacy systems still critical
* COBOL devs are in demand



Just tell me which direction you want to go.
