# monga-container-server

ssh-keygen -t rsa -b 4096 -C "your@email.com"

ssh -T git@github.com





Setting up a SSH Key
Make sure to follow the below steps while creating SSH Keys and using them. The best practice is create the SSH Keys on local machine not remote machine. Login with username specified in Github Secrets. Generate a RSA Key-Pair:

Generate rsa key
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Generate ed25519 key
ssh-keygen -t ed25519 -a 200 -C "your_email@example.com"
Add newly generated key into Authorized keys. Read more about authorized keys here.

Add rsa key into Authorized keys
cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'
Add ed25519 key into Authorized keys
cat .ssh/id_ed25519.pub | ssh b@B 'cat >> .ssh/authorized_keys'
Copy Private Key content and paste in Github Secrets.

Copy rsa Private key
clip < ~/.ssh/id_rsa
Copy ed25519 Private key
clip < ~/.ssh/id_ed25519
See the detail information about SSH login without password.

A note from one of our readers: Depending on your version of SSH you might also have to do the following changes:

Put the public key in .ssh/authorized_keys2
Change the permissions of .ssh to 700
Change the permissions of .ssh/authorized_keys2 to 640
If you are using OpenSSH
If you are currently using OpenSSH and are getting the following error:

ssh: handshake failed: ssh: unable to authenticate, attempted methods [none publickey]
Make sure that your key algorithm of choice is supported. On Ubuntu 20.04 or later you must explicitly allow the use of the ssh-rsa algorithm. Add the following line to your OpenSSH daemon file (which is either /etc/ssh/sshd_config or a drop-in file under /etc/ssh/sshd_config.d/):

CASignatureAlgorithms +ssh-rsa
Alternatively, ed25519 keys are accepted by default in OpenSSH. You could use this instead of rsa if needed:

ssh-keygen -t ed25519 -a 200 -C "your_email@example.com"






name: deploy container docker
on:
  pull_request:
    branches:    
      - deploy-container-docker

jobs:

  build:
    name: Clone repo
    runs-on: ubuntu-latest
    steps:
    - name: executing remote ssh commands using password
      uses: appleboy/ssh-action@v0.1.10
      with:
        host: ${{ secrets.HOST }}
        username: ${{ secrets.USERNAME }}
        password: ${{ secrets.PASSWORD }}
        port: ${{ secrets.PORT }}
        script: 
