# Smart 404 Revived

An updated version of the 2009/2010 sharepointsmart404 feature which can be used with SharePoint 2013, 2016, and 2019.

> Disclaimer: I didn't test it with every version of SharePoint on-prem. If you find any issues, please let me know by creating an issue in the repository.

![Smart 404 Revived](./assets/Smart404Title.png)

## Installation

### Create the `VanityURLS` list

The `VanityURLS` contains the mappings from your old SharePoint on-premises URLs to your new SharePoint Online URLs.

To create it, follow these steps:

1. From the root site collection of your on-premises SharePoint 2013, 2016, or 2019, go to **Site Contents**
2. Select **add an app**
3. From the list of available apps, select **Custom List**
4. When prompted to give the list a name, enter **VanityURLs** then select **Create**. If you want to change the name of this list, keep in mind that you'll have to change the code later.
5. Once SharePoint displays the list, select **List Settings** from the **List** ribbon.
6. From the **List Settings** page, select **Advanced Settings**
7. From the **Advanced Settings**, disable attachments by selecting **Disabled** under **Attachments to list items are:** then **OK**. This step is optional, but I like to keep things clean.
8. Back in the **List Settings**, under **Columns**, select **Create column** and create the following columns using the information below (use default settings unless specified):

    Name|Type|Required?|Notes
    ---|---|---|--
    RequestURL|Single line of text|Yes|Resist the urge to make this a Hyperlink column, even if that's what we'll put in this column
    RedirectURL|Single line of text|Yes|Also not a Hyperlink column!
    RedirectType|Choice|Yes|Available choices are:<br/>Exact Match<br/>Starts With<br/>Regular Expression<br/><br/>Default value: Exact Match
    RegExp|Single line of text|No|Used for **Regular Expression** match types
    Order|Number|No|
    
### Create the `RedirectLog` list

The `RedirectLog` records every time the Smart 404 displays. You can use it to detect if anyone has not updated their shortcuts (or is just being plain stubborn)

To create it, follow these steps:

1. From the root site collection of your on-premises SharePoint 2013, 2016, or 2019, go to **Site Contents**
2. Select **add an app**
3. From the list of available apps, select **Custom List**
4. When prompted to give the list a name, enter **RedirectLog** then select **Create**. If you want to change the name of this list, keep in mind that you'll have to change the code later.
5. Once SharePoint displays the list, select **List Settings** from the **List** ribbon.
6. From the **List Settings** page, select **Advanced Settings**
7. From the **Advanced Settings**, disable attachments by selecting **Disabled** under **Attachments to list items are:** then **OK**. This step is optional, but I like to keep things clean.
8. From the **Item-level permissions** section, select **Read items that were created by the user**. This helps keep things a bit more confidential.
9. Back in the **List Settings**, under **Columns**, select **Create column** and create the following columns using the information below (use default settings unless specified):

    Name|Type|Required?|Notes
    ---|---|---|--
    Title|Single line of text|Yes|
    RequestedUrl|Single line of text|Yes|
    RedirectedTo|Single line of text|No|
    Referrer|Single line of text|No|
    
### Configure the `Smart404` page

The `Smart404` page is an HTML file that contains the display templates for such messages as **Not Found**, and **Content Has Moved**, along with the Javascript to look up for vanity URLs and redirect users.

You can use the page as-is without changing it, but there are a few lines of code  that you can change to suit your needs:

Setting|Description|Code|Notes
---|---|---|--
timeSeconds|Controls the duration of the countdown before redirecting users|`var timeSeconds = 15;`| Default is **15** seconds. Long enough to just be annoying enough that it encourages users to change their bookmarks.
vanityURLListName|The title of your `VanityURLs` list|`var vanityURLsListName = "VanityURLs";`|Only need to change it if you decided to name your `VanityURLs` list something else
RedirectLogListName|The name of your `RedirectLog` list|`var redirectLogListName = "RedirectLog";`|Only need to change it if you decided to name your `RedirectLog` list something else
spoTenantUrl|Your SharePoint Online tenant URL|`var spoTenantUrl = null;`|Change if you'd like any **Not Found** results to automatically redirect to the SPO Search Page. For example, use `var spoTenantUrl = "https://yourtenant.sharepoint.com";`    
fqdnOnPremUrl|FQDN name for your SharePoint On-Prem server.|`var fqdnOnPremUrl = null;`|Use this option in combination with `machineOnPremUrl` if your on-prem SharePoint server used to be available as a machine name only and you don't want to have to enter two sets of vanity URLS (one for the machine name, and one for the FQDN name). E.g.: `var fqdnOnPremUrl = '//sharepoint.mycompany.com'`
machineOnPremUrl|The machine name of your on-prem SharePoint server|`var machineOnPremUrl = null;`|Use in combination with `fqdnOnPremUrl`. E.g.: `var machineOnPremUrl = '//sharepointprod01'`|
locTitleNotFound|The localized title to display for **Not Found** results|`var locTitleNotFound = "Not Found";`|Replace for anything else you want to display
locTitleRedirected|The localized title to display for **Page Has Moved** results|`   var locTitleRedirected = "Page Has Moved";`|Replace for anything you wish.


In addition there are three HTML display templates that you can change. You can customize the HTML to suit your needs (e.g.: add some useful links, add a cute image, etc.) -- just make sure to keep the `id` attribute of each `div` element the same (that's how the code decides what to show). 

The template `id`s are:

Template ID|Description
---|---
NotFoundTemplate|Displays a **Not Found** result.
RedirectingToSearchTemplate|Displays a **Not Found** result that will redirect users to the SharePoint Online search page (if the `spoTenantUrl` value isn't `null`)
RedirectingTemplate|Displays a **Page Has Moved** template, which will redirect users after a short time.

