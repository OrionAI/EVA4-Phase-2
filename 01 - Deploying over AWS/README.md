# Session 1 - Deploying over AWS 

Deploy a pretrained MobileNet V2 model on AWS

# Installations
Please follow the below steps:  
1. Install Docker from [here](https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/).
    - Download the below files:

        - docker-ce_19.03.9~3-0~ubuntu-bionic_amd64.deb
        - docker-ce-cli_19.03.9~3-0~ubuntu-bionic_amd64.deb
        - containerd.io_1.2.13-2_amd64.deb
    - To install the file use   
    ```[bash]
    $ sudo dpkg -i docker-ce_19.03.9~3-0~ubuntu-bionic_amd64.deb
    $ sudo dpkg -i docker-ce-cli_19.03.9~3-0~ubuntu-bionic_amd64.deb
    $ sudo dpkg -i containerd.io_1.2.13-2_amd64.deb
    ```  

    - To start docker on boot use this command
    `$ sudo systemctl enable docker`  
    - To check if docker is installed run this  `$ sudo docker run hello-world`

2. Install Node.js
    - Install curl using this command `$ sudo apt install curl`
    - Run below commands to install node.js   
    ```[bash]
    $ curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh  
    $ sudo bash nodesource_setup.sh  
    $ sudo apt-get install -y nodejs
    ```
3. Install Serverless framework
     - Use this command  `$ sudo npm install -g serverless`

# Download Pretrained MobilenetV2 
You can get the code for fetching the pretrained model from [here](https://pytorch.org/hub/pytorch_vision_mobilenet_v2/)
Use the below command to download:

```
import torch  
model = torch.hub.load('pytorch/vision:v0.6.0', 'mobilenet_v2', pretrained=True)  
model.eval()  
traced_model = torch.jit.trace(model, trorch.randn(1,3,224,224))  
traced_model.save('mobilenetV2.pt')  
```
# Setting up Serverless
After creating a IAM user account in AWS console management you will have **Access key** and a **Secret key**.
 
 Create a dedicated conda environment before starting.

1. Configure serverless  
`$ serverless config credentials --provider aws --key <access_key> --secret <secret_key>`

2. Create a directory for the project  
`$ severless create --template aws-python3 --name <service_name>`

The above command will create two file in the directory *handler.py* and *serverless.yml*.

3. To install a serverless plugin  
`$ Serverless plugin install -n serverless-python-requirements`  

Add the requirements.txt file in the directory.

4. Create a S3 bucket in your AWS account with the same name assigned to **S3_BUCKET** in handler.py and upload the pretrained model 'mobilenetV2.pt' in this bucket.

5. Add a deploy script in package.json file
```
{
  "name": "mobilenetV2-pytorch",
  "description": "",
  "version": "0.1.0",
  "dependencies": {},
  "scripts": {"deploy": "serverless deploy"},
  "devDependencies": {
    "serverless-python-requirements": "^5.1.0"
  }
}
```
6. Run  
`$ npm run deploy`

Your code is deployed on AWS!

You can verify the code by using Postman or Insomnia.

