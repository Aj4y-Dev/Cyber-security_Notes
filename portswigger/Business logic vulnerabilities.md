Business logic vulnerabilities are **flaws in the design or implementation of an application’s rules** that allow attackers to trigger unintended behavior.

They occur when:

- The application does something it _should not do_
- Or fails to prevent something it _should prevent_

“Business logic” simply means the **rules that define how the application works**.  
These rules are not always business-related — so they are also called:
- Application logic vulnerabilities
- Logic flaws
### Why They Are Hard to Detect

- They usually **don’t break the app**
- They work fine during normal usage
- Automated scanners often miss them
- They require **human thinking and understanding of the system**

## How Business Logic Works

Business logic enforces rules like:

- A user must log in before accessing dashboard
    
- A product must be paid before shipping
    
- A coupon must only be used once
    
- Quantity cannot be negative
    
- A user cannot escalate their role
    

If these rules are poorly implemented → logic flaw happens.