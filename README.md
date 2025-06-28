Step 1: Create and Launch an EC2 Instance
1.	Log in to AWS Console:
Go to https://console.aws.amazon.com
2.	Navigate to EC2 Dashboard:
Click Services → EC2 → Launch Instance
3.	Configure the Instance:
o	Name: node-nginx-app
o	AMI: Ubuntu Server 24.02 LTS
o	Instance Type: t2.micro (Free Tier eligible)
o	Key Pair: Create or select a .pem key
o	Security Group: Allow ports 22 (SSH), 80 (HTTP), and 3000 (Node.js)
4.	Launch the Instance
After configuration, click Launch Instance
5.	Connect via SSH:
6.	chmod 400 your-key.pem
7.	ssh -i your-key.pem ubuntu@<your-ec2-public-ip>


🧱 Step 2: Set Up Node.js on EC2
1.	Update packages:
sudo apt update && sudo apt upgrade -y
2.	Install Node.js and npm:
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
3.	Verify installation:
node -v
npm -v
🧪 Step 3: Create a Simple Node.js App
1.	Copy the project from github:
Git clone git@github.com:milintank/AWS_nodejs.git
2.	Initialize project:
npm init -y
3.	Install Express:
npm install express
4.	Run the app:
node index.js

🔁 Step 4: Install and Configure NGINX
1.	Install NGINX:
sudo apt install nginx -y
2.	Create NGINX config:
sudo nano /etc/nginx/sites-available/nodeapp
3.	Paste this config:
server {
  	    listen 80;
    server_name your-ec2-public-ip;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
4.	Enable the config:
sudo ln -s /etc/nginx/sites-available/nodeapp /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx

✅ Step 5: Test It
Open your browser and go to:
http://<your-ec2-public-ip>
You should see:
Hello from Node.js behind NGINX!
