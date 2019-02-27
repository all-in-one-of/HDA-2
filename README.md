# HDA
Houdini digital assets
1. "blast by id.hda" This asset helps to prepare particle simulation for meshing. "Add points" button runs python script, reads selection
made in viewport, creates pattern based on given attribute like particle "id" and paste group selection pattern to blast node (i.e. 
@id="1 2 3"). Also it can use points from second input to read it's "id" and remove identical particles from first input.
It uses hda PythonModule:
**********************
import toolutils
def s(pwd):
    sel = toolutils.sceneViewer().selectGeometry()
    node = sel.nodes()[0]
    bl = hou.node(pwd.path()+"/blast1")
    geo = bl.geometry()
    sel = sel.selectionStrings()
    prev_sel = bl.parm("group").unexpandedString().split("\"")[1]
    aname = pwd.parm("aname").eval()
    id_a = geo.findPointAttrib(aname)
    list_pts = geo.globPoints(sel[0])
    id_ls = [prev_sel]
    for i in list_pts:
        id_ls.append(str(i.attribValue(id_a)))
    parm = bl.parm("group")
    gr = "@`chs('../aname')`=\"" + " ".join(id_ls).lstrip() + "\""
    parm.set(gr)
    parm_pat = bl.parm("pat")
    parm_pat.set(len(prev_sel.split())+len(id_ls)-1)
def clr(pwd):
    parm = hou.node(pwd.path()+"/blast1").parm("group")
    parm.set("@`chs('../aname')`=\"\"")
    parm_pat = hou.node(pwd.path()+"/blast1").parm("pat")
    parm_pat.set(0)
************************
and this parameter callbacks for buttons:
1) kwargs["node"].hdaModule().s(kwargs["node"])
2) kwargs["node"].hdaModule().clr(kwargs["node"])
