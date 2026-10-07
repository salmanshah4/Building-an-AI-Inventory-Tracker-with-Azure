# Building-an-AI-Inventory-Tracker-with-Azure

### Link to Loom

<https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27>

### Objective

Create an Azure-based inventory tracking solution that updates stock levels from sales events, stores inventory data in SQL, and sends daily AI-generated restock recommendations by email. This SOP guides a team member through infrastructure setup, function processing, database preparation, and automated reporting.

### Key Steps

**1. Set up the Azure infrastructure with Terraform** [0:15](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=15)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ccde943d-d715-4f70-b8e8-af4048d008a0" />

- Create the core Azure resources using Terraform.
- Include the following in the main Terraform configuration: 
  - Resource group
  - Key Vault
  - Azure SQL Server and database
  - Service Bus namespace and queue
  - Azure OpenAI account
  - App Service Plan
  - Function App / Web App for processing sales events
- Confirm the resources are provisioned successfully before moving to application code.

 

**2. Organize the Terraform and application files** [0:30](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=30)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2fa19d94-95a9-4742-b99c-f555e3a08cf9" />

- Maintain a clear project structure for the deployment and app logic.
- Keep these files organized and version-controlled: 
  - `variables.tf`
  - `outputs.tf`
  - `main.tf`
  - Python function file
  - `function.json`
  - `host.json`
- Use the Terraform files to define infrastructure and the app files to define runtime behavior.

 

**3. Configure the Azure Function to process sales messages** [1:03](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=63)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9726f9cb-4ff4-4b81-be5c-3f110802077a" />

- Build the Azure Function so it listens to the Service Bus queue.
- Ensure each incoming sale message triggers the function automatically.
- On each message: 
  - Read the sale event
  - Update the product stock count in the database
- Use this function as the event-driven processor for inventory changes.

 

**4. Bind the function to the Service Bus queue correctly** [1:26](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=86)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/63e47f66-7bce-4b40-ab1f-c40afd24bed6" />

- Configure `function.json` to connect the function to the Service Bus queue.
- Verify the queue binding points to the correct queue name.
- Ensure the function is invoked for every message arriving in the queue.
- Keep session handling disabled if using a simple FIFO processing model.

 

**5. Validate queue processing behavior and ordering** [1:41](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=101)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a0e394a9-9f73-4777-82f0-65a5ebc56852" />

- Confirm messages are processed in the order they arrive.
- Keep `sessionEnabled` set to `false` when session-based grouping is not needed.
- Use this setup for a straightforward FIFO queue workflow.
- Test with sample sale events to make sure the function updates inventory as expected.

 

**6. Review the main Terraform deployment components** [2:24](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=144)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c642990a-084b-49d0-b50c-08e3a3d6341e" />

- Check that the main Terraform file includes all required resources and dependencies.
- Verify the deployment covers: 
  - Provider configuration
  - Resource group
  - Key Vault
  - Azure SQL Server and database
  - Service Bus namespace and queue
  - Azure OpenAI account
  - App Service Plan
  - Function App for sale event processing
- Confirm the Function App uses the same storage account as the main application runtime if required.

 

**7. Pass the Service Bus connection string to the function** [3:13](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=193)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/274d90de-5b8e-4615-8bd7-1dd3a41ea30c" />

- Add the Service Bus connection string to the function app configuration.
- Ensure the function can read from the queue using the correct connection setting.
- Match the binding name in `function.json` to the app setting name exactly.
- Confirm the queue name in the binding matches the queue created in Terraform.

 

**8. Build the daily Logic App for AI restock recommendations** [3:40](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=220)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e5fbde1a-301c-4d83-b4fa-5ca103a5ba97" />

- Create a Logic App that runs on a daily schedule.
- Configure it to execute a SQL query at 6:00 AM.
- Send the query results to Azure OpenAI for recommendation generation.
- Use the AI output to determine what items should be restocked.
- Set the Logic App to email the recommendation results automatically.

 

**9. Create and prepare the SQL database schema** [4:03](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=243)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5c92574e-340a-469e-b157-e765479e27c3" />

- Set up the database schema needed for inventory tracking.
- Create the required tables, including: 
  - Products
  - Suppliers
  - Stock movements
- Make sure the schema supports both sales updates and reporting queries.
- Confirm the database is ready before loading test data.

 

**10. Load test data and validate the sales flow** [5:01](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=301)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/58a27b9f-dbe8-4ee0-aef4-d96c529c3e38" />

- Insert test data into the inventory tables.
- Simulate sale events to verify the end-to-end flow.
- Confirm that sales messages: 
  - Enter the Service Bus queue
  - Trigger the Azure Function
  - Update stock counts in SQL
- Validate that the inventory data changes correctly after each test sale.

 

**11. Run the daily AI recommendation workflow and verify email delivery** [5:53](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=353)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/588d7bda-f955-40b4-9afd-db3edb4a9909" />

- Execute the Logic App workflow manually or wait for the scheduled run.
- Confirm the SQL query runs at the scheduled time.
- Verify Azure OpenAI returns a restock recommendation based on current stock levels.
- Check that the recommendation is delivered by email.
- Review the email subject and content to ensure it clearly identifies restock needs.

 

**12. Confirm the final inventory tracker behavior** [6:52](https://loom.com/share/58ee6f423cfd47f7a545c96b27da8b27?t=412)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/027cb773-a571-4904-a090-0f655f141f4e" />

- Validate the full solution works as an AI-powered inventory tracker.
- Ensure low-stock items trigger useful restock recommendations.
- Confirm the system can support ongoing sales activity and daily reporting.
- Use the email output as the operational alert for replenishment decisions.

### Cautionary Notes

- Ensure the Service Bus queue name in the function binding matches the queue created in Terraform exactly.
- Keep the connection string setting name consistent between the Function App configuration and `function.json`.
- If using FIFO processing, do not enable sessions unless the queue design requires them.
- Verify the SQL schema exists before running the function or Logic App workflows.
- Test with non-production data first to avoid accidental inventory changes.
- Confirm Azure OpenAI and email integrations are authorized and configured before scheduling automation.

### Tips for Efficiency

- Use Terraform to provision all infrastructure consistently and repeatably.
- Keep infrastructure code and application code in separate, clearly named files.
- Reuse the same storage account only if it fits your architecture and access requirements.
- Start with a small set of test products and sales events to validate the workflow quickly.
- Schedule the Logic App during off-peak hours, such as 6:00 AM, to reduce operational impact.
- Automate email notifications so restock decisions are delivered without manual checking.
