<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Fetch Data with AWS Lambda

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-lambda)

**Author:** Vijay Pratap Singh Hada  
**Email:** vijaypratapsinghhada9@gmail.com

---

## Fetch Data with AWS Lambda

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-lambda_p9thryj2)

---

## Introducing Today's Project!

In this project, I will demonstrate how to create a data tier for retrieving information from a database. I'm doing this project to learn about storing and managing data effectively, as well as how to access it whenever necessary. By using serverless technology like AWS Lambda and Amazon DynamoDB, I will explore practical methods for handling user data efficiently.

### Tools and concepts

The services I used in this project were DynamoDB for storing data, AWS Lambda for retrieving data from the DynamoDB table, and CloudWatch for logging changes related to the data. Key concepts I learned include how Lambda functions operate, the structure and use of JSON for policy setup, the importance of inline policies for specific permissions, and how to manage permission settings for DynamoDB tables. 

### Project reflection

This project took me approximately 3 hours and 10 minutes to complete. The most challenging part was writing the documentation, as I needed to ensure that I clearly explained each step and concept. It was most rewarding to see that my inline policy permissions were working correctly, allowing me to successfully run the Lambda function and retrieve data from the DynamoDB table. 

I did this project today to learn about setting up DynamoDB and how to use AWS Lambda functions to retrieve data effectively. I also gained insight into creating inline policy permissions, which enhance security for my DynamoDB data. This project helped me understand how the data tier works behind the scenes and why it's important to know where and how to store data, as well as how to access it when needed. Additionally, this knowledge is valuable for completing my three-tier architecture project. 

---

## Project Setup

To set up my project, I created a database using a well-known service for event-driven applications called DynamoDB, which is a serverless solution provided by AWS. I set up a table specifically for storing user data, as DynamoDB allows for quick and flexible data access. The partition key I chose is `userId`, which means that every item in the table will be uniquely identified by its userId. This helps manage and organize the data efficiently, ensuring that I can retrieve user information quickly whenever needed. 

In my DynamoDB table, I added a sample user item with userId, name, and email attributes. This shows how DynamoDB is schemaless, which means I can add as many attributes as I need without being restricted by a fixed structure. This flexibility is a key feature of DynamoDB, allowing each item to have different attributes. For my sample user, I used JSON format to define the data easily. This makes it simple to change or add new information later as my application grows and requires more data. 


![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-lambda_a112c3d5)

### AWS Lambda

AWS Lambda is a serverless computing service that allows you to run code without managing any computers or servers. It automatically handles the infrastructure for you and only runs your code when needed, which means you won’t pay for idle time. I'm using Lambda in this project to execute functions that retrieve data from my DynamoDB table. This makes the process efficient and scalable, allowing my application to handle various requests seamlessly. By utilizing Lambda, I can focus on writing code while AWS takes care of server management and infrastructure scaling.

---

## AWS Lambda Function

My Lambda function has an execution role, which is a specific set of permissions that determine what the function can do within my AWS account. This role ensures that my function does not have unlimited access to all AWS resources, which helps protect my account from security risks. By default, the role allows the function to write logs to CloudWatch, which is useful for monitoring and troubleshooting. If there are any issues when testing my Lambda function, I can check these logs for error messages to understand what went wrong. This controlled access is important for maintaining security while still enabling my function to perform its necessary tasks.

My Lambda function will retrieve user data from the DynamoDB table named "UserData" using the AWS SDK for JavaScript. When triggered, the function takes a userId from the incoming event, constructs a request to get the corresponding data from the table, and returns it. If the data is found, the function captures and logs the user information; if not, it logs a message indicating that no data was found for that userId. The code also includes error handling to ensure that any issues during the data retrieval process are properly logged, making it easier to diagnose problems.

The code uses AWS SDK, which is a set of tools that makes it easier for developers to interact with AWS services, like DynamoDB. My code uses the SDK to simplify the process of retrieving data from the database without writing all the low-level code myself. It provides pre-built functions and classes that help me perform tasks quickly and accurately. By using the AWS SDK, I can focus on the main logic of my application, while the SDK handles the details of connecting to AWS and managing requests, making my development process more efficient and less error-prone.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-lambda_a1b2c3d5)

---

## Function Testing

To test whether my Lambda function works, I went to the Test tab and entered a test event in the Event JSON editor. The test is written in JSON format, which is easy for Lambda to understand. I set the userId to "1" to check if my function could retrieve the corresponding item from the DynamoDB table. If the test is successful, I would see the expected user data returned, confirming that my Lambda function is working correctly and can fetch data as intended. 

