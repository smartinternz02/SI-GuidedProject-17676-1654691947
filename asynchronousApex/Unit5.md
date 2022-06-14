# [Schedule Jobe Using the Apex Scheduler](https://trailhead.salesforce.com/content/learn/modules/asynchronous_apex/async_apex_scheduled?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/asynchronous_apex_5.PNG)

## Code

### apex class
```
public without sharing class DailyLeadProcessor implements Schedulable {

    public void execute(SchedulableContext ctx){
        List<Lead> leads=[SELECT id,LeadSource FROM Lead WHERE LeadSource=null LIMIT 200];
        for(Lead l:leads){
            l.LeadSource='Dreamforce';
        }
        update leads;
    }
}

```
### test class

```
@isTest
private class DailyLeadProcessorTest {
    private static String CRON_EXP='0 0 0 ? * * *';
     
    @isTest 
    private static void testSchedulabelClass(){
        List <Lead> leads=new List<Lead>();
        for(Integer i=0;i<500;i++){
            if(i<250){
                leads.add(new Lead(LastName='Parichha',Company='Salesforce'));
            }
            else{
                leads.add(new Lead(LastName='Parichha',Company='Salesforce',LeadSource='Other'));
            }
            
        }
        insert leads;
        
        Test.startTest();
        String jobid=System.schedule('Process Leads',CRON_EXP,new DailyLeadProcessor());
        Test.stopTest();
        
        List<Lead> updatedLeads=[SELECT ID,LeadSource FROM Lead WHERE LeadSource='Dreamforce'];
        System.assertEquals(200,updatedLeads.size());
        
        List<CronTrigger> cts=[SELECT Id,TimesTriggered,NextFireTime FROM CronTrigger WHERE Id=:jobid ];
        System.debug('Next Fire Time'+cts[0].NextFireTime);
    }
}

```


