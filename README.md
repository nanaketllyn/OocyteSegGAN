## Proposed Method

This project presents a two-stage deep learning pipeline for histological oocyte segmentation based on a conditional Generative Adversarial Network (cGAN).

In the first stage, an adversarial segmentation model is used to generate an initial oocyte mask directly from the input histological image. The generator is built on a **U-Net++** architecture, which improves feature fusion across multiple resolution levels and helps preserve fine boundaries and texture details. To enhance the understanding of structures at different spatial scales, an **Atrous Spatial Pyramid Pooling (ASPP)** module is inserted at the bottleneck. This allows the model to capture both local details and broader morphological patterns of the oocytes.  
The generator is trained together with a **PatchGAN discriminator**, which evaluates local image–mask regions and acts as a morphological quality controller. By penalizing unrealistic contours and inconsistent local structures, the discriminator encourages the generator to produce more coherent and biologically plausible segmentations.

In the second stage, a lightweight post-processing step is applied to refine the generated mask. Small noisy regions around the oocyte are removed, the main connected component is preserved, and minor morphological corrections such as hole filling and contour smoothing are performed. Although simple, this refinement improves edge quality and foreground–background separation.

The full pipeline also includes a standardized pre-processing procedure, where COCO annotations are converted into segmentation masks, images and masks are resized to a fixed resolution, and the data are normalized before training. In single-oocyte scenarios, an optional guide factor, such as an approximate nucleus location, can be used to direct the network toward the target region.

Training is driven by a hybrid loss function that combines:
- an **adversarial loss**, which improves local realism and contour plausibility, and  
- a **segmentation loss**, based on **Cross-Entropy + Dice Loss**, which improves pixel-wise classification and overlap quality.

This combination enables the model to generate segmentation masks that are not only accurate in terms of overlap metrics but also visually consistent from a morphological perspective.

### Main Components
- **Generator:** U-Net++ with ASPP
- **Discriminator:** PatchGAN
- **Loss Function:** Adversarial + Cross-Entropy + Dice
- **Post-processing:** noise removal, hole filling, contour refinement

### Main Advantages
- Preserves fine histological details
- Captures multi-scale oocyte morphology
- Produces morphologically consistent masks
- Improves segmentation robustness under challenging histological conditions
