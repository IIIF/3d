# 3D Viewer JSON Demos

This document lists and details simple technical demo harnesses created to aid experimentation with IIIF 3D tests and concepts. For initial work relating to supporting the [core user story Issues](https://github.com/IIIF/3d/issues?q=is%3Aopen+is%3Aissue+label%3A%22core+user+story%22), the TSG determined a good starting point would be the creation and iterative refinement of simple code sandbox demos utilizing shared common JSON manifests of 3D content (annotations, etc.) to be displayed. Each demo includes a link to the demo using Code Sandbox. 

As of 6/13/2023, these demos specifically address the task described by [Issue 17: Demo harness: 3+ viewers using a common JSON annotation format](https://github.com/IIIF/3d/issues/17) supporting the user story [Issue 14: Annotate displayed 3D models with commentary](https://github.com/IIIF/3d/issues/14).

- [Aleph](#aleph)
- [Google Model Viewer](#google-model-viewer)
- [Sketchfab](#sketchfab)
- [Smithsonian Voyager](#smithsonian-voyager)
- [X3D](#x3d)

## Label Annotation JSON

All viewer demos listed here employ a common simple shared JSON manifest. This manifest describes two 3D point text label annotations, one pointing out an astronaut's visor and the other pointing out the astronaut's glove. These label annotations are provided in reference to a specific low-poly 3D model of an [astronaut](https://cdn.glitch.com/36cb8393-65c6-408d-a538-055ada20431b/Astronaut.glb) which is also used in each demo. The JSON manifest is included in this repository as a discrete [JSON file](https://github.com/IIIF/3d/blob/main/demo/json/label-annotation-issue-17.json), but it is also included in this document for reference. 

```
{
  "annotations": [
    {
      "id": 0,
      "normal": [ 0.294, 0.114, 0.949 ],
      "position": [ 0.017, 1.806, 0.341 ],
      "value": "visor"
    },
    {
      "id": 1,
      "normal": [ 0.472, 0.059, 0.880 ],
      "position": [ 0.518, 0.956, 0.122 ],
      "value": "glove"
    }
  ]
}
```

## The Demos

### Aleph

[Link to the demo](https://codesandbox.io/s/aleph-annotation-demo-common-annotation-json-8rbos9)

- Includes an editable text input box where annotation can be manually modified, new annotations can be loaded, etc.
- Uses a local file copy of the shared JSON manifest.
- Annotations can be copy-pasted between this and Google Model Viewer with identical results.

### Google Model Viewer

[Link to the demo](https://codesandbox.io/s/model-viewer-annotations-demo-3k5tqo)

- Includes an editable text input box where annotation can be manually modified, new annotations can be loaded, etc.
- Demo HTML includes the content of the shared JSON manifest.
- Annotations can be copy-pasted between this and Aleph with identical results. Annotations copy-pasted into Sketchfab demo produce different results for reasons unknown.

### Sketchfab

[Link to the demo](https://codesandbox.io/s/sad-lena-b39r00?file=/src/index.js)

- Includes an editable text input box where annotation can be manually modified, new annotations can be loaded, etc.
- Demo HTML includes the content of the shared JSON manifest.
- Annotations copy-pasted in Google Model Viewer produce different results for reasons unknown.

### Smithsonian Voyager

[Link to the demo](https://codesandbox.io/s/voyager-annotations-demo-forked-rqivl4?file=/index.html)

- Fetches annotation JSON manifest from GitHub.

### X3D

[Link to the demo](https://codesandbox.io/p/github/vincentmarchetti/x3d-remote-annotation)

- Uses a local file copy of the shared JSON manifest.

## Changelog

- 1/23/2023: Created this document to better record and track content evolving in [Issue #17](https://github.com/IIIF/3d/issues/17).
- 3/21/2023: Update status and link of the X3D demo
- 4/18/2023: Add JSON used in X3D demo
- 6/13/2023: Add a section for shared single JSON manifest, and update viewers to latest versions that all use the shared JSON manifest
