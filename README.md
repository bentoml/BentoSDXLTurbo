<div align="center">
    <h1 align="center">BentoSDXLTurbo</h1>
    <br>
    <strong>Image generation with speed and ease<br></strong>
    <i>Powered by BentoML 🍱</i>
    <br>
</div>
<br>

[Stable Diffusion XL Turbo](https://huggingface.co/stabilityai/sdxl-turbo) is a real-time text-to-image generation model utilizing a novel distillation technique called Adversarial Diffusion Distillation (ADD). This technology enables SDXL Turbo to generate images in a single step, significantly enhancing performance and reducing computational requirements without sacrificing image quality.

This project builds an image generation application using BentoML, powered by [diffusers](https://github.com/huggingface/diffusers) and SDXL Turbo.

## Prerequisites

- You have installed Python 3.8+ and `pip`. See the [Python downloads page](https://www.python.org/downloads/) to learn more.
- You have a basic understanding of key concepts in BentoML, such as Services. We recommend you read [Quickstart](https://docs.bentoml.com/en/1.2/get-started/quickstart.html) first.
- (Optional) We recommend you create a virtual environment for dependency isolation for this project. See the [Conda documentation](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) or the [Python documentation](https://docs.python.org/3/library/venv.html) for details.
- To run the Service locally, you need a Nvidia GPU with at least 12G VRAM.

## Install dependencies

```bash
git clone https://github.com/bentoml/BentoSDXLTurbo.git
cd BentoSDXLTurbo
pip install -r requirements.txt
```

## Run the BentoML Service

We have defined a BentoML Service in `service.py`. Run `bentoml serve` in your project directory to start the Service.

```python
$ bentoml serve .

2024-01-18T18:31:49+0800 [INFO] [cli] Starting production HTTP BentoServer from "service:SDXLTurboService" listening on http://localhost:3000 (Press CTRL+C to quit)
Loading pipeline components...: 100%
```

The server is now active at [http://localhost:3000](http://localhost:3000/). You can interact with it using the Swagger UI or in other different ways.

CURL

```bash
curl -X 'POST' \
  'http://localhost:3000/txt2img' \
  -H 'accept: image/*' \
  -H 'Content-Type: application/json' \
  -d '{
  "prompt": "A cinematic shot of a baby racoon wearing an intricate italian priest robe.",
  "num_inference_steps": 1,
  "guidance_scale": 0
}'
```

Python client

```python
import bentoml

with bentoml.SyncHTTPClient("http://localhost:3000") as client:
        result = client.txt2img(
            prompt="A cinematic shot of a baby racoon wearing an intricate italian priest robe.",
            num_inference_steps=1,
            guidance_scale=0.0
        )
```

## Deploy to BentoCloud

After the Service is ready, you can deploy the application to BentoCloud for better management and scalability. [Sign up](https://www.bentoml.com/) if you haven't got a BentoCloud account.

Make sure you have [logged in to BentoCloud](https://docs.bentoml.com/en/latest/bentocloud/how-tos/manage-access-token.html), then run the following command to deploy it.

```bash
bentoml deploy .
```

Once the application is up and running on BentoCloud, you can access it via the exposed URL.

**Note**: For custom deployment in your own infrastructure, use [BentoML to generate an OCI-compliant image](https://docs.bentoml.com/en/latest/guides/containerization.html).
