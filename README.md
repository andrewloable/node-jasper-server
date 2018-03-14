# jasper-node-server
Host Jasper Reports in NodeJS


## Download

```
git clone git:github.com/andrewloable/jasper-node-server.git
npm install
```

## Libraries

Put all Jasper Reports libraries (jar files) under the /libs folder.
The easiest way to get the libraries is to copy all jar files from JasperReports Server under 

```
<jasperserver>/apache-tomcat/webapps/jasperserver/WEB-INF/lib
```

## Configure

All reports, drivers and connections are set in settings.json.
A sample configuration is shown below.

```
{
    "reports":{
        "test1": {
            "jrxml": "/jrxml/test.jrxml"
        },
        "test2": {
            "jrxml": "/jrxml/test2.jrxml",
            "subreports":{
                "parametername1": {
                    "jrxml": "/jrxml/test2subreport1.jrxml"
                }
            }
        }
    },
    "drivers": {
        "postgresql": {
            "path": "/libs/postgresql-42.2.1.jar",
            "class": "org.postgresql.Driver"
        }
    },
    "connections": {
        "mytestdatabase": {
            "jdbc": "jdbc:postgresql://localhost:5432/mytestdatabase",
            "user": "myusername",
            "password": "mypassword"
        }
    }
}
```

## Run

Execute the command below.

```
node index.js
```

This will run an express app that listens to port 3000 and will accept connections from localhost only.

## API

* **/generate_pdf**
Generate a PDF report.

* **/generate_html**
Generate an HTML report.

* **/generate_rtf**
Generate an RTF report.

* **/generate_csv**
Generate a CSV report.

* **/generate_docx**
Generate a Docx report.

* **/generate_pptx**
Generate a Pptx report.

* **/generate_xlsx**
Generate an xlsx report.

* **/generate_odt**
Generate an ODT report.

## Examples

To generate a report into PDF

* **http**
```
POST /generate_pdf HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Cache-Control: no-cache

{
	"name": "test",
	"connection": "none",
	"parameters": {
		"Parameter1": "Parameter 1",
		"Parameter2": "PARAM2"
	}
}
```

* **go**
```
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "http://localhost:3000/generate_pdf"

	payload := strings.NewReader("{\n\t\"name\": \"test\",\n\t\"connection\": \"none\",\n\t\"parameters\": {\n\t\t\"Parameter1\": \"Parameter 1\",\n\t\t\"Parameter2\": \"PARAM2\"\n\t}\n}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("cache-control", "no-cache")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

* **nodejs request**
```
var request = require("request");

var options = { method: 'POST',
  url: 'http://localhost:3000/generate_pdf',
  headers: 
   { 'cache-control': 'no-cache',
     'content-type': 'application/json' },
  body: 
   { name: 'test',
     connection: 'none',
     parameters: { Parameter1: 'Parameter 1', Parameter2: 'PARAM2' } },
  json: true };

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});
```

* **curl**
```
curl -X POST \
  http://localhost:3000/generate_pdf \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d '{
	"name": "test",
	"connection": "none",
	"parameters": {
		"Parameter1": "Parameter 1",
		"Parameter2": "PARAM2"
	}
}'
```

* **python requests**
```
import requests

url = "http://localhost:3000/generate_pdf"

payload = "{\n\t\"name\": \"test\",\n\t\"connection\": \"none\",\n\t\"parameters\": {\n\t\t\"Parameter1\": \"Parameter 1\",\n\t\t\"Parameter2\": \"PARAM2\"\n\t}\n}"
headers = {
    'content-type': "application/json",
    'cache-control': "no-cache",
    }

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```

