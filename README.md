### **Rest Booking API Testing with Postman Newman**
This project demonstrates API testing using Postman, providing a collection of tests to validate various endpoints of the API. 

### **Features**

- Tests for GET, POST, PUT, PATCH, and DELETE requests
- Collection of tests covering different API endpoints
- Environment setup for easy switching between environments
- Pre-request scripts for data setup
- Test scripts for assertions and validations

## API Documentation:
- https://documenter.getpostman.com/view/22460689/2sA35BaPX7
  
### **Technology used:**
- Postman
- Newman
- JavaScript
- JSON

### **Prerequisite:**
- Node Js
- Newman
- Newman HTML Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
 ```console 
  git clone https://github.com/MinhazRafi/Automated-Testing-of-Rest-Booking-API-with-Newman-Report.git
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

## **Testing**

## Test Case Scenarios:

## _**1. Create New Booking**_

### Request URL: https://restful-booker.herokuapp.com/booking/
### Request Method: POST
### Pre-request Script:
```console 
var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
pm.environment.set("firstName", firstName)

var lastName = pm.variables.replaceIn("{{$randomLastName}}")
pm.environment.set("lastName", lastName)

var totalPrice = pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("totalPrice", totalPrice)

var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
pm.environment.set("depositPaid", depositPaid)
    
//Date
const date = require('moment')
const today = date()
    
// var checkinPast = today.subtract(7, 'd').format("YYYY-MM-DD")
var checkin = today.add(2, 'd').format("YYYY-MM-DD")
pm.environment.set("checkin", checkin)
    
var checkout = today.add(4, 'd').format("YYYY-MM-DD")
pm.environment.set("checkout", checkout)
    
var additionalNeeds = pm.variables.replaceIn("{{$randomProduct}}")
pm.environment.set("additionalNeeds", additionalNeeds)
```
  **Request Body:** 
 ```console
{
 "firstname" : "{{firstName}}",
 "lastname" : "{{lastName}}",
 "totalprice" : {{totalPrice}},
 "depositpaid" : {{depositPaid}},
 "bookingdates" : {
   "checkin" : "{{checkin}}",
   "checkout" : "{{checkout}}"
  },
 "additionalneeds" : "{{additionalNeeds}}"
}
```
  **Tests:**
 ```console
var jsonData = pm.response.json()
pm.environment.set("id", jsonData.bookingid)

var responseCode = pm.response.code
if(responseCode == 200){
    pm.test("Booking Data Created Successfully", function(){
        pm.response.to.have.status(200);
    })
} else {
    pm.test("Something went wrong. Please check again")
}
```
  **Response Body:**
 ```console 
{
    "bookingid": 5326,
    "booking": {
        "firstname": "Ulises",
        "lastname": "Donnelly",
        "totalprice": 238,
        "depositpaid": true,
        "bookingdates": {
            "checkin": "2024-03-25",
            "checkout": "2024-03-29"
        },
        "additionalneeds": "Mouse"
    }
}
```
 ## _**2. Get Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Pre-request Script: None
### Request Body: None
### Tests:
 ```console 
 var responseCode = pm.response.code
 console.log(responseCode)
        
 if(responseCode==200){
 var jsonData = pm.response.json()
        
 pm.test("First Name Validation", function(){
  pm.expect(pm.environment.get("firstName")).to.eql(jsonData.firstname)
 })
        
 pm.test("Last Name Validation", function(){
  pm.expect(pm.environment.get("lastName")).to.eql(jsonData.lastname)
 })
        
 pm.test("Total Price Validation", function(){
  pm.expect(pm.environment.get("totalPrice")).to.eql(jsonData.totalprice.toString())
 })
        
 pm.test("Deposit Paid Validation", function(){
   pm.expect(pm.environment.get("depositPaid")).to.eql(jsonData.depositpaid.toString())
 })
        
 pm.test("Check-In Date Validation", function(){
   pm.expect(pm.environment.get("checkin")).to.eql(jsonData.bookingdates.checkin)
 })
        
 pm.test("Check-Out Date Validation", function(){
    pm.expect(pm.environment.get("checkout")).to.eql(jsonData.bookingdates.checkout)
 })
        
 pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
 });
        
 pm.test("Additional Needs Validation", function(){
     pm.expect(pm.environment.get("additionalNeeds")).to.eql(jsonData.additionalneeds)
 })
 }
 else if(responseCode==404){
     pm.test("Not Found")
 }
 else if(responseCode==500){
     pm.test("Server Error")
 }
 else{
     pm.test("Something went wrong. Please check it")
 }
