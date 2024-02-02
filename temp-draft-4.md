
---

### Scratch Pad

Idea - split the spec into "vocabulary" and "model" ala Web Annos

New Classes
* Scene

describe handedness

Properties:
  * backgroundColor  (+canvas) rgb string ; default viewer choice

* [Camera]
  * PerspectiveCamera
  * OrthographicCamera


[note null for orbiting]
Properties:
 * lookAt reference to an annotation, or a point selector ; null
 * near float ; default viewer choice
 * far float ; default viewer choice

PerspectiveCamera:
  * fieldOfView float; default viewer choice

* [Light]
  * AmbientLight
  * DirectionalLight
  * PointLight
  * SpotLight

Properties:
 * color rbg string case insensitive; white
 * intensity Value instance ; default viewer choice; only unit:relative 
  
Directional/Spot:
 * lookAt (same as for cameras)


(Pull back into 4.0)

* Physical Dimensions (nolongeraservice) on Canvas and Scene

* SpecificResource
  * Style
  * RefinedBy
  * Choice

* [Selector]
  * PointSelector  -- add `z` property, clarify `t`, check language about non-existent dimensions
  * PolygonZSelector
  * FragmentSelector -- show refineBy of PointSelector

* [Transform]
  * ScaleTransform
  * RotationTransform
  * TranslateTransform

Need to double check the effects of translate then scale vs scale then translate



Properties:
  * x float ; 0 for Rotation/Translate / 1.0 for Scale
  * y float ; 0 for Rotation/Translate / 1.0 for Scale
  * z float ; 0 for Rotation/Translate / 1.0 for Scale

* Value

```
{
 "intensity": {"type": "Value", "value": 0.5, "unit": "relative"}
}
```

Properties:
 * value float, required, no default
 * unit string, required, no default  // @vocab in context
  

Properties of `painting` annotations:
  * exclude, array of strings, @vocab values
  * transforms, array of Transform instances
  
Initial Values:
  * Animations -- just gone
  * Audio -- add a canvas/model with the audio
  * Cameras -- reconstructable
  * Lights -- reconstructable

```
"exclude": [ "Audio", "Lights", "Cameras", "Animations" ]
```

Default Annotation positioning:

For content in Scenes:
(when no target selector)
For models - origin of model coord space
For Scenes - Origin of Scene
For Canvases - the 0,0 2d origin, extending downwards, x extending along x

For content in Canvases:
Explain ordering, expand "paint them in order"


### List of Recipes

