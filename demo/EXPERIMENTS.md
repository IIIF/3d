# Draft IIIF Manifest Experiments

As part of the work of the IIIF 3D TSG, it is necessary to create experimental demos that display and contextualize 3D content using draft manifests that represent potential expansions to the IIIF Presentation API. Ideally, these experiments should be implemented across a wide range of viewers and with 3D frameworks. The goal of these experiments is to determine and find issues, stressors, and "pain points" with the manifest drafts as they are expressed, so that the manifests can be modified and updated as needed to help resolve these issues. This document describes acceptance criteria for these demo experiments, laying out a series of milestones of increasing complexity for demo creators to implement. 

Experiment demos **MUST**:
* Satisfy one or more of the functionality milestones described below.
* Load one or more draft IIIF manifests as specified by a milestone. 
* Load, transform, position, and contextualize 3D model(s) and other resources (potentially lights, cameras, label annotations, 2D content, etc.) in 3D space specified by the draft IIIF manifest.
* Provide a default view and lighting conditions so that end users of the demo can quickly assess whether the demo satisfies acceptance criteria:

Experiment demos **SHOULD**:
* Load manifest and asset files from GitHub IIIF/3D repository URLs. Manifests and assets may be updated over time, and it is helpful if demos do not need to be updated each time this occurs. This may not always be feasible for all frameworks and viewers, though.
* Be expressed in CodeSandbox, so that we have a consistent and easily modifiable environment in which to iterate on demos and to demonstrate functionality to end users. Again, this may not be feasible in all cases.
* Implement a `textarea` input that contains the loaded draft IIIF manifest content, and which allows users to modify manifest content interactively, loading new viewer state dynamically from the modified manifest content on form submission. Having this functionality allows users to quickly modify parameters of the manifest and observe their impact on the demo, which among other benefits aids in diagnosing inconsistencies between demos.
* Satisfy previous milestone manifests in a backwards compatible fashion. The milestone manifests and acceptance criteria generally increase in complexity, and a demo for milestone 2 should ideally also be able to support milestone 1.

Experiment demos **COULD**:
* Load other IIIF manifests from URLs, with the milestone manifest as the default. This would allow users to easily try different manifests against a demo. 

Note: Currently (as of 1/24/2024), the position values included in the draft manifests are dummy values. They WILL be changed to more realistic values for the astronaut asset once experiments begin to be implemented.

## Experiment Milestones

### Milestone 1: Place Model in 3D Scene

Manifest: https://raw.githubusercontent.com/IIIF/3d/demo_milestones/demo/json/manifests/model_origin.json

Acceptance criteria: Astronaut asset is positioned at origin of 3D scene.

### Milestone 2: Place Model at Position in 3D Scene

Manifest: https://raw.githubusercontent.com/IIIF/3d/demo_milestones/demo/json/manifests/model_position.json

Acceptance criteria: Astronaut asset is positioned at specified coordinates of 3D scene.

### Milestone 3: Place Scale-Transformed Model in 3D Scene

Manifest: https://raw.githubusercontent.com/IIIF/3d/demo_milestones/demo/json/manifests/model_transform_scale.json

Acceptance criteria: Astronaut asset is transformed to specified scale and positioned at origin of 3D scene.

### Milestone 4: Place Scale-Transformed Model at Position in 3D Scene

Manifest: https://raw.githubusercontent.com/IIIF/3d/demo_milestones/demo/json/manifests/model_transform_scale_position.json

Acceptance criteria: Astronaut asset is transformed to specified scale and positioned at specified coordinates of 3D scene.

### Milestone 5: Place Scaled and Translated Model at Position in 3D Scene

Manifest: https://raw.githubusercontent.com/IIIF/3d/demo_milestones/demo/json/manifests/model_transform_translate_scale_position.json

Acceptance criteria: Astronaut asset is transformed to specified scale, translated, and positioned at specified coordinates of 3D scene.
