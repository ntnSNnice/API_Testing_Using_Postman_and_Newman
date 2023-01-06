# Content for API Testing Project 1
- [Introduction](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#introduction)    
- [Summary](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#summary)      
- [Requirements](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#requirements)
- [Environment Variables](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman#environment-variables)
- [Pre-request Script Details](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#pre-request-script-details)
  - [Create Booking](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#create-booking)   
  - [Update Booking](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#updating-booking)  
  - [Partially Update Booking](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#partially-updating-booking)
- [Request Body](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#request-body)
- [Assertions Details](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#assertions-details)   
  - [Create Booking](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/#create-booking-1)   
  - [Access Token Generate](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman#access-token-generate)   
  - [Update Booking](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman#update-booking)  
  - [Partial Update Booking](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman#partial-update-booking)   
  - [Delete Booking](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman#delete-booking)  
- [Create Test Suites](https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman#create-test-suites)      
      

# Introduction
This documentation explains how to run an API test with Postman on a booking website.    

# Summary    
I have completed an API testing on a Book booking website.
The following website is the website i have tested.
https://restful-booker.herokuapp.com/

**Tasks Done**
- CRUD Operation such as Create, Get, Put & Patch, Delete.
- Writing Pre-request Scripts using Dynamic Parameter.
- API Request & Response Chaining.
- Writing Test Scripts For Data Validation.
- Newman HTML & HTML Extra Report.
  
| Total API Requests | Pre-request scripts | Assertions | Failed tests | Skipped tests | 
| :-------- | :------- |  :------- | :------- | :------- | 
| 6 | 3 | 28 | 0 | 0 |

Here in the mentioned API, books can be booked with a centain check in and check out. Also, Bookings can be viewed, updated, and deleted .

  
<p align="center">
  <img src="https://github.com/ntnSNnice/API_Testing_Using_Postman_and_Newman/blob/main/API%20testing%20project%20-1/Newman%20html%20reports/Newman%20html%20extra.png" />
</p>


# Requirements  
**Postman**   
https://www.postman.com/   
**Node JS**   
https://nodejs.org/en/    

## Environment Variables

To run this project, you will need to add the following environment variables to your .env file

`Token`
`newBookingID`
`baseURL`
`first_name`
`last_name`
`total_price`
`deposit_paid`
`Check_In`
`Check_Out`
`additional_needs`

# Pre-request Script Details   
#### Create Booking
```bash

//set random FirstName variable
var firstname = pm.variables.replaceIn("{{$randomFirstName}}");
pm.environment.set("first_name",firstname);

//set random LastName variable
var lastname = pm.variables.replaceIn("{{$randomLastName}}");
pm.environment.set("last_name",lastname);

//set random integer as price variable
var totalprice = pm.variables.replaceIn("{{$randomInt}}");
pm.environment.set("total_price",totalprice);


//set random boolean as payment variable
var depositpaid = pm.variables.replaceIn("{{$randomBoolean}}");
pm.environment.set("deposit_paid",depositpaid);


//set the time
const moment = require('moment');
const today = moment();

//set check in date
pm.environment.set("Check_In",today.format("YYYY-MM-DD"));
//set check out date
pm.environment.set("Check_Out",today.add(_.random(1,30),'day').add(_.random(1,12),'month').format("YYYY-MM-DD"));


//set 3 item set to select a random among these 3 items
const items = ["Breakfast", "Lunch", "Dinner"];
pm.environment.set("additional_needs",_.shuffle(items)[0]);


```



#### Updating Booking
```bash
//set random FirstName variable
var firstname = pm.variables.replaceIn("{{$randomFirstName}}");
pm.environment.set("first_name",firstname);

//set random LastName variable
var lastname = pm.variables.replaceIn("{{$randomLastName}}");
pm.environment.set("last_name",lastname);

//set random integer as price variable
var totalprice = pm.variables.replaceIn("{{$randomInt}}");
pm.environment.set("total_price",totalprice);


//set random boolean as payment variable
var depositpaid = pm.variables.replaceIn("{{$randomBoolean}}");
pm.environment.set("deposit_paid",depositpaid);


//set the time
const moment = require('moment');
const today = moment();

//set check in date
pm.environment.set("Check_In",today.format("YYYY-MM-DD"));
//set check out date
pm.environment.set("Check_Out",today.add(_.random(1,30),'day').add(_.random(1,12),'month').format("YYYY-MM-DD"));


//set 3 item set to select a random among these 3 items
const items = ["Breakfast", "Lunch", "Dinner"];
pm.environment.set("additional_needs",_.shuffle(items)[0]);
```


#### Partially Updating Booking
```bash
//set random FirstName variable
var firstname = pm.variables.replaceIn("{{$randomFirstName}}");
pm.environment.set("first_name",firstname);
```


# Request Body
#### Create, Update
```bash
{
    "firstname" : "{{first_name}}",
    "lastname" : "{{last_name}}",
    "totalprice" : {{total_price}},
    "depositpaid" : {{deposit_paid}},
    "bookingdates" : {
        "checkin" : "{{Check_In}}",
        "checkout" : "{{Check_Out}}"
    },
    "additionalneeds" : "{{additional_needs}}"
    
}
```
# Request Body
Partial Update
```bash
{
    "firstname" : "{{first_name}}"
}
```


# Assertions Details    
#### Create Booking        
```bash
 var jsonData = pm.response.json();
pm.environment.set("newBookingID", jsonData.bookingid);




var code = pm.response.code;
switch(code)
{
    case 200:
            var jsonData = pm.response.json();
            //status code matched or not
            pm.test("Status code is 200", function () {
                pm.response.to.have.status(200);
            });
            
            //firstname matched or not
            pm.test("First Name Validation", function () {
                pm.expect(jsonData.booking.firstname).to.eql(pm.environment.get("first_name"));
            });

            //lastname matched or not
            pm.test("Last Name Validation", function () {
                pm.expect(jsonData.booking.lastname).to.eql(pm.environment.get("last_name"));
            });

            //total price matched or not
            var totalprice = pm.environment.get("total_price");
            pm.test("Total Price Validation", function () {
                pm.expect(jsonData.booking.totalprice).to.eql(parseInt(totalprice));
            });``

            //boolean value matched or not
            var depositpaid = pm.environment.get("deposit_paid");
            pm.test("Deposit Paid Validation", function () {
                pm.expect(jsonData.booking.depositpaid).to.eql(JSON.parse(depositpaid));
            });

            //check in date matched or not
            pm.test("CheckIn Validation", function () {
                pm.expect(jsonData.booking.bookingdates.checkin).to.eql(pm.environment.get("Check_In"));
            });

            //check out date matched or not
            pm.test("CheckOut Validation", function () {
                pm.expect(jsonData.booking.bookingdates.checkout).to.eql(pm.environment.get("Check_Out"));
            });

            //additional needs matched or not
            pm.test("Additional Needs Validation", function () {
                pm.expect(jsonData.booking.additionalneeds).to.eql(pm.environment.get("additional_needs"));
            });
            break;
    
    case 404:
           pm.test("Not Found");
           break;
    
    default:
           pm.test("Something went wrong");


}
```



#### Access Token Generate    
```bash   
var jsonData = pm.response.json()

//set token to environment
pm.environment.set("Token", jsonData.token)

//token matched or not
pm.test("Verify TokenID", function () { 
    pm.expect(jsonData.token).to.eql(pm.environment.get('Token'));
});

var StateCode = responseCode.code
//status code matched or not
switch(StateCode){
    case 200:
       pm.test("Verify status code 200", function () {
       console.log('StateCode')
       });
       break;
    case 201:
      pm.test("Verify status code 201", function () {
      console.log('StateCode')
      });
      break;
}
```   



#### Update Booking   
```bash
 var jsonData = pm.response.json();
pm.environment.set("newBookingID", jsonData.bookingid);




var code = pm.response.code;
switch(code)
{
    case 200:
            var jsonData = pm.response.json();
            //status code matched or not
            pm.test("Status code is 200", function () {
                pm.response.to.have.status(200);
            });
            
            //firstname matched or not
            pm.test("First Name Validation", function () {
                pm.expect(jsonData.firstname).to.eql(pm.environment.get("first_name"));
            });

            //lastname matched or not
            pm.test("Last Name Validation", function () {
                pm.expect(jsonData.lastname).to.eql(pm.environment.get("last_name"));
            });

            //total price matched or not
            var totalprice = pm.environment.get("total_price");
            pm.test("Total Price Validation", function () {
                pm.expect(jsonData.totalprice).to.eql(parseInt(totalprice));
            });``

            //boolean value matched or not
            var depositpaid = pm.environment.get("deposit_paid");
            pm.test("Deposit Paid Validation", function () {
                pm.expect(jsonData.depositpaid).to.eql(JSON.parse(depositpaid));
            });

            //check in date matched or not
            pm.test("CheckIn Validation", function () {
                pm.expect(jsonData.bookingdates.checkin).to.eql(pm.environment.get("Check_In"));
            });

            //check out date matched or not
            pm.test("CheckOut Validation", function () {
                pm.expect(jsonData.bookingdates.checkout).to.eql(pm.environment.get("Check_Out"));
            });

            //additional needs matched or not
            pm.test("Additional Needs Validation", function () {
                pm.expect(jsonData.additionalneeds).to.eql(pm.environment.get("additional_needs"));
            });
            break;
    
    case 404:
           pm.test("Not Found");
           break;
    
    default:
           pm.test("Something went wrong");


}
``` 

#### Partial Update Booking   
```bash
var code = pm.response.code;
switch(code)
{
    case 200:
            var jsonData = pm.response.json();

            //status code matched or not
            pm.test("Status code is 200", function () {
                pm.response.to.have.status(200);
            });

            pm.test("First Name Validation", function () {
                pm.expect(jsonData.firstname).to.eql(pm.environment.get("first_name"));
            });
            break;
    
    case 404:
           pm.test("Not Found");
           break;
    
    default:
           pm.test("Something went wrong");


}

```




#### Delete Booking 
```bash

var Code = responseCode.code

//status code matched or not
switch(StateCode){
    case 200:
       pm.test("Verify status code 200", function () {
       console.log('Code')
       });
       break;
    case 201:
      pm.test("Verify status code 201", function () {
      console.log('Code')
      });
      break;
}
```   

# Create Test Suites   

### Using Newman   


  Newman is a command-line Collection Runner for Postman. It enables you to run and test a Postman Collection directly from the command line.
#### Install Command    
```bash
npm install -g newman    
```
#### Run Command    
- newman run “Path/CollectionName.json” -e Path/EnvironmentName.json
- newman run “Collection Link” -e “Path”/EnvironmentName.json    

#### Create HTML Report  
 
#### Install Command      
```bash
npm install -g newman-reporter-html
```
**or**   
```bash
npm install -g newman-reporter-htmlextra    
```
#### Run Command      
```bash
newman run “Collection Link” -e “Path”/EnvironmentName.json -r cli,html    
```
**or**    
```bash
newman run “Collection Link” -e “Path”/EnvironmentName.json -r cli,htmlextra    
```
