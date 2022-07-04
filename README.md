# Gitpod Cloud Configuration and Start Tasks Templates

Example of different start tasks to get cloud tools intalled.

- [AWS](#aws)
- [Azure](#azure)
- [Hashicorp](#hashicorp)
- [Tunneling](#tunneling)

## Considerations

These start tasks are useful if you are not using prebuilds and just to quickly copy and paste the start tasks into a new repo. If you are using prebuilds these need to be implemented as Dockerfile.

## AWS

### Set AWS Enviroment Variables

If you want to use the AWS CLI, AWS SAM, AWS Copilot you'll need AWS credentials configured.

```sh
gp env AWS_DEFAULT_REGION=XXXX
gp env AWS_SECRET_ACCESS_KEY=XXXX
gp env AWS_ACCESS_KEY_ID=us-east-1
```

### AWS Command Line Interface (CLI)

```yml
tasks:
  - name: aws-cli
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
vscode:
  extensions:
    - amazonwebservices.aws-toolkit-vscode
```

### AWS Serverless Application Model (SAM)

```yml
tasks:
  - name: aws-sam
    init: |
      cd /workspace
      pip install -r requirements.txt
      wget https://github.com/aws/aws-sam-cli/releases/latest/download/aws-sam-cli-linux-x86_64.zip
      unzip aws-sam-cli-linux-x86_64.zip -d sam-installation
      cd $THEIA_WORKSPACE_ROOT
vscode:
  extensions:
    - amazonwebservices.aws-toolkit-vscode
```

### AWS Copilot

```yml
tasks:
  - name: copilot
    init: |
      brew install aws/tap/copilot-cli
vscode:
  extensions:
    - amazonwebservices.aws-toolkit-vscode
```

### AWS EKSCTL

```yml
tasks:
  - name: eksctl
    init: |
      brew install kubectl
      brew install helm
      brew tap weaveworks/tap
      brew install weaveworks/tap/eksctl
vscode:
  extensions:
    - amazonwebservices.aws-toolkit-vscode
```

## Kubernetes

### KubeCTL

```yml
tasks:
  - name: k8s
    init: |
      brew install kubectl
vscode:
  extensions:
    - ms-kubernetes-tools.vscode-kubernetes-tools
```

### ArgoCD

```yml
tasks:
  - name: argocd
    init: |
      cd /workspace
      sudo curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.0.4/argocd-linux-amd64
      sudo chmod +x /usr/local/bin/argocd
      cd $THEIA_WORKSPACE_ROOT
```

### JenkinsX
```yml
tasks:
  - name: jx
    init: |
      cd /workspace
      curl -L https://github.com/jenkins-x/jx/releases/download/v3.2.339/jx-linux-amd64.tar.gz | tar xzv
      chmod +x jx 
      sudo mv jx /usr/local/bin
      cd $THEIA_WORKSPACE_ROOT
```

## Hashicorp

## Terraform CLI

```yml
tasks:
  - name: terraform
    init: |
      sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
      curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
      sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
      sudo apt-get update && sudo apt-get install terraform
vscode:
  extensions:
    - hashicorp.terraform
```

## Azure

### Azure CLI

```yml
tasks:
  - name: az-cli
    init: |
      curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

To login without browser access:

```sh
az login --use-device-code
```

### Azure Functions

```yml
tasks:
  - name: az-func
    init: |
      curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
      sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
      sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
      sudo apt-get update
      sudo apt-get install azure-functions-core-tools-4
vscode:
  extensions:
    ms-azuretools.vscode-azurefunctions
```

## Serverless Tools

### Serverless Stack CLI

## Tunneling

There are edgecases where you need to tunnel your connection through your desktop computer

https://docs.serverless-stack.com/console#working-with-gitpod

eg. If you're using the SST you need to tunnel websockets.

If you had a linux enviroment eg. Windows Subsystem Linux you intall
and run this linux tool to tunnel

```sh
curl -OL https://gitpod.io/static/bin/gitpod-local-companion-darwin-arm64
chmod +x ./gitpod-local-companion-darwin-arm64
./gitpod-local-companion-darwin-arm64
```