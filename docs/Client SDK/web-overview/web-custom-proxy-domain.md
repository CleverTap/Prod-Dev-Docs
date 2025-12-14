---
title: Web Custom Proxy Domain
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Custom Proxy Domain

A custom proxy domain allows you to proxy all events raised from the CleverTap Web SDK through your required domain. If you want to use your application server, use a proxy domain to handle and/or relay CleverTap events. You can use AWS or Cloudflare to route event traffic through a domain you control.

# Data filtering with Custom Proxy

Using your own custom proxy domain allows you to collect data and enforce compliance or firewall rules effectively. Custom Proxy Domain ensures:

* Users have fine control over filtering, auditing, and/or cleaning data before sending it to CleverTap.
* Users can turn event collection on or off across all platforms quickly.
* Users can use their own application server and a proxy domain to handle and/or relay CleverTap events.

# Integration

To set up the proxy domain, you must have a domain and access to the DNS site settings. Follow the instructions below to configure an AWS Certificate and CloudFront distribution. Then integrate CleverTap SDK with your proxy domain configuration. You can do this via AWS Lambda or Cloudflare Workers.

## Prerequisites

* A registered domain, for example `eu1.yourdomain.com`.
* An AWS account with permissions for Lambda, API Gateway, and ACM.
* DNS access to your domain registrar or provider.

## AWS Lambda

To create a certificate using ACM in the required region:

