# [Apex Web Services](https://trailhead.salesforce.com/content/learn/modules/apex_integration_services/apex_integration_webservices?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/apex_integration_services_4.PNG)

## Code

### apex class
```
@RestResource(urlMapping='/Account/*/contacts')
global with sharing class AccountManager {
    @HttpGet
    global static Account getAccount() {
        RestRequest request = RestContext.request;
        // grab the caseId from the end of the URL
        String accountId = request.requestURI.substringBetween('Accounts/','/contacts');
        Account result =  [SELECT Id,Name,(SELECT Id,Name FROM Contacts)
                        FROM Account
                        WHERE Id = :accountId];
        return result;
        
    }
}

```
### test class

```
@IsTest
private class AccountManagerTest {
    @isTest static void testGetContactsByAccountId() {
        Id recordId = createTestRecord();
        // Set up a test request
        RestRequest request = new RestRequest();
        request.requestUri =
            'https://yourInstance.my.salesforce.com/services/apexrest/Accounts/'+recordId+'/contacts';
        request.httpMethod = 'GET';
        RestContext.request = request;
        // Call the method to test
        Account thisAccount = AccountManager.getAccount();
        // Verify results
        System.assert(thisAccount != null);
        System.assertEquals('Test record', thisAccount.Name);
    }
    
    // Helper method
    static Id createTestRecord() {
        // Create test record
        Account caseTest = new Account(
            Name='Test record');
        insert caseTest;
        Contact contactcase=new Contact(FirstName='Deependra',LastName='Parichha',AccountId=casetest.Id);
        insert contactcase;
        return caseTest.Id;
    }          
}
```

