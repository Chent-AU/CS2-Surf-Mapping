# CS2 Surf Mapping
A comprehensive guide on the workflows and techniques required to create surf maps for Counter-Strike 2 in the Source 2 Engine.

This repository will include the zipped source files required to view, edit and complete the examples provided in the guide.
If updated techniques or new information becomes available, please contact the contributors.

---

## Contents
- [Introduction](#introduction)
- [Ramps](#ramps)
  - [Ramp Bugs](#ramp-bugs)
  - [Ramp Construction](#ramp-construction)
  - [Ramp Design & Asthetics](#ramp-design--aesthetics)
- [Map Zoning & Design](map-zoning--design)
  - [Timer Plugin Zones](#timer-plugin-zones)
  - [Triggers & Teleports](#triggers--teleports)
  - [Exploit Prevention](#exploit-prevention)
  - [Moondomes & Fake Skyboxes](#moondomes--fake-skyboxes)
- [Lighting & Vis](#lighting--vis)
  - [Light Blocking](#light-blocking)
  - [Lighting Optimisation](#lighting-optimisation)
  - [Vis Hints & Optimisation](#vis-hints--optimisation)
- [Testing & Building](#testing--building)
  - [Map Config & Surf Settings](#map-config--surf-settings)
  - [Lighting Testing](#lighting-testing)
  - [Vis Build Skipping](#vis-build-skipping)
  - [Workshop & Server Access](#workshop--server-access)
  - [Building For Release](#building-for-release)
- [Version Control & Git](#version-control--git)
- [Special Thanks & Contributors](#special-thanks--contributors)

---

## Introduction
To include:
Ramp bugs in CS2
- The purpose of this guide (primarily new maps, not porting)
- The areas covered in the guide
- The reason for this guide
- The open-source nature of this guide, constructed by many members of the mapping community (hopefully)
- A declaration that this guide is not a tutorial for hammer, but specifically for the construction of surf maps
- Recommendation that the guide be read in its entirety before beginning a mappers first project

<br>
<br>
<br>

## Ramps
### Ramp Bugs
To Include:
- The differnce between ramp bugs in source 1 and source 2
- The rampbugfix plugin overview and link to explanation
- Overview of methods to reduce these rampbugs

### Ramp Construction
To Include:
- The method of curved ramp body construction (pre player-clip)
    - Aligning vertices
    - Segment width
    - Segment Rotation Angle
    - Ramp Size (include examples from well known maps)
- The latest / most effective ramp clipping method (Player clip method)
  - duplicate faces as player clip material
  - translate interior body-segment edges along ramp surface plane to overlap eachother.
  - thicken the faces (2 units)
  - The physics type of the ramp components
  - include methods of solidifying ramp (clipping end-caps / bottom, or using offset / lowered nodraw mesh)
- The method of combining ramp segments into single mesh
  - Deleting every second segment
  - Deleting interior faces
  - Using an interpolated bridge to connect open edge loops
  - For ramps made of even number of segments, deleting second last end segment after completing bridge interpolation, then interpolate to final segment.
  
### Ramp Design & Aesthetics
To Include:
- General good practise for design
  - Use of consistent materials and colours for interactable surfaces (end platforms, ramps, bhoppable surfaces, etc) to guide the player more easily on first playthrough
  - Use of easily distinguishable materials / colours from interactable surfaces from the rest of the map
- Use of face cut tool and bevel to create patterns and designs in curved ramp surfaces

<br>
<br>
<br>

## Map Zoning & Design
### Timer Plugin Zones
To Include:
- Linear and staged/hybrid map start/end zones
- Linear checkpoint zones
- Staged/hybrid stage start zones
- Timer-Killer trigger zones
- Timer-Reset trigger zones
  
### Triggers & Teleports
To Include:
- Telehop techniques
  - Trigger teleports
  - Landmark anchors for relative teleport
  - Rotation and facing-direction
  - Velocity Killers
- Reset Triggers
  - Using sharptimer `trigger_multiple`s
  - Using `trigger_teleport` tied entities
  - Staged/Hybrid vs Linear reset triggers
    
### Exploit Prevention
To Include:
- Timer-Killer trigger zones
- Reset trigger techniques
- Nodraw / playerclip encapsulation
  
### Moondomes & Fake Skyboxes
To Include:
- Explanation on changes from Source 1 Skybox Material
- How to create Moondomes
  - Stiching cubemap images
  - Using raw EXR files
  - Where to find skybox textures

<br>
<br>
<br>

## Lighting & Vis
### Light Blocking
To Include:
- Lighting leak prevention best practise
  - interior lighting (sealed geometry)
  - Light blocking whereever possible to reduce lighting calculations
    
### Lighting Optimisation
To Include:
- Reduce number of lights via:
  - increased brightness
  - reduced dropoff
  - increased range
  - increased bounce reflectivity
    
### Vis Hints & Optimisation
To Include:
- Importance of vis in map performance optimisation
- How to place vis hints

<br>
<br>
<br>

## Testing & Building
### Map Config & Surf Settings
To Include:
- Config options to include
- Where to find config options (in console on a public server)
- Recommended settings
- How to set config to be packed in vpk build
  
### Lighting Testing
To Include:
- Testing non-visual map elements (e.g. surfing), building without lighting and using `mat_fullbright 1;`
- Testing without lighting compression
  
### Vis Build Skipping
To Include:
- Explanation of what vis is
- Explanation of why vis build skipping is possible
- Possible side effects of vis build skipping
- How to vis build skip
  
### Workshop & Server Access
To Include:
- Explanation of workshop access types (private, friends only, unlisted, public)
- Explanation of workshop approval
- Explanation of CS2 server map access (can not use local files, can only use workshop links)
  
### Building For Release
To Include:
- Requirements for final build (vis, lighting, compression)
- Recommendations for preview media
  - Video of map being surfed
  - Photos of map
  - Inclusion of cfg in description

<br>
<br>
<br>

## Version Control & Git
To Include:
- .gitignore requirements to avoid commiting unneccessary files
- different addon isolation techniques
  - single repo single branch
  - single repo multi branch
  - multi repo

<br>
<br>
<br>

## Special Thanks & Contributors

