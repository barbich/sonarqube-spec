# sonarqube-spec

Dirty little program that fetches the API specification for Sonarqube and generates a OpenAPI compliant spec file.

## Usage

Export & process the API definitions from https://next.sonarqube.com/sonarqube/api/webservices/list and generate a OpenAPI compliant specification file in sonarqube-api.yaml/json
```bash
bash-5.2$ ./sonarqube-spec  -publicurl
INFO[0000] Saving YAML file to: sonarqube-api.yaml
INFO[0000] Saving JSON file to: sonarqube-api.json
bash-5.2
```

Export & process the API definitions from local file sonarqube_api_webservices_list_pretty.json and generate a OpenAPI compliant specification file in blabla.yaml/json
```bash
bash-5.2$ ./sonarqube-spec  -file sonarqube_api_webservices_list_pretty.json -outfile blabla
INFO[0000] Saving YAML file to: blabla.yaml
INFO[0000] Saving JSON file to: blabla.json
bash-5.2$
```
Note: sonarqube_api_webservices_list_pretty.json could be generated via a "curl http://squ_d8b25xxxxxxxxxxx:@localhost:9000/api/webservices/list"

Export & process the API definitions from URL http://squ_d8b25xxxxxxxxxxx:@localhost:9000/api/webservices/list (with auth token) and generate a OpenAPI compliant specification file
```bash
bash-5.2$ ./sonarqube-spec  -url http://squ_d8b25xxxxxxxxxxx:@localhost:9000/api/webservices/list
INFO[0000] Saving YAML file to: sonarqube-api.yaml
INFO[0000] Saving JSON file to: sonarqube-api.json
bash-5.2$
```

## Validation

The generated openapi specification can be validated via:
```bash
bash-5.2$ openapi-generator validate -i sonarqube-api.json
Validating spec (sonarqube-api.json)
No validation issues detected.
bash-5.2$
```
Or any other tool that validates OpenApi specs.

## Use cases
The generated openapi spec file can easily be used with [Restish]: rest.sh
(you still need to hand add the authentication token in the restish configuration file)

## Notes

-  the version number of the API will be extracted if possible from the Sonarqube site on http://squ_d8b25xxxxxxxxxxx:@localhost:9000/api/server/version if possible (or whatever url you gave) ... or it will default to "Unknowns Edition"
- the generate specification DO NOT encompass the proper response schemas ... this is not systematically documented in the Sonarqube webservices definition.

## To do
Merge v1 and v2 outputs ...
Github action to retrieve the latest version of the API for the sonarqube public site ...