The test displayed a 'success' message because the Lambda function ran without any coding errors. However, the function's response was actually unsuccessful in retrieving data from DynamoDB. This happened because I had not granted the Lambda function the necessary permissions to access the DynamoDB table. Although the code executed properly, it couldn't complete its intended task of fetching the required data due to these access restrictions. To resolve this issue, I need to update the execution role for my Lambda function and provide the appropriate permissions to read items from the DynamoDB table.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-lambda_u1v2w3x4)

---

## Function Permissions

To resolve the AccessDenied error, I will grant permission using the AmazonDynamoDBReadOnlyAccess policy. This is necessary because the error message indicated a failure related to dynamodb:GetItem, which means the Lambda function lacks the permission to retrieve items from the DynamoDB table. By attaching this policy, I ensure that my function can execute the action needed to retrieve data from the table, allowing it to perform its task correctly. 

There were four DynamoDB permission policies I could choose from, but I didn't pick AWSLambdaDynamoDBExecutionRole or AWSLambdaInvocation-DynamoDB because these policies are designed for different purposes. The AWSLambdaDynamoDBExecutionRole gives access to view changes in DynamoDB streams, while AWSLambdaInvocation-DynamoDB is used to trigger functions based on these data changes. However, since I need to retrieve items from the DynamoDB table, the only suitable policy for this use case is AmazonDynamoDBReadOnlyAccess. This policy specifically allows my Lambda function to access and get data from the table, making it the best choice for my needs.

I also didn't pick AmazonDynamoDBFullAccess, which provides complete permissions for DynamoDB, because this level of access can make my resources less secure. Granting full permissions is unnecessary when my Lambda function only needs to read data. AmazonDynamoDBReadOnlyAccess was the right choice because it specifically grants permissions for reading data only. This policy allows my Lambda function to retrieve data from the DynamoDB table without risking the ability to modify or delete important information. By choosing the more limited policy, I ensure better security for my application while still enabling the functionality I need.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-lambda_3ethryj2)

---

## Final Testing and Reflection

To validate my new permission settings, I re-ran the test event, which allowed me to execute the Lambda function again. The results were successful, and I received the user data that was fetched from DynamoDB. This success occurred because I updated the permissions by adding AmazonDynamoDBReadOnlyAccess, which enables my Lambda function to retrieve data from the DynamoDB table. With these correct permissions in place, my function can now access the necessary information it needs to operate properly. This confirmation shows that my setup is working as intended.

Web apps are a popular use case for using Lambda and DynamoDB. For example, I could use Lambda for news sites to fetch articles based on user queries, providing relevant content to readers. I could also apply Lambda in social media apps to retrieve user profiles or automatically gather all content linked to a profile. Additionally, Lambda can be helpful for e-commerce projects, allowing customers to find products, access product information, and view their order history by fetching data from DynamoDB. These applications demonstrate the flexibility and power of combining these technologies to create dynamic and responsive web experiences.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-lambda_p9thryj2)

---

## Enahancing Security

For my project extension, I challenged myself to enhance the security of my database. This will involve adding an inline policy, which offers more specific permission controls than other managed policies like AmazonDynamoDBReadOnlyAccess. By using an inline policy, I can restrict access to only the necessary permissions for my Lambda function, ensuring it can only interact with the UserData table. This approach improves security by minimizing the risk of unauthorized access and limits the function's capabilities to only what it truly needs.

To create the permission policy, I used the visual editor because it provides an easy and guided way to set up the policy. This approach is beginner-friendly and allows me to understand how the permissions work without getting overwhelmed by complex coding. By using the visual editor, I can easily select the appropriate options and fill in the necessary details for my DynamoDB table, which helps reinforce my learning. This method not only simplifies the process but also ensures that I set the right permissions accurately, enhancing the security of my database.

When updating a Lambda function's permission policies, you could risk accidentally breaking its functionality if the new permissions are not set correctly. I validated that my Lambda function still works by going back to the Test tab and running the test again. After creating the new permission policy for reading access to the DynamoDB table, I executed the function, and it successfully returned the expected items from the table. This confirmed that my function can still access the data it needs while maintaining improved security through the updated permissions.

![Image](http://learn.nextwork.org/blissful_yellow_calm_donkey/uploads/aws-compute-lambda_1qthryj2)

---

---
