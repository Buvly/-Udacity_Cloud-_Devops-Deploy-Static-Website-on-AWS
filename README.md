# Udacity-Cloud-Devops-Deploy-Static-Website-on-AWS
##steps Using AWS Console:

1- Create S3 bucket --> we will name it as static-website-bucket-AWSID(must be globally unique) --> Allow public access

2- Once it is created, go to Buckets --> click on bucket's name

3- Download the ready starter code from here: https://drive.google.com/open?id=15vQ7-utH7wBJzdAX3eDmO9ls35J5_sEQ
Click on Upload:
  Add files to upload the index.html file, and click "Add folder" to upload the css, img, and vendor folders.
  Do not select the udacity-starter-website folder. Instead, upload its content one-by-one.

##steps Using AWS CLI:

I) aws configure      --> to configure aws account

II) Upload files:
  aws s3api list-buckets
  Download and unzip the udacity-starter-website.zip:
    cd udacity-starter-website 
    aws s3api put-object --bucket static-website-bucket-AWSID --key index.html --body index.html     --> # Put a single file. 
    aws s3 cp vendor/ s3://my-bucket-202203081/vendor/ --recursive       --> # Copy over folders from local to S3 
    aws s3 cp css/ s3://my-bucket-202203081/css/ --recursive 
    aws s3 cp img/ s3://my-bucket-202203081/img/ --recursive 
    
3- Go to Permissions:  --> in permissions overview make sure that Objects can be public
    I) In Bucket Policy enter the below policy: 
    
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AddPerm",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::static-website-271795443445/*"]
    }
  ]
}

4- Go to Properties: --> in Static website hosting click on edit
  I) Enable Static website hosting
  II) In Index document write --> index.html
  
5- Go to Properties: --> in Static website hosting you will find the website endpoint
  for example: http://static-website-271795443445.s3-website-us-east-1.amazonaws.com
  
6- Distributing the website using Cloud Front --> search for cloud front
  I) Distributions --> click “Create Distribution”.
  II) Origin domain --> http://static-website-271795443445.s3-website-us-east-1.amazonaws.com   (Don't select the bucket from the drop down list)
  III) Viewer protocol policy --> Redirect HTTP to HTTPS
  IV) leave the rest of config as default, Click create (it may take 10-30 minutes (or more) to cache the S3 page. Once the caching is complete, the CloudFront domain name URL will stop redirecting to the S3 object URL.)
  V) Once it is created, you will find the domain name under distributions paste it in URL  --> https://doze3hrz8zbnm.cloudfront.net
