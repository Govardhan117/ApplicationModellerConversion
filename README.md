# Application Modeller Conversion Tool
The Blue Prism Application Modeller Conversion Tool has been designed to allow quick conversion of application modeller elements and attributes. The principal audience for this utility is those wishing to convert Internet Explorer objects to Chrome, Firefox or Edge Chromium browsers, but it also has the scope to support customized conversions between other application modes.

The current version of the application is **v1.1.0**.

You may wish to read [this blog article](https://community.blueprism.com/blogs/bruce-liu1/2020/09/20/accelerating-your-migration-away-from-the-internet?CommunityKey=1e516cfe-4d1f-4de9-a9eb-58d15bf38c81)  to understand the background why this application was created in the first place, and things you should be aware of.

## Factors that May Impact the Conversion Outcome

* Browsers Do Behave Differently
* Blue Prism Function Gaps
* Developer Choices
* Accuracy in Predefined Conversion Rules Files

We are more than happy to hear your suggestions on how we can improve the application and the conversion rules files that have been created with the tool. You may use [Blue Prism Community](https://community.blueprism.com/home) to make us aware of it.

## Conversion Rules Files Catalogue

### [ConversionRules_ChromeEdgeFirefox_6.3.0-6.8.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_ChromeEdgeFirefox_6.3.0-6.8.x.xml)
For conversion of Blue Prism objects created using Chrome/Edge/Firefox mode to Chrome/Edge/Firefox mode, e.g. conversion from Chrome to Edge. This conversion rules file supports Blue Prism versions from 6.3.0 to 6.8.x.

Note, object name conversion is not performed.

### [ConversionRules_ChromeEdgeFirefox_6.9.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_ChromeEdgeFirefox_6.9.x.xml)
For conversion of Blue Prism objects created using Chrome/Edge/Firefox mode to Chrome/Edge/Firefox mode, e.g. conversion from Chrome to Edge. This conversion rules file supports Blue Prism versions from 6.9.x onwards, including **v6.10**. 

Note, object name conversion is not performed.

### [ConversionRules_IE-ChromeEdgeFirefox_6.3.0-6.8.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_IE-ChromeEdgeFirefox_6.3.0-6.8.x.xml)
For conversion of Blue Prism objects created using Internet Explorer mode to Chrome/Edge/Firefox modes. This conversion rules file supports Blue Prism versions from 6.3.0 to 6.8.x.

### [ConversionRules_IE-ChromeEdgeFirefox_6.9.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_IE-ChromeEdgeFirefox_6.9.x.xml)
For conversion of Blue Prism objects created using Internet Explorer mode to Chrome/Edge/Firefox modes. This conversion rules file supports Blue Prism versions from 6.9.x onwards, including **v6.10**.

### [ConversionRules_AA-UIA_6.x.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_AA-UIA_6.x.x.xml)
For conversion of Blue Prism objects from Active Accessibility (AA) to UI Automation (UIA). Such objects or release must have gone through conversion into Chrome/Edge/Firefox mode. This conversion rules file supports all Blue Prism v6 versions. For more details please see below.

#### Notes on Active Accessibility (AA) to UI Automation (UIA) Conversion Usage
The AA to UIA conversion is only possible with an updated version of the plugin *BPAMConversionToolPlugin.dll*. Please kindly obtain a new version from this site. Note that no update to the core Blue Prism Application Modeller Conversion Tool application is needed. If in doubt, just download the [BP_AM_Converter.zip](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/BP_AM_Converter.zip) file again and overwrite your local copy.

This conversion rule file only supports conversion of elements from AA to UIA when the Blue Prism object or objects found in the release have already been converted to use Chrome/Edge/Firefox, i.e. apptypeinfo > id is either *BrowserAttach* or *BrowserLaunch*. Other AA elements found will not be converted.

To prepare an Internet Explorer (IE) object with AA elements for conversion, i.e. apptypeinfo > id is either *HTMLAttach* or *HTMLLaunch*, please firstly convert the object or release into Chrome/Edge/Firefox. You may use one of conversion rules files prefixed with "ConversionRules_IE-ChromeEdgeFirefox_" to achieve this, please kindly pick one such file that is most appropriate for your Blue Prism version. Once you have obtained the new object or release file from the conversion, you can then use it along with this conversion rule file as the inputs to your AA to UIA conversion.

It is important to note that UIA elements are often slow for large and complex web pages. If you are thinking of converting from IE based AA elements to Chrome/Edge/Firefox based UIA elements, you may wish to respy such elements in Chrome/Edge/Firefox browser modes using CSS Selector and XPath Selector technique. To gain further understanding of this technique, please consider [this Blue Prism University course](https://university.blueprism.com/learn/course/16924/Spying%2520Using%2520CSS%2520Selector%2520and%2520Xpath).

#### Notes on AA to UIA Conversion Logic
The key to success is the conversion between AA Role to UIA Control Type.

The AA Role to UIA Control Type is defined by the mapping found in [this](https://docs.microsoft.com/en-us/windows/win32/winauto/uiauto-msaa#:~:text=Microsoft%20Active%20Accessibility%20is%20the,products%20and%20automated%20testing%20tools) article from Microsoft. The mapping is implemented with some exceptions as detailed below:
* AA role **ROLE_SYSTEM_CLIENT** is not mapped to any UIA Control Type. This means AA object with Role set to **Client** is not going to work once converted into UIA. You may wish to manually set the target UIA Control Type or respy the element altogether.
* There exist 3 mappings possible for AA role **ROLE_SYSTEM_LIST**, i.e., **DataGrid**, **Header** and **List**. **List** is currently set as the target UIA Control Type. Changes can be made in the plugin source code to have a different mapping set if needed. Once it is done, you must replace existing **BPAMConversionToolPlugin.dll** with the newly compiled file.
* There exist 2 mappings possible for AA role **ROLE_SYSTEM_LISTITEM**, i.e., **DataItem** and **ListItem**. **ListItem** is currently set as the target UIA Control Type. Changes can be made in the plugin source code to have a different mapping set if needed. Once it is done, you must replace existing **BPAMConversionToolPlugin.dll** with the newly compiled file.

It is observed that AA spied objects will often have attributes related to parent level elements selected by default. Although it is possible to define mapping on the parent level attributes, a decision has been made to drop them altogether when converting to UIA mode as:
* Blue Prism spied UIA objects do not usually utilise any of the parent level attributes. 
* Including parent level attributes in UIA elements will often result in no matching elements found error.
That being said, changes may be made to the conversion rules file to enable such mappings if needed.