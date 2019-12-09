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
    RedirectType|Choice|Yes|Available choices are:<br/>Exact Match<br/>Starts With<br/><br/>Default value: Exact Match
    