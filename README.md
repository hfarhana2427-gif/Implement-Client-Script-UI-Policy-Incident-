Project Overview
This project demonstrates how to implement Client Scripts and UI Policies on the Incident table in ServiceNow to improve form behavior, enforce field requirements, and provide a better user experience.

Client Scripts execute JavaScript on the client side, while UI Policies dynamically control field visibility, mandatory status, and read-only behavior based on specified conditions.

Objectives
Implement Client Scripts on the Incident table.
Make fields mandatory based on Incident priority.
Validate required fields before form submission.
Display informational messages when a Caller is selected.
Configure UI Policies to control field behavior dynamically.
Improve data quality and the Incident management process.
Technologies Used
Platform: ServiceNow
Module: Incident Management
Scripting Language: JavaScript
Features: Client Scripts and UI Policies
Project Implementation
1. Client Script – Priority Validation
Type: onChange
Table: Incident
Field: Priority

Requirement: Make the Short Description field mandatory when the Incident priority is Critical (1).

function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading) {
        return;
    }

    if (newValue == '1') {
        g_form.setMandatory('short_description', true);
    } else {
        g_form.setMandatory('short_description', false);
    }
}
Expected Result: Short Description becomes mandatory when Priority is Critical.

2. Client Script – Resolution Notes Validation
Type: onSubmit
Table: Incident

Requirement: Prevent submission when an Incident is being resolved without Resolution Notes.

function onSubmit() {
    var state = g_form.getValue('state');
    var notes = g_form.getValue('close_notes');

    if (state == '6' && notes.trim() == '') {
        g_form.addErrorMessage(
            'Enter Resolution Notes before resolving the Incident.'
        );

        g_form.setFocus('close_notes');
        return false;
    }

    return true;
}
Expected Result: The form cannot be submitted as Resolved until Resolution Notes are entered.

Note: Verify that the Resolved state value is 6 in your instance.

3. Client Script – Caller Information
Type: onChange
Table: Incident
Field: Caller (caller_id)

Requirement: Display an informational message when a Caller is selected.

function onChange(control, oldValue, newValue, isLoading) {
    if (isLoading || newValue == '') {
        return;
    }

    var callerName = g_form.getDisplayValue('caller_id');

    g_form.addInfoMessage(
        'Selected Caller: ' + callerName
    );
}
Expected Result: A message displays the selected Caller’s name.

UI Policy Implementation
1. Require Resolution Notes
Table: Incident

Condition: State is Resolved

Configuration:

Enable the UI Policy.
Enable On load.
Enable Reverse if false.
Add a UI Policy Action for Close notes.
Set Mandatory to True.
Leave Visible and Read-only unchanged.
Expected Result: Resolution Notes become mandatory when the Incident is Resolved and the policy reverses its setting when the condition is false.

2. Hide Assignment Group
Table: Incident

Condition: State is New

Configuration:

Enable the UI Policy.
Enable Reverse if false.
Add a UI Policy Action for Assignment group.
Set Visible to False.
Leave Mandatory and Read-only unchanged.
Expected Result: Assignment Group is hidden while the Incident state is New and becomes visible when the condition is false.

Note: This policy applies to all records matching the New state condition, including saved records.

Steps to Configure in ServiceNow
Log in to your ServiceNow instance.
Navigate to System Definition → Client Scripts.
Click New and select the Incident table.
Create each Client Script as a separate record.
Select the appropriate script type and field.
Add the corresponding JavaScript code.
Navigate to System UI → UI Policies.
Create the required UI Policies for the Incident table.
Configure their conditions and UI Policy Actions.
Save the configurations and test them on an Incident form.
Testing
Test Case	Expected Result
Set Priority to Critical	Short Description becomes mandatory
Set Priority to a non-critical value	Mandatory setting is removed by the script
Resolve an Incident without Resolution Notes	Submission is blocked
Select a Caller	Caller information message appears
Set State to Resolved	Resolution Notes become mandatory
Set State to New	Assignment Group is hidden
Best Practices
Use UI Policies for straightforward field visibility, mandatory, and read-only requirements.
Use Client Scripts for custom client-side logic and form submission validation.
Create separate Client Script records for different script types and field events.
Verify field names and choice values in your ServiceNow instance.
Test scripts for both new and existing Incident records.
Use server-side validation when a rule must be enforced across all record creation and update methods.
Conclusion
This project demonstrates the practical implementation of Client Scripts and UI Policies in ServiceNow Incident Management. These configurations improve form usability, support data quality, and help guide users through the Incident resolution process.

Author
ServiceNow Developer

Project Status
documentation link:
https://drive.google.com/file/d/1iem23fDqanZ4YqanrCOc5vQM53h_NJqG/view?usp=sharing
demo link:
https://drive.google.com/file/d/1i97CPqProspYvR2xSLy28WEZUMNGQhSw/view?usp=sharing


Completed — sample implementation and configuration guide.
