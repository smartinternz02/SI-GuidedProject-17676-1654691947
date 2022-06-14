# [Apex Soap Callouts](https://trailhead.salesforce.com/content/learn/modules/apex_integration_services/apex_integration_soap_callouts?trailmix_creator_id=trailblazerconnect&trailmix_slug=salesforce-developer-catalyst)

## Problem Statement

![Image](https://github.com/DeependraParichha1004/Trailhead-Solutions/blob/main/Img/apex_integration_services_3.PNG)

## Code

### apex class
```
public class ParkLocator {
    public static string[] country(String country){
        parkService.ParksImplPort park= new parkService.ParksImplPort();
        return park.byCountry(country);
    }
}

```
### test class

```
@isTest
public class ParkLocatorTest {
    @isTest static void testcallout(){
        Test.setMock(WebServiceMock.class, new ParkServiceMock());
        String country='United States';
        List<String> result=ParkLocator.country(Country);
        List<String> expectedres=new List<String>{'Yellowstone', 'Mackinac National Park', 'Yosemite'};
        System.assertEquals(result,expectedres);
    }
}

```
###  unit tests

```
@isTest
global class ParkServiceMock implements WebServiceMock {
   global void doInvoke(
           Object stub,
           Object request,
           Map<String, Object> response,
           String endpoint,
           String soapAction,
           String requestName,
           String responseNS,
           String responseName,
           String responseType) {
        // start - specify the response you want to send
        ParkService.byCountryResponse response_x=new ParkService.byCountryResponse();
        response_x.return_x = new List<String>{'Yellowstone', 'Mackinac National Park', 'Yosemite'};
        // end
        response.put('response_x', response_x); 
   }
}

```

