# [Use Future Methods](https://trailhead.salesforce.com/content/learn/modules/asynchronous_apex/async_apex_future_methods?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/asynchronous_apex_2.PNG)

## Code

### apex class
```
public class AccountProcessor {
    @future
    public static void countContacts(List<Id> accountIds){
        List<Account> accounts=[SELECT Id,(SELECT Id FROM Contacts) FROM Account WHERE Id IN:accountIds];
            for(Account acc:accounts){
                acc.Number_Of_Contacts__c=acc.Contacts.size();
            }
        update accounts;
    }
}

```
### test class

```
@isTest
private class AccountProcessorTest {
    
    @isTest
    private static void countContactsTest() {
        
        List<Account> accounts=new List<Account>();
        for(Integer i=0;i<300;i++){
            accounts.add(new Account(Name='TestContact'+i));
        }
        insert accounts;
        
        List<Contact> contacts=new List<Contact>();
        List<Id> accountids=new List<Id>();
        for(Account acc:accounts){
            contacts.add(new Contact(FirstName=acc.Name,LastName='TestContact',AccountId=acc.Id));
            accountids.add(acc.Id);
        }
        insert contacts;
        
        Test.startTest();
        AccountProcessor.countContacts(accountids);
        Test.stopTest();
   }

}
```


