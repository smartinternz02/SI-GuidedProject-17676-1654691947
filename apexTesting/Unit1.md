# [ Get Started with Apex Unit Tests](https://trailhead.salesforce.com/content/learn/modules/apex_testing/apex_testing_intro?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/apex_testing_1.PNG)

## Code

```
trigger ClosedOpportunityTrigger on Opportunity (after insert,after update) {
    
    List<Task> taskList=new List<Task>(); 

    for(Opportunity Opp:Trigger.New){
        if(Trigger.isInsert || Trigger.isUpdate)
          if(opp.StageName=='Closed Won')
              taskList.add(new task(Subject='Follow Up Test Task',
                                 WhatId=opp.Id));
    }
    
    if(taskList.size()>0)
        insert taskList;

}
```
