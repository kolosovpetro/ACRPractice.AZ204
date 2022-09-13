# Azure Container Registry Practice

[![Run Build and Test](https://github.com/kolosovpetro/ACRPractice.AZ204/actions/workflows/run-build-and-test-dotnet.yml/badge.svg)](https://github.com/kolosovpetro/ACRPractice.AZ204/actions/workflows/run-build-and-test-dotnet.yml)

Well, yet another practice today, this time we fight against Azure Container registry.
The scope of current kata is to reach the deep understanding of how to build, push and deploy containerized apps to the
azure container registry and container instances.
So, the following to be done:

- Create `DockerFile` and describe its steps in details
- Build `DockerFile` locally with appropriate tag*s
- Create `Azure container registry` using CLI
- Push image to the `ACR` using appropriate authorization
- Deploy image to the `Azure container instances`

## Infrastructure provision

- Create resource group: `az group create --name "rg-acr-practice" --location "westus"`
- Define registry name as powershell variable: `$registryName="pkolosovregistry"`
- Check that ACR name is available: `az acr check-name --name $registryName`
- Create ACR: `az acr create --resource-group "rg-acr-practice" --name $registryName --sku "Basic"`
- List all container registries inside subscription: `az acr list`
- List all container registries inside subscription: `az acr list --query "max_by([], &creationDate).name" --output tsv`

## DockerFile creation

- It is done and commented directly
  there: [DockerFile](https://github.com/kolosovpetro/ACRPractice.AZ204/blob/master/ACRPractice.AZ204/Dockerfile)

## Build DockerFile and tag it properly

- Define powershell variable for container registry name:
    - Compute way: `$ACR_NAME=$(az acr list --query "max_by([], &creationDate).name" --output tsv)`
    - Hardcoded way: `$ACR_NAME=pkolosovregistry`
- Define ACR url:
    - Compute way: `$ACR_URL="$ACR_NAME.azurecr.io"`
    - Hardcoded way: `$ACR_URL="pkolosovregistry.azurecr.io"`
- Define powershell variable for ACR repository: `$ACR_REPOSITORY="acr-practice-repository"`
- Define powershell version variable: `$IMAGE_VERSION="1.0"`
- Define image tag: `$IMAGE_TAG="acr-practice-image-$IMAGE_VERSION"`
- Build docker image:
    - Compute way: `docker build -t "${ACR_REPOSITORY}:$IMAGE_TAG" .`
    - Hardcoded way: `docker build -t "acr-practice-repository:acr-practice-image-1.0" .`
- Run image locally to test: `docker run -d -p 9000:80 --name acr-practice-test-run "${ACR_REPOSITORY}:$IMAGE_TAG"`
- Navigate and verify app: `http://localhost:9000/swagger/index.html`

## Notes

- To build on behalf of Azure: `az acr build --registry $acrName --image ipcheck:latest .`