## Jekyll FAQ

### To add a milestone directory for example manifests:

1. Add a directory in `manifests/`
2. Add an entry in `_data/manifests.yml` for example:

```
- name: Supplementing Annotations
  dir: 9_supplementing_annotations
  desc: Manifests in this group feature supplementing annotations describing the 3D scene.
```
3. Add a README.md with a minimum of:

```
---
layout: manifest
---
```
4. Add any example manifests to that directory. The label and and summary will show on the manifest page. 

### To add a screenshot for a viewer to a particular manifest add the following to the front matter of the README.md file:


```
---
layout: manifest

examples:
  model_origin.json:
    - screenshot.png
    - screenshot2.png  
---
```

Where screenshot.png and screenshot2.png are in the directory with the manifest. 

### Add a viewer

Add a viewer to the `_data/viewers.yml` file that looks like this:

```
- name: IIIF 3D viewer
  url: https://example.com/iiif/?iiif-content=##manifest##
```

The `##manifest##` will be replaced with the Manifest URL in the viewer link. The name of the viewer link will be the name in this file. 