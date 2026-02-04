# Lab: Host a Static Website on Amazon S3

**Estimated Time:** 60 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** AWS Account with IAM user access, completed S3 Fundamentals lesson

## Learning Objectives

By the end of this lab, you will be able to:
- Configure an S3 bucket for static website hosting
- Upload website files to S3
- Set appropriate bucket policies for public read access
- Configure index and error documents
- Access your website via the S3 website endpoint
- Understand S3 website hosting limitations and use cases

## Scenario

Your team needs to quickly deploy a landing page for a new product launch. Instead of setting up servers, you'll use Amazon S3's static website hosting feature to serve HTML, CSS, and JavaScript files directly from S3. This is a cost-effective, scalable solution for static content.

## Lab Overview

You will:
1. Create a new S3 bucket configured for website hosting
2. Create simple HTML/CSS website files
3. Upload the files to S3
4. Configure bucket policy for public access
5. Set index and error documents
6. Test your website using the S3 endpoint
7. (Bonus) Add custom error page and subdirectory

## Prerequisites

- AWS Account with Free Tier access
- IAM user with S3 permissions (AmazonS3FullAccess or equivalent)
- AWS CLI configured OR access to AWS Console
- Basic HTML/CSS knowledge (templates provided)
- Text editor for creating web files

---

## Part 1: Create and Configure S3 Bucket for Website Hosting

### Step 1: Create a New S3 Bucket

Create a bucket specifically for your static website.

**Using AWS Console:**
1. Navigate to S3 service
2. Click **Create bucket**
3. Configure:
   - **Bucket name:** `my-static-website-[your-initials]-2026` (must be globally unique)
   - **Region:** Choose nearest region (e.g., us-east-1)
   - **Block Public Access settings:** UNCHECK "Block all public access"
     - ⚠️ Acknowledge the warning (we need public access for website hosting)
   - Leave other settings as default
4. Click **Create bucket**

**Using AWS CLI:**
```bash
aws s3 mb s3://my-static-website-[your-initials]-2026 --region us-east-1
```

**📸 Screenshot Required:** Bucket creation confirmation

### Step 2: Enable Static Website Hosting

Configure the bucket to serve web content.

**Using AWS Console:**
1. Click on your newly created bucket
2. Go to **Properties** tab
3. Scroll to **Static website hosting** section
4. Click **Edit**
5. Configure:
   - **Static website hosting:** Enable
   - **Hosting type:** Host a static website
   - **Index document:** `index.html`
   - **Error document:** `error.html`
6. Click **Save changes**
7. Note the **Bucket website endpoint** URL (e.g., `http://my-static-website-xyz-2026.s3-website-us-east-1.amazonaws.com`)

**Using AWS CLI:**
```bash
aws s3 website s3://my-static-website-[your-initials]-2026/ \
  --index-document index.html \
  --error-document error.html
```

**📸 Screenshot Required:** Static website hosting configuration showing endpoint URL

---

## Part 2: Create Website Files

### Step 3: Create HTML and CSS Files

Create simple website files on your local machine.

**Create `index.html`:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My AWS Static Website</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>Welcome to My S3 Static Website</h1>
            <p class="subtitle">Hosted on Amazon S3 - Cloud Engineering Bootcamp Lab</p>
        </header>
        
        <main>
            <section class="card">
                <h2>About This Site</h2>
                <p>This website is hosted entirely on Amazon S3 using the static website hosting feature. No servers required!</p>
            </section>
            
            <section class="card">
                <h2>Key Benefits of S3 Static Hosting</h2>
                <ul>
                    <li>💰 Cost-effective - pay only for storage and bandwidth</li>
                    <li>🚀 Scalable - handles traffic spikes automatically</li>
                    <li>⚡ Fast - leverages AWS global infrastructure</li>
                    <li>🔒 Reliable - 99.999999999% durability</li>
                </ul>
            </section>
            
            <section class="card">
                <h2>What You Learned</h2>
                <ul>
                    <li>Created and configured an S3 bucket</li>
                    <li>Enabled static website hosting</li>
                    <li>Set bucket policies for public access</li>
                    <li>Uploaded website files to S3</li>
                    <li>Accessed content via S3 endpoint</li>
                </ul>
            </section>
        </main>
        
        <footer>
            <p>Cloud Engineering Bootcamp © 2026 | Powered by Amazon S3</p>
        </footer>
    </div>
