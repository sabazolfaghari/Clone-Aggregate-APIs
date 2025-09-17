# Clone-Aggregate-APIs
A WSO2 Synapse API example demonstrating request cloning, parallel calls, and response aggregation into a single result.

The API works by:
1. Receiving a single POST request containing `name` and `age`.
2. **Cloning** the message into two branches:
   - **first_sequence** → builds a SOAP request for `name`
   - **second_sequence** → builds a SOAP request for `age`
3. Sending each branch to the SOAP Echo service.
4. **Aggregating** both SOAP responses and returning them to the client.

## This repository includes:

- The Synapse **API** XML (`main.xml`) – exposes `POST /parallel/calls`
- Two Synapse **sequences**:
  - `first_sequence.xml` – calls `echoString` with `<name>`
  - `second_sequence.xml` – calls `echoInt` with `<age>`
- Example Postman request & sample aggregated response

## How to Run

1. **Deploy in WSO2 EI / Micro Integrator**  
   - Copy `main.xml`, `first_sequence.xml`, and `second_sequence.xml` to your WSO2 Synapse deployment folders.  
   - Ensure the **Echo** backend service is reachable.
   - Restart WSO2.

2. **Test with Postman or curl**

## Example request
```bash
curl --location 'http://localhost:8280/parallel/calls' \
--header 'Content-Type: application/json' \
--data '<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
  <soapenv:Header/>
  <soapenv:Body>
    <body>
      <name>Alex</name>
      <age>34</age>
    </body>
  </soapenv:Body>
</soapenv:Envelope>
'
```

** Example Resonse
```bash
<ns:echoStringResponse xmlns:ns="http://echo.services.core.carbon.wso2.org">
    <return>Alex</return>
</ns:echoStringResponse>
