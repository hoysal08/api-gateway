 ## Authentication Guide 

 # Flow 

 1. Client App generates a Oauth token using the fireBase social login
 2. Send the received token to the request "/user-details" by setting the code in the header with key "authorization"
 3. The step 3 will return a response with the header "authorization" of our custom code, which you would have to include in all the requests here after(so store it in the cookie or local storage)
 4. Route the user back to login page, if you get 401 on any of the requests.
 # Steps to integrate the Auth into the Client application 

-> Setup Firebase SDK in your vue project 

-> Init the firebase sdk with the give config’s and call the functions used in this sample project take reference from here(https://github.com/hoysal08/firebase-auth-article) 

```
const firebaseConfig = { 
  apiKey: "AIzaSyCGQ9vD8waHzZbUYRM7KzlNUnqWZ0tOcr0", 
  authDomain: "social-auth-b2fd4.firebaseapp.com", 
  projectId: "social-auth-b2fd4", 
  storageBucket: "social-auth-b2fd4.appspot.com", 
  messagingSenderId: "607920028862", 
  appId: "1:607920028862:web:7b4d2b07c0c9bbe0f172ef", 
  measurementId: "G-ZENJKFKZE3" 
}; 
```

-> Call Gateway with <GATEWAY_IP>:<GATWAY_PORT>/user-details 
with setting header “authorization” 

Code Example -> 
```
 const authToken = "Bearer YourAuthTokenHere"; // Replace with your actual token 
 const apiUrl = "https://example.com/api/some-endpoint"; // Replace with your actual API URL 

// Create a fetch request with the authorization header 
 fetch(apiUrl, { 
   method: "GET", // Replace with the HTTP method you need (GET, POST, PUT, etc.) 
   headers: { 
     "Authorization": authToken, // Attach the authorization header 
     // You can also include other headers if needed 
     "Content-Type": "application/json", // Example of adding a JSON content type header 
   }, 
 }) 
   .then((response) => { 
     if (!response.ok) { 
       throw new Error("Network response was not ok"); 
     } 
     return response.json(); // Assuming the response is in JSON format 
   }) 
   .then((data) => { 
     // Handle the response data here 
     console.log(data); 
   }) 
   .catch((error) => { 
     // Handle any errors that occurred during the fetch 
     console.error("Fetch error:", error); 
   }); 
```

-> The response from the above step will return you a response with the header attached with a new custom token. You can access the token by

``` 
fetch('your-api-endpoint', {
  method: 'GET', // or 'POST', 'PUT', etc.
  headers: {
    'Authorization': 'Bearer your-jwt-token'
    // Add other headers as needed
  }
})
  .then(response => {
    // Check if the response has the Authorization header
    const authHeader = response.headers.get('Authorization');
    if (authHeader) {
      // The Authorization header is present in the response
      console.log('Authorization Header:', authHeader);
      // You can now use authHeader as needed
    } else {
      // The Authorization header is not present in the response
      console.log('Authorization Header is not present');
    }

    // Handle the response data here
    if (response.ok) {
      // Request was successful
      return response.json(); // Parse the JSON response
    } else {
      // Request failed (e.g., 401 Unauthorized)
      throw new Error('Request failed');
    }
  })
  .then(data => {
    // Handle the parsed response data here
    console.log('Response Data:', data);
  })
  .catch(error => {
    // Handle errors here
    console.error('Error:', error);
  });
```

-> Attach the recevied token in all the following responses.