</body>
</html>
```

**Create `styles.css`:**
```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
    line-height: 1.6;
    color: #333;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    padding: 20px;
}

.container {
    max-width: 800px;
    margin: 0 auto;
    background: white;
    border-radius: 10px;
    box-shadow: 0 10px 40px rgba(0,0,0,0.2);
    overflow: hidden;
}

header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 40px 30px;
    text-align: center;
}

header h1 {
    font-size: 2.5em;
    margin-bottom: 10px;
}

.subtitle {
    font-size: 1.1em;
    opacity: 0.9;
}

main {
    padding: 30px;
}

.card {
    background: #f8f9fa;
    border-left: 4px solid #667eea;
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 5px;
}

.card h2 {
    color: #667eea;
    margin-bottom: 15px;
}

.card ul {
    list-style: none;
    padding-left: 0;
}

.card li {
    padding: 8px 0;
    border-bottom: 1px solid #e0e0e0;
}

.card li:last-child {
    border-bottom: none;
}

footer {
    background: #2d3748;
    color: white;
    text-align: center;
    padding: 20px;
    font-size: 0.9em;
}
```

**Create `error.html`:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>404 - Page Not Found</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        .error-container {
            text-align: center;
            padding: 60px 20px;
        }
        .error-code {
            font-size: 6em;
            color: #667eea;
            font-weight: bold;
        }
        .back-link {
            display: inline-block;
            margin-top: 20px;
            padding: 12px 30px;
            background: #667eea;
            color: white;
            text-decoration: none;
            border-radius: 5px;
            transition: background 0.3s;
        }
        .back-link:hover {
            background: #764ba2;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="error-container">
            <div class="error-code">404</div>
            <h1>Oops! Page Not Found</h1>
            <p>The page you're looking for doesn't exist on this S3 static website.</p>
            <a href="/" class="back-link">Go Back Home</a>
        </div>
    </div>
</body>
</html>
```

**Save all three files in a local directory** (e.g., `~/website-files/`)

---

## Part 3: Upload Files and Configure Bucket Policy

### Step 4: Upload Website Files to S3

**Using AWS Console:**
1. In your S3 bucket, click **Upload**
2. Click **Add files**
3. Select `index.html`, `styles.css`, and `error.html`
4. Click **Upload**
5. Wait for upload to complete

**Using AWS CLI:**
```bash
cd ~/website-files/
aws s3 cp index.html s3://my-static-website-[your-initials]-2026/
aws s3 cp styles.css s3://my-static-website-[your-initials]-2026/
aws s3 cp error.html s3://my-static-website-[your-initials]-2026/
```

**📸 Screenshot Required:** S3 bucket showing uploaded files

### Step 5: Configure Bucket Policy for Public Read Access

To allow public access to your website, you need a bucket policy.

