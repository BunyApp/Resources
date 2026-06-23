Aidoku's `settings.json` file is a JSON array which declares all the customization options available. Each item in this array must be a settings group.
```json
[
  {
    "type": "group",
    "title": "Setting group header (optional)",
    "footer": "Setting group footer (optional)",
    "items": [
      
    ]
  }
]
```
Inside the `items` array, you can declare other settings as an object described below.

# Setting items
## Setting page
Related settings can be further grouped into its own page:
```json
{
  "type": "page",
  "title": "Page title",
  "requires": "setting.that.needs.enabling",
  "requiresFalse": "setting.that.needs.disabling",
  "items": [
  
  ]
}
```
## Common properties
All setting items support these common properties:
| Key             | Type     | Description                                                                                                                                          |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `key`           | `string` | The key to access the setting's value with.                                                                                                          |
| `title`         | `string` | The setting's displayed name. (not supported for [text fields](#text-field))                                                                         |
| `notification`  | `string` | A notification posted whenever the value for this setting changes. Can be handled with `handle_notification`. (not supported for [buttons](#button)) |
| `default`       | `any`    | The default value for this setting. The type of this value depends on the setting type.                                                              |
| `requires`      | `string` | Require that another setting (of type `switch`) is enabled before this one can be modified.                                                          |
| `requiresFalse` | `string` | Require that another setting (of type `switch`) is disabled before this one can be modified.                                                         |


## Notification
Notification are basically like events that a source can listen to when changes happen to settings. What the source decides to do with the notification is up to the developer to implement, through the `handle_notification` function:
```rust
use aidoku::prelude::*;
use aidoku::std::String;

#[handle_notification]
fn handle_notification(_notification: String) {
    todo!()
}
```

## Types
Aidoku has support for various type of settings, which are declared with the `type` key.

In most cases, the type of the `default` key will also be the value type when obtained from `defaults_get`.

- [Switch](#switch)
- [Stepper](#stepper)
- [Text field](#text-field)
- [Button](#button)
- [Links](#links)
- [Segment](#segment)
- [Select](#select)
- [Multi-select and multi-single-select](#multi-select-and-multi-single-select)
- [Login](#login)

### Switch
`"type": "switch"`
| Key             | Type      | Description                                                           |
|-----------------|-----------|-----------------------------------------------------------------------|
| `default`       | `boolean` | The default value for this setting.                                   |
| `subtitle`      | `string`  | The setting's subtitle, displayed as small gray text under the title. |
| `authToEnable`  | `boolean` | Requires Face ID/Touch ID authentication to enable the switch.        |
| `authToDisable` | `boolean` | Requires Face ID/Touch ID authentication to disable the switch.       |

A simple on/off switch. Its value is stored and retrieved as boolean.

### Stepper
`"type": "stepper"`
| Key             | Type      | Description                         |
|-----------------|-----------|-------------------------------------|
| `default`       | `float`   | The default value for this setting. |
| `minimumValue`  | `float`   | The minimum value for this stepper. |
| `maximumValue`  | `float`   | The maximum value for this stepper. |

A stepper which can increase/decrease its value by 1 each time.

### Text field
`"type": "text"`
| Key                      | Type      | Description                                                                                                                                                                                               |
|--------------------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `default`                | `string`  | The default value for this setting.                                                                                                                                                                       |
| `placeholder`            | `string`  | The placeholder text for this field.                                                                                                                                                                      |
| `autocapitalizationType` | `integer` | Controls the auto-capitalization behavior for this text field. Refer to [UITextAutocapitalizationType](https://developer.apple.com/documentation/uikit/uitextautocapitalizationtype) for possible values. |
| `autocorrectionType`     | `integer` | The auto-correction behavior of the text field. Refer to [UITextAutocorrectionType](https://developer.apple.com/documentation/uikit/uitextautocorrectiontype) for possible values.                        |
| `spellCheckingType`      | `integer` | The spell-checking behavior of the text field. Refer to [UITextSpellCheckingType](https://developer.apple.com/documentation/uikit/uitextspellcheckingtype) for possible values.                           |
| `keyboardType`           | `integer` | Specify the kind of keyboard to display for the text field. Refer to [UIKeyboardType](https://developer.apple.com/documentation/uikit/uikeyboardtype) for possible values.                                |
| `returnKeyType`          | `integer` | Specify the text string that displays in the Return key. Refer to [UIReturnKeyType](https://developer.apple.com/documentation/uikit/uireturnkeytype) for possible values.                                 |

A text field that accepts user input.

### Button
`"type": "button"`

| Key           | Type      | Description                                                   |
|---------------|-----------|---------------------------------------------------------------|
| `action`      | `string`  | Notification that will be emitted when the button is pressed. |
| `destructive` | `boolean` | Marks this button as destructive (red text).                  |


### Links
`"type": "link"`

| Key           | Type      | Description                                                             |
|---------------|-----------|-------------------------------------------------------------------------|
| `url`         | `string`  | The URL to open.                                                        |
| `destructive` | `boolean` | Marks this button as destructive (red text).                            |
| `external`    | `boolean` | Marks this link as external (open in Safari instead of in-app browser). |

A button which can open URLs.

### Segment
`"type": "segment"`
| Key       | Type       | Description                         |
|-----------|------------|-------------------------------------|
| `default` | `string`   | The default value for this setting. |
| `values`  | `string[]` | A list of options.                  |

A horizontal bar consisting of different options.

### Select
`"type": "select"`
| Key          | Type       | Description                                                         |
|--------------|------------|---------------------------------------------------------------------|
| `default`    | `string`   | The default value for this setting.                                 | 
| `values`     | `string[]` | A list of options.                                                  |
| `titles`     | `string[]` | Display name for the options.                                       |
| `authToOpen` | `boolean`  | Require Touch ID/Face ID authentication to see the list of options. |

A list of options which the user can select one value from.

### Multi-select and multi-single-select
`"type": "multi-select"` or `"type": "multi-single-select"`
| Key          | Type       | Description                                                         |
|--------------|------------|---------------------------------------------------------------------|
| `default`    | `string[]` | The default value for this setting.                                 | 
| `values`     | `string[]` | A list of options.                                                  |
| `titles`     | `string[]` | Display name for the options.                                       |
| `authToOpen` | `boolean`  | Require Touch ID/Face ID authentication to see the list of options. |

A list of options which the user can select one or many values from. The value is stored and read as an array.

### Login
`"type": "login"`
| Key           | Type       | Description                                                                              |
|---------------|------------|------------------------------------------------------------------------------------------|
| `method`      | `string`   | Currently only `oauth` is supported.                                                     |
| `key`         | `string`   | The defaults key which stores the OAuth callback URL.                                    |
| `urlKey`      | `string`   | The defaults key which stores the OAuth URL. Useful if the URL is determined at runtime. |
| `logoutTitle` | `string`   | The title displayed when the user is logged in.                                          |

Allow the user to login to a service (currently only OAuth is supported). The callback URL must have the `aidoku://` protocol. If `notification` is set, it is emitted on both login and logout events.
 