```
  **Response Body:**
 ```console
 {
    "firstname": "Ulises",
    "lastname": "Donnelly",
    "totalprice": 238,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-25",
        "checkout": "2024-03-29"
    },
    "additionalneeds": "Mouse"
}
```
## _**3. Create A Token For Authentication.**_
### Request URL: https://restful-booker.herokuapp.com/auth
### Request Method: POST
### Pre-request Script: None
### Tests: None
### Request Body:
 ```console 
{
	"username": "admin",
	"password": "password123"
}
```
  **Response Body:**
 ```console 
{
    "token": "b6e307fbd81912a"
}
```

 ## _**4. Update the Booking Details**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: PUT
### Pre-request Script:
```console 
var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
console.log(firstName)
pm.environment.set("firstName", firstName)
    
var lastName = pm.variables.replaceIn("{{$randomLastName}}")
console.log(lastName)
pm.environment.set("lastName", lastName)
    
var totalPrice = pm.variables.replaceIn("{{$randomInt}}")
pm.environment.set("totalPrice", totalPrice)
    
var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
pm.environment.set("depositPaid", depositPaid)
    
// Date
const date = require('moment')
const today = date()
    
// var checkinPast = today.subtract(7, 'd').format("YYYY-MM-DD")
var checkin = today.add(2, 'd').format("YYYY-MM-DD")
pm.environment.set("checkin", checkin)
    
var checkout = today.add(4, 'd').format("YYYY-MM-DD")
pm.environment.set("checkout", checkout)
    
var additionalNeeds = pm.variables.replaceIn("{{$randomProduct}}")
pm.environment.set("additionalNeeds", additionalNeeds)
```
  **Request Body:** 
 ```console
{
"firstname" : "{{firstName}}",
"lastname" : "{{lastName}}",
"totalprice" : {{totalPrice}},
"depositpaid" : {{depositPaid}},
"bookingdates" : {
  "checkin" : "{{checkin}}",
  "checkout" : "{{checkout}}"
   },
"additionalneeds" : "{{additionalNeeds}}"
}
```
  **Tests:** 
 ```console
    var responseCode = pm.response.code
    
    if(responseCode == 200)
    {
        pm.test("Booking Data Updated Successfully", function(){
            pm.response.to.have.status(200);
        })
    }
   else
    {
        pm.test("Something went wrong. Please check again")
    }
```
  **Response Body:**
 ```console 
  {
    "firstname": "Emory",
    "lastname": "Langworth",
    "totalprice": 851,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-25",
        "checkout": "2024-03-29"
    },
    "additionalneeds": "Chair"
}
```
## _**5. Check After The Update**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Request Body: None
### Pre-request Script: None
### Tests:
```console
var responseCode = pm.response.code
console.log(responseCode)
    
 if(responseCode==200){
  var jsonData = pm.response.json()
    
pm.test("First Name Validation", function(){
 pm.expect(pm.environment.get("firstName")).to.eql(jsonData.firstname)
})
    
pm.test("Last Name Validation", function(){
 pm.expect(pm.environment.get("lastName")).to.eql(jsonData.lastname)
})
    
pm.test("Total Price Validation", function(){
 pm.expect(pm.environment.get("totalPrice")).to.eql(jsonData.totalprice.toString())
})
    
pm.test("Deposit Paid Validation", function(){
 pm.expect(pm.environment.get("depositPaid")).to.eql(jsonData.depositpaid.toString())
})
    
pm.test("Check-In Date Validation", function(){
 pm.expect(pm.environment.get("checkin")).to.eql(jsonData.bookingdates.checkin)
})
    
pm.test("Check-Out Date Validation", function(){
 pm.expect(pm.environment.get("checkout")).to.eql(jsonData.bookingdates.checkout)
})
    
pm.test("Status code is 200", function () {
 pm.response.to.have.status(200);
});
    
pm.test("Additional Needs Validation", function(){
 pm.expect(pm.environment.get("additionalNeeds")).to.eql(jsonData.additionalneeds)
 })
}
  else if(responseCode==404)
{
  pm.test("Not Found")
}
  else if(responseCode==500)
{
  pm.test("Internal Server Error")
}
  else
{
  pm.test("Something went wrong. Please check it")
}
```
 **Response Body:** 
 ```console
{
    "firstname": "Emory",
    "lastname": "Langworth",
    "totalprice": 851,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-25",
        "checkout": "2024-03-29"
    },
    "additionalneeds": "Chair"
}
```

## _**6. Partial Update Booking Details**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: PATCH
### Pre-request Script:
```console
var partialUpdatedFirstName = pm.variables.replaceIn("{{$randomFirstName}}")
var partialUpdatedLastName = pm.variables.replaceIn("{{$randomLastName}}")
var partialUpdatedTotalPrice = pm.variables.replaceIn("{{$randomInt}}")
var partialUpdatedDepositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
var partialUpdatedAdditionalNeeds = pm.variables.replaceIn("{{$randomProduct}}")
    
