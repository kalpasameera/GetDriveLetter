# GetDriveLetter
Get drive letter when USB drive connect to the PC.


// Assuming 'EditForm' submission is successful
SubmitForm(EditForm);
ForAll(
    EditForm.LastSubmit.'Department Code',
    Collect(
        TempCollection,
        Filter(
            DepartmentMaster,
            'Department Code'.Value = Value
        )
    )
);

// Step 2: Filter by Job Title using the previously filtered collection
ClearCollect(
    LocalDepartmentMaster,
    Filter(
        TempCollection,
        JobTitle.Value in EditForm.LastSubmit.'Job Title'.Value
    )
);

ForAll(
    LocalDepartmentMaster,
    Collect(
        TasksStatus,
        {
            'Emplyee Code': {
                '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedReference",
                Id: ID,  // ID of the DepartmentMaster item
                Value: EmployeeCode  // Assuming EmployeeCode is a simple text field, adjust if it's another lookup
            },
            'Task Id': {
                '@odata.type': "#Microsoft.Azure.Connectors.SharePoint.SPListExpandedReference",
                ID: EditForm.LastSubmit.ID,
                Value: EditForm.LastSubmit.Title // ID of the newly created task
            }  // Default status or dynamically set based on some other criteria
        }
    )
);
Clear(TempCollection);
Clear(LocalDepartmentMaster)
