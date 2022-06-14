#[Test Apex Triggers](https://trailhead.salesforce.com/content/learn/modules/apex_testing/apex_testing_triggers?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/apex_testing_2.PNG)

## Code

### trigger
```
trigger RestrictContactByName on Contact (before insert, before update) {
    //check contacts prior to insert or update for invalid data
	For (Contact c : Trigger.New) {
		if(c.LastName == 'INVALIDNAME') {	//invalidname is invalid
			c.AddError('The Last Name "'+c.LastName+'" is not allowed for DML');
		}

	}
}

```
### test case
```
@isTest
public class TestRestrictContactByName {
    @isTest static void TestRestrictContactByName_test1(){
        Contact con=new Contact(FirstName='Deepu',LastName='INVALIDNAME');
        insert con;
    }
}

```