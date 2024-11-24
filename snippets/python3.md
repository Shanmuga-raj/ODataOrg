# Step 0: Install Node.js
Download and install the [Python](https://www.python.org/downloads/), then install the [Requests](https://requests.readthedocs.io/en/latest/) library from python [package manager](https://pypi.org/project/requests/) then you are ready to run the below code snippets.

# Step 1: Requesting resources
```py
import requests

serviceRoot = "http://services.odata.org/v4/TripPinServiceRW/"
response    = requests.get(serviceRoot +"People")
status_code = response.status_code

message, body = ("Success: ", str(response.json())) if status_code == 200 else ("Error: ", response.text)
print( message + str(status_code) + "\n" + body)
```
# Step 2: Requesting an individual resource
```py
import requests

serviceRoot = "http://services.odata.org/v4/TripPinServiceRW/"
response = requests.get(serviceRoot + "People('russellwhyte')")
status_code = response.status_code

message, body = ("Success: ", str(response.json())) if status_code == 200 else ("Error: ", response.text)
print( message + str(status_code) + "\n" + body)
```
# Step 3: Queries
```py
import requests

serviceRoot = "http://services.odata.org/v4/TripPinServiceRW/"
response = requests.get(serviceRoot + "People?$top=2 & $select=FirstName, LastName & $filter=Trips/any(d:d/Budget gt 3000)")
status_code = response.status_code

message, body = ("Success: ", str(response.json())) if status_code == 200 else ("Error: ", response.text)
print( message + str(status_code) + "\n" + body)
```
# Step 4: Creating a new resource
```py
import requests

serviceRoot = "https://services.odata.org/v4/TripPinServiceRW/(S(xbn3udfvj11w1prdix00w5fc))/People"

headers = {
        'OData-Version': '4.0',
        'Content-Type': 'application/json;odata.metadata=minimal',
        'Accept': 'application/json',
}

newPerson = {
    "UserName": "shanmugaraj",
    "FirstName": "Shanmuga",
    "LastName": "Raj",
    "Gender": "Male",
    "Emails": [ "raj@example.com" ],
    "AddressInfo": [
        {
            "Address": "1 Suffolk Ln.",
            "City": {
                "CountryRegion": "United States",
                "Name": "Boise",
                "Region": "ID"
            }
        }
    ]
}

response = requests.post(serviceRoot, headers=headers, json=newPerson)
status_code = response.status_code

message, body = ("Success: ", str(response.json())) if status_code == 201 else ("Error: ", response.text)

print( message + str(status_code) + "\n" + body)
```
# Step 5: Relating resources
```py
import requests

serviceRoot = "http://services.odata.org/v4/TripPinServiceRW/"

payload = {
    "@odata.id": "http://services.odata.org/V4/TripPinServiceRW/People('russellwhyte')/Trips(0)"
}

response = requests.post(serviceRoot +"People('lewisblack')/Trips/$ref", json=payload)
status_code = response.status_code

message, body = ("Success: ", str(response.json())) if status_code == 201 else ("Error: ", response.text)
print( message + str(status_code) + "\n" + body)
```
# Step 6: Invoking a function
```py
import requests

serviceRoot = "http://services.odata.org/v4/TripPinServiceRW/"
response = requests.get(serviceRoot + "People('russellwhyte')/Trips(0)/Microsoft.OData.SampleService.Models.TripPin.GetInvolvedPeople()")
status_code = response.status_code

message, body = ("Success: ", str(response.json())) if status_code == 200 else ("Error: ", response.text)
print( message + str(status_code) + "\n" + body)
```