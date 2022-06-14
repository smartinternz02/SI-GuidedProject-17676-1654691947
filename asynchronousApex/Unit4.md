# [Control Process With Queueable Apex](https://trailhead.salesforce.com/content/learn/modules/asynchronous_apex/async_apex_queueable?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/asynchronous_apex_4.PNG)

## Code

### apex class
```
public without sharing class AddPrimaryContact implements Queueable {
    private Contact contact;
    private String state;
    
    public AddPrimaryContact(Contact inputcontact,String inputstate){
        this.contact=inputcontact;
        this.state=inputstate;
    }
    public void execute(QueueableContext context){
        List<Contact> contacts=new List<Contact>();
        
        List<Account> accounts=[SELECT Id FROM Account WHERE BillingState= :state LIMIT 200];
        
        
        for (Account acc: accounts){
            Contact clonecontact=contact.clone();
            clonecontact.AccountId=acc.Id;
            contacts.add(clonecontact);
        }
        insert contacts;
    }
    
}

```
### test class

```
@isTest
private class AddPrimaryContactTest {
    @isTest
    private static void testQueueableClass(){
        List<Account> accounts=new List<Account>();
        for(Integer i=0;i<500;i++){
            Account acc=new Account(Name='Test Account');
            if(i<250){
                acc.BillingState='NY';
            }
            else{
                acc.BillingState='CA';
            }
            accounts.add(acc);
        }
        insert accounts;
        
        Contact contact=new Contact(FirstName='Deependra',LastName='Parichha');
        insert contact;
        
        Test.startTest();
        Id jobId=System.enqueueJob(new AddPrimaryContact(contact,'CA'));
        Test.stopTest();
        
        List<Contact> contacts=[SELECT Id FROM Contact WHERE Contact.Account.BillingState='CA' ];
        System.assertEquals(200,contacts.size());
    }
}

```