1. [Create Lambda Function](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-1-create-lambda-function)
2. [Add Lambda Proxy Code](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-2-add-lambda-proxy-code)
3. [Create API Gateway](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-3-create-api-gateway)
4. [Create API Proxy Resource](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-4-create-api-proxy-resource)
5. [Set up Lambda Integration](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-5-set-up-lambda-integration)
6. [Deploy the API](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-6-deploy-the-api)
7. [Set up Custom Domain in API Gateway](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-7-set-up-custom-domain-in-api-gateway)
8. [Update DNS Record](https://developer.clevertap.com/docs/web-custom-proxy-domain#step-8-update-dns-record)

### Step 1: Create Lambda Function

1. Navigate to [AWS Lambda Console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions)  and click **Create Function**.

<Image alt="AWS Lambda Console" align="center" border={true} src="https://files.readme.io/1fb9158e350de4ab13faf09867b2a4e2ebf02c6a9581377b0086ac34c7685752-image.png">
  AWS Lambda Console
</Image>

2. Select **Author from scratch**.
3. Enter a name (for example, `clevertap-proxy-lambda`)
4. Choose the latest default node runtime.
5. Under **Permissions**, select **Create a new role with basic Lambda permissions**.

<Image alt="Basic Information" align="center" border={true} src="https://files.readme.io/e84af91b73254d95c3f30aa30a11a8c1db1087dc4f5d2deda09291accdf19a80-image.png">
  Basic Information
</Image>

6. Click **Create Function** at the bottom of the page.

### Step 2: Add Lambda Proxy Code

Refer to the below console:

<Image alt="AWS Console" align="center" border={true} src="https://files.readme.io/5a468a3a8cc46ba4382118663a1273b0d4ef78ae0dc30b9395bc2d4379ea7773-image.png">
  AWS Console
</Image>

Replace the default code with this proxy implementation:

```javascript
import https from 'https';
import url from 'url';
import zlib from 'zlib';

export const handler = async (event) => {
    return new Promise((resolve, reject) => {
        try {
            // Log the incoming request for debugging
            console.log('Received event:', JSON.stringify(event, null, 2));
            
            // Extract query parameters from the event
            const queryParams = event.queryStringParameters || {};
            
            // Create URL object for the target
            const targetUrl = new url.URL('https://eu1.clevertap-prod.com/a');
            
            // Add all query parameters to the target URL
            Object.keys(queryParams).forEach(key => {
                targetUrl.searchParams.append(key, queryParams[key]);
            });
            
            console.log(`Proxying request to: ${targetUrl.toString()}`);
            
            // Extract headers from the event, excluding host header
            const headers = { ...event.headers };
            if (headers.Host || headers.host) {
                delete headers.Host;
                delete headers.host;
            }
            
            // Accept compressed responses but handle decompression ourselves
            headers['accept-encoding'] = 'gzip, deflate';
            
            // Set up the request options
            const requestOptions = {
                method: event.httpMethod || 'GET',
                headers: headers
            };
            
            // Make the request to the target endpoint
            const req = https.request(targetUrl, requestOptions, (res) => {
                const encoding = res.headers['content-encoding'];
                let responseBody = [];
                
                // Set up appropriate decompression stream based on content-encoding
                let stream = res;
                
                // Handle compressed responses
                if (encoding === 'gzip') {
                    stream = res.pipe(zlib.createGunzip());
                } else if (encoding === 'deflate') {
                    stream = res.pipe(zlib.createInflate());
                }
                
                // Collect response data as Buffer chunks to handle binary data correctly
                stream.on('data', (chunk) => {
                    responseBody.push(chunk);
                });
                
                // When the response is complete
                stream.on('end', () => {
                    console.log(`Response status: ${res.statusCode}`);
                    
                    // Combine all chunks into a single Buffer and then convert to string
                    const bodyBuffer = Buffer.concat(responseBody);
                    const bodyString = bodyBuffer.toString();
                    
                    // Convert response headers to a plain object
                    const responseHeaders = {};
                    Object.keys(res.headers).forEach(key => {
                        // Skip content-encoding since we've already decompressed
                        if (key.toLowerCase() !== 'content-encoding') {
                            responseHeaders[key] = res.headers[key];
                        }
                    });
                    
                    // Ensure content type is set correctly for JSONP
                    if (bodyString.trim().startsWith('(') || 
                        bodyString.includes('callback(') || 
                        bodyString.includes('jsonp(')) {
                        responseHeaders['content-type'] = 'application/javascript';
                    }
                    
                    // Return the response from the target to the caller
                    resolve({
                        statusCode: res.statusCode,
                        headers: responseHeaders,
                        body: bodyString,
                        isBase64Encoded: false
                    });
                });
                
                // Handle stream errors
                stream.on('error', (error) => {
                    console.error('Error processing response stream:', error);
                    handleError(error);
                });
            });
            
            // Handle request errors
            req.on('error', (error) => {
                console.error('Error making request:', error);
                handleError(error);
            });
            
            // Helper function for handling errors
            function handleError(error) {
                // Check if the original request was expecting JSONP
                const callback = queryParams.callback || 'callback';
                
                if (callback) {
                    // Return error as JSONP
                    resolve({
                        statusCode: 500,
                        headers: {
                            'content-type': 'application/javascript'
                        },
                        body: `${callback}({"error": "Internal Server Error", "message": "${error.message}"});`,
                        isBase64Encoded: false
                    });
                } else {
                    // Return error as regular JSON
                    resolve({
                        statusCode: 500,
                        headers: {
                            'content-type': 'application/json'
                        },
                        body: JSON.stringify({
                            message: 'Internal Server Error',
                            error: error.message
                        }),
                        isBase64Encoded: false
                    });
                }
            }
            
            // Send the request body if it exists
            if (event.body) {
                req.write(event.body);
            }
            
            // End the request
            req.end();
            
        } catch (error) {
            console.error('Error in proxy lambda:', error);
            
            // Check if the original request was expecting JSONP
            const callback = event.queryStringParameters?.callback || 'callback';
            
            if (callback) {
                // Return error as JSONP
                resolve({
                    statusCode: 500,
                    headers: {
                        'content-type': 'application/javascript'
                    },
                    body: `${callback}({"error": "Internal Server Error", "message": "${error.message}"});`,
                    isBase64Encoded: false
                });
            } else {
                // Return error as regular JSON
                resolve({
                    statusCode: 500,
                    headers: {
                        'content-type': 'application/json'
                    },
                    body: JSON.stringify({
                        message: 'Internal Server Error',
                        error: error.message
                    }),
                    isBase64Encoded: false
                });
            }
        }
    });
};

```

> 📘 Note
>
> Update the region in the above script on line 15 in `targetUrl`, as it is currently hardcoded to [https://eu1.clevertap-prod.com/a](https://eu1.clevertap-prod.com/a).

### Step 3: Create API Gateway

1. Navigate to [API Gateway Console](https://us-east-1.console.aws.amazon.com/apigateway/main/apis?region=us-east-1).
2. Click **Create API**.

<Image alt="API Gateway Console" align="center" border={true} src="https://files.readme.io/3287cf294c79892b1f3dc2e864d37fd7c83352dc7b6e766c9f8c17efc6e2da22-image.png">
  API Gateway Console
</Image>

3. Select **REST API** and click **Build**.

<Image alt="Rest API" align="center" src="https://files.readme.io/4ab3231869719f9ca15ea475968ed7b241423e74ef96776897a8bc3b530e2087-image.png">
  Rest API
</Image>

4. Set API name (for example, `clevertap-proxy-api`), and choose **Regional** endpoint type. 
5. Click **Create API**.

<Image alt="Create REST API" align="center" border={true} src="https://files.readme.io/bd4ff09ee64a632ef9173816ba0f476a0f79680f2773e37324891eaca233fd9d-image.png">
  Create REST API
</Image>

### Step 4: Create API Proxy Resource

1. Click **Actions** → **Create Resource** in the left sidebar.

<Image alt="API Resource" align="center" src="https://files.readme.io/d609a01120c2705bdae9716efaad8152f2854049b69da3bd67f77e58b78494d2-image.png">
  API Resource
</Image>

2. Enable **Configure as proxy resource**.
3. Enter:

   * Resource Name: `proxy`
   * Resource Path: `{proxy+}`

<Image alt="Create Resource" align="center" border={true} src="https://files.readme.io/db14ea3599b79a47c1f66b50f1ad617b4f9776728447ea17df8b02a737e0a502-image.png">
  Create Resource
</Image>

4. Click **Create Resource**.

### Step 5: Set up Lambda Integration

1. Select the `ANY` method that was created, and click **Integration Request**.

<Image alt="Lambda Integration" align="center" src="https://files.readme.io/965a1ae8c274b3159d63227096ab29097ff88011fabfcc5d1fcb3a5f57ad4f13-image.png">
  Lambda Integration
</Image>

2. Choose **Lambda Function** and check **Use Lambda Proxy Integration**.
3. Enter your Lambda function name in the Lambda Function field.

<Image alt="Lambda Proxy Integration " align="center" border={true} src="https://files.readme.io/dc6bd343b4fdacac4fc790cd4a859c582fd9dd06bc742e78303d66eb46535e53-image.png">
  Lambda Proxy Integration 
</Image>

4. Click **Save**, and accept permission prompts if required.

### Step 6: Deploy the API

1. Click **Actions** → **Deploy API** at the top right.

![](https://files.readme.io/2ced10f7699d3855bc0c55e58e8ddee6988d1dad58e3d858b0e08898e650846a-image.png)

2. Create a stage (for example, `prod`) and click **Deploy**.

<Image alt="Deploy API" align="center" border={true} src="https://files.readme.io/9b18fd184bfa55dc32a94d2de2eabde18831fb26a745ffddd48da09ccbc221c2-image.png">
  Deploy API
</Image>

3. Note the displayed **Invoke URL** as it is required for the custom domain setup.

<Image alt="Invoke URL" align="center" border={true} src="https://files.readme.io/3c857e4ed74db6b0ac41b1b6ab778594086e5a08cef5cd2f5e6ea50384071031-image.png">
  Invoke URL
</Image>

### Step 7: Set up Custom Domain in API Gateway

1. In API Gateway, go to **[Custom Domain Names](https://us-east-1.console.aws.amazon.com/apigateway/main/publish/domain-names?api=unselected\&experience=rest®ion=us-east-1)**.
2. Click **Add domain name**.

<Image alt="Custom Domain Names" align="center" border={true} src="https://files.readme.io/1415af51e67d30903e1f9d96aa283df081e79f25b9dc81dc4cd1e0359db134d9-image.png">
  Custom Domain Names
</Image>

3. Set:
   1. Domain Name: `eu1.yourdomain.com`
   2. Endpoint Type: **Regional**  (use your own region)
   3. SSL Certificate: Use an existing ACM certificate or create one via [AWS Certificate Manager](https://us-east-1.console.aws.amazon.com/acm/home?region=us-east-1#/certificates/list)

<Image alt="Public Domain Configuration" align="center" border={true} src="https://files.readme.io/a51cedd75d15ca9de8a5e8a2d68450bf095e495b701bae7c5f79ae7b538f0201-image.png">
  Public Domain Configuration
</Image>

4. Click **Create**. Then, under Custom domain names, navigate to *API Mapping* and click **Add mapping**. 
5. Choose your API and stage. Click **Save**.
6. Note the generated **API Gateway domain** (it will look like `xyz123.execute-api.region.amazonaws.com`).

<Image alt="API Mapping" align="center" border={true} src="https://files.readme.io/23fede4e208ba359f0ad6aca46fb6f579331c45a39668155bad67cf3ce373d52-image.png">
  API Mapping
</Image>

### Step 8: Update DNS Record

1. Go to DNS settings in your DNS provider's website (such as Cloudflare, Hostinger, Godaddy).
2. Add a **CNAME** record:
   1. **Name**: `eu1` (region you put above)
   2. **Target**: The API Gateway domain name (e.g., `abc123.execute-api.us-east-1.amazonaws.com`)
   3. If using cloudflare, click the gray cloud icon to bypass Cloudflare proxy.
3. Save the record.

## Cloudflare Workers Setup

To setup Cloudflare Workers:

1. Navigate to [Cloudflare Workers & Pages](https://dash.cloudflare.com/57c7ac550535a9f0cff91d9d83a8683f/workers-and-pages).
2. Click **Create**. 

<Image alt="Workers and Pages" align="center" border={true} src="https://files.readme.io/792d5d133c906278f282c51b49699b7a315243952d9689b3a5bb8c40b0cdb14d-image.png">
  Workers and Pages
</Image>

3. Then click **Hello World**.

<Image alt="Templates" align="center" border={true} src="https://files.readme.io/4989390b40ad51c13892f0ba49b258a9207c0ad1c4a7276c56e4380d10323c6e-image.png">
  Templates
</Image>

4. Click **Deploy**.

<Image alt="Deploy" align="center" border={true} src="https://files.readme.io/2924c8de3edde68362d3213a6fb0caaa7c19eeb6a5cd03c6fc9a7aed9c592da3-image.png">
  Deploy
</Image>

5. Once it is deployed, click **Edit Code**.

<Image alt="Edit Code" align="center" border={true} src="https://files.readme.io/84e68de9c11a6bc3b524e094282b684691a0bb25bf82d43d9bafe5b387b3e9fc-image.png">
  Edit Code
</Image>

6. Copy the following code and paste it in the editor. Click **Deploy**.

```javascript
// Cloudflare Worker script to proxy requests to CleverTap

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  // The URL to forward requests to
  const requestUrl = new URL(request.url)
  // You can change the region. Do no
  const targetUrl = 'https://eu1.clevertap-prod.com/a' + requestUrl.search
  
  // Clone the request headers
  const headers = new Headers(request.headers)

  
  // Remove headers that might cause issues
  headers.delete('host')
  headers.delete('cf-connecting-ip')
  headers.delete('cf-ipcountry')
  headers.delete('cf-ray')
  headers.delete('cf-visitor')
  
  // Create the request options for forwarding
  const requestOptions = {
    method: request.method,
    headers: headers,
    redirect: 'follow'
  }
  
  // For POST requests, include the body
  if (request.method === 'POST') {
    // Clone the request body
    const body = await request.clone().text()
    requestOptions.body = body
  }
  
  try {
    // Forward the request to CleverTap
    const response = await fetch(targetUrl, requestOptions)
    
    // Clone the response headers
    const responseHeaders = new Headers(response.headers)
    
    // Add CORS headers to allow requests from your domain
    responseHeaders.set('Access-Control-Allow-Origin', '*')
    responseHeaders.set('Access-Control-Allow-Methods', 'GET, POST, OPTIONS')
    responseHeaders.set('Access-Control-Allow-Headers', 'Content-Type')
    
    // Create a new response with the original response and modified headers
    const modifiedResponse = new Response(response.body, {
      status: response.status,
      statusText: response.statusText,
      headers: responseHeaders
    })
    
    return modifiedResponse
  } catch (error) {
    // Return an error response if something goes wrong
    return new Response(JSON.stringify({ error: 'Failed to proxy request' }), {
      status: 500,
      headers: {
        'Content-Type': 'application/json',
        'Access-Control-Allow-Origin': '*'
      }
    })
  }
}
```

> 📘 Note
>
> Update the region in the above script on line 11 in `targetUrl`, as it is currently hardcoded to [https://eu1.clevertap-prod.com/a](https://eu1.clevertap-prod.com/a).

![](https://files.readme.io/056db4b4054892dc7ddc45695324302b4df39d9b06bf67f872db5fe8362cf1d6-image.png)

7. Once deployed go to **Settings → Custom Domains**. Add your custom domain by clicking **+ Add**.

<Image alt="Settings Page" align="center" border={true} src="https://files.readme.io/fa5bedd83e15cb42c8d426af34ec492e060cd40ccaa874b67fcd0bca426be02d-image.png">
  Settings Page
</Image>

<Image alt="Domains and Routes" align="center" width="80% " src="https://files.readme.io/0ab3f4c884cf1a809237d7714d3b169e27bf09098caceaf49990589c25a6c746-image.png">
  Domains and Routes
</Image>

8. Your cloudflare setup is done. When calling `clevertap.init`, ensure that you pass the proxy server. For example, `clevertap.init('ACCOUN_ID', ‘eu1’, ‘your-domain.com’)`.

# Integrate CleverTap to Use Proxy Domain

Update your Web SDK integration:

```javascript
clevertap.init('ACCOUNT_ID', 'eu1', 'yourdomain.com');
```

# Debug and Test

After integration, the logged events are available on the CleverTap dashboard. Use the Web SDK’s logging feature to debug requests and response.

```javascript
clevertap.setLogLevel(LOG_LEVEL) // set the DEBUG level 
// Here LOG_LEVEL is an integer that can be any of the following: 
//  0: disable all logs
//  1: display only errors
//  2: display errors and info
//  3: display all logs
```

For more details, refer to [Web SDK Quick Start Guide](https://developer.clevertap.com/docs/web-quickstart-guide).
