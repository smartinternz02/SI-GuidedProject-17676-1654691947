# [Apex Rest Callouts](https://trailhead.salesforce.com/content/learn/modules/apex_integration_services/apex_integration_rest_callouts?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)
## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/apex_integration_services_2.PNG)

## Code

### apex class
```
public class AnimalLocator {
    public static String getAnimalNameById(Integer x){
        Http http=new Http();
        HttpRequest req=new HttpRequest();
        req.setEndpoint('https://th-apex-http-callout.herokuapp.com/animals/'+x);
        req.setMethod('GET');
        Map<String,Object> animal=new Map<String,Object>();
        HttpResponse res=http.send(req);
        if(res.getStatusCode() == 200) {
            // Deserializes the JSON string into collections of primitive data types.
            Map<String, Object> results = (Map<String, Object>) JSON.deserializeUntyped(res.getBody());
            
            
      animal = (Map<String,Object>) results.get('animal');
        }
return (String)animal.get('name');
    }
}

```
### test class

```
@isTest
private class AnimalLocatorTest {
    @isTest static void AnimalLocatorMock1(){
        Test.setMock(HttpCalloutMock.class,new AnimalLocatorMock());
        String result=AnimalLocator.getAnimalNameById(3);
        String expectedRes='chicken';
        System.assertEquals(result,expectedRes);
    }
}

```
###  unit tests

```
@isTest
global class AnimalLocatorMock implements HttpCalloutMock {
    global HttpResponse respond(HttpRequest request){
        HttpResponse response=new HttpResponse();
        response.setHeader('Content-Type','application/json');
        response.setBody('{"animals":["majestic badger","fluffy bunny","scary bear","chicken"]}');
        response.setStatusCode(200);
        return response;
    }

}

```

