# Lead-Capture-Automation-n8n

## Overview

This n8n workflow automates the intake of user submissions via a form and routes the data based on the selected **Occupation**. It integrates with **Google Sheets** and **Gmail**, performing the following:

1. Accepts a form submission.
2. Appends or updates the user data in a Google Sheet.
3. Filters out students and proceeds with non-student data.
4. Uses a **Switch** node to branch logic based on the `Occupation` field.
5. Sends a personalized email based on the occupation.
6. Merges results and notifies a lead via Gmail.

---

## ⚙️ Nodes Summary

### 1. **Form Trigger**: `On form submission`
- Displays a form with fields: First Name, Last Name, Email, Occupation
- Occupation options: *Student, Engineer, Retired*
- Starts the workflow when submitted.

### 2. **Google Sheets Node**: `Append or update row in sheet`
- Appends or updates a row in a connected Google Sheet.
- Uses "Email" as the matching key to prevent duplicates.
- Columns mapped: First Name, Last Name, Email, Occupation

### 3. **Filter Node**: `Filter`
- Filters out any user with `Occupation = Student`
- Only non-students proceed further.

### 4. **Switch Node**: `Switch`
- Checks the value of `Occupation`
  - If **Engineer** → routes to Engineer email
  - If **Doctor** → routes to Doctor email (note: "Doctor" isn't listed in form options, but logic allows it)

### 5. **Gmail Node**: `Send a message`
- Sends an email: `"Engineer Lead"` to `test2@gmail.com`  
  (when Occupation = Engineer)

### 6. **Gmail Node**: `Send a message1`
- Sends an email: `"Doctor Lead"` to `test2@gmail.com`  
  (when Occupation = Doctor)

### 7. **Merge Node**
- Merges the output from the two conditional email branches

### 8. **Gmail Node**: `Send a message2`
- Sends a summary/lead notification email to `testlead@gmail.com`:
  - Subject: `"New Lead"`
  - Message: `"You have a new lead, messaged"`

 ## 🧠 Logic Flow Diagram

Form Submission
      ↓
Append/Update Google Sheet
      ↓
  Filter (≠ Student)
      ↓
   Switch (Engineer / Doctor)
     ↓             ↓
Engineer Email   Doctor Email
     ↓             ↓
        → Merge →
             ↓
     Send Final Notification



---

## 🔐 Credentials Used

- **Google Sheets** (OAuth2)
- **Gmail** (OAuth2)

> Make sure these credentials are configured and active in your n8n instance.

---

## 📌 Notes

- The form provides "Student", "Engineer", and "Retired", but the logic also checks for "Doctor".
- The final email consolidates branches and notifies a central lead address.



