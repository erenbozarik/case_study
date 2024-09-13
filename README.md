![ComfyUI Workflow](/cover_readme.jpg)
# Van Gogh Style Transfer Workflow - ComfyUI

This repository contains a ComfyUI workflow for transforming input images into a Van Gogh-style output using Stable Diffusion and ControlNet. Follow the instructions below to set up the environment and use the provided workflow.

## Prerequisites

Before using this workflow, you need to set up **ComfyUI** on your local machine or cloud environment. Please follow the official installation guide provided by ComfyUI:

- [ComfyUI Installation Guide](https://github.com/comfyanonymous/ComfyUI)

Ensure that all dependencies such as Python, PyTorch, and the required models (Stable Diffusion, ControlNet) are properly installed and configured.

## Workflow Description

This workflow utilizes various nodes to process and apply Van Gogh-style transformation on an input image using Canny edge detection and ControlNet. Below is a breakdown of the key nodes and their functions:

### Workflow Nodes

1. **LoadImage (Node ID: 24)**
   - **Description**: This node loads the input image. It supports common image formats like JPEG and PNG.
   - **Outputs**: 
     - `IMAGE`: The image to be processed.

2. **Canny (Node ID: 25)**
   - **Description**: Applies the Canny edge detection algorithm to the input image. The result is fed into ControlNet for style guidance.
   - **Inputs**: 
     - `IMAGE`: The image from the LoadImage node.
   - **Outputs**: 
     - `IMAGE`: The edge-detected image.

3. **ControlNetLoader (Node ID: 28)**
   - **Description**: Loads the pre-trained ControlNet model (in this case, `control_v11p_sd15_canny.pth`) that guides the style transfer process.
   - **Outputs**:
     - `CONTROL_NET`: ControlNet model used in the workflow.

4. **CheckpointLoaderSimple (Node ID: 29)**
   - **Description**: Loads the pre-trained Van Gogh-style Stable Diffusion model (`vanGoghDiffusion_v1.ckpt`), along with its CLIP and VAE components. These components are crucial for generating the Van Gogh-style image.
   - **Outputs**:
     - `MODEL`: The model used for generating images.
     - `CLIP`: The CLIP model used for text encoding.
     - `VAE`: The VAE component used in image reconstruction.

5. **CLIPTextEncode (Node ID: 18 and 19)**
   - **Description**: Encodes the positive and negative prompts to condition the model. The positive prompt defines the Van Gogh-style characteristics, while the negative prompt eliminates undesired visual elements.
   - **Outputs**:
     - `CONDITIONING`: Text-based conditioning for Stable Diffusion.

6. **ControlNetApply (Node ID: 27)**
   - **Description**: Combines the edge-detected image from Canny with the ControlNet model, guiding the Van Gogh-style transformation.
   - **Outputs**:
     - `CONDITIONING`: Further conditioned data for KSampler.

7. **KSampler (Node ID: 20)**
   - **Description**: Samples the latent image space using the conditioned prompts and model. The transformation into the Van Gogh style happens in this node.
   - **Outputs**:
     - `LATENT`: The processed latent image.

8. **VAEDecode (Node ID: 31)**
   - **Description**: Converts the latent image into the final Van Gogh-style image using the VAE component.
   - **Outputs**:
     - `IMAGE`: The final Van Gogh-style image.

9. **SaveImage (Node ID: 32)**
   - **Description**: Saves the final generated image in the output directory.

### Node Connections

The nodes are connected as follows:

1. The input image is loaded by the `LoadImage` node.
2. Canny edge detection is applied to the image.
3. The ControlNetLoader and CheckpointLoaderSimple nodes load the necessary models.
4. The positive and negative prompts are processed using `CLIPTextEncode`.
5. The conditioned image is processed through `ControlNetApply` and sent to `KSampler`.
6. The latent image is sampled and decoded into the final Van Gogh-style image.
7. Finally, the image is saved using `SaveImage`.

## How to Use

1. **Set Up ComfyUI**: Follow the [ComfyUI Installation Guide](https://github.com/comfyanonymous/ComfyUI) to set up the required environment.
2. **Import the Workflow**: Import the provided `.json` workflow file into ComfyUI.
3. **Load an Image**: Upload the image you wish to transform.
4. **Run the Workflow**: Start the workflow to generate a Van Gogh-style version of your image.
5. **View the Output**: The transformed image will be saved in the specified output directory.

## Workflow Screenshot

Below is a screenshot of the ComfyUI workflow with the full node setup:

![ComfyUI Workflow](/case_study_capture_130924.jpg)

## Sample Output (Before and After)

Here is an example of a before and after comparison of the Van Gogh style transformation:

![Before-After](/Before-after.jpg) 

## Troubleshooting

- **Model Not Found**: Ensure that the correct models (e.g., `vanGoghDiffusion_v1.ckpt` and `control_v11p_sd15_canny.pth`) are downloaded and placed in the appropriate directories.
- **Slow Processing**: Make sure you have a capable GPU for faster image generation, as the process can be resource-intensive.
