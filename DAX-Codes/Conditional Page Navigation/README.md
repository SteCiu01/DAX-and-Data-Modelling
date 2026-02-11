# Conditional Page Navigation

Restrict navigation to specific report pages for non-authorised users and redirect them to an access request page.

> **Note**
> Power BI does not provide native Page Level Security.
> The approach described below implements **conditional page navigation**, not true security enforcement.

---

#### Scenario

We have a Power BI report with two pages:

* **Operational Page** – accessible to all users
* **Executive Page** – contains sensitive information and must be accessible only to management

The requirement is to prevent non-authorized users from navigating to the Executive Page and redirect them to a dedicated access request page.

---

#### Solution Overview

Page access is controlled by dynamically driving navigation based on the user’s membership in an Azure AD security group.

---

#### Step 1 – Azure AD Security Group

Create an Azure AD security group, for example: Security_AzGroup_Executives

Only users who are authorized to view the Executive Page should be members of this group.
Group membership is managed centrally in Azure AD.

---

#### Step 2 – Import Group Membership into Power BI

Using Power Automate:

1. Retrieve the members of the Azure AD security group
2. Export the list to a CSV file
3. Store the file in a SharePoint location
4. Load the CSV into Power BI on a scheduled refresh

Name the table: Dim_Security_AzGroup_Executives

Mandatory column: userPrincipalName (email address)

---

#### Step 3 – Request Access Page

Create a Power BI page called: Request Access

This page is shown to users who are not authorized to access the Executive Page.

Typical content:

* Explanation of restricted access
* Instructions or links to request permissions
* Ownership or support contact details

---

#### Step 4 – Access Control Measure

Create the following DAX measure:

```
Executive Access =
IF (
    USERPRINCIPALNAME()
        IN VALUES ( 'Dim_Security_AzGroup_Executives'[userPrincipalName] ),
    "Executive Page",
    "Request Access"
)
```

The measure evaluates whether the current user belongs to the executive security group and returns the destination page name accordingly.

---

#### Step 5 – Conditional Page Navigation

Configure navigation buttons as follows:

1. Insert a **Blank Button**
2. Go to **Format → Action**
3. Set:

   * **Type**: Page navigation
   * **Destination**: fx
   * **Field value**: `[Executive Access]`
4. Apply styling and position the button consistently across pages

Navigation behavior:

* Authorized users are routed to the Executive Page
* Unauthorized users are redirected to the Request Access page

---

#### Result

* Executives can access sensitive content
* Non-authorized users are blocked from navigation
* Access management is centralized in Azure AD

---

#### Limitations

This approach provides **UI-level access control only**.

It does **not** prevent:

* Direct access via page URLs (see below how to overcome this)
* Access via Analyze in Excel (you need to block this feature)

#### Block Direct access via page URLs

Use a numeric return type (recommended for filters):

```
Executive Access Visuals =
IF (
    USERPRINCIPALNAME()
        IN VALUES ( 'Dim_Security_AzGroup_Executives'[userPrincipalName] ),
    1,
    0
)
```

How to Apply It Correctly

1. Apply this measure [Executive Access Visuals] as a Visual-Level Filter for every sensitive visual on the Executive Page.
2. Set condition: is 1

Result:

Executives → visuals render normally

Others → visuals do not render at all

**Pro Tip: Add an Explicit “No Access” Message**

Create a card or text box that:

1. Shows only when Executive Access Visuals = 0
2. Explains that access is restricted and how to request it
3. This avoids a blank page and improves UX.

