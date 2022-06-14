# [Get Started With Apex Triggers](https://trailhead.salesforce.com/en/content/learn/modules/apex_triggers/apex_triggers_intro)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/apex_trigger_1.PNG)

## Code
```
trigger AccountAddressTrigger on Account (before insert, before update) {
    for(Account account: Trigger.New){
        if(account.Match_Billing_Address__c==True){
            account.ShippingPostalCode = account.BillingPostalCode;
        }
    }
}

```


