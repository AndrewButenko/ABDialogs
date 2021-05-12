# AB Dialogs
AB Dialogs is a managed solution that provides developer a configurable wrapper and convenient way to open dialogs in new UCI interface of Dataverse and process results of the call.

Here is the list of steps you need to perform in order to use this dialog in your Model Driven Apps:
1. Download and install the latest release of ABDialog solution
1. Add the reference to "ab_/Dialogs/Dialog.js" webresource to the place where you plan to call the dialog from:
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
    //Settings for all of fields available on the step, required
    FieldSettings: FieldSetting[];
}

//This is a setting description of the particular field
interface FieldSetting {
    //Name of the particulat dialog input, should be uniqie accross the dialog 
    //settings, required
    Name: string;
    //Label that will be show nearby the field, required
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