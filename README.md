# OCI-Api-Gateway-Tutorial
Tutorial on OCI API gateway

**Oracle API Gateway**




**Index:**
1.Permissions required to enable API Gateway
2.Authentication in API Gateway
3.Creating an API Gateway
4.Create an API using Swagger/Open API 2.0
5.API Gateway Help












Permissions required to enable API Gateway:

1. Navigate to Policies in OCI Console.

![image](https://user-images.githubusercontent.com/34638862/123542981-1d1ee000-d76a-11eb-888f-0f419b4b5820.gif)


2.Add new policies as mentioned below.

![image](https://user-images.githubusercontent.com/34638862/123542987-290aa200-d76a-11eb-890b-b952eff2a18e.gif)

Statements example:
Allow group adminGroup to manage api-gateway-family in compartment bir_oci_training
Allow group adminGroup to manage virtual-network-family in compartment bir_oci_training    (access to VCN)
Allow group adminGroup to read functions-family in compartment bir_oci_training   (This is to enable functions)
Allow group adminGroup to manage all-resources in compartment bir_oci_training
3.Next go to Dynamic groups and add below statement.

![image](https://user-images.githubusercontent.com/34638862/123542991-2dcf5600-d76a-11eb-90cc-c279240aae46.gif)

Under matching Rules add the statement.
Like: 
ALL {resource.type = 'ApiGateway', resource.compartment.id = 'ocid1.compartment.oc1..aaaaaaaahyudxrhkeg7esfrcuxom2hbw5eeco3vxgv3gqlu2lgkgnob2w7ba'}

![image](https://user-images.githubusercontent.com/34638862/123542999-37f15480-d76a-11eb-8f69-59e103b17395.gif)

Make sure under Networking, you have a VCN created with public and private subnets.


Incase you don’t know how to create a VCN follow this link: https://docs.oracle.com/en-us/iaas/Content/GSG/Tasks/creatingnetwork.htm

![image](https://user-images.githubusercontent.com/34638862/123543006-3e7fcc00-d76a-11eb-90e1-4168e8536c93.gif)


LAB 1:
Creating an API Gateway:

4.Now under Developer services navigate to Gateways.

![image](https://user-images.githubusercontent.com/34638862/123543011-43dd1680-d76a-11eb-9775-932036690e2d.gif)

5.Click on Create Gateway and fill in the required details. Make sure to select a public subnet in this case.

![image](https://user-images.githubusercontent.com/34638862/123543019-4b9cbb00-d76a-11eb-9576-785fc2230e7d.gif)

6.After we click on Create a new API Gateway will be created. This may take a couple of minutes.

![image](https://user-images.githubusercontent.com/34638862/123543022-4fc8d880-d76a-11eb-81e6-98e9a9c7fb82.gif)


7. The image icon will turn Green in color. Now click on Create Deployment and fill in the details.

![image](https://user-images.githubusercontent.com/34638862/123543029-58b9aa00-d76a-11eb-93c1-d04711dd2a2d.gif)

Please note there are two ways to create a gateway. We can configure it manually or use an “Existing Deployment API”.

In a typical scenario a API deployment file would look like this: Sample file.


{
  "routes": [
    {
      "path": "/hello",
      "methods": ["GET"],
      "backend": {
        "type": "ORACLE_FUNCTIONS_BACKEND",
        "functionId": "ocid1.fnfunc.oc1.phx.aaaaaaaaab______xmq"
      }
    },
    {
      "path": "/weather",
      "methods": ["GET"],
      "backend": {
        "type": "HTTP_BACKEND",
        "url": "https://api.weather.gov"
      }
    }
  ]
}


**Authentication in API Gateway**

Please note: 
Api Gateway also supports adding Authentication, CORS or Rate limiting to the existing APIs.

![image](https://user-images.githubusercontent.com/34638862/123543042-640cd580-d76a-11eb-8eb6-0eea8e4abd46.gif)

Under Authentication we have two options- JWT and Custom.

![image](https://user-images.githubusercontent.com/34638862/123543051-6a02b680-d76a-11eb-871e-d9a5e9424cd7.gif)


To enable JWT authentication/Custom we would need an Authentication Server like Oracle IDCS where the token configuration needs to be done.

To know more about the required configurations follow this below link:
https://docs.oracle.com/en-us/iaas/Content/APIGateway/Tasks/apigatewayaddingauthzauthn.htm

![image](https://user-images.githubusercontent.com/34638862/123543062-71c25b00-d76a-11eb-99c0-eec7fd3336d9.gif)

We can also configure CORS policies for our apis.

![image](https://user-images.githubusercontent.com/34638862/123543071-7a1a9600-d76a-11eb-9117-17f1e939e047.gif)

For more info on CORS follow this below link:
https://docs.oracle.com/en-us/iaas/Content/APIGateway/Tasks/apigatewayaddingcorssupport.htm

In API Gateway we can also configure the number of requests that we want to allow at a given time using the Rate Limiting Policy as shown below.

![image](https://user-images.githubusercontent.com/34638862/123543080-843c9480-d76a-11eb-9942-3faf60379d52.gif)

To know more on rate limiting follow below link:
https://docs.oracle.com/en-us/iaas/Content/APIGateway/Tasks/apigatewaylimitingbackendaccess.htm


In this lab we are not configuring any of these features. So, lets get back to our lab.

Now we will create an API from scratch.
Don’t fill anything else and click on Next.

![image](https://user-images.githubusercontent.com/34638862/123543086-8d2d6600-d76a-11eb-8822-6d61c8cfb5e8.gif)

7.Now we are creating a Deployment. So fill in all the following details.

![image](https://user-images.githubusercontent.com/34638862/123543089-94ed0a80-d76a-11eb-930f-eb9282602af1.gif)

Here we can provide any path name we want.
Select method as GET and type as HTTP. 
URL: https://reqres.in/api/users 

Note: This URL we are using is basically a sample URL that returns a list of users.
Set the connection timeout values and click on Next.

Please note: We can also select an option like “Stock Response” create a JSON response of our choice.
Here the JSON is placed in the BODY.

See below screenshot for reference.

![image](https://user-images.githubusercontent.com/34638862/123543093-9c141880-d76a-11eb-980b-0e47a41df4b7.gif)

8.On the next screen verify all details and click on “Create”.

![image](https://user-images.githubusercontent.com/34638862/123543099-9fa79f80-d76a-11eb-9ed5-4a9da98c271f.gif)

9.Now it will take a couple of minutes for the api to get created.

![image](https://user-images.githubusercontent.com/34638862/123543104-a46c5380-d76a-11eb-9ba5-4db6d268f2f8.gif)

Once created we should get a confirmation like below.

![image](https://user-images.githubusercontent.com/34638862/123543107-a9c99e00-d76a-11eb-8bd5-cf58763fc10c.gif)

11. Click on the Show button below “Endpoint” to see and then Copy the endpoint url.

![image](https://user-images.githubusercontent.com/34638862/123543115-b3530600-d76a-11eb-93b4-4503e33966b8.gif)

12.Now once the url is copied we can go to our browser and paste the url.

![image](https://user-images.githubusercontent.com/34638862/123543120-b8b05080-d76a-11eb-8ffa-3cce7c676f48.gif)

Eg: https://fqb6qmux6zbgxl5ibsudnuk4zm.apigateway.ap-tokyo-1.oci.customer-oci.com/userdetails
Output:

Once we paste the url we see that we get a “Not Found” error. We get this error because we have not defined the complete path for our url including the endpoint. 

13.Now navigate back to the api “myuserapi” and click on the “Edit” option.

![image](https://user-images.githubusercontent.com/34638862/123543131-c2d24f00-d76a-11eb-8b52-d240fc122bfb.gif)

14.Look for the path name that we had entered earlier. Eg: /api
Basically this is the endpoint path that we need to enter after the url.

![image](https://user-images.githubusercontent.com/34638862/123543137-c665d600-d76a-11eb-812f-0696ea508ffb.gif)

15.Now add this path at the end of the existing url and paste it in browser. 
Eg: https://fqb6qmux6zbgxl5ibsudnuk4zm.apigateway.ap-tokyo-1.oci.customer-oci.com/userdetails/api

Great!! Now we should be able to see a response.

![image](https://user-images.githubusercontent.com/34638862/123543143-cd8ce400-d76a-11eb-822f-c2f4f09f3b21.gif)

This completes our hands on Lab 1 for Oracle API Gateway.











LAB 2:

Create a API using a Swagger file/open API 2.0

First we navigate to API in OCI console.

![image](https://user-images.githubusercontent.com/34638862/123543161-e0071d80-d76a-11eb-97e6-790ac3b5e3a9.gif)

In the API page, click on “Create API”. This would launch a screen to enter name and upload your Swagger file.


Select a yaml file and click on Upload.

You can download the YAML file from this Object Storage link: 
https://objectstorage.us-ashburn-1.oraclecloud.com/n/sehubjapacprod/b/Github_resources/o/formsapi.yaml

![image](https://user-images.githubusercontent.com/34638862/123543164-e5fcfe80-d76a-11eb-9beb-6802edcdb0a9.gif)

Once done. Click on Create button to create the API.

![image](https://user-images.githubusercontent.com/34638862/123543169-eeedd000-d76a-11eb-887f-114bde2b832a.gif)

Next step would be to click on “Deploy” to create the api url. 

![image](https://user-images.githubusercontent.com/34638862/123543185-f90fce80-d76a-11eb-8596-b59fa8b21b77.gif)

Fill in the rest of the details. Once done click on “Next” to go to the next screen.

![image](https://user-images.githubusercontent.com/34638862/123543210-ff9e4600-d76a-11eb-9c31-fd02e1a7944d.gif)

In this page we can see that the Route rules are already defined for us one after the other.

![image](https://user-images.githubusercontent.com/34638862/123543219-05942700-d76b-11eb-940b-43b90a09a27a.gif)

Note: This swagger file that we used has multiple POST request defined in it. Therefore we have multiple route rules displayed here. 

Click Next to continue.

![image](https://user-images.githubusercontent.com/34638862/123543227-0cbb3500-d76b-11eb-8473-f7ab62d029f0.gif)

Verify the deployment information shown and then click on “Create”.

![image](https://user-images.githubusercontent.com/34638862/123543236-15137000-d76b-11eb-94d0-50ca83d5a905.gif)

We should be able to see the status of the deployment getting created.

![image](https://user-images.githubusercontent.com/34638862/123543239-1a70ba80-d76b-11eb-8cf5-35eee61a4e61.gif)

Once created, click on myformsapi to navigate to the next page.
We should be able to see the Endpoint generated. Copy the endpoint.
Eg: https://fqb6qmux6zbgxl5ibsudnuk4zm.apigateway.ap-tokyo-1.oci.customer-oci.com/formsapi

![image](https://user-images.githubusercontent.com/34638862/123543243-1fce0500-d76b-11eb-9641-6bfcece8b99c.gif)

Now please note this is the common endpoint. To get the individual endpoints we will have to navigate to the route rules and identify each required endpoint.

We can click on the EDIT option on top to navigate back to the route rules page.

![image](https://user-images.githubusercontent.com/34638862/123543255-33796b80-d76b-11eb-8c11-408d7789a627.gif)

Under PATH we can find our first endpoint. Eg: /form/02551Q
Please copy this endpoint and add it to the end of the actual endpoint url.


So the first POST endpoint is 
Eg: https://fqb6qmux6zbgxl5ibsudnuk4zm.apigateway.ap-tokyo-1.oci.customer-oci.com/formsapi/form/02551Q

Now try this endpoint in a tool from where we can make a POST call. 
In my case, I am using Postman for testing this.

**Screenshot from Postman:**
 
![image](https://user-images.githubusercontent.com/34638862/123543277-4db34980-d76b-11eb-967c-bf3a3bd0ff0e.gif)
 
![image](https://user-images.githubusercontent.com/34638862/123543270-44c27800-d76b-11eb-9fcc-69b68c1aacfb.gif)

We can see a 200 OK response. This confirms our API endpoint is working.

We can repeat similar steps for the other endpoints to test them.

This completes our Lab 2 tutorial on API Gateway.





**API Gateway Help:**
To explore more on Oracle API Gateway follow below links:

API Gateway Overview:
https://docs.oracle.com/en-us/iaas/Content/APIGateway/Concepts/apigatewayoverview.htm

Authentication in API Gateway:
https://docs.oracle.com/en-us/iaas/Content/APIGateway/Tasks/apigatewayaddingauthzauthn.htm

Call a Function using API Gateway:
https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/functions/func-api-gtw/01-summary.htm

API Gateway Blogs:
https://blogs.oracle.com/developers/creating-your-first-api-gateway-in-the-oracle-cloud




















Created by Saikat Dey
