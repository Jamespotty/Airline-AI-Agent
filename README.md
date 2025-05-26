# Airline-AI-Agent


# Build an Airline AI Agent Using Langflow & DataStax Astra DB

This guide walks you through how to set up a simple AI agent for an airline use case using **Langflow** and **DataStax Astra DB**.

To follow along, we recommend watching the accompanying YouTube video, which covers:

* Setting up your database
* Creating your Langflow AI agent

---

## Step 1: Set Up Your Database

Start by creating a **free account** on DataStax Astra DB. Use this referral link to get started: [https://bit.ly/ds-langflow](https://bit.ly/ds-langflow)

Once your account is ready, create a new database instance.

---

### Load Flight Ticket Data

1. In your newly created Astra DB, create a collection named `flight_tickets`.
2. Upload the data file `data/tickets.json` to this collection.

If you don’t have this file or want to create your own sample, use the following prompt with your preferred LLM (e.g., ChatGPT, Claude, etc.) to generate realistic flight ticket data:

```
Generate a sample JSON file with flight tickets including the following attributes:

_id  
passengerId  
passengerName  
flightNumber  
departureAirport  
arrivalAirport  
departureDateTime  
arrivalDateTime  
seatNumber  
ticketClass  
ticketPrice  
bookingDate  
bookingReference
```

---

### Load Invoice Data

To store invoice data, use Astra’s **CQL Console** (available from your database dashboard) and run the following CQL command to create a Cassandra table:

```sql
CREATE TABLE default_keyspace.invoices (
    customer_id uuid,
    flight_id uuid,
    invoice_id uuid,
    installment_due_dates map<int, timestamp>,
    installment_plan map<int, decimal>,
    issue_date timestamp,
    payment_method text,
    status text,
    total_amount decimal,
    PRIMARY KEY (customer_id, flight_id, invoice_id)
) WITH CLUSTERING ORDER BY (flight_id ASC, invoice_id ASC);
```

After creating the table, load the invoice data by running the SQL commands found in the `data/invoice.sql` file directly in the CQL Console.

---

## Step 2: Build the Langflow Agent

You can either follow the steps in the YouTube video or fast-track the setup by importing a pre-built agent flow.

Use the JSON file provided here:
📂 [Airline Agent.json](airline_agent/Airline%20Agent.json)

This file can be directly imported into Langflow to recreate the AI agent workflow.

---

## Optional: Enable Tracing with Langsmith

If you’d like to monitor or debug your Langflow agent, Langsmith provides great tools for tracing.

To enable it, set the following environment variables in your development environment:

```
LANGCHAIN_TRACING_V2=true  
LANGCHAIN_API_KEY=lsv2_...
```

Access the Langsmith dashboard here:
🔗 [https://smith.langchain.com/](https://smith.langchain.com/)

