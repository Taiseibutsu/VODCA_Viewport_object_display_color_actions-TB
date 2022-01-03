bl_info = {
    "name": "TB_Display_Color",
    "author": "Taiseibutsu",
    "version": (0, 1),
    "blender": (2, 80, 0),
    "location": "View3D > UI",
    "description": "Quickly Set Display Colors",
    "warning": "",
    "doc_url": "",
    "category": "Display",
}


import bpy
from bpy.props import BoolProperty, FloatProperty, StringProperty, IntProperty, FloatVectorProperty, EnumProperty

bpy.types.WindowManager.hide_mat = bpy.props.BoolProperty()
bpy.types.WindowManager.hide_obj = bpy.props.BoolProperty()

class TB_displayproperties(bpy.types.PropertyGroup):
    object_mode : bpy.props.EnumProperty(
        name = "Enumerator/Dropdown",
        description = "Mode to set object display color",
        items= [('SELECTION','Selection','Set color to Selection', 'RESTRICT_SELECT_OFF', 0),
                ('COLLECTION','Collection','Set color to Selected Colection', 'OUTLINER_COLLECTION', 1),
                ('SCENE', 'Scene', 'Set color to Scene','SCENE_DATA',2),
                ('ALL','All','Set color to All Objects', 'BLENDER', 3)
        ]
    )
    material_mode : bpy.props.EnumProperty(
        name = "Enumerator/Dropdown",
        description = "Mode to set material display color",
        items= [('SELECTION','Selection','Set color to Selection', 'RESTRICT_SELECT_OFF', 0),
                ('COLLECTION','Collection','Set color to Selected Colection', 'OUTLINER_COLLECTION', 1),
                ('SCENE', 'Scene', 'Set color to Scene','SCENE_DATA',2),
                ('ALL','All','Set color to All Objects', 'BLENDER', 3)
        ]
    )   
    object_color : FloatVectorProperty(  
       name="object_color",
       subtype='COLOR',
       default=(0.5, 0.9, 1.0, 1.0),
       min=0.0, max=1.0,
       description="Color to object display",
       size=4
       )
    material_color : FloatVectorProperty(  
       name="material_color",
       subtype='COLOR',
       default=(1.0, 0.9, 0.5, 1.0),
       min=0.0, max=1.0,
       size=4,
       description="Color to material display"
       )       
    colortoscene : bpy.props.PointerProperty(type=bpy.types.Scene)
    colortoactivesceene : bpy.props.BoolProperty(default = True, description = "Transfer Color to Active Scene")
    colortocollection : bpy.props.PointerProperty(type=bpy.types.Collection)
    colortocollectionactive : bpy.props.BoolProperty(default = False, description = "Transfer Color to ActiveCollection")

class TB_OP_set_object_color(bpy.types.Operator):
    """Set Object Material Display Operator"""
    bl_idname = "tb_ops.set_obj_col"
    bl_label = "Set Object Color Display"

    def execute(self, context):
        tbdp = bpy.context.scene.tb_display_properties
        
        if tbdp.object_mode =='SELECTION':  
            for ob in bpy.context.selected_objects:
                if ob.type in ['MESH','CURVE','META','SURFACE','FONT','GPENCIL']:
                    ob.color = tbdp.object_color
        if tbdp.object_mode =='ALL':
            for ob in bpy.data.objects:
                ob.color = tbdp.object_color   
        if tbdp.object_mode =='SCENE':
            if tbdp.colortoactivesceene:
                for ob in bpy.context.scene.objects:
                    ob.color = tbdp.object_color  
            else:
                if tbdp.colortoscene != None:
                    for ob in tbdp.colortoscene.objects:
                        ob.color = tbdp.object_color    
        if tbdp.object_mode =='COLLECTION':
            if tbdp.colortocollectionactive:
                for ob in bpy.context.collection.objects:
                    ob.color = tbdp.object_color 
            else:
                if tbdp.colortocollectionactive != None:
                    for ob in tbdp.colortocollection.objects:
                        ob.color = tbdp.object_color                                      
        return {'FINISHED'}
    
    
class TB_OP_mat_object_color(bpy.types.Operator):
    """Set Material Color Display Operator"""
    bl_idname = "tb_ops.set_mat_col"
    bl_label = "Set Material Color Display"

    def execute(self, context):
        tbdp = bpy.context.scene.tb_display_properties
        if tbdp.material_mode =='SELECTION':  
            for ob in bpy.context.selected_objects:
                if ob.type in ['MESH','CURVE','META','SURFACE','FONT'] and ob.active_material:
                    ob.active_material.diffuse_color = tbdp.material_color
        if tbdp.material_mode =='ALL':
            for ob in bpy.data.objects:
                if ob.type in ['MESH','CURVE','META','SURFACE','FONT']and ob.active_material:
                    ob.active_material.diffuse_color = tbdp.material_color   
        if tbdp.material_mode =='SCENE':
            if tbdp.colortoactivesceene:
                for ob in bpy.context.scene.objects:
                    if ob.type in ['MESH','CURVE','META','SURFACE','FONT'] and ob.active_material:
                            ob.active_material.diffuse_color = tbdp.material_color  
            else:
                if tbdp.colortoscene != None:
                    for ob in tbdp.colortoscene.objects:
                        if ob.type in ['MESH','CURVE','META','SURFACE','FONT'] and ob.active_material:
                            ob.active_material.diffuse_color = tbdp.material_color    
        if tbdp.material_mode =='COLLECTION':
            if tbdp.colortocollectionactive:
                for ob in bpy.context.collection.objects:
                    if ob.type in ['MESH','CURVE','META','SURFACE','FONT'] and ob.active_material:
                        ob.active_material.diffuse_color = tbdp.material_color 
            else:
                if tbdp.colortocollectionactive != None:
                    for ob in tbdp.colortocollection.objects:
                        if ob.type in ['MESH','CURVE','META','SURFACE','FONT'] and ob.active_material:
                            ob.active_material.diffuse_color = tbdp.material_color                                      
        return {'FINISHED'}
    