// Date
const date = require('moment')
const today = date()
    
var partialUpdatedCheckIn = today.add(2, 'd').format("YYYY-MM-DD")
var partialUpdatedCheckOut = today.add(4, 'd').format("YYYY-MM-DD")
    
pm.environment.set("partialUpdatedAdditionalNeeds", partialUpdatedAdditionalNeeds)
pm.environment.set("partialUpdatedTotalPrice", partialUpdatedTotalPrice)
```
 **Request Body:** 
 ```console
 {
 "totalprice" : {{partialUpdatedTotalPrice}},
 "additionalneeds" : "{{partialUpdatedAdditionalNeeds}}"
 }
```
 **Tests:** 
 ```console
 var responseCode = pm.response.code
    
 if(responseCode == 200){
 pm.test("Booking Data Partial Updated Successfully", function(){
 pm.response.to.have.status(200);
 })
 }
else
{
  pm.test("Something went wrong. Please check again")
 }
```
 **Response Body:** 
 ```console
{
    "firstname": "Emory",
    "lastname": "Langworth",
    "totalprice": 236,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-25",
        "checkout": "2024-03-29"
    },
    "additionalneeds": "Ball"
}
```
## _**7. Check After The Partial Update**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Pre-request Script: None
### Request Body: None
### Tests:
```console
var responseCode = pm.response.code
console.log(responseCode)
    
if(responseCode==200){
var jsonData = pm.response.json()
 
pm.test("First Name Validation", function()
{
pm.expect(pm.environment.get("firstName")).to.eql(jsonData.firstname)
})

pm.test("Last Name Validation", function()
{
pm.expect(pm.environment.get("lastName")).to.eql(jsonData.lastname)
})

pm.test("Total Price Validation", function()
{
pm.expect(pm.environment.get("partialUpdatedTotalPrice")).to.eql(jsonData.totalprice.toString())
})
  
pm.test("Deposit Paid Validation", function()
{
pm.expect(pm.environment.get("depositPaid")).to.eql(jsonData.depositpaid.toString())
})
  
pm.test("Check-In Date Validation", function()
{
pm.expect(pm.environment.get("checkin")).to.eql(jsonData.bookingdates.checkin)
})

pm.test("Check-Out Date Validation", function()
{
pm.expect(pm.environment.get("checkout")).to.eql(jsonData.bookingdates.checkout)
})
  
pm.test("Status code is 200", function ()
{
 pm.response.to.have.status(200);
});

pm.test("Additional Needs Validation", function()
{
pm.expect(pm.environment.get("partialUpdatedAdditionalNeeds")).to.eql(jsonData.additionalneeds)
})

}
else if(responseCode==404)
{
pm.test("Not Found")
}
else if(responseCode==500)
{
pm.test("Server Error")
}
else
{
pm.test("Something went wrong. Please check it")
}
```
 **Response Body:** 
 ```console
{
    "firstname": "Emory",
    "lastname": "Langworth",
    "totalprice": 236,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-25",
        "checkout": "2024-03-29"
    },
    "additionalneeds": "Ball"
}
```
## _**8. Delete Booking Record**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: DELETE
### Pre-request Script: None
### Request Body: None
### Tests: None

## _**9. Check After The Delete**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Pre-request Script: None
### Request Body: None
### Tests:
```console
var responseCode = pm.response.code
    
console.log(responseCode)
    
if(responseCode==404){
pm.test("Deleted Successfully")
}
else if(responseCode==500)
{
 pm.test("Internal Server Error")
}
else
{
 pm.test("Something went wrong. Please check it")
}
```
 **Response Body:** 
```console
Not Found
```
## Run Command:  
- Run Command for Console: 
```console 
newman run Booking.postman_collection.json -e Booking.postman_environment.json 
```
- Run Command for Report: 
```console 
newman run Booking.postman_collection.json -e Booking.postman_environment.json -r cli,htmlextra
```

## Newman Report Summary:
![image](https://github.com/MinhazRafi/Automated-Testing-of-Rest-Booking-API-with-Newman-Report/assets/77951772/ba6be7cc-8d7a-4fb4-bd03-13719debb0fb)
![image](https://github.com/MinhazRafi/Automated-Testing-of-Rest-Booking-API-with-Newman-Report/assets/77951772/3f8057e6-82c1-4b84-8c63-1b0e8553811e)
![image](https://github.com/MinhazRafi/Automated-Testing-of-Rest-Booking-API-with-Newman-Report/assets/77951772/10fc1c83-3906-4647-a599-b625dbd96297)
![image](https://github.com/MinhazRafi/Automated-Testing-of-Rest-Booking-API-with-Newman-Report/assets/77951772/779e20cb-eb42-467c-96c8-89e35b164221)
