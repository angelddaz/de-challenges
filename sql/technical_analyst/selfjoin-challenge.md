The following billing table stores credit card failures. When someone hits a credit card failure, they are entered here. 
They will be retried again after 3 days. If they fail again, they will have another entry in the table. 
If they pass, they won’t appear again.

Table Name: Billing_table
Fields:
    Company_id – unique identifier for a company
    Billing_failure_date – date on which the company’s credit failed processing

Company_id  billing_failure_date
123         01/01
123         01/04
234         02/15
345         03/31
345         04/02
123         10/11


1. Find all unresolved failure records.
