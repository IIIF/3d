# Draft IIIF Manifest Experiments

As part of the work of the IIIF 3D TSG, it is necessary to create experimental demos that display and contextualize 3D content using draft manifests that represent potential expansions to the IIIF Presentation API. Ideally, these experiments should be implemented across a wide range of viewers and 3D frameworks. The goal of these experiments is to determine and find issues, stressors, and "pain points" with the manifest drafts as they are expressed, so that the manifests can be modified and updated as needed to help resolve these issues. This document describes acceptance criteria for these demo experiments, laying out a series of milestones of increasing complexity for demo creators to implement. 

Experiment demos **MUST**:
* Satisfy one or more of the functionality milestone groups described below.
* Load one or more draft IIIF manifests as specified by a milestone group. 
* Load, transform, position, and contextualize 3D model(s) and other resources (potentially lights, cameras, label annotations, 2D content, etc.) in 3D space as specified by the draft IIIF manifest.
* Provide a default view and lighting conditions so that end users of the demo can quickly assess whether the demo satisfies acceptance criteria.

Experiment demos **SHOULD**:
* Load manifest and asset files from GitHub IIIF/3D repository URLs.  Manifests and assets may be updated over time, and it is helpful if demos do not need to be updated each time this occurs. This may not always be feasible for all frameworks and viewers, though.
* Be expressed in CodeSandbox, so that we have a consistent and easily modifiable environment in which to iterate on demos and to demonstrate functionality to end users. Again, this may not be feasible in all cases.
* Implement a `textarea` input that contains the loaded draft IIIF manifest content, and which allows users to modify manifest content interactively, loading new viewer state dynamically from the modified manifest content on form submission. Having this functionality allows users to quickly modify parameters of the manifest and observe their impact on the demo, which among other benefits aids in diagnosing inconsistencies between demos.
* Satisfy previous milestone manifests in a backwards compatible fashion. The milestone manifests and acceptance criteria generally increase in complexity, and a demo for milestone group 2 should ideally also be able to support milestone group 1.

Experiment demos **COULD**:
* Load other IIIF manifests from URLs, with the milestone manifest as the default. This would allow users to easily try different manifests against a demo. 

## Experiment Milestones

Manifests are separated into numbered milestone groups, where each new group introduces a new behavior or resource type. The groups are described below, and a link to the manifests associated with the group is provided. Any manifest URL in this GitHub repo can be converted into a fetchable manifest URL by replacing the `https://github.com/IIIF/3d/blob/` at the beginning of the URL with `https://raw.githubusercontent.com/IIIF/3d/`. For example, the GitHub URL `https://github.com/IIIF/3d/blob/main/manifests/1_basic_model_in_scene/model_origin.json` becomes `https://raw.githubusercontent.com/IIIF/3d/main/manifests/1_basic_model_in_scene/model_origin.json`. Demos can use the fetchable URL to load the manifest directly from the web.

{% include manifests.md %}