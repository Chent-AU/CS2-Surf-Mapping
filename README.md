# CS2 Surf Mapping
A guide on the workflows and techniques required to create surf maps for Counter-Strike 2 in the Source 2 Engine.

This repository will include the zipped source files required to view, edit and complete the examples provided in the guide.
If updated techniques or new information becomes available, please contact the contributors.

Note that this guide is not designed as a tutorial for the tools and capabailities of the Hammer editor. There are many available tutorials on how to use the tools described in this guide online, this guide is for the specific techniques required to make building and testing Surf maps in CS2.

---

# Contents
- [Introduction](#introduction)
- [Ramps](#ramps)
  - [Ramp Bugs](#ramp-bugs)
  - [Ramp Types](#ramp-types)
  - [Ramp Construction](#ramp-construction)
    - [Size & Steepness](#size--steepness) 
    - [Straight Ramps](#straight-ramps)
    - [Vertically Curved Ramps](#vertically-curved-ramps)
    - [Horizontally Curved Ramps](#horizontally-curved-ramps)
  - [Ramp Design & Asthetics](#ramp-design--aesthetics)
- [Map Design](#map-design)
  - [Obstacles](#obstacles)
  - [Moondomes & Fake Skyboxes](#moondomes--fake-skyboxes)
- [Zoning](zoning)
  - [Timer Plugin Zones](#timer-plugin-zones)
  - [Triggers & Teleports](#triggers--teleports)
  - [Exploit Prevention](#exploit-prevention)
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

# Introduction
#### Purpose of This Guide
The primary objectives of this guide are to:​
- Demystify the surf mapping process: Break down complex concepts into manageable steps.​
- Highlight common issues: Address known bugs and quirks specific to CS2 surf mapping.​
- Share effective workflows: Offer insights into efficient mapping practices, including version control and testing methodologies.​
- Foster community collaboration: Encourage knowledge sharing and collective problem-solving among mappers.
#### What This Guide Covers
This guide encompasses the following topics:​
- Ramp Construction and Design: Techniques for building functional and aesthetically pleasing ramps.​
- Map Zoning and Triggers: Setting up timers, teleports, and other essential map elements.​
- Lighting and Optimization: Strategies for effective lighting and performance enhancement.​
- Testing and Building: Best practices for efficient map testing, compiling, and deployment.​
- Version Control with Git: Managing your mapping projects using Git for collaboration and version control.​

Each section is crafted to provide step-by-step instructions, accompanied by examples and visual aids where applicable.

#### Community Contributions
The CS2 surf mapping community thrives on collaboration. If you have insights, techniques, or resources that could benefit fellow mappers, we encourage you to contribute to this guide. Together, we can build a robust knowledge base that supports both current and future creators.
#### Before You Begin
It is strongly recommended that you read through the entire guide before beginning your first surf map project. Understanding the full scope of surf-specific mechanics, common pitfalls, and Source 2 workflows will save you hours of frustration and ensure your mapping journey starts on solid ground.

<br>
<br>
<br>

# Ramps
### Ramp Bugs
Ramp bugs are a significant challenge in CS2 surf mapping, often disrupting player momentum and leading to frustrating gameplay experiences. Unlike in Source 1, where ramp bugs typically result in players getting stuck or abruptly stopping, CS2 introduces new behaviors where velocity can be unpredictably redirected, causing players to veer off course or lose speed unexpectedly.​
#### Causes of Ramp Bugs
According to the [CS2KZ rampbug fix analysis](https://gist.github.com/zer0k-z/2eb0c230c8f2c62b5c46d36353cf8d8d), ramp bugs in CS2 are primarily caused by tiny, invisible slopes—often no larger than 0.06 units—that form at the edges or intersections of mesh triangles. These micro-slopes are not present in the map's design but are generated upon loading the map, leading to inconsistencies across different CS2 versions. Notably, these bugs became more prevalent following the "Call to Arms" update, which significantly increased their frequency.​
#### Mitigation Strategies
While a definitive fix from Valve is pending, mappers can employ several strategies to minimize the occurrence of ramp bugs:​
- **Use Convex Hull Collision:** Applying convex hull collision physics and tying an entity to the ramp can reduce ramp bugs without causing client-side prediction errors. However, this method, while highly effective, doesn't eliminate all ramp bugs, especially those occurring at the end of ramps.​
- **Implement Server-Side Plugins:** Plugins that intercept movement functions like TryPlayerMove and CategorizePosition can adjust player movement to avoid ramp bugs. These plugins work by detecting potential bug scenarios and slightly repositioning the player to maintain momentum. It's important to note that while effective, this approach may introduce prediction errors for players with high latency.

<br>
<br>

### Ramp Types
The following is a list of basic ramp types, these types can be combined and merged as well as used individually - e.g. a curved vertical ramp with a trapezoid end.
<br>
<br>
*Note - full and inverted ramp variants can be easily created by mirroring a half ramp, so it is best to create whatever ramp you desire as a half ramp, and then duplicate as neccessary to reduce repeated work.*
<br>

#### Straight Half Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/straight_half.gif "Straight Half Ramp")
<br>
<br>
#### Straight Full Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/straight_full.gif "Straight Full Ramp")
<br>
<br>
#### Straight Inverted Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/straight_inverted.gif "Straight Inverted Ramp")
<br>
<br>
#### Trapezoid Half Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/trapezoid_half.gif "Trapezoid Half Ramp")
<br>
<br>
#### Trapezoid Full Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/trapezoid_full.gif "Trapezoid Full Ramp")
<br>
<br>
#### Trapezoid Inverted Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/trapezoid_inverted.gif "Trapezoid Inverted Ramp")
<br>
<br>
#### Reverse-Trapezoid Half Ramp

![Alt Text]()
<br>
<br>
#### Reverse-Trapezoid Full Ramp

![Alt Text]()
<br>
<br>
#### Reverse-Trapezoid Inverted Ramp

![Alt Text]()
<br>
<br>
#### Vertical Curved Half Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/vertical_curved_half.gif "Vertical Curved Half Ramp")
<br>
<br>
#### Vertical Curved Full Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/vertical_curved_full.gif "Vertical Curved Full Ramp")
<br>
<br>
#### Vertical Curved Inverted Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/vertical_curved_inverted.gif "Vertical Curved Inverted Ramp")
<br>
<br>
#### Horizontal Curved Half Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/horizontal_curved.gif "Horizontal Curved Half Ramp")
<br>
<br>
#### Horizontal Curved Full Ramp

![Alt Text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_types/horizontal_curved_full.gif "Horizontal Curved Full Ramp")


<br>
<br>

### Ramp Construction
#### Size & Steepness
To Include:
- How size and steepness impacts a map (must be > 45 degrees)
- Examples of sizes of ramps on well known maps
- Recommended sizes and height / width ratios

<br>
<br>

#### Straight Ramps
1. For straight ramps, constructing ramps anti-rampbug is relatively straight forward. Create a simple ramp segment by placing a rectangular block *(LShift+B)*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/make_block_square.gif "Using LShift+B to place a rectangular block.")

2. Use the clip tool *(LShift+X)* to cut the block into a triangle, ensure `cap new surfaces` is enabled.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/cut_block.gif "Cut block to ramp shape.")

3. Extend the ramp to the desired length using the selection tool *(LShift+S)*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/straight_ramp/extend_body.gif "Extend the ramp to desired size.")

4. Select the surfable surfaces of the ramp in `face mode` and copy them *(Ctrl+C)*
Then select the ramp in `mesh mode` and hide it with `quickhide` *(H)*.
Then paste the faces you copied with `paste special` *(Ctrl+LShift+V)* (You don't need to alter any paste settings you can just click `ok`)

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/straight_ramp/copy_surface_paste_special_hide_ramp_body.gif "Copy and paste faces, and hide ramp body.")

5. Search for and find the `toolsplayerclip` material in the `material browser`, and apply it to the faces *(LShift+T)*

![alt text](https://github.com/Chent-AU/CS2-Surf-Mapping/blob/main/media/gifs/straight_ramp/apply_toolsplayerclip.gif "Apply the 'toolsplayerclip' material.")

6. Use the `thicken faces` *(G)* tool to increase the thickness to `2 units`. Ensure `from center` is disabled.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/straight_ramp/thicken_clip_faces.gif "Thicken the faces of the ramp clip")

7. Select the clip in `mesh mode` and in the `object properties` menu change the `physics type` from `Default` to `Mesh`.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/straight_ramp/change_clip_phys_deafult_to_mesh.gif "Change clip physics type to mesh.")

8. Ensure the clip is selected in `mesh mode`, unhide the ramp body *(U)* and hide the clip *(H)*. Select the ramp body in `mesh mode`. In the `object properties` menu change the `physics type` from `Default` to `None`.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/straight_ramp/change_ramp_body_phys_to_none.gif "Change ramp body physics type to none.")

9. Select the bottom and end cap faces of the ramp body. Use `copy` *(Ctrl+C)* and `paste special` *(Ctrl+LShift+V)* to duplicate the faces. Apply the `toolsplayerclip` material *(LShift+T)*, and then in `mesh mode`, change the `physics mode` of this clip from `None` to `Mesh`.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/straight_ramp/clip_end_caps_and_change_phys_to_mesh.gif "Clip end caps and bottom and change physics to mesh.")

10. Now you can unhide all the ramp components *(U)* and your ramp is ready to be surfed.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/straight_ramp/unhide_all.gif "Unhide all components.")

The straight ramnp is now complete and ready to be surfed on.

<br>
<br>

#### Vertically Curved Ramps
Creating a curved ramp, either vertically or horizontally curved is a little more complicated, but still easy if you follow this method closely.
1. Create and cut a single ramp segment using the straight ramp method ([steps 1 & 2](#straight-ramps))

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/make_block_square.gif "Using LShift+B to place a rectangular block.")
![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/cut_block.gif "Cut block to ramp shape")

2. Duplicate this segment *(LShift+ArrowKey)*. Enter `rotation mode` *(R)* and select the duplicated ramp segment. Center the `rotation pivot` in the selection *(Alt+Home)* and rotate the segment.
   Select all the vertices of the rotated segment in `vertex mode` *(Double Click)*, and drag the bottom corner vertex to align with the original ramp segment in the `2D Side View`

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/duplicate_and_align_first_segment.gif "Duplicate, rotate and align the bottom-corner vertex of the ramp segment.")

3. Repeat step 2, selecting all the ramp segments when duplicating, and all the vertices when aligning. Ensure the rotation amount is consistent between each ramp segment. *(The example uses 5° per ramp segment)*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/create_more_duplicated_segments_and_align.gif "Repeat steo 2, until desired ramp length is met.")

4. Select the surfable surfaces of the ramp in `face mode` and copy them *(Ctrl+C)*
Then select the ramp in `mesh mode` and hide it with `quickhide` *(H)*.
Then paste the faces you copied with `paste special` *(Ctrl+LShift+V)* (You don't need to alter any paste settings you can just click `ok`)

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/duplicate_faces_and_hide_ramp_body.gif "Select, and duplicate the ramp body faces, and hide the ramp body meshes.")

5. Search for and find the `toolsplayerclip` material in the `material browser`, and apply it to the faces *(LShift+T)*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/apply_toolsplayerclip.gif "Apply the toolsplayerclip material to clips.")

7. In `edge mode` select the interior edges of the ramp segments. In the `Editing` menu, switch to `Use selection's local space axes for gizmo`. Ensure your `grid size` is 1 - 8 units, and use the `translate` tool to slide the edges so that the faces overlap once, and the top vertices are almost touching.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/translate_edges.gif "Translate edges along clip body surface.")

7. Use the `thicken faces` *(G)* tool to increase the thickness to `2 units`. Ensure `from center` is disabled.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/thicken_faces.gif "Thicken faces of clip surface.")

8. Select the clips in `mesh mode` and in the `object properties` menu change the `physics type` from `Default` to `Multiple Convex Hulls`. Then select the mesh of the first ramp clip, and change its `physics type` from `Multiple Convex Hulls` to `Mesh`

*Note, 'the first ramp clip' is the clip at the start of the ramp, where start and end are defined by the direction the ramp will be surfed when playing the map.*

*Note, if your curved ramp contains both long straight section(s) and curved section(s), ensure the ramp clip(s) for the straight section(s) have `Physics Type` set to `Mesh`.*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/change_ramp_clip_physics_and_end_clip_physics.gif "Change physics clip physics types.")

9. Ensure the ramp clips are selected in `mesh mode`, now unhide the ramp body *(U)*, and hide the clip surfaces *(H)*. Then delete every second segment *(Del)*, and the interior facing faces of the remaining segments.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/hide_clip_and_delete_body_intermediate_segments.gif "Unhide ramp body, hide clips and delete interval segments.")

10. In `edge mode`, select the adjacent `open loops` of edges between the deleted interior faces *(LShift+Double Click)*. Create an `interpolated bridge` between the `edge loops` *(Alt+B)*.
Ensure `twists` is set to `0` and `steps` is set to `1`. Press enter to confirm. Repeat this for all of the gaps in the ramp.
If you have an even number of segments, you must then delete all the faces of the final segment and then bridge the remaining final gap.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/bridge_edges.gif "Bridge the ramp segments")<br>

*Note, if your ramp body is made of an even number of segments, you will have to delete the faces of the end segment and bridge them like in the gif below*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/even_segments_last_piece.gif "Bridge final segment of even number of segment ramp.")

11. Now select the ramp body in `mesh mode`. In the `object properties` menu change the `physics type` from `Default` to `None`.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/change_ramp_body_phys_to_none.gif "Change the ramp body physics to none.")

12. Finally, select the bottom and end cap faces of the ramp body. Use `copy` *(Ctrl+C)* and `paste special` *(Ctrl+LShift+V)* to duplicate the faces. Apply the `toolsplayerclip` material *(LShift+T)*, and then in `mesh mode`, change the `physics mode` of this clip from `None` to `Mesh`.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/clip_end_and_bottom_and_change_phys.gif "Clip end and bottom of ramp body.")

