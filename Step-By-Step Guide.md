## Detailed step-by-step guide to building a web application hosting project using Amazon EC2 for the web server and Amazon S3 for storing static website files:

**Step 1: Set Up Amazon EC2 Instance**

1. **Sign in to the AWS Management Console**: Go to https://aws.amazon.com/ and sign in to the AWS Management Console.

2. **Launch an EC2 Instance**:
   - Navigate to the EC2 dashboard.
   - Click on the "Launch Instance" button.
   - Choose an Amazon Machine Image (AMI), such as Amazon Linux or Ubuntu.
   - Select an instance type based on your requirements (e.g., t2.micro for free tier).
   - Configure instance details, including network settings, subnet, and IAM role.
   - Add storage (default settings are usually sufficient).
   - Configure security groups to allow inbound HTTP (port 80) and SSH (port 22) traffic.
   - Review and launch the instance, then create or select an existing key pair for SSH access.

3. **Connect to the EC2 Instance**:
   - Once the instance is running, note down its public IP address or public DNS.
   - Use an SSH client (e.g., PuTTY on Windows, Terminal on macOS/Linux) to connect to the instance using the provided key pair and the SSH command:
     ```
     ssh -i your-key.pem ec2-user@your-instance-public-ip
     ```
   - Replace `your-key.pem` with the path to your private key file, and `your-instance-public-ip` with the public IP address of your EC2 instance.

4. **Install and Configure Web Server (e.g., Apache or Nginx)**:
   - Update the package repository:
     ```
     sudo yum update -y   # For Amazon Linux
     sudo apt update      # For Ubuntu
     ```
   - Install Apache web server (for Amazon Linux):
     ```
     sudo yum install httpd -y
     ```
     Or Nginx (for Ubuntu):
     ```
     sudo apt install nginx -y
     ```
   - Start the web server:
     ```
     sudo service httpd start   # For Apache
     sudo service nginx start   # For Nginx
     ```
   - Test the web server by accessing the public IP address of your instance in a web browser. You should see the default page if Apache or Nginx is installed correctly.

**Step 2: Set Up Amazon S3 Bucket**

1. **Create an S3 Bucket**:
   - Navigate to the S3 dashboard.
   - Click on the "Create bucket" button.
   - Enter a unique bucket name, select a region.
   - In Object Ownership select ACLs enabled.
   - Configure options such as versioning, logging, and tags (optional).
   - Set permissions to allow public access to objects if you want the website to be publicly accessible.

2. **Upload Website Files to S3**:
   - Click on the newly created bucket.
   - Upload your website files (HTML, CSS, JavaScript, images, etc.) to the bucket.
   - Ensure that the main HTML file is named `index.html` if you want it to serve as the default landing page.

3. **Enable Static Website Hosting**:
   - In the bucket properties, go to the "Static website hosting" section.
   - Select "Use this bucket to host a website" and enter the index document (e.g., `index.html`).
   - Optionally, configure error document settings.
   - Note down the endpoint URL provided, which will be used to access the website.

**Step 3: Configure Domain and DNS (Optional)**

If you want to use a custom domain for your website, you'll need to configure DNS settings to point to the S3 endpoint or an Elastic Load Balancer (ELB) if you're using it. This involves updating the DNS records with your domain registrar to include a CNAME record or an Alias record pointing to the S3 endpoint or ELB hostname.

**Step 4: Test the Web Application**

1. Access the website by visiting the S3 endpoint URL or custom domain (if configured).
2. Verify that the website is serving the content correctly and all links and resources are loading properly.

---

### Check Bucket Policy:

Navigate to your S3 bucket in the AWS Management Console.
- Go to the "Permissions" tab.
- Check if there's a bucket policy defined. If so, ensure that it allows public access to the objects in the bucket.
- Here's an example of a bucket policy that grants public read access to all objects in the bucket:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```
Replace "your-bucket-name" with the name of your S3 bucket.

🥂 Congratulations! You've successfully set up a web application hosting project using Amazon EC2 for the web server and Amazon S3 for storing static website files. You can further enhance the project by adding features such as SSL/TLS encryption, custom domain setup, logging, and monitoring using additional AWS services.
