| [Tutorials Home](Tutorials.md)    | [Previous](Configuration.md) | [Next](LinkingProjects.md) |
| ------------- |:-------------:| -----:|
# Importing and Exporting Point Clouds
######Latest update April 2, 2014 by Samuel Charreyron

## Overview
There exists a myriad of [graphics file formats](http://en.wikipedia.org/wiki/Category:Graphics_file_formats) which can be used to represent point clouds.  Nevertheless most contain superfluous functionality which isn't necessary for ICP.  Consequently, Pointmatcher only supports a small number of file formats which are detailed in the following sections.

## Table of Supported File Formats
| File Type | Extension | Versions Supported | Descriptors Supported | Additional Information |
| --------- |:---------:|:------------------:|:---------------------:|---------|
| Comma Separated Values | .csv | NA | yes (see table of descriptor labels) | |
| Visualization Toolkit Files | .vtk | Legacy format versions 3.0 and lower (ASCII only) | yes | Only polydata and unstructured grid VTK Datatypes supported.  More information can be found  [here](http://www.vtk.org/VTK/img/file-formats.pdf).|
| Polygon File Format | .ply | 1.0 (ASCII only) | yes (see table of descriptor labels) | | 

## Comma Separated Values (CSV) Files
The most simple file format supported to store clouds is a plain text file containing comma separated values.  Data is structured in a table format and is separated into rows or lines and columns.  Columns are delimited by commas, tabs, or semicolons.  2D and 3D point features are supported as well as a limited number of point descriptors.

It is common and best practice that the first line of the CSV file be a header identifying each data column.  Features are identified by the following names: "x", "y", and "z" if the point cloud in 3D.  Descriptors are divided into columns and these must be appropriately identified.  

If the first line does not include a header, the user will be asked to identify which columns correspond to the "x", "y", and "z" feature fields.  Any additional content including descriptors is ignored.

## Visualization Toolkit (VTK Legacy) Files
So as to allow the visualization of point clouds in [Paraview](http://www.paraview.org/), Pointmatcher supports importing and exporting of Paraview's native VTK format.  VTK files can be used to represent a wide variety of 3D graphics including point clouds.  The VTK standard comprises of a simpler legacy format and a newer XML based format.  As of now Pointmatcher only supports the legacy format.

VTK files can contain different geometrical topologies which are represented in different dataset types.  These types each have different file structures and can be one of the following :
    
* STRUCTURED_POINTS
* STRUCTURE_GRID
* UNSTRUCTURED_GRID
* POLYDATA
* RECTILINEAR_GRID
* FIELD

As of writing, Pointmatcher only supports the POLYDATA (used to represent polygonal data) and UNSTRUCTURED_GRID (used to describe any unstructured combination of geometrical cells) types.  While VTK files can be encoded in plain text (ASCII) or binary, only ASCII files are supported.

Descriptors are encoded in VTK files as dataset attributes which can be any of the following all of which are supported in Pointmatcher:

* SCALARS
* COLOR_SCALARS
* VECTORS
* NORMALS
* TENSORS 

Data contained in these attributes are loaded into the Datapoints feature matrices.  In the same way, Dataset descriptors are converted to the appropriate VTK attributes when exporting a point cloud.  The following table details which descriptors are exported to VTK files and the corresponding VTK attribute that is used to encode them.

*Mapping Between Pointmatcher Descriptors and VTK attributes*

| Pointmatcher Descriptor Label | VTK Dataset Attribute |
| ----------------------------- | --------------------- | 
| normals                       | NORMALS               |
| eigVectors                    | TENSOR                | 
| color                         | COLOR_SCALARS         | 
| Any other 1D descriptor       | SCALARS               |
| Any other 3D descriptor       | VECTORS               |  

## Polygon File Format (PLY or Stanford Triangle) Files
The PLY file format was developed at the Stanford Graphics Lab for storing 3D graphics in a relatively straight-forward fashion.  PLY files are flexible and data is structured by defining elements and properties.  Elements usually represent some geometrical construct such as a vertex or triangular face.  Elements are associated to scalar properties such as the a x, y, z coordinate in the case of vertices.  Properties can contain any information however including colors, normal vector components, densities etc...

PLY files contain a header section at the top of the file which defines the elements and properties that are used in the file.  The rest of the file contains numerical data.  The PLY format exists in plain text (ASCII) and in binary, however Pointmatcher only supports the plain text version.

The PLY format does not prescribe labels to elements or properties, and therefore files must be encoded with appropriate labels in order to be read by Pointmatcher.  For information on which properties are supported by Pointmatcher, refer to the table in the next section.

## Descriptor Property Identifiers (VTK and CSV)
| Property Label | Description | Feature or Descriptor | Pointmatcher Descriptor Label |
| -------------- | -------------| --------------------- | ---------------- |
| x              | x component of point | feature               |   NA        |
| y              | y component of point | feature               |     NA      |
| z              | z component of  point | feature               |      NA     |
| nx             | x component of normal vector at point | descriptor            | normals          |
| ny             | y component of normal vector at point | descriptor            | normals          |
| nz             | z component of normal vector at point | descriptor            | normals          |
| normal_x             | x component of normal vector at point | descriptor            | normals          |
| normal_y             | y component of normal vector at point | descriptor            | normals          |
| normal_z             | z component of normal vector at point | descriptor            | normals          |
| red            | red value (0-255) of RGB color code | descriptor |  color |
| green          | green value (0-255) of RGB color code | descriptor |  color |
| blue           | blue value (0-255) of RGB color code | descriptor |  color |
| intensity    | laser scan intensity at point | descriptor | intensity |
| eigValues       | eigen value of nearest neighbors at point | descriptor | eigValue |
| eigVectorsX       | x component of eigen vector of nearest neighbors at point | descriptor | eigVectors |
| eigVectorsY      | y component of eigen vector of nearest neighbors at point | descriptor | eigVectors |
| eigVectorsZ      | z component of eigen vector of nearest neighbors at point | descriptor | eigVectors |
| densities | point density at point | descriptor | densities |
   