You can now unhide all the components *(U)* and use the completed ramp. Ensure the if you move or rotate the ramp, that you select **ALL** components.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/curved_ramp/unhide_final_product.gif "Unhide all components.")

<br>
<br>

#### Horizontally Curved Ramps

<br>
<br>

### Ramp Design & Aesthetics
#### Ramp Consistency
​Incorporating consistent materials and colors for interactive surfaces, such as end platforms, ramps, and bhoppable areas, is essential for intuitive navigation. This consistency allows players to quickly recognize and interact with these elements, enhancing their overall experience. By differentiating these materials and colors from the rest of the map, you create clear visual cues that guide players seamlessly through the environment. ​It is of course, not strictly neccessary, or required, but in general makes it easier for players to learn the routes and limits of the map.

#### Decorating Ramps
Utilizing tools like the face cut tool and beveling techniques enables the creation of intricate patterns and designs on curved ramp surfaces. These methods not only add aesthetic appeal but also contribute to a more immersive and engaging gameplay environment.
Similar effects can be achieved by well-designed custom textures, or by modelling workflows through blender, but that is outside the scope for this guide. Many tutorials and guides for this can be found on youtube.

*View here the `face cut tool` *(C)*, to cut an indent into the faces of the ramp surface.*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_decoration/cut_faces.gif "Cut ramp surface.")

