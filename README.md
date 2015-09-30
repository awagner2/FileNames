# GPS_Workflow
Daily GPS Workflow
Documents GPS synchronization process for CDOT's GIS Data Management Unit.

A.	Incorporate GPS updates.

1.	Copy the ArcGIS Mobile Project onto the local work station.
2.	Run the Synchronize Mobile Cache tool, selecting Upload Only and the relevant feature class(es).
3.	All new updates will have a recent date field. Sort by date to find the most recent updates.
4.	Perform QA on the updated records. If any changes were made on the local workstation, recreate the mobile cache for your GPS unit because any future synchronization with the GPS unit will write over your edits.
5.	Optional: If you made edits but don’t have the chance to recreate the mobile cache, you should save your edits to a master copy of the feature class and only copy over records that have been newly updated from the synchronized feature class daily. I used ModelBuilder to Join tables, Select newly updated records, and Calculate the fields from synced FC to master FC. 

B.	Handling Photos: Reattach photos properly when attached with a name like img_123. 
This causes a problem because when photos are downloaded, they need to be identifiable by their RampID. So attachments need to be downloaded and reattached.

1.	Check whether the photo attachments were synced along with the data. (This function does not always run smoothly.) 2.	Prepare the ADACurbRamps_ATTACH table by creating a field called FILENAME and filling it in with what the attachment needs to be called. I joined with the feature class and performed a field calculation like [FILENAME] = RAMPID + “.jpg”. You can adjust and run PrepareAttachments.py.
3.	Export the attachments you need (all or select only those with a FILENAME). Specify the ATTACH table you are pulling from and an empty folder to put the renamed photos in. Files will be named according to the FILENAME value. You can adjust and run ExportAttachments.py. After this, delete the FILENAME field.
4.	Add the attachments now that they are named according to the RAMPID. Use the following code to enable attachments, create a match table, and add attachments. You can adjust and run AddAttachments.py.
5.	Cut images from the temporary folder over to the folder where all photos are stored. If there is a duplicate, skip copying it. For duplicates, if they are the same image, keep only one. If both images are needed, rename one of them like “RAMPID II.jpg”.
6.	Since I’m keeping a separate (master) copy of the ADACurbRamps feature class than the one that gets synced up daily, I don’t have to do this step. Delete the old “img_123” attachments. If there are records with more than one attachment check which or both to keep.

C.	Handling Photos: when they didn’t sync

1.	If photos didn’t sync, copy the photos from the mobile project to a folder on the desktop.
2.	Photos are likely named by their GUIDs so they can easily be given a RampID name instead. 
3.	Pull the GUIDS and RampIDs from the newly updated records into a file called RenameFile.txt.
4.	Rename the photos from the GUID to the RampID using RenameFiles.py.
5.	Starting at step B-4, add the photos as attachments to the geodatabase.
