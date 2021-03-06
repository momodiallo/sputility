### SPUtility.js core API

* **SPUtility.GetSPField(strFieldName)** - Get a field on DispForm, EditForm, or NewForm by the field's display name. Returns an SPField object (see below).
* **SPUtility.GetSPFieldByInternalName(strFieldName)** - Get a field by its internal column name. Returns an SPField object.
* **SPUtility.GetSPFields()** - Get an object containing every field on the page. Field display name is the key and fieldParams is the value. May change in future versions.
* **SPUtility.GetSPFieldsInternal()** - Get an object containing every field on the page. Field internal column name is the key and fieldParams in the value. (added in version 0.14.0)
* **SPUtility.HideSPField(strFieldName)** - Shortcut for hiding a field by display name (faster than GetSPField if you need to hide a ton of fields at once).
* **SPUtility.ShowSPField(strFieldName)** - Shortcut for showing a field by display name (faster than GetSPField if you need to show a ton of fields at once).
* **SPUtility.Setup(settings)** - Configure how SPUtility works. Especially useful for different number formats and/or languages. See [#Setup](#Setup) below for more information. (added in version 0.14.0)

{{
// GetSPField - The main function! Get a field on the current page using the field's display name
// Returns a SPField object depending on the field's type (see section below)
var myField = SPUtility.GetSPField('Title');

// NEW! As of 0.9.1, get field's using their internal column name
myField = SPUtility.GetSPFieldByInternalName('Date_x0020_and_x0020_Time');

// HideSPField - hide a field
SPUtility.HideSPField('Title');

// ShowSPField - opposite of Hide
SPUtility.ShowSPField('Title');

// GetSPFields - get an object containing all the fields on the page
var fields = SPUtility.GetSPFields();
for (fieldName in fields) {
   // do something with the field
   var field = SPUtility.GetSPField(fieldName);
}
}}
### SPFields
These objects are returned from a call to {{SPUtility.GetSPField}}. All fields support the following functions:
* GetValue - Gets the fields value. *
* SetValue - Sets the fields value. *
* GetDescription - Get a field's description (added in version 0.11.0)
* SetDescription - Set a field's description (added in version 0.11.0)
* MakeReadOnly - Gets the value of the field and replaces any controls with a label preventing the user from editing the field
* MakeEditable - The opposite of MakeReadOnly. Makes a field editable again. Does nothing if the field wasn't read only.
* Hide - Hide's the entire field's row from view.
* Show - Show's the field's row.

Depending on the type of the field (ex: Choice field or a text field), the functions can perform different operations. SPUtility.js attempts to simplify the interface to each field type as much as possible.

 * Refer to each field's page for more field-specific information (see the matrix on the [Documentation](Documentation) page).

### Common SPField Functions

These functions work nearly identically for each field type:

{{
// Get the SPField object
var myField = SPUtility.GetSPField('Title');

// Make the field read only
myField.MakeReadOnly();

// Allow the user to edit the field again
myField.MakeEditable();

// Hide the field
myField.Hide();

// Show the field again
myField.Show();
}}
### Common SPField Properties

For each field type, multiple properties are set. These make it easier to reference specific field elements. Every field has the following properties set:

* Name - Display name of the field
* InternalName - Internal column name
* IsRequired - True if the field is required
* Type - SharePoint field type
* Controls - The element containing all the field's controls
* ControlsRow - The parent table row containing the field's controls
* Label - The element containing the column's name
* LabelRow - The parent table row for the column's name
* ReadOnlyLabel - Default is null, until MakeReadOnly is called, then it holds the reference to the read-only span element

{{
// get the field
var myField = SPUtility.GetSPField('My Generic Field');

// Controls references a SPAN element containing all the field specific controls (for example a textbox for Single line of Text fields)
myField.Controls;

// ControlsRow is generally used only on Survey forms. It references the TR element containing the field's controls. It is null otherwise.
myField.ControlsRow;

// IsRequired is a boolean value for whether or not the field is required.
myField.IsRequired;

// Label references the element that contains the display name of the field (ex: My Generic Field, the same value you pass to GetSPField).
myField.Label;

// LabelRow references the TR element containing the label. This is always set for all forms. 
myField.LabelRow;

// Name is a string containing the fields name (ex: My Generic Field).
myField.Name;

// ReadOnlyLabel references the DIV for the read only label. It is null until MakeReadOnly is called for the first time.
myField.ReadOnlyLabel;

// Type is a string containing the fields type (ex: SPFieldText).
myField.Type;
}}

### Setup {anchor:Setup}

Reconfigure SPUtility.js to work with your locale! Here are the default settings which SPUtility.js uses:
{{
{
   'timeFormat': '12HR',      // 12HR or 24HR
   'dateSeparator': '/',      // separates month/day/year with / or .
   'decimalSeparator': '.',   // separates decimal from number
   'thousandsSeparator': ',', // separates thousands in number
   'stringYes': 'Yes',        // Text for when boolean field is True
   'stringNo': 'No'           // Text for when boolean field is False
}
}}

The settings are:
* timeFormat - Set the time format to either 12HR or 24HR. When in 12HR mode, dates will be month/day/year. When in 24HR mode, dates will be day/month/year.
* dateSeparator - Set the date separator. Used to separate year from month from day. Ex: 3/10/2015.
* decimalSeparator - Set the decimal separator (usually a period or a comma character) which is used to separate integer from fractional part of a number.
* thousandsSeparator - Set the thousands separator (usually a period or a comma character).
* stringYes - For Yes/No fields, the text to display when the field is in read-only mode and the field is checked.
* stringNo - For Yes/No fields, the text to display when the field is in read-only mode and the field is NOT checked.

For example, someone in Spain might want to change the settings to:
{{
SPUtility.Setup({
   'timeFormat': '24HR',
   'decimalSeparator': ',',
   'thousandsSeparator': '.',
   'stringYes': 'Si',
});
}}

### Deprecated methods (will be removed in a later release)

* **SPUtility.GetTimeFormat()** - Gets the time format which is used to format date/time fields. '12HR' (default) or '24HR'. (added in version 0.11.0)
* **SPUtility.SetTimeFormat(format)** - Set the time format to either 12HR or 24HR. When in 12HR mode, dates will be month/day/year. When in 24HR mode, dates will be day/month/year. (added in version 0.11.0)
* **SPUtility.GetDateSeparator()** - Get the date separator. Default is '/'. (added in version 0.11.2)
* **SPUtility.SetDateSeparator(separator)** - Set the date separator. Used to separate year from month from day. Ex: 3/10/2015. (added in version 0.11.2)
* **SPUtility.GetDecimalSeparator()** - Get the decimal separator. Default is a period '.' (added in version 0.13.0)
* **SPUtility.SetDecimalSeparator(separator)** - Set the decimal separator (usually a period or a comma character) which is used to separate integer from fractional part of a number. (added in version 0.13.0)
* **SPUtility.GetThousandsSeparator()** - Get the thousands separator. Default is a comma ',' (added in version 0.13.0)
* **SPUtility.SetThousandsSeparator(separator)** - Set the thousands separator (usually a period or a comma character). (added in version 0.13.0)
