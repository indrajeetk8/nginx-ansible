Create a website/ folder with your full static site (HTML, CSS, etc.)

Use Ansible to copy the entire site to /var/www/html/

Serve it using Nginx

🧱 Folder Structure (Example)
pgsql
Copy
Edit
nginx-ansible/
├── install_nginx.yml
├── hosts.ini
└── website/
    ├── index.html
    ├── styles.css
    └── images/
        └── logo.png
🔹 Step 1: Add Your Static Site
Put your website files inside the website/ folder:

Example index.html:

html
Copy
Edit
<!DOCTYPE html>
<html>
<head>
  <title>My Static Site</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <h1>🚀 Deployed via Ansible!</h1>
  <img src="images/logo.png" alt="Logo" width="200">
</body>
</html>
Example styles.css:

css
Copy
Edit
body {
  font-family: Arial;
  background-color: #f5f5f5;
  text-align: center;
  color: #333;
}
✏️ Step 2: Update Your install_nginx.yml Playbook
We’ll use the copy module with recursive: yes:

yaml
Copy
Edit
---
- name: Install Nginx and Deploy Static Site
  hosts: webserver
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start and enable nginx
      service:
        name: nginx
        state: started
        enabled: yes

    - name: Copy static website to Nginx root
      copy:
        src: website/
        dest: /var/www/html/
        owner: www-data
        group: www-data
        mode: '0755'
        force: yes
🚀 Step 3: Run the Playbook
From your nginx-ansible/ folder:

bash
Copy
Edit
ansible-playbook -i hosts.ini install_nginx.yml
🧪 Step 4: Verify in Browser
Go to:

cpp
Copy
Edit
http://<your-ec2-public-ip>
