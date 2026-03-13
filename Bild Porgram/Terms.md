

---

# What You ARE Allowed To Do (as a researcher)

- Test the **Bilt platform**
    
    - Website: `biltrewards.com`
        
    - Mobile app
        
    - Program features (points, payments, rewards)
        
- Find **security vulnerabilities**, such as:
    
    - Authentication issues
        
    - Authorization / IDOR
        
    - Payment manipulation
        
    - Account takeover
        
    - Data exposure
        
    - Logic bugs in rewards / points
        
    - Transaction manipulation
        
    - Credit reporting bugs
        
    - Linked card issues
        
- Test **normal user flows**, like:
    
    - account creation
        
    - linking cards
        
    - rent payment flow
        
    - rewards earning
        
    - rewards redemption
        
    - tier status logic
        
- Analyze **how points are calculated**
    
    - rent payments
        
    - mortgage payments
        
    - purchases
        
    - linked card transactions
        
- Investigate **business logic flaws**
    
    - incorrect points calculation
        
    - negative balance manipulation
        
    - reward abuse
        
    - tier status bypass
        

---

# Things That Might Be Interesting Security Targets

From a security perspective, these areas are **very interesting**:

### 1. Points & Rewards Logic

Potential bugs:

- earn more points than expected
    
- duplicate rewards
    
- bypass point limits
    
- negative points abuse
    
- refund logic issues
    

### 2. Payment System

Look at:

- Rent payments
    
- Mortgage payments
    
- Payment Account
    
- Linked cards
    
- Transaction monitoring
    

Possible bugs:

- fake payment confirmation
    
- payment replay
    
- unauthorized rent payment
    
- bypass payment limits
    

---

### 3. Card Linking System

They allow users to link:

- Visa
    
- Mastercard
    
- Amex
    
- Discover
    

Possible bugs:

- linking someone else's card
    
- bypass eligibility rules
    
- card unlink logic flaws
    
- transaction monitoring bypass
    

---

### 4. Account Logic

Potential vulnerabilities:

- multiple accounts abuse
    
- points farming
    
- referral abuse
    
- reward stacking
    

They **explicitly mention abuse of multiple accounts**, which means this system may have logic flaws.

---

### 5. Transaction Monitoring System

They rely on third parties:

- Fidel Ltd
    
- Rewards Network
    
- Mastercard
    
- Visa
    
- Amex
    
- Discover
    

Possible attack surface:

- delayed transaction verification
    
- duplicate transaction processing
    
- incorrect merchant validation
    

---

# Things That Are NOT Allowed

According to the terms, the following can get you banned or legal trouble:

### Fraud or Abuse

They explicitly forbid:

- buying or selling points
    
- transferring points to unauthorized accounts
    
- creating multiple accounts to farm rewards
    
- abusing promotions
    

---

### Illegal Actions

Not allowed:

- violating laws
    
- damaging the platform intentionally
    
- fraudulent transactions
    
- payment manipulation for fraud
    

---

### Program Manipulation

They specifically mention misuse like:

- artificially generating points
    
- exploiting reward mechanics
    
- misrepresenting transactions
    

---

# What Happens If They Think You Abused It

They can:

- suspend your account
    
- terminate your membership
    
- remove all points
    
- create a **negative point balance**
    
- charge you for points
    
- pursue legal action
    

---

# Important Legal Restrictions

### Arbitration Clause

If you have a dispute:

- You **cannot sue in court**
    
- No **jury trial**
    
- No **class action**
    

Everything goes to **private arbitration in New York**.

---

# Critical Things Bug Bounty Hunters Should Notice

These lines are **very revealing** for security research:

They mention possible abuse cases:

- illegitimate accumulation of points
    
- repeated account creation
    
- transferring points improperly
    
- payment misuse
    

When companies write these rules, it often means:

**They already had abuse issues in the past.**

---

# Very Interesting Attack Surfaces

Based on the program design, the **most promising areas** for bugs:

1️⃣ **Points earning logic**

2️⃣ **Refund → points reversal**

3️⃣ **Linked card transaction monitoring**

4️⃣ **Rent payment processing**

5️⃣ **Multiple account detection**

6️⃣ **Tier status calculation**

7️⃣ **Payment account credentials**

---

# Security Mindset for This Platform

Think like this when testing:

```
How can I earn points without paying?
How can I duplicate rewards?
How can I manipulate transaction verification?
How can I bypass account restrictions?
How can I abuse reward redemption?
```

Those questions lead to **business logic bugs**, which are often **high bounty payouts**.

---
