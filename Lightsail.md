### Choose Operating Systems (Free for first month)
- Press `Create instance`
- First we will change the Instance location to Tokyo, Press `Change AWS Region and Availability Zone`
- We choose `Linux/Unix` platform and choose `CentOs` in the `Select a blueprint --- OS Only`
- Then skip to the `instance plan` selection and choose `$3.50 USD`
- U could change the VPS name in `Identify your instance`

### Connect to your VPS instance
You will see a VPS Instance with 512 MB RAM, 1 vCPU, 20 GB SSD and a default public ip. We will learn how to connect to your VPS.
- Press the `Terminal icon` beside your VPS instance. There will be a new window with `ssh` connection.
- Input `sudo passwd root` to change the password of root user. Then you will input your own password twice.
- Now we have changed the password of root. However, the aws uses openssl to ensure the security of ssh connection. We will modify the `/etc/ssh/sshd_config` to open the user-password login.
- Stay in the window of terminal, input `sudo vi /etc/ssh/sshd_config`, find the option of `PasswordAuthentication` and change the `no` to `yes`.
- Input `systemctl restart sshd.service` to activate the revision.
- We have finished the config of user-password login and you can now login to your VPS with ssh client(PUTTY etc.) or terminal(`ssh root@your_vps_ip`)

