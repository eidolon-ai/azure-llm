# AZURE LLM CONFIGURATION

This repository contains the configuration for the Azure LLM services.

## Configuration

1. Create an [Azure OpenAPI Resource](https://ai.azure.com/resource) (in this demo we used `custom-azure-deployment`).
   1. get `Key` and add it to .env as AZURE_OPENAI_API_KEY (`make .env` will prompt you for this value)
   2. note the `Endpoint` and update the `azure_endpoint` field in `azure_agent.yaml` .
3. Deploy a model by creating a [deployment](https://ai.azure.com/resource/deployments) for your resource a model named (in this demo we used `custom-azure-deployment`).
4. Update the `model` field in `hello_world_agent.yaml` to use your deployment name.

```bash filename=resources/4o_apu.yaml
# resources/4o_apu.yaml
apiVersion: server.eidolonai.com/v1alpha1
kind: Reference
metadata:
  name: azure-gpt4o

spec:
  implementation: GPT4o
  llm_unit:
    implementation: "AzureLLMUnit"
    azure_endpoint: https://eidolon-azure.openai.azure.com  # resource azure endpoint
    model:
      name: custom-azure-deployment  # your custom deployment name
```

<details>
<summary>The example agent already points to this apu.</summary>

```yaml filename=resources/agent.yaml
# resources/agent.yaml
apiVersion: server.eidolonai.com/v1alpha1
kind: Agent
metadata:
   name: hello-world

spec:
   implementation: SimpleAgent
   apu:
      implementation: azure-gpt4o  # points to your apu resource
```
</details>

## Testing your Azure Agent
To verify your deployment is working, run the tests in "record-mode" and see if they pass:
```bash
make test ARGS="--vcr-record=all"
```

<details>
<summary>What's up with the `--vcr-record=all` flag? 🤔</summary>

> Eidolon is designed so that you can write cheap, fast, and deterministic tests by leveraging pyvcr.
> 
> This records http request/responses between test runs so that subsequent calls never actually need to 
go to your llm. These recordings are stored as `cassette` files.
> 
> This is normally great, but it does mean that when you change your config these cassettes are no longer valid.
> `--vcr-record=all` tells pyvcr to ignore existing recordings and re-record them again using real http requests.
</details>


## Try it out!
If your tests are passing, try chatting with your agent using the Eidolon webui: 

First start the backend server + webui
```bash
make docker-serve
```

then visit the [webui](http://localhost:3000/) and start experimenting!
