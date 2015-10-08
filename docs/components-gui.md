# GUI components

## gui:button

A button. Can be bound to a specific control or execute a script when clicked.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **values** : _Property (array)_ : array of values to send to bound control when button clicked
- **on-click** : _Property (string text/x-praxis-script)_ : script to run when button is clicked (advanced)
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:combobox

A combobox. Automatically picks up allowed values from the bound control if specified.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **values** : _Property (array)_ : array of selectable values, or empty to pick up the allowed values from the bound control
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:filefield

Specify a file. When clicked, a file selection dialog is opened.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:h-rangeslider

A range slider. Currently only supports binding to a range property that accepts input and output points. The only components that support that so far are `audio:sampleplayer`, `audio:sampling:player` and `audio:sampling:looper`.

If a minimum and maximum value are not set, the range slider will attempt to configure these from the bound control. If these are not specified by the control they will default to 0 and 1 respectively.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **minimum** : _Property (empty or number)_
- **maximum** : _Property (empty or number)_
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:h-slider

A horizontal slider.

If a minimum, maximum or scale value are not set, the slider will attempt to configure these from the bound control. If these are not specified by the control they will default to 0, 1 and Linear respectively.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **minimum** : _Property (empty or number)_
- **maximum** : _Property (empty or number)_
- **scale** : _Property (empty or string)_
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:panel

A panel to contain other components

- **label** : _Property (string)_
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:tabs

Arrange child components in tabs. Ideally, use a `gui:panel` for the children of this component. The value of label on each child component will be used as the tab name.

- **label** : _Property (string)_
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui-textfield

A simple textfield that can be bound to any control.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:togglebutton

A toggle button.

When syncing to a bound control, the value of on-value is used to determine whether the button appears selected.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **on-value** : _Property_ : value sent to the bound control when the button is selected
- **off-value** : _Property_ : values sent to the bound control when the button is de-selected
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:v-slider

A vertical slider.

If a minimum, maximum or scale value are not set, the slider will attempt to configure these from the bound control. If these are not specified by the control they will default to 0, 1 and Linear respectively.

- **label** : _Property (string)_
- **binding** : _Property (control address or empty)_ : address of the control to bind to
- **minimum** : _Property (empty or number)_
- **maximum** : _Property (empty or number)_
- **scale** : _Property (empty or string)_
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)

## gui:xy-pad

An XY pad. Different controls can be bound to vertical and horizontal movement.

If minimum, maximum or scale value are not set, the pad will attempt to configure these from the bound controls. If these are not specified by the control they will default to 0, 1 and Linear respectively.

- **label** : _Property (string)_
- **binding-x** : _Property (control address or empty)_ : address of the control to bind horizontal movement to
- **binding-y** : _Property (control address or empty)_ : address of the control to bind vertical movement to
- **minimum-x** : _Property (empty or number)_
- **minimum-y** : _Property (empty or number)_
- **maximum-x** : _Property (empty or number)_
- **maximum-y** : _Property (empty or number)_
- **scale-x** : _Property (empty or string)_
- **scale-y** : _Property (empty or string)_
- **layout** : _Property (string)_ : a layout string (based on the format of MigLayout)
