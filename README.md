A detailed step-by-step guide to set up a self-hosting server for personal web blog using **Ubuntu Server** on **VirtualBox**.

---

# Setting Up a Self-Hosting Server for a Personal Web Blog on Ubuntu Server using VirtualBox

## Prerequisites
- **VirtualBox**: Install [VirtualBox](https://www.virtualbox.org/) on your laptop.
- **Ubuntu Server ISO**: Download the latest version of [Ubuntu Server](https://ubuntu.com/download/server).
- Basic understanding of terminal commands.

## Step 1: Create a Virtual Machine

1. **Open VirtualBox**.
2. Click on **New** to create a new VM.
3. **Name your VM** (e.g., "UbuntuServer"), choose **Linux** as the type, and **Ubuntu (64-bit)** as the version.
4. Allocate **Memory**: Recommended at least **2048 MB** (2 GB).
5. Select **Create a virtual hard disk now** and click **Create**.
6. Choose **VDI (VirtualBox Disk Image)** and click **Next**.
7. Select **Dynamically allocated** and click **Next**.
8. Allocate at least **20 GB** for the hard disk and click **Create**.

## Step 2: Configure the Virtual Machine

1. Select the VM and click on **Settings**.
2. Under the **System** tab:
   - Uncheck **Floppy** in the Boot Order.
3. Under the **Storage** tab:
   - Click on **Empty** under Controller: IDE.
   - Click on the disk icon on the right and select **Choose a disk file**.
   - Locate and select the downloaded Ubuntu Server ISO.
4. Under the **Network** tab:
   - Change **Adapter 1** to **Bridged Adapter** to allow the VM to connect to your local network.

## Step 3: Install Ubuntu Server

1. Start the VM by selecting it and clicking **Start**.
2. The VM will boot from the ISO. Follow the installation prompts:
   - Select your language and keyboard layout.
   - Choose **Install Ubuntu Server**.
   - Select your network connection and configure it (DHCP is usually fine).
   - Choose **Guided - use entire disk** to set up disk partitions.
   - Follow the prompts to configure your time zone.
   - Create a user account and password.
   - Install OpenSSH server for remote access (recommended).
3. Wait for the installation to complete and reboot the VM.

## Step 4: Update the System

1. Log into your server with the credentials you created during installation.
2. Update the package index and upgrade installed packages:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

## Step 5: Install Required Software

### 5.1 Install Apache Web Server

1. Install Apache:
   ```bash
   sudo apt install apache2 -y
   ```
2. Enable the Apache service to start on boot:
   ```bash
   sudo systemctl enable apache2
   ```
3. Start the Apache service:
   ```bash
   sudo systemctl start apache2
   ```

### 5.2 Install MySQL Database Server

1. Install MySQL:
   ```bash
   sudo apt install mysql-server -y
   ```
2. Secure the MySQL installation:
   ```bash
   sudo mysql_secure_installation
   ```
   - Follow the prompts to set the root password and remove test users.

### 5.3 Install PHP

1. Install PHP and necessary extensions:
   ```bash
   sudo apt install php libapache2-mod-php php-mysql -y
   ```
2. Restart Apache to load PHP:
   ```bash
   sudo systemctl restart apache2
   ```

### 5.4 Install Node.js (Optional for Backend)

1. Install Node.js:
   ```bash
   sudo apt install nodejs npm -y
   ```

### 5.5 Install Python (Optional for Backend)

1. Install Python:
   ```bash
   sudo apt install python3 python3-pip -y
   ```

## Step 6: Set Up Your Blog Directory

1. Create a directory for your blog:
   ```bash
   sudo mkdir /var/www/html/myblog
   ```
2. Change the ownership to the current user (replace `yourusername`):
   ```bash
   sudo chown -R $USER:$USER /var/www/html/myblog
   ```

## Step 7: Upload Your Blog Files

1. You can upload your blog files using `scp`, `rsync`, or any other file transfer method.
2. Example using `scp` from your local machine:
   ```bash
   scp -r /path/to/your/blog/* username@your_server_ip:/var/www/html/myblog
   ```

## Step 8: Configure Apache for Your Blog

1. Create a new configuration file for your blog:
   ```bash
   sudo nano /etc/apache2/sites-available/myblog.conf
   ```
2. Add the following configuration:
   ```apache
   <VirtualHost *:80>
       ServerAdmin admin@myblog.com
       DocumentRoot /var/www/html/myblog
       ServerName your_server_ip
       <Directory /var/www/html/myblog>
           AllowOverride All
           Require all granted
       </Directory>
   </VirtualHost>
   ```
3. Enable the new site:
   ```bash
   sudo a2ensite myblog.conf
   ```
4. Restart Apache:
   ```bash
   sudo systemctl restart apache2
   ```

## Step 9: Testing Your Blog

1. Open your web browser and enter your server’s IP address:
   ```
   http://your_server_ip
   ```
2. You should see your blog!

## Step 10: Make Your Server Accessible Externally (Optional)

1. **Port Forwarding**: Configure port forwarding in your router to forward port 80 (HTTP) to your server’s IP.
2. **Dynamic DNS**: If you don't have a static IP, consider using a Dynamic DNS service to point a domain to your server's IP.
3. **Firewall Configuration**: Ensure your firewall allows incoming connections on port 80 (HTTP).

### Check Open Ports
```bash
sudo ufw allow 'Apache Full'
sudo ufw enable
```

## Step 11: Security Considerations

- Keep your server updated:
  ```bash
  sudo apt update
  sudo apt upgrade -y
  ```

- Use strong passwords for your MySQL and user accounts.
- Consider using HTTPS by installing Certbot:
  ```bash
  sudo apt install certbot python3-certbot-apache -y
  sudo certbot --apache
  ```

---

## Conclusion
Successfully set up a self-hosting server for your personal web blog using Ubuntu Server on VirtualBox.
