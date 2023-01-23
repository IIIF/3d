# 3D Viewer JSON Demos

This document lists and details simple technical demo harnesses created to aid experimentation with IIIF 3D tests and concepts. For initial work relating to supporting the [core user story Issues](https://github.com/IIIF/3d/issues?q=is%3Aopen+is%3Aissue+label%3A%22core+user+story%22), the TSG determined a good starting point would be the creation and iterative refinement of simple code sandbox demos utilizing similarly basic JSON manifests of 3D content (annotations, etc.) to be displayed. Each demo includes a link to the demo using Code Sandbox or another interactive code platform, as well (where possible) an expression of the JSON used in the demo. 

As of 1/23/2023, these demos most specifically attempt to address the task described by [Issue 17: Demo harness: 3+ viewers using a common JSON annotation format](https://github.com/IIIF/3d/issues/17) and the user story described by [Issue 14: Annotate displayed 3D models with commentary](https://github.com/IIIF/3d/issues/14).

## The Demos

### Aleph

[Link to the demo](https://codesandbox.io/s/aleph-annotation-demo-teh3pf?file=/index.html)

JSON
```
{
  "nodes": [
    [
      "Visor",
      {
        "normal": "0.29259561389217825 0.11383937564155769 0.9494358342113489",
        "position": "0.017023790299119268 1.8062894401300538 0.34094835034109217",
        "scale": 0.025,
        "title": "Visor"
      }
    ],
    [
      "Glove",
      {
        "normal": "0.47124483745139667 0.05608789306010443 0.8802172751244348",
        "position": "0.5175891304384852 0.9555791975125096 0.12190797586809543",
        "scale": 0.025,
        "title": "Glove"
      }
    ]
  ]
}
```

### Google Model Viewer

[Link to the demo](https://codesandbox.io/s/model-viewer-annotations-demo-3k5tqo)

JSON
```
[
    {
        "id": 0,
        "normal": "0.29259561389217825 0.11383937564155769 0.9494358342113489",
        "position": "0.017023790299119268 1.8062894401300538 0.34094835034109217",
        "value": "visor"
    },
    {
        "id": 1,
        "normal": "0.47124483745139667 0.05608789306010443 0.8802172751244348",
        "position": "0.5175891304384852 0.9555791975125096 0.12190797586809543",
        "value": "glove"
    }
]
```

### Sketchfab

Currently, this demo does not load the external annotation set specified in [#17](https://github.com/IIIF/3d/issues/17). It also uses JSFiddle rather than Code Sandbox.

[Link to the demo](https://jsfiddle.net/nebulousflynn/uykbgjaw/1/)

### Smithsonian Voyager

[Link to the demo](https://codesandbox.io/s/voyager-annotations-demo-o9l1rq?file=/index.html)

JSON
```
"annotations": [
  {
      "id": "qs8H9mXZSr8p",
      "titles": {
          "EN": "visor"
      },
      "leads": {
          "EN": ""
      },
      "position": [
          -0.0142194,
          0.783568,
          0.3500529
      ],
      "direction": [
          -0.2855858,
          0.1212762,
          0.9506486
      ],
      "scale": 0.0603153,
      "color": [
          0,
          0.61,
          0.87
      ]
  },
  {
      "id": "J2gO1sGNobxk",
      "titles": {
          "EN": "glove"
      },
      "leads": {
          "EN": ""
      },
      "position": [
          0.5040075,
          -0.0485156,
          0.1340355
      ],
      "direction": [
          0.3971733,
          0.2949306,
          0.8690623
      ],
      "scale": 0.08,
      "color": [
          0,
          0.61,
          0.87
      ]
  }
]
```

### X3D

Currently, this demo loads one of the annotations describes in [#17](https://github.com/IIIF/3d/issues/17) programattically, without an external JSON file.

[Link to the demo](https://codesandbox.io/s/interesting-colden-ectroz)


## Changelog

- 1/23/2023: Created this document to better record and track content evolving in [Issue #17](https://github.com/IIIF/3d/issues/17).
