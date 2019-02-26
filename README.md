# HDA
Houdini digital assets
1. "blast by id.hda" This asset helps to prepare particle simulation for meshing. "Add points" button runs python script, reads selection
made in viewport, creates pattern based on given attribute like particle "id" and paste group selection pattern to blast node (i.e. 
@id="1 2 3"). Also it can use points from second input to read it's "id" and remove identical particles from first input.
