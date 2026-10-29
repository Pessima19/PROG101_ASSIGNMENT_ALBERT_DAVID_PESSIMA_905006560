# PROG101_ASSIGNMENT_ALBERT_DAVID_PESSIMA_905006560
This is assignment 1 of Programming Logic and Design about community water supply monitoring system


 PROBLEM DESCRIPTION

In many communities in Sierra Leone, people have problem with water supply. The taps and boreholes and wells sometimes stop working and nobody knows on time. Water may be dirty or not flowing. Sometimes water pump is broken for many days and people have to walk far to find clean water. There is no system to monitor which water point is working and which one is not.


BACKGROUND

Water is very important for life. Without clean water, people get sick with cholera, typhoid and diarrhea. In Freetown and in villages like Port Loko, Bombali and Koinadugu, Guma Valley and SALWACO try to supply water but it is difficult to know the status of all water points every day. People call on phone or go to office to report, but this is slow. So we need a simple computer system that can help monitor water supply.

The idea of this system is to allow community members to report water problems using phone, and for the water company to see the report and fix it quickly.


 COMMUNITY IMPACT

This system will help the community a lot:

People will get clean water faster
It will reduce sickness from dirty water
It will save time — no need to walk far for water
Water company will know which area has problem
It will help government to plan better


WHO IS AFFECT?

Women, children, schools, clinics — because they are the ones who fetch water every day.


WHY THIS PROBLEM EXIST?

No monitoring system, broken pipes are reported late, no record keeping, and no central database for water points.How my solution addresses it: My system will use simple logic — Input (report water problem), Processing (check location and problem type), Output (send alert to technician). It will use decision structures (if water is dirty, do this) and repetition (check all water points every day) and modules (Login, Report, Monitor, Alert).


SDG LINK

This project supports SDG 6: Clean Water and Sanitation — to ensure everyone has clean water.

4. ALGORITHM DESIGN
System Name: Community Water Supply Monitoring System
Problem: No system to monitor broken water points in communities
4.1 Problem Analysis
Input: reporterContact, community, facilityLocation, faultType, description, reportIDProcess: Validate input, Check duplicate, Assign Priority (HIGH/MEDIUM/LOW), Send Alert, Count ReportsOutput: ReportID, Status, Priority, Summary ReportData Store: waterReportDatabase


 Main Algorithm

javascriptALGORITHM CommunityWaterSupplyMonitoringSystem

1. START
2. DISPLAY "Community Water Supply Monitoring System"
3. CALL LoginModule()
4. IF login is invalid THEN EXIT
5. REPEAT
     DISPLAY MENU: 1.Report 2.Priority Review 3.Report Summary 4.Exit
     INPUT choice
     IF choice = 1 THEN CALL ReportRegistration()
     ELSE IF choice = 2 THEN CALL PriorityReview()
     ELSE IF choice = 3 THEN CALL ReportSummary()
   UNTIL choice = 4
6. END



ALGORITHM DESIGN


 Algorithm for Submitting a Report
 Start.
 Display the reporting form.
 Input the reporter's necessary contact details.
 Input community, facility/location, fault category, and description.
 Check whether all required fields are complete.
 If required information is missing, display an error and request correction.
 Validate the category and location information.
 If the information is invalid, request correction.
 Check whether a similar unresolved report exists for the same facility and fault.
 If a possible duplicate exists, show it to an authorized reviewer or offer the reporter the existing reference number where appropriate.
 If no duplicate is confirmed, generate a unique reference number.
 Save the report with status Pending Review.
 Display the reference number and confirmation.
 End.


ALGORITHM FOR REVIEWING AND UPDATING A REPORT


Start.
Verify that the user is an authorized officer.
Retrieve reports awaiting review.
If no reports are awaiting review, display a message.
Otherwise, display the reports and their locations.
Officer selects a report and reviews the details.
Apply defined urgency criteria and assign follow-up.
Update status to Assigned or In Progress when appropriate.
Save the update and record who made it.
When action is verified, update the status to Resolved.
Close the report according to the local procedure.
Repeat while more reports need review.
End.


ALGORITHM FOR GENERATING A SUMMARY

Start.
Retrieve report records.
Set counters for Pending Review, Assigned, In Progress, Resolved, and Closed to zero.
Examine each report.
Increase the counter matching the report's status.
Repeat until all reports have been examined.
Display the total number of reports in each status.
Optionally group reports by community or fault category.



PSEUDOCODE

