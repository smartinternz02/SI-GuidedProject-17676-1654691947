# [Use Batch Apex](https://trailhead.salesforce.com/content/learn/modules/asynchronous_apex/async_apex_batch?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/asynchronous_apex_3.PNG)

## Code

### apex class
```
public class LeadProcessor implements Database.Batchable<sObject> {
    public Database.QueryLocator start(Database.BatchableContext dbc){
        return Database.getQueryLocator([SELECT Id,Name FROM Lead]);
    }
    public void execute(Database.BatchableContext dbc,List<Lead> leads){
        for(Lead l:leads){
            l.LeadSource='Dreamforce';
        }
        update leads;
    }
    public void finish(Database.BatchableContext dbc){
        System.debug('Done');
    }
}

```
### test class

```
@isTest
private class LeadProcessorTest {
    @isTest
    private static void testBatchClass(){
        List<Lead> leads=new List<Lead>();
        for(Integer i=0;i<200;i++){
            leads.add(new Lead(LastName='Parichha',Company='Salesforce'));
        
        }
        insert leads;
        
        Test.startTest();
        LeadProcessor lp=new LeadProcessor();
        Id batchid=Database.executeBatch(lp,200);
        test.stopTest();
        
        List<Lead> updatedleads=[SELECT Id FROM Lead WHERE Leadsource='Dreamforce'];
        System.assertEquals(200, updatedleads.size());
    }
}
```