*And then using `local selection axis` the faces can be indented and duplicated to make a window into the ramp.*

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/ramp_decoration/indent_faces.gif "Indent ramp surface.")

<br>
<br>
<br>

# Map Design
### Obstacles
Here is a list of obstacles you might wish to include into your map.
<br>
<br>

#### Spin
A normal spin will only allow the player to turn in one direction.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/obstacles/spin.gif "Default Spin")
<br>
<br>

#### Royal Spin
A royal spin will force the player to change the direction of their rotation part way into the spin.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/obstacles/royal_spin.gif "Royal Spin")
<br>
<br>

#### Free Spin
A free spin will allow the player to spin in their desired direction.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/obstacles/free_spin.gif "Free Spin")
<br>
<br>

#### Drop Strafe
A drop strafe will allow the player to strafe left and right instead of spinning (assuming there is enough room).

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/obstacles/drop_strafe.gif "Drop Strafe")
<br>
<br>

#### Window
A window can be used as a speedcheck, or as a precision test depending on its height, distance and size. It can be made bhoppable or can be made to reset the player if they do not pass through cleanly.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/obstacles/window.gif "Window")
<br>
<br>

#### Ramp Strafes
Ramp strafes can be added to make maintaining units more difficult or to make the setup to jumps more precise.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/obstacles/ramp_strafe.gif "Ramp Strafe")
<br>
<br>

#### Pillars / Air Strafes
Pillars (Or any other object / obstacle) can be used to force the player to strafe through the air.

![alt text](https://raw.githubusercontent.com/Chent-AU/CS2-Surf-Mapping/refs/heads/main/media/gifs/obstacles/pillars.gif "Pillars")
<br>
<br>


To Include:
- a section on each common obstacle in a surf map and a description and build guide (and a difficulty rating or range in surf map tier)
  - Spin
  - Free Spin
  - Royal Spin
  - Drop Strafe
  - Ramp Stafe
  - Window
  - Pillars / Columns / Airstrafe
  - Headcheck
  - Headsurf
  - Bhop
  - Slide
  - Speedcheck (highjump and longjump)

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

# Zoning
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

<br>
<br>
<br>

# Lighting & Vis
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

# Testing & Building
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

# Version Control & Git
To Include:
- .gitignore requirements to avoid commiting unneccessary files
- different addon isolation techniques
  - single repo single branch
  - single repo multi branch
  - multi repo

<br>
<br>
<br>

# Special Thanks & Contributors

