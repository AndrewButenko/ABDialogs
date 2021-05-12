# AB Dialogs
AB Dialogs is a managed solution that provides the developer a configurable wrapper and convenient way to open dialogs in the new UCI interface of Dataverse and process results of the call.

Here is the list of steps you need to perform in order to use this dialog in your Model-Driven Apps:
1. Download and install [the latest release](/AndrewButenko/ABDialogs/releases/download/1.0.0.0/ABDialogs_1_0_0_0_managed.zip) of ABDialogs solution
1. Add the reference to the "ab_/Dialogs/Dialog.js" webresource to the place where you plan to call the dialog from:
    * When you plan to use it in the form scripts, just add the reference as a regular JavaScript webresource
    * When you plan to use it in the ribbon scripts, add the reference to the webresource before using it in your code using "Custom JavaScript" Enable Rule
1. Prepare your "Dialog Configuration" object that has the following structure:
```typescript
//This is high-level settings
interface DialogSettings {
    //Header that will be shown at the top of the dialog, required
    DialogHeaderText: string;
    //It's possible to have multistep dialogs and TabSettings has to contain all
    //of the step available, required
    TabSettings: TabSetting[];
}

//This is a setting description of the particular tab
interface TabSetting {
    //Helper information that will be shown on top of fields, optional
    TabHeaderText?: string;
    //Settings for all of the fields available on the step, required
    FieldSettings: FieldSetting[];
}

//This is a setting description of the particular field
interface FieldSetting {
    //Name of the particular dialog input, should be unique across the dialog 
    //settings, required
    Name: string;
    //Label that will be shown beside the field, required
    Label: string;
    //Type of the field/input that will be rendered, any other type will be rendered
    //just as a label, required
    FieldType: "string" | "wholenumber" | "date" | "boolean" | "choice" | "choices";
    //Initial value of the field, optional
    Value: any;
    //Array that contains value/label pairs used in "boolean", "choice" and "choices"
    //types of fields, required for mentioned types, ignored for other
    ChoiceValues?: ChoiceValue[];
    //UI formatting used for "boolean" type of field, "checkbox" is used by default, optional
    BooleanStyle?: "checkbox" | "radio" | "toggle";
    //UI formatting of "string" field that allows to enter information in multi-line format,
    //1 is used by default, ignored for other than "string" types
    LinesNumber?: number;
}

interface ChoiceValue {
    //Value used
    Value: any;
    //Display Label
    Label: string;
}
```
Call **AB.Dialogs.open** function to open the dialog. This function returns the promise.
When a user closes the dialog using the "X" button or "Cancel" button, the promise is resolved with **null** value and returns the object with all values selected in dialog otherwise.

Here is an example of JavaScript webresource that is used to show 2-Tabs Dialog with 2 inputs each:

```javascript
var AB = AB || {};
AB.DialogDemo = (function () {

    function openDialog() {
        var dialogParameters = {
            DialogHeaderText: "Custom Header Here",
            Tabs: [{
                TabHeaderText: "This is Text Header",
                FieldSettings: [
                    {
                        FieldType: "string",
                        Label: "Text Field Label",
                        Name: "text1",
                        Value: "Initial Value of Text Field",
                    }, {
                        FieldType: "date",
                        Label: "Date Field",
                        Name: "date1",
                        Value: new Date()
                    }]},{
                FieldSettings: [{
                        FieldType: "boolean",
                        Label: "Two Values Select",
                        Name: "bool2",
                        Value: false,
                        BooleanStyle: "toggle",
                        ChoiceValues: [{
                            Value: false,
                            Label: "False is selected"
                        }, {
                            Value: true,
                            Label: "True is selected"
                        }]
                    }, {
                        FieldType: "choices",
                        Label: "Dropdown",
                        Name: "dd1",
                        Value: undefined,
                        ChoiceValues: [1, 2, 3, 4].map(function(o) {return { Value: o, Label: 'Option ' + o }})
                    }
                ]
            }]
        };

        AB.Dialogs.open(dialogParameters).then(function(result){
            console.log(result);
        }, Xrm.Navigation.openAlertDialog)
    }

    return {
        openDialog: openDialog
    };
})();
```

If a user leaves text and datetime field unchanged, sets the boolean field to true and selects "Option 2" and "Option 3" in choices control here is the object that will be returned to the callback:
```json
{
    "text1": "Initial Value of Text Field",
    "date1": "2021-05-12T07:09:16.197Z",//DateTime fields are returned as DateTime fields, no need to include additional parsing
    "bool2": true,
    "dd1": [2, 3]
}
```