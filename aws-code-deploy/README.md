# Deploying nodejs code with AWS Code Deploy
* Requires 2 iam roles
  * For the Code Deploy
    > Use the policy provided by AWS
  * For the EC2 instance on which the code needs to be deployed
    > For EC2 it just needs a policy to access S3

* Make sure the EC2 instance has AWS code deploy agent installed
  > `$ cd /home/ec2-user`
  
  > `$ curl -O https://aws-codedeploy-ap-south-1.s3.amazonaws.com/latest/install`
  
  > `$ chmod +x ./install`
  
  > `$ sudo ./install auto`

  *Use specific region in place of ap-south-1 on step 2 above*

* Use PM2
## Typical `appspec.yml`
```yml
version: 0.0
os: linux
files:
  - source: /
    destination: /home/ec2-user/<app>
permissions:
  - object: /
    pattern: "**"
    owner: ec2-user
    group: ec2-user
hooks:
  ApplicationStop:
    - location: /scripts/stop.sh
      timeout: 10
      runas: ec2-user
  BeforeInstall:
    - location: /scripts/setup.sh
      timeout: 180
      runas: ec2-user
  AfterInstall:
    - location: /scripts/afterinstall.sh
      timeout: 360
      runas: ec2-user
  ApplicationStart:
    - location: /scripts/start.sh
      timeout: 30
      runas: ec2-user
```
## Scripts
### `setup.sh`
```bash
#!/bin/bash
source /home/ec2-user/.bash_profile

# First install node
curl --silent --location https://rpm.nodesource.com/setup_8.x | sudo bash -
yum -y install nodejs

# Install Yarn
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
yum -y install yarn

# Install pm2
npm install -g pm2
pm2 update
```

### `afterinstall.sh` (typical react)
```bash
#!/bin/bash
source /home/ec2-user/.bash_profile

# Install all packages via yarn
cd /home/ec2-user/<app>
yarn install

# Run build steps
cd /home/ec2-user/<app>
lerna run build

# Remove source maps or other files from the build (create-react-app)
# Might not be needed when coming after AWS Code Build
cd /home/ec2-user/<app>/packages/server/build/public
find . -type f -name '*.map' -exec rm {} +
```

### `start.sh`
```bash
#!/bin/bash
source /home/ec2-user/.bash_profile

# Stop the app (Ignore error if the app is not registered)
pm2 stop app || true

# Start the app
cd /home/ec2-user/<app>/packages/server
pm2 start build/index.js -n app --update-env
```

### `stop.sh`
```bash
#!/bin/bash
source /home/ec2-user/.bash_profile

# Kill all node applications
pm2 stop app || true
```