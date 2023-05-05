If you need to create a self signed certificate , then use 
`openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes`
Enter the CN : <DNS name>

  This will create key.pem and cert.pem in the location, which can be used to create the secrets. 
  
  The secret can be created using the below command.
  
  `kubectl create secret tls ing-sec --cert=cert.pem --key=key.pem`
  
  This would create the secret ing-sec which can be passed in the tls block in the ingress.yaml to secure the ingress using https.
  
  
  *Refer the ingress.yaml to see how the tls is enabled in ingress*
