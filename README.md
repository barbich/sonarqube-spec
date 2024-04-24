# sonarqube-spec

Dirty little program that fetches the API specification for Sonarqube and generates a OpenAPI compliant spec file.
The fetched source data is the v1 and v2 API specification from a server or file. These are then combined to generate 1 single OpenAPI compliant specification.

## Usage
```bash
bash-5.2$ ./sonarqube-spec -h
Usage of ./sonarqube-spec:
  -debug
        Debug
  -defaulturl
        Use http://localhost:9000/ for the url.
  -inputv1 string
        Name of the sonarqube webservices export file from /api/webservices/list or Sonarqube base URL.
  -inputv2 string
        Name of the sonarqube webservices export file from /api/v2/api-docs or Sonarqube base URL. (if missing we reuse the inputv1 value.)
  -outfile string
        File base name to save to (extension will be added)
  -publicurl
        Use https://next.sonarqube.com/sonarqube/ for the url.
bash-5.2$
```

## Usage Examples

Export & process the API definitions from https://next.sonarqube.com/sonarqube/api/webservices/list and generate a OpenAPI compliant specification file in sonarqube-api.yaml/json
```bash
bash-5.2$ ./sonarqube-spec  -publicurl --debug
time="2024-04-23T23:19:57+01:00" level=debug msg="Using https://next.sonarqube.com/sonarqube/ for inputV1 and inputV2"
time="2024-04-23T23:19:57+01:00" level=debug msg="Using https://next.sonarqube.com/sonarqube/ for inputV1 and https://next.sonarqube.com/sonarqube/ inputV2"
time="2024-04-23T23:19:57+01:00" level=debug msg="Error reading https://next.sonarqube.com/sonarqube/ as file .... trying as URL"
time="2024-04-23T23:19:57+01:00" level=info msg="Reading URL successfully, size 328326"
time="2024-04-23T23:19:57+01:00" level=debug msg="Reading OAS data"
time="2024-04-23T23:19:57+01:00" level=debug msg="Error reading https://next.sonarqube.com/sonarqube/ as file .... trying as URL"
time="2024-04-23T23:19:57+01:00" level=debug msg="Reading : 51943"
time="2024-04-23T23:19:57+01:00" level=debug msg="Generating NewDocument from OAS data"
time="2024-04-23T23:19:57+01:00" level=debug msg="Using version number: 10.6.0.90276"
time="2024-04-23T23:19:57+01:00" level=debug msg="Finished processing document."
time="2024-04-23T23:19:57+01:00" level=debug msg="Writing to file:"
openapi: "3.0.1"
info:
    title: "SonarQube Web API v2"
    description: "The SonarQube API v2 is a REST API which enables you to interact with SonarQube programmatically. Endpoint listed here should work as expected.\nHowever, you should not consider the API stable for now as it is still under development. New releases of SonarQube can bring changes to existing endpoint definitions.\n"
    version: 10.6.0.90276
servers:
    - url: "http://next.sonarqube.com/sonarqube/api/v2"
      description: "Generated server url"
paths:
    /v2/authorizations/groups:
....

bash-5.2
```

Export & process the API definitions from local file sonarqube_api_webservices_list_pretty.json and generate a OpenAPI compliant specification file in blabla.yaml
```bash
bash-5.2$ ./sonarqube-spec  -debug --outfile blabla.yaml -inputv1 sonarqube_api_webservices_list_v1.json -inputv2 sonarqube_api_webservices_list_v2.json
DEBU[0000] Using sonarqube_api_webservices_list_v1.json for inputV1 and sonarqube_api_webservices_list_v1.json inputV2
INFO[0000] Reading file successfully, size 496071
DEBU[0000] Reading OAS data
DEBU[0000] Generating NewDocument from OAS data
DEBU[0000] Retrieving version returned error: Get "sonarqube_api_webservices_list_v1.json/api/server/version": unsupported protocol scheme ""
DEBU[0000] Using version number: Unknowns Edition
DEBU[0000] Finished processing document.
INFO[0000] Saving file to: blabla.yaml
bash-5.2$
```
Note: sonarqube_api_webservices_list_v1.json could be generated via a "curl http://squ_d8b25xxxxxxxxxxx:@localhost:9000/api/webservices/list" and sonarqube_api_webservices_list_v2.json via "curl http://squ_d8b25xxxxxxxxxxx:@localhost:9000/api/v2/api-docs"

Export & process the API definitions from URL http://squ_d8b25xxxxxxxxxxx:@localhost:9000/ (with auth token) and generate a OpenAPI compliant specification file as xx.json
```bash
bash-5.2$ ./sonarqube-spec --inputv1 http://squ_d8b25xxxxxxxxxxx:@localhost:9000/ -outfile xx.json
INFO[0000] Reading URL successfully, size 291825
INFO[0000] Saving content to file: xx.json
bash-5.2$
```

## Validation

The generated openapi specification can be validated via:
```bash
bash-5.2$ openapi-generator validate -i xx.json
Picked up _JAVA_OPTIONS: -DmaxYamlCodePoints=99999999
Validating spec (xx.json)
No validation issues detected.
bash-5.2$
```
Or any other tool that validates OpenApi specs.

## Use cases
The generated openapi spec file can easily be used with [Restish]: rest.sh
(you still need to hand add the authentication token in the restish configuration file)

## Notes

-  the version number of the API will be extracted if possible from the Sonarqube site on http://squ_d8b25xxxxxxxxxxx:@localhost:9000/api/server/version if possible (or whatever url you gave) ... or it will default to "Unknowns Edition"
- the generate specification DOES NOT encompass the proper response schemas ... this is not systematically documented in the Sonarqube webservices definition.

## To do
Github action to retrieve the latest version of the API for the sonarqube public site ...

