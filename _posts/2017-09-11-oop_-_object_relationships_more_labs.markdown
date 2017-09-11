---
layout: post
title:  "OOP - Object Relationships (more labs)"
date:   2017-09-11 22:07:59 +0000
---


*----- OO - Kickstarter Lab*

This lab was easier to understand. I was able to utilize the test spec (in ##) to understand the lab as though it were pseudocode.

```
class Backer

  attr_accessor :backed_projects, :name #so projects can be added to a Backer's list and so that the backer can report on the projects they back.

  def initialize(name)
    @backed_projects = []
    @name = name
  end

  #when backer added a project to its list of backed projects, that project also adds the backer to its list of backers

  def back_project(project)
    @backed_projects << project
      # backer = self.new(name) ##logan = Backer.new("Logan")
      # project_name = backer ##hoverboard = Project.new("Awesome Hoverboard")
      # backer.back_project ##logan.back_project(hoverboard)
      project.backers << self #(backer) ##expect(hoverboard.backers).to include(logan)
  end

end

class Project

  attr_accessor :backers, :title #so Projects can add backers to their list of backers and report on the backers who support them.

  def initialize(title)
    @backers = []
    @title = title
  end

  def add_backer(backer)
    @backers << backer
    #new_project = Projects.new ##ropes = Project.new("All The Ropes")
    #backer = Backer.new(name) ##arel# = Backer.new("Arel")
    #new_project.add_backer(backer) #ropes.add_backer(arel)
    backer.backed_projects << self #(new_project) ##expect(arel.backed_projects).to include(ropes)
  end

end
```

Breakdown:

*Backer class*

```.back_project(project) ```

* simple test: accepts a Project as an argument and stores it in a backed_projects array ``` @backed_projects ```

   ``` @backed_projects << project ```

* advanced test: adds the backer to the project's backers array

   * test spec that I used to understand how to execute its codes
   
   ``` logan = Backer.new("Logan") ```
	 
	 We create/instantiating a backer object (logan) of the Backer class =>  ``` backer = self.new(name) ```
	 
   ``` hoverboard = Project.new("Awesome Hoverboard") ```
	 
	 We create/instantiating a project object (hoverboard) of the Project class =>  ``` project_name = backer ```
	 
   ``` logan.back_project(hoverboard) ```
	 
	 the backer object (logan) calls on the back_project method of the Backer class, while passing in the project object (hoverboard) => ``` backer.back_project ```
	 
	 ``` expect(hoverboard.backers).to include(logan) ```
	 
	 the expectation is that the project object (hoverboard) calls on the backers method of the Project class => ``` project.backers << self #(backer) ```
    
   * final code
   
	 ```
	  def back_project(project)
		 @backed_projects << project
		 project.backers << self #(backer)
		end
	 ```

*Project class*

* simple test: accepts a Backer as an argument and stores it in a backers array ``` @backers ```

   ``` @backers << backer ```

* also adds the project to the backer's backed_projects array

   * test spec that I used to understand how to execute its codes
   
   ``` ropes = Project.new("All The Ropes") ```
	 
	 We create/instantiating a project object (ropes) of the Project class => ``` new_project = Projects.new ```
	 
   ``` arel = Backer.new("Arel") ```
	 
	 We create/instantiating a backer object (arel) of the Backer class => ``` backer = Backer.new(name) ```
	 
   ``` ropes.add_backer(arel) ```
	 
	 the project object (ropes) calls on the add_backer method of the Project class, while passing in the backer object (arel) => ``` new_project.add_backer(backer) ```
	 
	 ``` expect(arel.backed_projects).to include(ropes) ```
	 
	 the expectation is that the backer object (arel) calls on the backed_projects method of arrays of the Backer class => ``` backer.backed_projects << self ``` self = #(new_project)
    
   * final code
   
	 ```
	  def back_project(project)
	   @backers << backer
	   backer.backed_projects << self
	  end
	 ```

*----- end lab*
	 
*----- OO - Banking lab*
	 
Reading the tests and synthesizing the descriptions and expectations in a way of pseudocode similar to the above lab, got me through this lab. I struggled with the valid? method from BankAccount class and a couple of the methods from the Transfer class. I had to remember how transfers are made via bank accounts to execute the Transfer class methods.

So my takeaway from this lab is to:

* **continue synthesizing the test specs as a way of pseudocode**

* **ALWAYS consider real-world applications/process**

   * in this case, remember that transfers are made if you meet the 3 expectations:
      
      * your account must be valid (how is it valid? Look at BankAccount class to determine the validity)
      * your status should be pending
      * the amount you're transferring should be less than your balance (because you need to still have money in your account and you don't want to transfer more that your balance)
      * when your account is valid, status is pending, and the amount you plan to transfer is less than your balance, you can make the transfer. Your status will change to say your transfer is complete.
      * If none of the creiters are met, your transfer is rejected and the bank will notify you to check your account balance.


Below is my process. 

