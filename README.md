# Labellisation Project

This project focuses on creating a dataset for training an AI model to realistically place products in room scenes.

## Data Requirements

For each training sample, we need:

- `scene.png`: Room scene image containing the object (ground truth for model training).
- `product.png`: Product packshot of the ground truth object in scene.png but on white/neutral background.
- `bbox`: Bounding box coordinates (x,y,x,y) specifying where to inpaint the product.
- `prompt.txt` (Optional): Prompt describing the product / product type or reference.

## Rule Set

To succesffully train the model we enforce a strict set of rules over the training data.

Basics:
- Identity: The product in scene image SHOULD be perfectly identical to product.png (same object, color and shape).
- Rotation: Angle differences and rotations between product in product.png and scene.png are allowed but should not be greater than 90 degrees. (ex: data/bad_samples/too_much_rotation)
- Products covers too much of the scene: The product in scene image SHOULD NOT take more than 90% of the scene image. (ex: data/bad_pair_mask_too_large)
- Occlusion: The scene image SHOULD NOT have the item in product.png occluded too much by other objects.

Biases:
- Several products instances: The scene image SHOULD NOT have multiple instances of the item in product.png (ex: data/bad_samples/several_product_instances)
- Other items with identical style: It can happen on furniture shooting that there are other objects with identical style in the scene (ex: a chair with the same color and style as the couch). In that case we should treat this as if it was several instances of the same product and discard it. 


## Data Generation Approaches

### 1. Synthetic Data Generation: use the model's own ouptput as training data. 

This approach allows us to leverage our existing model to bootstrap the training dataset.

1. Use the model through Palazzo app or custom frontend
2. Manually label the generations as positive/negative
3. Store the scene, product image and bounding box coordinates for each generation.


### 2. Real Data Collection

1. Scraping or deal with your clients
2. Organize the data 1 folder per product (packshots and product in scene all in the same folder)
3. Make pairs  (product_packshot, product_in_scene). We have a script for that using "white-levels" to detect packshot pictures.
4. Have manual annotators draw bounding boxes around the product in the scene images and samples that follow the Rule Set.
5. Export those as per the format described in the Data Requirements section.



