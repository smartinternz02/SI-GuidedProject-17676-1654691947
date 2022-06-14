# [Test Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_testing/apex_testing_data?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/apex_testing_3.PNG)

## Code

```
public class RandomContactFactory {
    public static List<Contact> generateRandomContacts(Integer numcnt,String l){
        List<Contact> contacts=new List<Contact>();
        for(Integer i=0;i<numcnt;i++){
            Contact cnt=new Contact(FirstName='Test '+i,LastName=l);
            contacts.add(cnt);
        }
        return contacts;
    }
}

```