class TEST_PT_panel(bpy.types.Panel):
    """Creates a Panel"""
    bl_space_type = "VIEW_3D"
    bl_region_type = "UI"
    bl_label = "TB Set Color Display"
    bl_category = "TB_OPS"
    
    def draw_header(self,context):
        layout = self.layout
        layout.label(icon='COLOR')
    def draw(self, context):
        layout = self.layout
        row = layout.row(align=True)
        
        wm = context.window_manager
        tbdp = bpy.context.scene.tb_display_properties
        if wm.hide_mat:
            icn_mat = "HIDE_ON"
        else:
            icn_mat = "HIDE_OFF"
        if wm.hide_obj:
            icn_obj = "HIDE_ON" 
        else:
            icn_obj = "HIDE_OFF"
                                   
        row.prop_enum(context.space_data.shading,"color_type",'OBJECT',icon='OBJECT_DATA')
        row.prop(wm,"hide_obj",text="",icon=icn_obj)
        row.separator()
        row.prop_enum(context.space_data.shading,"color_type",'MATERIAL',icon='MATERIAL')
        row.prop(wm,"hide_mat",text="",icon=icn_mat)

        
        split = layout.split()
        
        if not wm.hide_obj:        
            col_1 = split.column(align=True)
            col_1.prop(tbdp,"object_color",text="")
            col_1.separator()
            if tbdp.object_mode in ['SCENE','COLLECTION']:
                split1 = col_1.split(align=True,factor=0.1)
            else:
                split1 = col_1.split(align=True)
            #col_2_1 = split1.column(align=True)
            if tbdp.object_mode == 'SCENE':
                col_1_1_2 = split1.column(align=True)
                col_1_1_2.prop(tbdp,"colortoactivesceene",text="",icon='PIVOT_ACTIVE')
                if not tbdp.colortoactivesceene:
                        col_1_1_1 = split1.column(align=True)
                        col_1_1_1.prop(tbdp,"colortoscene",text="")
            if tbdp.object_mode == 'COLLECTION':
                col_1_1_1 = split1.column(align=True)
                col_1_1_1.prop(tbdp,"colortocollectionactive",text="",icon='PIVOT_ACTIVE')                                       
                if not tbdp.colortocollectionactive:
                    col_1_1_1 = split1.column(align=True)
                    col_1_1_1.prop(tbdp,"colortocollection",text="")
            col_1_2 = split1.column(align=True)
            col_1_2.prop(tbdp, "object_mode",text="")
            col_1.operator("tb_ops.set_obj_col",text="Set Object Color Display",icon='FILE_REFRESH') 
        
        if not wm.hide_mat:
            col_2 = split.column(align=True)
            col_2.prop(tbdp,"material_color",text="")
            col_2.separator()
            if tbdp.material_mode in ['SCENE','COLLECTION']:
                split1 = col_2.split(align=True,factor=0.1)
            else:
                split1 = col_2.split(align=True)
            #col_2_1 = split1.column(align=True)
            if tbdp.material_mode == 'SCENE':
                col_2_1_2 = split1.column(align=True)
                col_2_1_2.prop(tbdp,"colortoactivesceene",text="",icon='PIVOT_ACTIVE')
                if not tbdp.colortoactivesceene:
                        col_2_1_1 = split1.column(align=True)
                        col_2_1_1.prop(tbdp,"colortoscene",text="")
            if tbdp.material_mode == 'COLLECTION':
                col_2_1_1 = split1.column(align=True)
                col_2_1_1.prop(tbdp,"colortocollectionactive",text="",icon='PIVOT_ACTIVE')                                       
                if not tbdp.colortocollectionactive:
                    col_2_1_1 = split1.column(align=True)
                    col_2_1_1.prop(tbdp,"colortocollection",text="")
            col_2_2 = split1.column(align=True)
            col_2_2.prop(tbdp, "material_mode",text="")
            col_2.operator("tb_ops.set_mat_col",text="Set Material Color Display",icon='FILE_REFRESH') 
       

classes = (
    TB_displayproperties,
    TEST_PT_panel,
    TB_OP_set_object_color,
    TB_OP_mat_object_color,
    )

def register():
    for cls in classes:
        bpy.utils.register_class(cls)
    
    bpy.types.Scene.tb_display_properties = bpy.props.PointerProperty(type=TB_displayproperties)


def unregister():
    for cls in classes:
        bpy.utils.unregister_class(cls)

    del bpy.types.Scene.test_tool

if __name__ == "__main__":
    register()