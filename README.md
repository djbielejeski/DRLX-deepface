# Diffusion Reinforcement Learning X

DRLX is a library for distributed training of diffusion models via RL. It is meant to wrap around 🤗 Hugging Face's [Diffusers](https://huggingface.co/docs/diffusers/) library and uses [Accelerate](https://huggingface.co/docs/accelerate/) for Multi-GPU and Multi-Node (as of yet untested)

**News (09/27/2023): Check out our blog post with some recent experiments [here](https://carper.ai/enhancing-diffusion-models-with-reinforcement-learning/)!**

📖 **[Documentation](https://DRLX.readthedocs.io)**

# Setup

First make sure you've installed [OpenCLIP](https://github.com/openai/CLIP.git). Afterwards, you can install the library from pypi:

```sh
python -m venv venv
venv\Scripts\activate.bat
pip install -r requirements.txt
```

# How to use

Currently we have only tested the library with Stable Diffusion 1.4, 1.5, and 2.1, but the plug and play nature of it means that realistically any denoiser from most pipelines should be usable. Models saved with DRLX are compatible with the pipeline they originated from and can be loaded like any other pretrained model. Currently the only algorithm supported for training is [DDPO](https://arxiv.org/abs/2305.13301).

```python
from drlx.reward_modelling.aesthetics import Aesthetics
from drlx.pipeline.pickapic_prompts import PickAPicPrompts
from drlx.trainer.ddpo_trainer import DDPOTrainer
from drlx.configs import DRLXConfig

# We import a reward model, a prompt pipeline, the trainer and config

pipe = PickAPicPrompts()
config = DRLXConfig.load_yaml("configs/my_cfg.yml")
trainer = DDPOTrainer(config)

trainer.train(pipe, Aesthetics())
```

And then to use a trained model for inference:

```python
pipe = StableDiffusionPipeline.from_pretrained("out/ddpo_exp")
prompt = "A mad panda scientist"
image = pipe(prompt).images[0]
image.save("test.jpeg")
```

# Accelerated Training

```bash
accelerate config
accelerate launch -m [your module]
```

# Roadmap  

- [x] Initial launch and DDPO
- [x] PickScore Tuned Models
- [ ] DPO
- [ ] SDXL support
