Task 1 : test normal request 
curl -X GET -i -w '\n' localhost:5000

Task 2 : Send custom HTTP code back with a tuple
curl -X GET -i -w '\n' localhost:5000/no_content
it should be display a output status with code 204 and content-type: application/json

Task 3 : send custom HTTP code with method make_response()
curl -X GET -i -w '\n' localhost:5000/exp
the output should be had a code 200, content-type : application/json and display output json { "message" : "Hello World" }

Task 4 : create data and test where it was implemented or not.
curl -X GET -i -w '\n' localhost:5000/data
the result should be a code 200 as json and diplay message { "message": "Data of length 5 found" }

Task 5 : create respone to get data person.
curl -X GET -i -w '\n' "localhost:5000/name_search?q=Abdel"
the result should be code 200, content-type as a json and display message json with property first_name as a Abdel
//
if we dont input an arguments "q=", they would respone as invalid with code 422, content-type json and display message { "message": "Invalid input parameter" }
//
id we search where the first_name doesnt exist in the data we make. its should display with a code 404, content-type: json and display message { "message": "Person not found" }

Task 6 : create respone to count data for our crud system later.
curl -X GET -i -w '\n' "localhost:5000/count"
The Output should be a code 200, content-type json and display message { "data count" : 5 }

Task 7 : create search method by a name uuid
curl -X GET -i localhost:5000/person/66c09925-589a-43b6-9a5d-d1601cf53287
The output should display code 200, content-type json and display message data tree person
//
if you pass invalid uuid 
curl -X GET -i localhost:5000/person/not-a-valid-uuid
it will display code 404, content-type text/html and message 
```html
<!doctype html>
<html lang=en>
<title>404 Not Found</title>
<h1>Not Found</h1>
<p>The requested URL was not found on the server. If you entered the URL manually, please check your spelling and try again.</p>
```
//
if you pass a data that does not exist. the method should return with 404, content-type json and display message
{ "message": "person not found" }

Task 8 : create delete request by person and id
curl -X DELETE -i localhost:5000/person/66c09925-589a-43b6-9a5d-d1601cf53287
the method should display code 200, content-type json and message uuid. check the data length using 
curl -X GET -i localhost:5000/count, it should be decrease from 5 to 4
//
if u pass an invalid UUID the server should return a 404 message conten-type text/html
```html
<!doctype html>
<html lang=en>
<title>404 Not Found</title>
<h1>Not Found</h1>
<p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
```
//
finally if u pass valid UUI that doesnt exist in the data list. the method should return 404, content-type json and display message { "message": "person not found" }

Task 9 : Parse JSON with method POST from request body
curl -X POST -i -w '\n' \
  --url http://localhost:5000/person \
  --header 'Content-Type: application/json' \
  --data '{
        "id": "4e1e61b4-8a27-11ed-a1eb-0242ac120002",
        "first_name": "John",
        "last_name": "Horne",
        "graduation_year": 2001,
        "address": "1 hill drive",
        "city": "Atlanta",
        "zip": "30339",
        "country": "United States",
        "avatar": "http://dummyimage.com/139x100.png/cc0000/ffffff"
}'
you should see an output code 200, content type json and display message uuid
//
if u pass empty 
curl -X POST -i -w '\n' \
  --url http://localhost:5000/person \
  --header 'Content-Type: application/json' \
  --data '{}'
the server should return a code 422, content-type json and display message { "message": "Invalid input parameter" }

Task 10 : creating method error handle
curl -X POST -i -w '\n' http://localhost:5000/notvalid
the server should return a code 404, content-type html and display message { "message": "API not found" }