BEGIN
    DISPLAY "COMMUNITY WATER SUPPLY MONITORING SYSTEM"
    INPUT reporterContact, community, facilityLocation, faultType, description

    WHILE community IS EMPTY OR facilityLocation IS EMPTY 
          OR faultType IS EMPTY OR description IS EMPTY DO
        DISPLAY "Required information is missing."
        INPUT community, facilityLocation, faultType, description
    END WHILE

    IF faultType IS VALID AND facilityLocation IS VALID THEN
        possibleDuplicate = CALL CheckDuplicate(facilityLocation, faultType)
        
        IF possibleDuplicate = TRUE THEN
            DISPLAY "A similar report may already exist."
            DISPLAY "Please review the existing report before creating another."
        ELSE
            reportID = CALL GenerateUniqueID()
            status = "Pending Review"
            SAVE reportID, reporterContact, community, 
                 facilityLocation, faultType, description, status TO database
            DISPLAY "Report submitted successfully."
            DISPLAY "Report ID: ", reportID
            DISPLAY "Status: ", status
        END IF
    ELSE
        DISPLAY "Invalid information. Please correct it."
    END IF
END




PRIORITY REVIEW

BEGIN
    INPUT reportID
    report = CALL FindReport(reportID)

    IF report = NOT_FOUND THEN
        DISPLAY "Report not found"
        EXIT
    END IF

    IF report.faultType = "No Water" OR report.faultType = "Dirty Water" 
       OR report.faultType = "Broken Pump" THEN
        priority = "HIGH"
        CALL SendAlert(report.community)
        DISPLAY "HIGH PRIORITY - Alert Sent"
    ELSE IF report.faultType = "Minor Leakage" THEN
        priority = "MEDIUM"
    ELSE
        priority = "LOW"
    END IF

    UPDATE report WITH priority
    SAVE report TO database
END




REPORT SUMMARY 
BEGIN - Report Summary Module

    DISPLAY "COMMUNITY WATER SUPPLY MONITORING SYSTEM - REPORT SUMMARY"
    
    // Initialize counters
    SET totalReports = 0
    SET highPriorityCount = 0
    SET mediumPriorityCount = 0
    SET lowPriorityCount = 0
    SET pendingCount = 0
    SET resolvedCount = 0
    SET inProgressCount = 0

    // Check if database is empty - Decision
    IF database IS EMPTY THEN
        DISPLAY "No reports deposited yet."
        EXIT
    END IF

    // Repetition - Loop through all reports
    FOR EACH report IN database DO
        totalReports = totalReports + 1

        // Decision 1: Count by Priority
        IF report.priority = "HIGH" THEN
            highPriorityCount = highPriorityCount + 1
        ELSE IF report.priority = "MEDIUM" THEN
            mediumPriorityCount = mediumPriorityCount + 1
        ELSE IF report.priority = "LOW" THEN
            lowPriorityCount = lowPriorityCount + 1
        END IF

        // Decision 2: Count by Status
        IF report.status = "Pending Review" THEN
            pendingCount = pendingCount + 1
        ELSE IF report.status = "In Progress" THEN
            inProgressCount = inProgressCount + 1
        ELSE IF report.status = "Resolved" THEN
            resolvedCount = resolvedCount + 1
        END IF

    END FOR

    // Calculate
    SET resolutionRate = (resolvedCount / totalReports) * 100

    // Display Final Summary
    DISPLAY "----------------------------------------"
    DISPLAY "Total Reports Deposited: ", totalReports
    DISPLAY "----------------------------------------"
    DISPLAY "By Priority:"
    DISPLAY "  - HIGH Priority: ", highPriorityCount
    DISPLAY "  - MEDIUM Priority: ", mediumPriorityCount
    DISPLAY "  - LOW Priority: ", lowPriorityCount
    DISPLAY "----------------------------------------"
    DISPLAY "By Status:"
    DISPLAY "  - Pending Review: ", pendingCount
    DISPLAY "  - In Progress: ", inProgressCount
    DISPLAY "  - Resolved: ", resolvedCount
    DISPLAY "----------------------------------------"
    DISPLAY "Resolution Rate: ", resolutionRate, "%"
    DISPLAY "----------------------------------------"

    IF resolutionRate < 50 THEN
        DISPLAY "Warning: Low resolution rate - Need more technicians"
    ELSE
        DISPLAY "Performance: Good"
    END IF

END


FLOWCHART

<img width="1024" height="1536" alt="05E37EBC-73DC-4420-99A9-F8BC3FE6EB62" src="https://github.com/user-attachments/assets/200a3786-4623-40ab-ba4b-3a9c4ec9a526" />
