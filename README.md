# Example React App

This repository contains the source code for the Example React App. The following instructions will guide you through the process of setting up and deploying this app on an AWS EC2 instance using Nginx.

## Prerequisites

Before you begin, ensure you have the following:

- An AWS account
- An EC2 instance running Ubuntu
- A domain name pointing to your EC2 instance's public IP address
- Node.js and npm installed on your EC2 instance
- Nginx installed on your EC2 instance

## Installation

1. **SSH into your EC2 instance:**

    ```sh
    ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
    ```

2. **Update the package list and install necessary packages:**

    ```sh
    sudo apt update
    sudo apt install git nginx -y
    ```

3. **Clone the repository:**

    ```sh
    mkdir -p /home/ubuntu/apps
    cd /home/ubuntu/apps
    git clone https://github.com/your-username/example-react-app.git
    ```

4. **Navigate to the app directory and install dependencies:**

    ```sh
    cd example-react-app
    npm install
    ```

5. **Build the React app:**

    ```sh
    npm run build
    ```

## Configure Nginx

1. **Create a new Nginx configuration file for the app:**

    ```sh
    sudo nano /etc/nginx/sites-available/example-react-app
    ```

2. **Add the following content to the file:**

    ```nginx
    server {
        listen 80;
        server_name yourdomain.com www.yourdomain.com;

        root /home/ubuntu/apps/example-react-app/build;
        index index.html;

        location / {
            try_files $uri /index.html;
        }

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root /usr/share/nginx/html;
        }
    }
    ```

3. **Enable the configuration:**

    ```sh
    sudo ln -s /etc/nginx/sites-available/example-react-app /etc/nginx/sites-enabled/
    ```

4. **Test the Nginx configuration:**

    ```sh
    sudo nginx -t
    ```

5. **Reload Nginx to apply the changes:**

    ```sh
    sudo systemctl reload nginx
    ```

## Update DNS Settings

1. **Log in to your domain registrar's website.**
2. **Find the DNS settings for your domain.**
3. **Create an A record pointing to your EC2 instance's public IP address:**

    - **Name:** `@` (or leave it blank to represent the root domain)
    - **Type:** `A`
    - **Value:** Your EC2 instance's public IP address
    - **TTL:** Default or 3600 seconds

## Secure Your Site with SSL (Optional)

1. **Install Certbot:**

    ```sh
    sudo apt install certbot python3-certbot-nginx -y
    ```

2. **Obtain and install the SSL certificate:**

    ```sh
    sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
    ```

3. **Follow the prompts to complete the SSL certificate installation.**

## Access Your App

- Open your web browser and navigate to `http://yourdomain.com` or `https://yourdomain.com`
