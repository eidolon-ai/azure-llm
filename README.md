# AZURE LLM CONFIGURATION

This repository contains the configuration for the Azure LLM services.

## Configuration

1. Create an [Azure OpenAPI Resource](https://ai.azure.com/resource) (in this demo we used `custom-azure-deployment`).
   1. get 'Key' and add it to .env as AZURE_OPENAI_API_KEY
   2. get 'Endpoint' and add it azure_config.yaml as `azure_openai_api_endpoint`. 🚨 Note the /openai suffix
3. Deploy a model by creating a [deployment](https://ai.azure.com/resource/deployments) for your resource a model named (in this demo we used `custom-azure-deployment`).
4. Update the `hello_world_agent.yaml` model to use your deployment name.

To verify your deployment is working, comment out `@pytest.mark.vcr()` from tests/test_agent_machine.py and succefully run the 
tests 
```bash
make test
```

Alternatively, run the server and test manually using the webui 

```bash
make docker-serve
```
