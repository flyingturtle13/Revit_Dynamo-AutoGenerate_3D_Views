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
2. Using one of the example models (vertical multi-story structure and long horizontal, multi-zone structure) in Example Models folder, open the Revit file and associated Excel file.
3. In the Excel file, update the view names to desired naming convention in the View_Name sheet.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81877144-39959400-9539-11ea-831f-d3a66e83344d.png" width="400">
   </p>
4. Still in Excel file, update coordinates with desired values for X-Min, X-Max, Z-Min, X-Max, Y-Max, Z-Max in respective sheets corresponding row in View_Name sheet. User will most likely need to refere to the model plan views to take measurements.
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81876408-7791b880-9537-11ea-968a-44b57677f09a.png" width="400">
   </p>
      Note: </br>
      (1) In this case, the project units are in feet so enter values for decimal feet.</br>
      (2) Coordinates to be inputted are based on project Inernal Origin so make sure to locate Internal Origin using Project Base Point or Survey Point.
       <p align="center">
        <img src="https://user-images.githubusercontent.com/44215479/81876274-1a960280-9537-11ea-8178-fbaf0e0232ea.png" width="600">
       </p>
      (3) Hint: use elevation views as reference to set Z-coordinate value for minimums and maximums
       <p align="center">
        <img src="https://user-images.githubusercontent.com/44215479/81877677-8f1e7080-953a-11ea-8eb2-9d5839776f00.png" width="400">
       </p>
5. After Excel file setup is complete, open the Dynamo add-in in Revit and select Dynamo script included here.
3. Ensure Clockwork and Rhythm packages are installed. If not, use Package --> Search for Package --> Clockwork or Rhtyhm in Dynamo.
4. As described in ***Dynamo Script Layout 1.***, use 'File Path' node to select included Excel file (*Project02Example-ViewsCreation-User_Input.xlsx*) or file where modifications were done in (2.). Also select a base 3D view in 'Views' node per ***Dynamo Script Layout 1.***. </br>
   **Note:**
   1. View properties in base 3D view will persist in duplicated 3D views.</br>
6. Select *Run* and check Revit for results.</br> 
   <p align="center">
    <img src="https://user-images.githubusercontent.com/44215479/81877300-88dbc480-9539-11ea-8b61-66fbeef9e7a3.png" width="800">
   </p> 

