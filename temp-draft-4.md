
---

### Scratch Pad

Idea - split the spec into "vocabulary" and "model" ala Web Annos


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

| Class               | Description  |
| ------------------- | ------------ |
| `PerspectiveCamera` | `PerspectiveCamera` mimics the way the human eye sees, in that objects further from the camera are smaller |
| `OrthographicCamera` | `OrthograficCamera` removes visual perspective, resulting in object size remaining constant regardless of its distance from the camera |

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

The point may instead be defined using a short-hand form of a URI Fragment at the end of the `id` of the Scene as the `target` of the Annotation. The name of the fragment parameter is `xyz` and its value is the x, y and z values separated by commas. Each value can be expressed as either an integer or a floating point number.

The annotation above could be expressed as its fragment-based equivalent:

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

The Annotation with a Selector on the targt can paint a resource at a point other than the origin, however it will be at its initial scale and rotation, which may not be appropriate for the scene that is being constructed.

This specification defines a new class of manipulations for SpecificResources called a `Transform`, with three specific sub-classes. Each Transform has three properties, `x`, `y` and `z` which determine how the Transform affects that axis in the local coordinate space.


| Class           | Description  |
| --------------- | ------------ |
| ScaleTransform  | A ScaleTransform applies a multiplier to one or more axes in the local coordinate space. A point that was at 3.5, after applying a ScaleTransform of 2.0 would then be at 7.0. If an axis value is not specified, then it is not changed, resulting in a default of 1.0 |
| RotateTransform | A RotateTransform rotates the local coordinate space around the given axis in a counter-clockwise direction around the axis itself (e.g. around a pivot point of 0 on the axis). A point that was at x=1,y=1 and was rotated 90 degrees around the x axis would be at x=1,y=0,z=1. If an axis value is not specified, then it is not changed, resulting in a default of 0.0 |
| TranslateTransform | A TranslateTransform moves all of the objects in the local coordinate space the given distance along the axis. A point that was at x=1.0, after applying a TranslateTransform of x=1.0 would be at x=2.0. If an axis value is not specified then it is not changed, resulting in a default of 0.0 |

Transforms are added to a SpecificResource using the `transform` property. The value of the property is an array, which determines the order in which the transforms are to be applied. The resulting state of the first transform is the input state for the second transform, and so on. Different orders of the same set of transforms can have different results, so attention must be paid when creating the array and when processing it.

The point around which RotateTransform rotates the space is the origin. This "pivot point" cannot be changed directly, but instead a TranslateTransform can be used to move the desired pivot point to the be at the origin, then the RotateTransform applied.

Transforms are only used in the Presentation API when the SpecificResource is the `body` of the Annotation, and are applied before the resource is painted into the scene at the point given in the `target`.

```json
{
    "type": "SpecificResource",
    "source": {
        "id": "https://example.org/iiif/assets/model1.glb",
        "type": "Model"
    },
    "transform": [
      {
        "type": "RotateTransform",
        "x": 0.0,
        "y": 180.0,
        "z": 0.0
      },
      {
        "type": "TranslateTransform",
        "x": 1.0,
        "y": 0.0,
        "z": 0.0
      }
    ]
}
```

### Relative Rotation

It is useful to be able to rotate a light or camera resource such that it is facing another object or point in the Scene, rather than calculating the angles within the Scene's coordinate space. This is accomplished with a property called `lookAt`, on DirectionalLight, SpotLight and all Cameras. The value of the property is either a PointSelector or the URI of an Annotation which paints something into the current Scene.

If the value is a PointSelector, then the light or camera resource is rotated around the x and y axes such it is facing the given point. If the value is an Annotation which targets a point, via a PointSelector, URI fragment or other mechanism, then the direction the resource is facing that point.

<div style="background: #A0F0A0; padding: 10px; padding-left: 30px; margin-bottom: 10px">
❓What happens if the Annotation targets a Polygon or other non-Point? Calculate centroid? Error? First point given in the Poly / center of a sphere?
</div>

This rotation happens after the resource has been added to the Scene, and thus after any transforms have taken place in the local coordinate space. As the z axis is not affected by the rotation, any RotateTransform that changes z will be retained, but any change to x or y will be lost.

```json
"lookAt": {
    "type": "PointSelector",
    "x": 3,
    "y": 0,
    "z": -10
}
```

## Nesting (Julie)

scene in scene
canvas in scene
excludes


## Scenes with Durations (Mike)

duration
point selector refined by duration
restrictions on annotating container w/duration into a Scene w/o a duration



## New Property Definitions (Rob)


##### Notes on Existing Properties

