# AB Dialogs
AB Dialogs is a managed solution that provides developer a configurable wrapper and convenient way to open dialogs in new UCI interface of Dataverse and process results of the call.

Here is the list of steps you need to perform in order to use this dialog in your Model Driven Apps:
1. Download and install the latest release of ABDialog solution
1. Add the reference to "ab_/Dialogs/Dialog.js" webresource to the place where you plan to call the dialog from:
    * When you plan to use it in the form scripts, just add the reference as a regular JavaScript webresource
    * When you plan to use it in the ribbon scripts, add the reference to the webresource before using it in your code using "Custom JavaScript" Enable Rule