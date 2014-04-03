[In this branch, the PolyLine is be mended to be right, and you needn't use LwPolyLine(It will cause fault), in this branch, LwPolyLine is the same as PolyLine.  And the test.py is a set of examples.]
    #coding=utf-8

    import sdxf

    '''生成绘图空间'''
    d = sdxf.Drawing()



    '''设置图层名字和颜色'''
    #可以是已有图册
    #在没有设置图层时候，直接添加到一个新名字的图层
    #图层将以默认颜色与新名字来添加新的图层
    #Layers are used to organize entities.
    #Layers can be assigned colors to make drawings easier to read.
    #An entity can be assigned to a new layer on the fly, without explicitly defining the layer first.
    #Layer(name="layer_name",color=3)
    d.layers.append( sdxf.Layer(name="layer1", color=3) )



    '''块blocks'''
    #Blocks are reusable symbols.
    #A block is defined once and can then be appended to the drawing using the Insert entity.
    #A block can be inserted multiple times into a drawing, at different points.
    #命名block
    b = sdxf.Block('test_block')
    #添加元素进入block
    b.append( sdxf.Text('Hello, I am a block!', color = 2 ) )
    #将block添加到绘图空间的block预插入集合中去
    d.blocks.append(b)
    #在绘图空间中使用block
    d.append( sdxf.Insert('test_block', point=(3,3), cols=5, rows=5, colspacing=32, rowspacing=15) )




    '''添加图元'''

    #Text(String, point=)
    '''Text'''
    d.append( sdxf.Text('Hello World!',point=(3,0), layer="layer1") )


    #Line(points = [ point1_tuple, point2_tuple ] )
    '''Line'''
    d.append( sdxf.Line(points=[(0, 0), (1, 1)], layer="layer2") )


    #center (x, y, z) - The center of the circle from which the arc is to be taken. Z is optional.
    #radius - The radius from the center to the arc
    #startAngle - The angle, in degrees, for the start of the arc.
    #endAngle - The angle, in degrees, for the end of the arc
    #Arc(center=(3,0),radius=2,startAngle=0,endAngle=90)
    '''Arc'''
    d.append( sdxf.Arc( (3,0), 5, 45, 90, layer="layer_arc") )


    #Draws a circle
    #center (x,y,z) - the center of the circle. Z is optional.
    #radius - the radius of the circle
    #Circle( center=(3,0), radius=3 )
    '''Circle'''
    d.append( sdxf.Circle( (3,0), 3, layer="layer_arc") )


    #Insert
    #Blocks are added to a file using the Insert entity.
    #The block must be added to the Blocks table before it can be used.
    #name - Block name (defined when the block was added to the Blocks table)
    #point - Insertion point (x,y,z) to add the block
    #xscale - x scale factor; optional, defaults to 1
    #yscale - y scale factor; optional, defaults to 1
    #zscale - z scale factor; optional, defaults to 1
    #cols - column count; optional, defaults to 1
    #colspacing - column spacing; optional, defaults to 0
    #rows - row count; optional, defaults to 1
    #rowspacing - row spacing; optional, defaults to 0
    #rotation - rotation angle; optional, defaults to 0
    #例子详见blocks章节
    '''Insert'''


    #LwPolyLine/PolyLine
    #开源库中的LwPolyLine是错的
    #开源库中的PolyLine是假的
    #对PolyLine进行了重写，然后让LwPolyLine等同于PolyLine
    #建议只使用PolyLine
    #flag为1则是闭合多段线
    #如果不传入flag,flag默认为0
    '''PolyLine'''
    linePoints = [ (0,0), (1,3), (1,0), (2,2) ]
    d.append( sdxf.LwPolyLine( points=linePoints, flag=1, color=3, layer="layer_lw" ) )


    '''保存成dxf格式的文件'''
    #saveas("文件的路径和名称")
    d.saveas( "test.dxf" )

SDXF is a python library for creating lines, circles, and polygons in DXF (Document Exchange Format). DXF is an ASCII format published by AutoCAD and readable by a variety of programs.

The library was created by a guy named [Stani](http://www.stani.be) and appears to be abandoned. I couldn't find any documentation for it so I [wrote my own](http://www.kellbot.com/sdxf-python-library-for-dxf). It's not being actively maintained, but if you've got a bug fix feel free to fork it and submit a pull request.

If anyone wants to put together some test scripts to make sure things don't get broken along the way, please do!

[Stani's DXF library for python][1] is an excellent tool for writing lines, circles, and polygons in DXF format. DXF (Document Exchange Format) is an ASCII file format published by AutoCad which can be read by a variety of programs, including Blender, Maya, CorelDraw, and a [host of others][2].

I'll attempt to document the library here as I figure it out, but am no longer actively working with this library. As of July 2012, it [has a new home on GitHub][1]! If you have an update / bug fix, you can submit a pull request.

[Download SDXF 1.1.1][3]

### Changes in 1.1.1

*   Added support for LWPOLYLINE, so you can now make a continuous polyline instead of several separate lines

## Example Code

This will draw a line from (0,0) to (1,1) and "Hello World" at (3,0)

    import sdxf
    
    d = sdxf.Drawing()
    
    #set the color of the text layer to green
    d.layers.append(sdxf.Layer(name = "textlayer", color=3))
    
    #add drawing elements
    d.append(sdxf.Text('Hello World!',point=(3,0), layer="textlayer"))
    d.append(sdxf.Line(points=[(0, 0), (1, 1)], layer="drawinglayer"))
    
    d.saveas('hello_world.dxf')

## Overview

### Entities

An Entity is an individual drawing object, like a line or circle. These are appended to the Drawing and rendered in the order they're added.

### Layers

Layers are used to organize entities. Layers can be assigned colors to make drawings easier to read. An entity can be assigned to a new layer on the fly, without explicitly defining the layer first.

    Layer(name="mynewlayer",color=8)

### Blocks

Blocks are reusable symbols. A block is defined once and can then be appended to the drawing using the [Insert][4] entity. A block can be inserted multiple times into a drawing, at different points.

    # define the block, a Solid and an Arc
    b = Block('test')
    b.append(Solid(points=[(0,0,0),(1,0,0),(1,1,0),(0,1,0)],color=1))
    b.append(Arc(center=(1,0,0),color=2))
    
    #create a new drawing
    d = Drawing()
    
    #add the block to the Blocks table, so it can be referenced later
    d.blocks.append(b)
    
    #add entities to the drawing, including the block using the Insert entity
    d.append(Circle(center=(1,1,0),color=3))
    d.append(Face(points=[(0,0,0),(1,0,0),(1,1,0),(0,1,0)],color=4))
    d.append(Insert('test',point=(3,3,3),cols=5,colspacing=2))
    d.append(Line(points=[(0,0,0),(1,1,1)]))

## Supported Entities

These entities are currently in the library. In addition to passing the arguments for the individual entity type, you can pass common arguments (group codes) which are available for all entities.

### Common Group Codes

*   color - the color of the entity. Represented by a number. See the [Color List][5] below.
*   extrusion - ?
*   layer - which layer to place the element on. You do not need to explicitly declare a layer before assigning entities to it
*   lineType - ?
*   lineTypeScale - ?
*   thickness - thickness of the entity lines
*   parent - ?

### Arc

Draws an arc (part of a circle).

*   center (x, y, z) - The center of the circle from which the arc is to be taken. Z is optional.
*   radius - The radius from the center to the arc
*   startAngle - The angle, in degrees, for the start of the arc.
*   endAngle - The angle, in degrees, for the end of the arc

    Arc(center=(3,0),radius=2,startAngle=0,endAngle=90)

### Circle

Draws a circle.

*   center (x,y,z) - the center of the circle. Z is optional.
*   radius - the radius of the circle

    Arc(center=(3,0),radius=2)

### Face

Creates a 3d face. A 3d face takes 4 points, which may or may not all be on the same plane.

### Insert

Blocks are added to a file using the Insert entity. The block must be added to the Blocks table before it can be used.

*   name - Block name (defined when the block was added to the Blocks table)
*   point - Insertion point (x,y,z) to add the block
*   xscale - x scale factor; optional, defaults to 1
*   yscale - y scale factor; optional, defaults to 1
*   zscale - z scale factor; optional, defaults to 1
*   cols - column count; optional, defaults to 1
*   colspacing - column spacing; optional, defaults to 0
*   rows - row count; optional, defaults to 1
*   rowspacing - row spacing; optional, defaults to 0
*   rotation - rotation angle; optional, defaults to 0

    Insert('test',point=(3,3,3),cols=5,colspacing=2)

### Line

Makes a line! Takes a list containing two points. Points can be (x,y) or (x,y,z)

    Line(points=[(0,0),(1,1)])

### LwPolyLine

Makes a line with vertexes. Takes a list of points.

    linePoints = [(0,0),(1,1),(1,0)]
    LwPolyLine(points=linePoints,flag=1)

### Polyline

In the current implementation, actually just makes a bunch of Lines rather than a polyline.

### Point

A point in space. Takes a point (duh).

### Solid

From what I can tell, this creates a 3D solid by taking either 3 or 4 points and then extruding the face in various directions.

### Text

Renders a string as text at the given point.

    Text('Hello World!',point=(3,0))

### Mtext

I think this is like Text but supports line breaks. In this version of SDXF it just creates multiple Text entities.

## Extras

These are not actual DXF entities, but are classes that make building other shapes easier.

### Rectangle

Creates a rectangle using 4 lines. Could probably be modified to use LwPolyLine instead.

*   point - lower left (?) corner point
*   width - rectangle width
*   height - rectangle height
*   solid - ?
*   line - ?

### Line List

Creates a bunch of lines from a set of points. Currently used instead of PolyLine.

*   points - list of verticies
*   closed - whether to close the shape; defaults to 0

## Color List

The colors may vary depending on the rendering software used.  
1 - Red  
2 - Yellow  
3 - Green  
4 - Cyan  
5 - Blue  
6 - Magenta  
7 - White  
8 - Black

 [1]: https://github.com/nycresistor/SDXF
 [2]: http://en.wikipedia.org/wiki/AutoCAD_DXF#Software_which_supports_DXF
 [3]: https://github.com/downloads/nycresistor/SDXF/sdxf.py
 [4]: http://www.kellbot.com/sdxf-python-library-for-dxf/#entity_insert
 [5]: http://www.kellbot.com/sdxf-python-library-for-dxf/#color_list