* `id`: Scenes MUST have `id`, per Canvases
* `type`: All resources MUST have `type`
* `label`: Scenes SHOULD have a `label`, per Canvases
* `items`: Scenes SHOULD have `items`, per Canvases

##### backgroundColor

This property sets the background color behind any painted resources on a spatial resource, such as a Canvas or Scene.

The value _MUST_ be string, which defines an RGB color. It SHOULD be a hex value starting with "#" and is treated in a case-insensitive fashion. If this property is not specified, then the default value is client-dependent.

 * A Canvas _MAY_ have the `backgroundColor` property<br/>
   Clients _SHOULD_ render `backgroundColor` on any resource type.
 * A Scene _MAY_ have the `backgroundColor` property<br/>
   Clients _SHOULD_ render `backgroundColor` on any resource type.
 * Other resources _MUST NOT_ have the `backgroundColor` property.

```json
"backgroundColor": "#FFFFFF"
```

<div style="background: #A0F0A0; padding: 10px; padding-left: 30px; margin-bottom: 10px">
❓Can you set bgColor on a transparent image? An area? Conflict with `style` on a SpecificResource?
</div>

##### near

This property gives the distance from the camera from which objects are visible. Objects closer to the camera than the `near` distance cannot be seen.

The value is a non-negative floating point number. If this property is not specified, then the default value is client-dependent.

* A Camera _MAY_ have the `near` property<br/>
  Clients _SHOULD_ process the `near` property on Cameras.

```json
"near": 1.5
```

##### far

This property gives the distance from the camera after which objects are no longer visible. Objects further from the camera than the `far` distance cannot be seen.

The value is a non-negative floating point number, and _MUST_ be greater than the value for `near` on the same camera. If this property is not specified, then the default value is client-dependent.

* A Camera _MAY_ have the `far` property<br/>
  Clients _SHOULD_ process the `far` property on Cameras.

```json
"far": 200.0
```

##### fieldOfView

_Summary here_

The value _MUST_ be a floating point number greater than 0 and less than 180. If this property is not specified, then the default value is client-dependent.

* A PerspectiveCamera _SHOULD_ have the `fieldOfView` property.<br/>
  Clients _SHOULD_ process the `fieldOfView` property on Cameras.

```json
  "fieldOfView": 50.0
```

##### angle

_Summary here_

The value _MUST_ be a floating point number greater than 0 and less than 90. If this property is not specified, then the default value is client-dependent.

* A SpotLight _SHOULD_ have the `angle` property.<br/>
  Clients _SHOULD_ process the `angle` property on SpotLights.

```json
  "angle": 15.0
```

##### lookAt

_Summary here_

The value _MUST_ be a JSON object, conforming to either a reference to an Annotation with an `id` and a `type` of "Anntoation", or a PointSelector. If this property is not specified, then the default value is null -- the camera or light is not looking at anything.

```json
"lookAt": {
    "type": "PointSelector",
    "x": 3,
    "y": 0,
    "z": -10
}
```

##### color

This property sets the color of a Light.

The value _MUST_ be string, which defines an RGB color. It SHOULD be a hex value starting with "#" and is treated in a case-insensitive fashion. If this property is not specified, then the default value is "#FFFFFF".

 * A Light _SHOULD_ have the `color` property<br/>
   Clients _SHOULD_ render `color` on any resource type.
 * Other resources _MUST NOT_ have the `color` property.

```json
"color": "#FFA0A0"
```

##### intensity

This property sets the strength or brightness of a Light.

The value _MUST_ be a JSON object, that has the `type`, `value` and `unit` properties. All three properties are required. The value of `type` _MUST_ be the string "Value". The value of `value` is a floating point number. The value of `unit` is a string, drawn from a controlled vocabulary of units. If this property is not specified, then the default intensity value is client-dependent.

This specification defines the unit value of "relative" which constrains the value to be a linear scale between 0.0 (no brightness) and 1.0 (as bright as the client will render).

* A Light _SHOULD_ have the `intensity` property.<br/>
  Clients _SHOULD_ process the `intensity` property on Lights.

```json
{
 "intensity": {"type": "Value", "value": 0.5, "unit": "relative"}
}
```

##### exclude

_Summary here_

_On Annotation, a list of strings drawn from table_

| Value      | Description |
|------------|-------------|
| Audio      | |
| Animations | |
| Cameras    | |
| Lights     | |

```json
"exclude": [ "Audio", "Lights", "Cameras", "Animations" ]
```

##### transform

_Summary here_

The value of this property is an array of JSON objects, each of which is a Transform.

##### x

##### y

##### z
 

## Future Work (Tom)

* Commentary Annotations
* Interactions
* Animations

