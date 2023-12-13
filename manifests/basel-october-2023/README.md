These manifest drafts were created by Rob Sanderson to reflect progress during the Basel working meeting in October 2023. The manifests represent a series of steps of increasing complexity, moving from a minimal example of placing a model at scene origin to transforming a model with translation and scaling before positioning it at a specified location in a scene. These were all copied from https://github.com/IIIF/api/blob/rob_3d/source/presentation/4.0/examples.md.

`model_origin.json`: Simplest example, just places a model at scene origin. The target solely references a scene URI. 

`model_position.json`: Demonstrates use of a PointSelector target to position model at specified location in scene.

`model_transform_scale.json`: Demonstrates use of a model annotation transform, scaling the model before placing it at scene origin.

`model_transform_scale_position.json`: Combines scale transform with PointSelector target, scaling model before placing it specified location in scene.

`model_transform_translate_scale_position.json`: Adds another transform type, a translate transform, to both translate and scale the model before placing the model's coordinate origin at a specified location in a scene (the model itself will be translated away from that origin based on the translate transform).