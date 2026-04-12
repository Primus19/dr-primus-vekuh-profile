# Dr. Primus Vekuh - Personal Portfolio

A stunning, Apple/Tesla-inspired personal portfolio website for Dr. Primus Vekuh, showcasing his expertise in AI, Cloud Computing, Blockchain, and his entrepreneurial ventures.

## Tech Stack

- **HTML5** - Semantic markup
- **CSS3** - Custom properties, Grid, Flexbox, animations
- **Vanilla JavaScript** - Particle effects, scroll animations, counter animations
- **GitHub Actions** - CI/CD pipeline for automated S3 deployment

## Project Structure

```
primus-vekuh-portfolio/
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions CI/CD pipeline
├── css/
│   └── style.css               # Main stylesheet
├── js/
│   └── main.js                 # JavaScript animations & interactivity
├── images/
│   ├── hero-bg.jpg             # Hero section background
│   ├── ai-abstract.jpg         # AI section background
│   ├── blockchain-bg.jpg       # Blockchain section background
│   ├── cloud-bg.jpg            # Cloud section background
│   ├── section-bg-dark.jpg     # Dark section background
│   ├── primus-headshot.webp    # Profile photo
│   ├── primus-hero.jpg         # Hero photo
│   ├── primus-speaker.jpg      # Speaker photo
│   └── primus-talk.jpg         # Talk photo
├── index.html                  # Main HTML file
├── .gitignore
└── README.md
```

## Deployment to AWS S3

### Prerequisites

1. An AWS account
2. An S3 bucket configured for static website hosting
3. AWS CLI credentials (Access Key ID and Secret Access Key)

### S3 Bucket Setup

1. **Create an S3 bucket:**
   ```bash
   aws s3 mb s3://your-bucket-name --region us-east-1
   ```

2. **Enable static website hosting:**
   ```bash
   aws s3 website s3://your-bucket-name \
     --index-document index.html \
     --error-document index.html
   ```

3. **Set the bucket policy for public access:**
   ```json
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

4. **Disable Block Public Access** (in S3 bucket settings) if needed for public hosting.

### GitHub Actions CI/CD Setup

The repository includes a GitHub Actions workflow (`.github/workflows/deploy.yml`) that automatically deploys to S3 on every push to the `main` branch.

#### Required GitHub Secrets

Go to your GitHub repository **Settings > Secrets and variables > Actions** and add these secrets:

| Secret Name | Description | Example |
|---|---|---|
| `AWS_ACCESS_KEY_ID` | Your AWS IAM access key ID | `AKIAIOSFODNN7EXAMPLE` |
| `AWS_SECRET_ACCESS_KEY` | Your AWS IAM secret access key | `wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY` |
| `S3_BUCKET_NAME` | Your S3 bucket name | `primus-vekuh-portfolio` |
| `CLOUDFRONT_DISTRIBUTION_ID` | (Optional) CloudFront distribution ID | `E1ABCDEF123456` |

#### IAM Policy for Deployment

Create an IAM user with the following policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject",
        "s3:ListBucket",
        "s3:PutObjectAcl"
      ],
      "Resource": [
        "arn:aws:s3:::your-bucket-name",
        "arn:aws:s3:::your-bucket-name/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "cloudfront:CreateInvalidation",
      "Resource": "*"
    }
  ]
}
```

### Manual Deployment

You can also deploy manually using the AWS CLI:

```bash
# Sync all files to S3
aws s3 sync . s3://your-bucket-name \
  --delete \
  --exclude ".git/*" \
  --exclude ".github/*" \
  --exclude "README.md" \
  --exclude ".gitignore"

# Your site will be available at:
# http://your-bucket-name.s3-website-us-east-1.amazonaws.com
```

### Optional: CloudFront CDN

For HTTPS and better performance, set up a CloudFront distribution:

1. Create a CloudFront distribution pointing to your S3 bucket
2. Set the default root object to `index.html`
3. Add the `CLOUDFRONT_DISTRIBUTION_ID` secret to GitHub for automatic cache invalidation

## Local Development

Simply open `index.html` in a browser, or use a local server:

```bash
# Python
python3 -m http.server 8080

# Node.js
npx serve .
```

## Features

- Particle background animation with neural network connections
- Smooth scroll reveal animations
- Animated number counters
- Parallax scrolling effects
- Responsive design (mobile, tablet, desktop)
- Apple/Tesla-inspired dark theme
- Glassmorphism navigation bar
- Gradient text effects
- Interactive hover states

## License

All rights reserved. Dr. Primus Vekuh.
