# Revit Dynamo - Auto-Generate 3D Views
When creating 3D views in Revit, it becomes a tedious task when many have to be created due to multi-level or multi-sectioning structures.  This Dynamo script can auto-generate multiple views while auto naming and pre-setting the Section Box using the provided Excel file.  User must do most of the work in the spreadsheet and create a base 3D view in the Revit project for the script to duplicate and modify.  An example is covered in the Running Script & User Implementation Instructions section.  This workflow is most beneficial when levels or zones per project require more than than 10 to 20.  

**Note:**
1. The Dynamo script (*ProjectSetup-AutoGenerate_3D_Views.dyn*) and accompanying Excel file (*ProjectName-ViewsCreation-User_Input.xlsx*) are included.
2. Example Revit models and Excel file are included to test the script.

## Getting Started
Environment setup regarding script development logistics.

* IDE:
  * Revit Dynamo

* Language:
  * Visual Programming / Python for Custom Node Creation

* Output File Type:
  * dyn

* Additional Library Packages Implemented: </br>
  * Clockwork
  * Rhythm

## Dynamo Script Development & Structure
#### Script features and specs.
* Software Required
  - Revit (any version with Dynamo 2.1 and above installed)
    
* User Interface
  - Run script through Dynamo add-in

* Setup Requirements and Capabilities
  - User to create a default 3D view in the Revit project.  The script uses this view to duplicate and modify based on user preferences.
  - User to modify the included Excel spreadsheet per project.
    - View_Name sheet serves as source for script to name all the views to be created (script will determine number of views by counting number of rows in the sheet)
    - X-Min and Y-Min serve as the coordinates to set the bottom left when looking in plan of the Section Box in the associated view (by row in Excel)
    - X-Max and Y-Max serve as the coordinates to set the upper right when looking in plan of the Section Box in the associated view (by row in Excel)
    - Z-Min and Z-Max serve as the coordinates setting the bottom and top plane of the Section Box in elevation
    - **It is important to note that all coordinates inputted have to be float (decimal units) and positive and negative coordinates are based on internal project origin**
  
#### Dynamo Script Layout
Overall graph view
  <p align="center">
   <img src="https://user-images.githubusercontent.com/44215479/81870528-e0266880-952a-11ea-8b0f-02bbc5366c6a.png" width="1200">
  </p>

1. User Input
   * Before running script, user to select or check correct target Excel file is selected for importing required data
   * User to also select 3D view from the Revit project to use as a base to duplicate and modify per Excel file inputs
     - View templates, element visibility, etc. are respected when duplicating the view (similar to manually doing it)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81871671-241a6d00-952d-11ea-8cdb-ce1e2722f08e.png" width="600">
   </p>

2. Create Views Backend Process
   * This process (List.Cycle node) takes the base 3D view and determines how many times to iterate over view by counting number of rows in the View_Name sheet (All sheets in Excel file should have same number of rows with data)
   * Then the View.Duplicate node creates a copy of the base 3D view and renames based on Excel file input
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81872005-c9354580-952d-11ea-83cf-35adc9894603.png" width="600">
   </p>

3. Section Box Dimensions Backend Process
   * The X,Y,Z minimum and maximum specified values are imported from the Excel file and used to generate a bounding box
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81872423-8de74680-952e-11ea-9f2c-23ffc1c34732.png" width="600">
   </p>

4. New 3D View with Adjusted Section Box Created in Revit Project
   * View3D.SetSectionBox takes the newly created, custom named 3D view and modifies the Section Box using output from (3.).
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81872739-32698880-952f-11ea-8249-9584ac353a9f.png" width="600">
   </p>
  
#### Mapping Script to UI
1. Dynamo Script Layout 2. - autoamates having to right-click and selecting duplicate view (To reiterate, important for user to select correct 3D view to duplicate as it respects the view properties)
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81873392-8d4faf80-9530-11ea-8600-c918b950210d.png" width="600">
   </p>

2. Dynamo Script Layout 2. - In the script, the view name is taken from the associated Excel file sheet row and applied to 3D view
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81873584-e881a200-9530-11ea-90b7-69aa8fff09e2.png" width="600">
   </p>
   
3. Dynamo Script Layout 3 and Dynamo Script Layout 4. - The script helps the most in the Section Box adjustment process.  In the Revit model, the section box is requires a few iterations to set it as desired.  With the script it sets it based on coordinates specified.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81873727-35657880-9531-11ea-8873-c7deabeaa99b.png" width="800">
   </p>
   
## Running Script & User Implementation Instructions
1. Clone or download project. </br>
2. In own Revit project or Pile Cap Location & Offset Export example in Model Example folder, open Dynamo and open *Structural-Pile_Cap_Location_And_Offset.dyn*.
3. Ensure Clockwork package is installed. If not, use Package --> Search for Package --> Clockwork in Dynamo.
4. As described in ***Dynamo Script Layout 1.***, use 'File Path' node to select included Excel file (*Pile_Cap_Schedule-Dynamo.xlsx*) and select a reference level in *Level* node (select level pile caps are associated to in model).
5. Modify or add search 'String' node(s) for pile cap foundations and column types as described in ***Dynamo Script layout 2.*** and ***Dynamo Script layout 4.***
6. Select *Run* and check Excel spreadsheet output.</br>
   a. Raw output from Dynamo script specified sheet in User Input.
      <p align="center">
       <img src="https://user-images.githubusercontent.com/44215479/81785613-58e9de00-94b3-11ea-9017-8a953f7dd090.png" width="600">
      </p>
   b. Auto-generated values for distribution and for structural engineer to add loads at top of pile caps.
      <p align="center">
       <img src="https://user-images.githubusercontent.com/44215479/81785906-c5fd7380-94b3-11ea-8c5f-207235d3d3bc.png" width="1000">
      </p>
   c. QC check for correct number of pile caps and types being reported for quantity takeoff.
      <p align="center">
       <img src="https://user-images.githubusercontent.com/44215479/81786055-0826b500-94b4-11ea-8761-f2781384420b.png" width="600">
      </p>