**Using AWS Console:**
1. Go to **Permissions** tab
2. Scroll to **Bucket policy** section
3. Click **Edit**
4. Paste this policy (replace `YOUR-BUCKET-NAME`):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-statis-website-tb-2026/*"
        }
    ]
}
```

5. Replace `YOUR-BUCKET-NAME` with your actual bucket name
6. Click **Save changes**

**Using AWS CLI:**
Create a file `bucket-policy.json`:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::my-static-website-[your-initials]-2026/*"
        }
    ]
}
```

Apply the policy:
```bash
aws s3api put-bucket-policy \
  --bucket my-static-website-[your-initials]-2026 \
  --policy file://bucket-policy.json
```

**📸 Screenshot Required:** Bucket policy configuration

---

## Part 4: Test Your Website

### Step 6: Access Your Website

1. Go back to **Properties** tab
2. Scroll to **Static website hosting**
3. Click on the **Bucket website endpoint** URL
4. Your website should load!

**Test the error page:**
- Try accessing a non-existent page: `http://[your-endpoint]/nonexistent.html`
- You should see your custom 404 error page

**📸 Screenshot Required:** 
- Working website in browser
- Custom error page

---

## Part 5: CLI Commands Summary

Document all CLI commands you used:

```bash
# Create bucket
aws s3 mb s3://my-static-website-xyz-2026 --region us-east-1

# Enable website hosting
aws s3 website s3://my-static-website-xyz-2026/ \
  --index-document index.html \
  --error-document error.html

# Upload files
aws s3 cp index.html s3://my-static-website-xyz-2026/
aws s3 cp styles.css s3://my-static-website-xyz-2026/
aws s3 cp error.html s3://my-static-website-xyz-2026/

# Set bucket policy
aws s3api put-bucket-policy \
  --bucket my-static-website-xyz-2026 \
  --policy file://bucket-policy.json

# Get website URL
aws s3api get-bucket-website --bucket my-static-website-xyz-2026
```

---

## Verification Checklist

- [ ] S3 bucket created with public access enabled
- [ ] Static website hosting enabled with index and error documents configured
- [ ] Website files (index.html, styles.css, error.html) uploaded
- [ ] Bucket policy applied for public read access
- [ ] Website accessible via S3 endpoint
- [ ] Custom error page displays when accessing non-existent pages
- [ ] Website loads correctly with styling

---

## Bonus Challenges

### Challenge 1: Add an About Page
- Create `about.html` with your bio
- Upload to S3
- Link from index.html
- Test navigation

### Challenge 2: Organize with Subdirectories
- Create `images/` folder in bucket
- Add a logo image
- Reference in your HTML
- Test image loading

### Challenge 3: Sync Entire Directory
```bash
aws s3 sync ./website-files/ s3://my-static-website-xyz-2026/ --delete
```

---

## Common Issues and Troubleshooting

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| 403 Forbidden | Bucket policy not applied | Check bucket policy allows public GetObject |
| 404 Not Found | File not uploaded or wrong name | Verify file exists in bucket root |
| Website doesn't load styling | CSS path incorrect | Ensure `styles.css` is in same directory as HTML |
| Can't access endpoint | Static hosting not enabled | Enable static website hosting in Properties |
| Images don't load | Incorrect path or not uploaded | Check file paths and upload status |

---

## Cleanup Instructions

To avoid charges (minimal for this lab):

1. **Empty the bucket:**
   ```bash
   aws s3 rm s3://my-static-website-[your-initials]-2026/ --recursive
   ```

2. **Delete the bucket:**
   ```bash
   aws s3 rb s3://my-static-website-[your-initials]-2026
   ```

Or via Console:
1. Select bucket
2. Click **Empty** → confirm
3. Click **Delete** → type bucket name → confirm

---

## Key Takeaways

✅ **S3 Static Website Hosting** is a simple, cost-effective way to host static content  
✅ **Bucket Policies** control public access to your content  
✅ **Index and Error Documents** provide user-friendly navigation  
✅ **S3 Website Endpoints** follow format: `http://bucket-name.s3-website-region.amazonaws.com`  
✅ **No server management** required - AWS handles scaling and availability  

---

## Next Steps

- Explore **CloudFront** for CDN capabilities and HTTPS
- Learn about **Route 53** for custom domain mapping
- Investigate **S3 versioning** for file management
- Study **S3 server access logging** for analytics

**Congratulations!** You've successfully deployed a static website on Amazon S3! 🎉
