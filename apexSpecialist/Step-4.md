# [Apex Specialist](https://trailhead.salesforce.com/en/content/learn/superbadges/superbadge_apex)


### WarehouseSyncShedule.apxc :-

```
global with sharing class WarehouseSyncSchedule implements Schedulable{
    global void execute(SchedulableContext ctx){
        System.enqueueJob(new WarehouseCalloutService());
    }
}

```