```
class BankAccount
  #one instance of the class can transfer money to another instance through a Transfer class

  attr_accessor :balance, :status
  attr_reader :name #can't change its name; expect { avi.name = "Bob" }.to raise_error

  def initialize(name)
    @name = name #initializes with a name; expect(avi.name).to eq("Avi")
    @balance = 1000 #always initializes with balance of 1000; expect(avi.balance).to eq(1000)
    @status = "open" #always initializes with a status of 'open'; expect(avi.status).to eq("open")
  end

  def deposit(deposit_amount) #can deposit money into its account
    self.balance += deposit_amount #(another way to write it = @balance = @balance + deposit)
    # expect(avi.balance).to eq(1000)
    # avi.deposit(1000)
    # expect(avi.balance).to eq(2000)
  end

  def display_balance #can display its balance
    "Your balance is $#{@balance}."
    # expect(avi.display_balance).to eq("Your balance is $#{avi.balance}.")
  end

  def valid? # is valid with an open status and a balance greater than 0
    # @broke = BankAccount.new("Kat Dennings");     @broke = self.new(name)
    # @broke.balance = 0;     @broke.balance = 0
    # @closed = BankAccount.new("Beth Behrs")     @closed = self.new(name)
    # @closed.status = "closed";     @closed.status = "closed"
    # expect(avi.valid?).to eq(true)
    # expect(@broke.valid?).to eq(false)
    # expect(@closed.valid?).to eq(false)
    if self.status == "open" && self.balance > 0
      true
    else self.status != "open" && self.balance <= 0
      false
    end
  end

  def close_account #can close its account
    # avi.close_account
    # expect(avi.status).to eq("closed")
    self.status = "closed"
  end

end

----

class Transfer
  # your code here

  #Transfer class acts as a space for a transaction between two instances of the BankAccount class

  attr_accessor :status
  attr_reader :sender, :receiver, :amount

  def initialize(sender, receiver, amount) #initializes with a sender, receiver, amount
    @sender = sender #expect(transfer.sender).to eq(amanda)
    @receiver = receiver #expect(transfer.receiver).to eq(avi)
    @status = "pending" #always initializes with a status of 'pending'; expect(transfer.status).to eq("pending") #Transfer start as a "pending" status
    @amount = amount #expect(transfer.amount).to eq(50)
  end

  def valid? #can check that both accounts are valid
    # expect(avi.valid?).to eq (true) => receiver
    # expect(amanda.valid?).to eq(true) => sender
    # expect(transfer.valid?).to eq(true)
    # - calls on the sender and receiver's #valid? methods

    # transfer_class = File.read("lib/transfer.rb")
    #
    # expect(amanda).to receive(:valid?).and_return(true)
    # expect(avi).to receive(:valid?).and_return(true)
    #
    # transfer.valid?

    sender.valid? && receiver.valid? #valid? from BankAccount class instance method
  end

  #Transfer instances runs checks first before transfer is complete
  #Transfer instances can reject a transfer if the BankAccount accounts aren't valid or if sender doesn't have the money.
  #When Transfer executed, status turns into a "complete" status/state
  #statuses = "pending", "complete", "rejected", "reversed"
  #completed transfer can be reversed and go into "reversed" status

  def execute_transaction #can execute a successful transaction between two accounts

    #if the accounts are valid, the sender's balance is greater than the amount to send (if the sender doesn't have the amount to send, reject) and its status is pending, reduce the sender's balance by the amount, credit the receiver's balance by the amount; change status to complete. Otherwise, reject the transfer.

      # transfer.execute_transaction
      # expect(amanda.balance).to eq(950) sender
      # expect(avi.balance).to eq(1050) receiver
      # expect(transfer.status).to eq("complete")

    if valid? && sender.balance > amount && self.status == "pending" #Transfer instances can reject a transfer if the BankAccount accounts aren't valid or if sender doesn't have the money. This valid? is the Transfer class instance method of valid?
      sender.balance -= amount
      receiver.balance += amount
      self.status = "complete"
    else
      reject_transfer
    end

    # - each transfer can only happen once
    #   transfer.execute_transaction
    #   expect(amanda.balance).to eq(950) sender
    #   expect(avi.balance).to eq(1050) receiver
    #   expect(transfer.status).to eq("complete")
    #   transfer.execute_transaction
    #   expect(amanda.balance).to eq(950)
    #   expect(avi.balance).to eq(1050)

    # - rejects a transfer if the sender doesn't have a valid account
    #   expect(bad_transfer.execute_transaction).to eq("Transaction rejected. Please check your account balance.")
    #   expect(bad_transfer.status).to eq("rejected")
  end

  def reject_transfer
    self.status = "rejected" #the Transfer class status is rejected
    "Transaction #{self.status}. Please check your account balance."
  end

  def reverse_transfer #can reverse a transfer between two accounts   #completed transfer can be reversed and go into "reversed" status

    #if the accounts are valid, the receiver's balance is greater than the amount to send (if the receiver doesn't have the amount to send, reject) and its status is complete, reduce the receiver's balance by the amount, credit the sender's balance by the amount; change status to reversed. Otherwise, reject the transfer.

    # transfer.execute_transaction
    # expect(amanda.balance).to eq(950) sender
    # expect(avi.balance).to eq(1050) receiver
    # transfer.reverse_transfer
    # expect(avi.balance).to eq(1000)
    # expect(amanda.balance).to eq(1000)
    # expect(transfer.status).to eq("reversed")

    if valid? && receiver.balance > amount && self.status == "complete"
      receiver.balance -= amount
      sender.balance += amount
      self.status = "reversed"
    else
      reject_transfer
    end
    # - it can only reverse executed transfers
    #   transfer.reverse_transfer
    #   expect(amanda.balance).to eq(1000)
    #   expect(avi.balance).to eq(1000)

    #self.execute_transaction
  end

end


```
	 
*----- end lab*
