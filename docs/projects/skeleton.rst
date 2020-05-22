.. _skeleton-project:

Skeleton Tool (GSoC-2020) [IN PROGRESS]
=====================================================

Implementation Plan
~~~~~~~~~~~~~~~~~~~

The Main functions of the tool are:

- Create and Transform Bones:
    - Single-click on an empty space in the workarea would add a default bone with all the handles. 
    - Mouse-Down and Drag will add bone between the points dragged.
    - The handles will have their usual functions for transforming the bones.
- Set Parent-Child Relations:
    - I’d like to propose “Active bone” for this. This can be set by Clicking on a bone. Active Bone can be differentiated by adding a red outline or a blue highlight.
    - Newly created bone is set as a child to the "Active bone". The newly created bone will then be set to "Active".
    - For changing relations, we’ll add an option “Set as child to current Active Bone” in the context menu of the bone. The meaning is self-explanatory.


- If time allows,
    - In the tool options, we can show current active bone and it's child bones so that, when we set root bone to "Active", we get an idea of the structure of the skeleton
    - Find a way to also add linking bones to the layers. (Hard)


Milestones
~~~~~~~~~~
- From May 4th to June 1st: (Community Bonding Period)
    - Will start taking opinions regarding the tool and expectations of the community.
    - Will start working on the project for a head-start.

- From June 1st to June 29th: (Eat- Sleep – Code -Repeat)
    - Will start working with full force to get the basic "click to Active Bone" and "drag-click to draw bone" working.

- From June 29th to July 3rd: 
    - Phase 1 evaluation submission

- From July 3rd to July 27th:
    - Implement the remaining features.
    - Adding the option “Make child to Current Active Bone” and make it work.

- From July 27th to July 31st: 
    - Phase 2 evaluation Submission

- From July 31st to August 24th:
    - Extensive testing and implementing missing features. (if any)
    - Polishing the UI.

- From August 24th to August 31st: 
    - Final Project Submission

- After GSoC: 
    - I’ll keep on contributing to Synfig Studio as I enjoy contributing to anything that involves Art and computers. <3


