# Single Responsibility Principle 😉
> The Single Responsibility Principle states that each software module should have one and only reason to change 🤔
## Example in Golang
1. **Without SRP** - */no-rsp* 😶
    * There we have a `EmailService` struct, that have a single method called `Send`
    * The responsibility of `EmailService` is not just send emails otherwise to store an email message into DB **and** it via SMTP
    <br/>
      > As soon as describing the responsibility of some code struct requires the usage of the world "and", it already breaks the SRP
    
    * **WE BROKEN SRP!!!** 😟
      1. Function `send` is responsibility for both storing a message in DB and send message over SMTP Protocol.
      2. Structure `EmailService`, it also has two responsibility, storing inside DB and sending message.
    
    * **Consequences**
      1. When change a table structure or type of storage, we need to *change a code for sending email over SMTP*.
      2. When we need to integrate Mailgun or Mailjet, we need to *change a code for storing data in the MySQL DB*
      3. If we choose different integration of sending emails in the application, each integration need to have a logic to store in database.
      4. Suppose we decided to split the application's responsibility in two teams, one for maintaining a database and the second one for integrating email providers. In that case, they will work on the same code.
      5. The code is practically untestable with unit test.

2. **With SRP** - */rsp* 😎
    * Here we provide two new structs. The first one is `EmailDBRepository` as an implementation for the `EmailRepository` interface. It includes support for persisting data in the underlying database.

    * The second structure is `EmailSMTPSender` that implements the `EmailSender` interface. This struct is responsible for only email sending over SMPT protocol.

    * Finally, the new `EmailService` contains interfaces from above and delegates the request for email sending.

    - ##### Note
        A question may appear: does `EmailService` still have **multiple** **responsibilities**, as it still holds a logic for storing and sending emails? Does it look like we just made an abstraction, but **the duties are still** there?

        Here, that is not the case. `EmailService` does not hold the responsibility for storing and sending emails. **It delegates** them to the structs below. Its responsibility is to delegate requests for processing emails to the underlying services.

        > There is a difference between holding and delegating responsibility. If an adaptation of a particular code can remove the whole purpose of responsibility, we talk about holding. If that responsibility still exists even after removing a specific code, then we talk about delegation.

        <span>*Source : [Practical SOLID in Golang: Single Responsibility Principle](https://levelup.gitconnected.com/practical-solid-in-golang-single-responsibility-principle-20afb8643483)*</span>
