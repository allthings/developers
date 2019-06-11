## S3 access for external ERP data-, Consumption data- or Document-providers  

External data providers should have the possibility to upload CSV-data manually (with an S3-Client) or programmatically into an alltings s3-bucket.  
  
The credentials for external dataproviders shouldn't be managed in the alltings account. Therefore a separate AWS-Account `allthings-import` was created.  

Use the steps to create new credentials for a new external data-provider.

1. Login to the aws-console and switch role to `allthings-import`
  
2. Open `IAM-Management` and create a new user.  
    Use the name of the app as username, not the name of the dataprovider.    
    Choose access-type `programmatic  assess`   
    
3. Add the new user to group `import` (attached policies should be `import-access`)  
    _Important: If the uploaded data from this user should be imported via nisaba, add the user to group `nisaba-import` instead._

4. Download the credentials, maybe test them in cyberduck and send them to the customer vie a save channel (keybase).  
    Also add the credentials to the keybase-git-repository.  
    
