## Sending requests to Nginx
Nginx is exposed in port 4000. It redirects the requests based on the nginx.conf configuration file, which defines all the backends and the rules to perform redirections, in this case, based on the path (greeting and loan-prequalification)

### Starting the compose
docker compose up -d

### Calling Greeting decision service
```
curl --location 'http://localhost:4000/greeting' \
--header 'Content-Type: application/json' \
--data '{
    "Name" : "Javier"
}'
```

### Calling loan-prequalification decision service
```
curl --location 'http://localhost:4000/loan-prequalification' \
--header 'Content-Type: application/json' \
--data '{
    "Credit Score": {
        "FICO": 690
    },
    "Applicant Data": {
        "Age": 32,
        "Marital Status": "S",
        "Employment Status": "Employed",
        "Existing Customer": true,
        "Monthly": {
            "Income": 3000,
            "Repayments": 2,
            "Expenses": 2,
            "Tax": 2,
            "Insurance": 2
        }
    },
    "Requested Product": {
        "Type": "Standard Loan",
        "Rate": 2,
        "Term": 20,
        "Amount": 30000
    }
}'
```

### Build the DMN projects
- Start app in dev mode: ```mvn clean quarkus:dev```
- Build using JVM: ```mvn clean package```
- Build using JVM and build Docker image (requires Docker): ```mvn -Pcontainer clean package```
- Build using native image: ```mvn -Pnative clean package```
- Build using native image and build Docker image (requires Docker): ```mvn -Pcontainer -Pnative clean package```