[https://github.com/IIIF/3d/tree/main/manifests](https://github.com/IIIF/3d/tree/main/manifests)

model_origin
model_position
model_transform_scale
model_transform_scale_position
model_transform_translate_scale_position
model_transform_rotate_position
model_transform_translate_rotate_position
model_transform_translate_rotate_translate_position // for Zoe

model_transform_negative_scale_position (-ve; mirror)
multiple_models (mike manifest)

scene_without_model ??
scene_with_model_and_bgcolor

perspective_camera_no_props
perspective_camera_fov
postioned_camera
positioned_camera_lookat_point
positioned_camera_lookat_anno
orthographic_camera

ambient_light
directional_light_transform_rotate
directional_light (position, lookat defines vector)
point_light
spotlight

lights_with_intensity

multiple_cameras_choice
multiple_lights_different_colors
multiple_cameras_and_lights (default first camera)

exclude_cameras
exclude_lights
exclude_cameras_and_lights

iiif_canvas_in_scene (polygonz explain order of points determines normal; back view is mirror)
iiif_canvas_with_bgcolor_in_scene (back view is canvas bgcolor)
scene_with_duration
iiif_canvas_with_image_no_img_service

video_on_canvas_in_scene_refinedby_t_mediafrag #2253

scene_in_scene
scene_in_scene_transform

default_xyz_1_0 (show scale 1 and rotate,transform 0)

comment_basic (pointSelector no t)
comments_point_and_fragment (one with pointSelector with t, one refinedBy fragment t=100,120)
comment_polygonz

infinite_canvas_demo

canvas_positioned_in_scene_then_duplicated_with_rotations

---

<div style="background: #bbddff; padding: 10px; padding-left: 30px; margin-bottom: 10px">
    🛈 The following draft is not a complete Presentation API specification and omits definition and discussion of many terms defined in <a href="https://iiif.io/api/presentation/3.0/">version 3.0</a>. For example, <b>Scene</b> (defined below) will share many of the same properties as <b>Canvas</b> (defined in Presentation 3.0). Please refer to that specification for any undefined terms and for the conceptual model on which this document builds.<br/><br/>
    <a href="https://docs.google.com/spreadsheets/d/1JIlgH9VIsdIiQInqWYMm-OgMMs5-_JKDgQfB5m2SvIM/edit#gid=0">This spreadsheet</a> lists properties of <b>Scene</b>; those undefined below can be found in <a href="https://iiif.io/api/presentation/3.0/">IIIF Presentation API 3.0</a>.
</div>

---

### 2.1. Defined Types

#### 2.1.1. Containers

##### Timeline

A virtual container that represents a bounded temporal range, without any spatial coordinates.

##### Canvas

A virtual container that represents a bounded, two-dimensional space and has content resources associated with all or parts of it. It may also have a bounded temporal range in the same manner as a Timeline.

##### Scene

A virtual container that represents a boundless three-dimensional space and has content resources positioned at locations within it. Rendering a Scene requires the use of Cameras and Lights. It may also have a bounded temporal range in the same manner as a Timeline.



---

# IIIF 3D Draft 1

## Scenes

A Scene in IIIF is a virtual container that represents a boundless three-dimensional space and has content resources, lights and cameras positioned at locations within it. It may also have a duration to allow the sequencing of events and timed media. Scenes have infinite height (y axis), width (x axis) and depth (z axis), where 0 on each axis (the origin of the coordinate system) is the treated as the center of the scene's space. 
The positive y axis points upwards, the positive x axis points to the right, and the positive z axis points forwards (a [right-handed cartesian coordinate system](link to wikipedia)).

The axes of the coordinate system are measured in arbitrary units and these units do not necessarily correspond to any physical unit of measurement. This allows arbitrarily scaled models to be used, including very small or very large, without needing to deal with very small or very large values. If there is a correspondence to a physical scale, then this can be asserted using the [physical dimensions pattern](link to physical dims).

![Right handed cartesian coordinate system](https://raw.githubusercontent.com/IIIF/3d/eds/assets/images/right-handed-cartesian.png =250x)

As with other containers in IIIF, Annotations are used to target the Scene to place content such as 3d models into the scene. Annotations are also used to add lights and cameras. A Scene can have multiple models, lights, cameras and other resources, allowing them to be grouped together. Scenes and other IIIF containers, such as Canvases, may also be embedded within Scenes, as described below in the [nesting section][fwd-ref-to-nesting]. 

```json
{
  "id": "https://example.org/iiif/scenes/1",
  "type": "Scene",
  "label": {"en": ["Chessboard"]},
  "backgroundColor": "#000000",
  "items": [
   "Note: Annotations Live Here"  
  ]
}
```

## Cameras

A Camera provides a view of a region of the Scene's space from a particular position within the Scene; the client constructs a viewport into the Scene and uses the view of one or more Cameras to render that region. The size and aspect ratio of the viewport is client and device dependent.

<div style="background: #A0F0A0; padding: 10px; padding-left: 30px; margin-bottom: 10px">
❓Does this prevent extension cameras from requiring a fixed aspect ratio? 
</div>

This specification defines two types of Camera:
* `PerspectiveCamera` which mimics the way the human eye sees, in that objects further from the camera are smaller.
* `OrthographicCamera` which removes visual perspective, resulting in object size remaining constant regardless of its distance from the camera.

Cameras are positioned within the Scene facing in a defined direction. If the position is not specified, then it defaults to the origin. If the direction the camera is facing is not specified, it defaults to facing along the z axis towards negative infinity. These defaults are modified through the Annotation which adds the Camera to the Scene, described below in the [section on Annotations][].

The region of the Scene's space that is observable by the camera is bounded by two planes orthogonal to the direction the camera is facing, given in the `near` and `far` properties, and a vertical projection angle that provides the top and bottom planes of the region. For PerspectiveCameras the full angular extent from the top plane to the bottom plane is defined by the `fieldOfView` property and specified in degrees. For OrthographicCameras, the projection is parallel. The `fieldOfView` angle MUST be greater than 0 and less than 180.

The `near` property, thus, defines the minimum distance from the camera at which something in the space must exist in order to be viewed by the camera. Anything nearer to the camera than this distance will not be viewed. Conversely the `far` property defines a maximum distance from the camera at which something in the space must exist in order to be viewed by the camera. Anything further away will not be viewed.

If any of these properties are not specified explicitly, they default to the choice of the client implementation.

![image goes here](https://raw.githubusercontent.com/IIIF/3d/eds/assets/images/near-far.png =250x)

If the Scene does not have any Cameras defined within it, then the client MUST provide a default Camera. The type, properties and position of this default camera are client-dependent.

```json
{
  "id": "https://example.org/iiif/camera/1",
  "type": "PerspectiveCamera",
  "near": 1.0,
  "far": 100.0,
  "fieldOfView": 45.0    
}
```

## Lights

One or more Lights MUST be present within the Scene in order to have objects within it be visible to the Cameras. 

This specification defines four types of Light:

| Class | Description  |
| ----- | ------------ |
| `AmbientLight` | AmbientLight evenly illuminates all objects in the scene, and does not have a direction or position. |
| `DirectionalLight` | DirectionalLight emits in a specific direction as if it is infinitely far away and the rays produced from it are all parallel. It does not have a specific position. |
| `PointLight` | PointLight emits from a single point within the scene in all directions. |
| `SpotLight` | SpotLight emits a cone of light from a single point in a given direction. |

Lights defined in this specification have a `color` and an `intensity`. The color is given as an RGB value, such as "#FFFFFF" for white. The intensity is the strength or brightness of the light, and described using a `Value` construct.

SpotLight has an additional property of `angle`, specified in degrees, which is the angle from the direction that the Light is facing to the outside extent of the cone. 

![Angle of cone](https://raw.githubusercontent.com/IIIF/3d/eds/assets/images/angle-of-cone.png =250x)

Lights that require a position and/or direction have these through the Annotation which associates them with the Scene. If a Light does not have an explicit direction, then the default is in the negative y direction (downwards). If a Light does not have an explicit position in the coordinate space, then the default is at the origin.

This specification does not define other aspects of Lights, such as the rate of decay of the intensity of the light over a distance, the maximum range of the light, or the penumbra of a cone. Implementation of these aspects is client-dependent.

If there are no Lights present within the Scene, then the viewer MUST add at least one Light. The types and properties of Lights added in this way are client-dependent.

```json
{
  "id": "https://example.org/iiif/light/1",
  "type": "AmbientLight",
  "color": "#FFFFFF",
  "intensity": {"type": "Value", "value": 0.6, "unit": "relativeUnit"}
}
```

## Painting Annotations

Annotations follow the [Web Annotation][org-w3c-webanno] data model and are used to associate models, lights, cameras, and IIIF containers such as Canvases, with Scenes. They have a `type` of "Annotation", a `body` (being the resource to be added to the scene) and a `target` (being the scene or a position within the scene). They must have a `motivation` property with the value of "painting" to assert that the resource is being painted into the Scene, rather than the Annotation being a comment about the Scene.

A construct called a Selector is used to select a part of a resource, such as a point within a Scene. The use of a Selector necessitates the creation of a `SpecificResource` that groups together the resource being selected (the `source`) and the instance of the Selector. This SpecificResource can then be used as either the `body` or the `target` of the Annotation.

All resources that can be added to a Scene have an implicit (e.g. Lights, Cameras) or explicit (e.g. Models, Scenes), local coordinate space. If a resource does not have an explicit coordinate space, then it is positioned at the origin of its coordinate space. In order to add a resource with its local coordinate space into a Scene with its own coordinate space, these spaces must be aligned. This done by aligning the origins of the two coordinate spaces.

Annotations may use a type of Selector called a `PointSelector` to select a different point than the Scene's origin to be aligned with. PointSelectors have three spatial properties, `x`, `y` and `z` which give the value on that axis. They also have a temporal property `instant` which can be used if the Scene has a duration, which gives the temporal point in seconds from the start of the duration, the use of which is defined in the [section on Scenes with Durations]().

Example Annotation that positions a model at a point within a Scene:

```json 
{
    "id": "https://example.org/iiif/3d/anno1",
    "type": "Annotation",
    "motivation": ["painting"],
    "body": {
        "id": "https://example.org/iiif/assets/model1.glb",
        "type": "Model"
    },
    "target": {
        "type": "SpecificResource",
        "source": [
          {
            "id": "https://example.org/iiif/scene1",
            "type": "Scene"
          }
        ],
        "selector": [
          {
            "type": "PointSelector",
            "x": -1.0,
            "y": 0.0,
            "z": 1.0
          }
        ]
    }
}
```

### URI Fragments

The point may instead be defined using a short-hand form of a URI Fragment at the end of the `id` of the Scene as the `target` of the Annotation.



```json 
{
    "id": "https://example.org/iiif/3d/anno1",
    "type": "Annotation",
    "motivation": ["painting"],
    "body": {
        "id": "https://example.org/iiif/assets/model1.glb",
        "type": "Model"
    },
    "target": "https://example.org/iiif/scene1#xyz=-1,0,1"
}
```


## Transforms


 * lookAt reference to an annotation, or a point selector ; null


## Nesting

scene in scene
canvas in scene
excludes


## Scenes with Durations

## Future Work

* Commentary Annotations
* Interactions
* Animations


