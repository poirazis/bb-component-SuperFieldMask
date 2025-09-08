# @poirazis/bb-component-SuperFieldTextMask

A Budibase component for text input with advanced masking capabilities for consistent data entry.

## 🚀 Features

- **Input Masking**: Pattern-based input restrictions for formats like phone numbers, dates, SSNs, credit cards, etc.
- **Mask Presets**: Pre-built masks for common use cases (phone, SSN, credit card, date, ZIP code)
- **Custom Masks**: Support for custom input mask patterns using IMask library
- **Field Binding**: Connect to data fields with auto-generated labels
- **Validation**: Built-in validation rules with custom error messages
- **User Experience**: Auto-focus, inline icons, and visual states
- **Styling**: Customizable alignment, label positioning, and responsive sizing
- **Events**: On-change handling with full context (triggers on focus loss)
- **State Management**: Integrated state machine for robust input handling

## 📦 Installation & Setup

### Clone and Build

1. Clone the repository:

   ```bash
   git clone https://github.com/poirazis/bb-component-SuperFieldTextMask.git
   cd bb-component-SuperFieldTextMask
   ```

2. Install dependencies:

   ```bash
   yarn install
   ```

3. Build the component:

   ```bash
   yarn build
   ```

4. The built files will be in the `dist/` directory.

## 📝 Usage

1. Add the component to your Budibase form or screen
2. Bind to a data field or configure as standalone
3. Select a mask preset or enter a custom mask pattern
4. Configure validation and other options as needed

### Common Use Cases

- Phone number inputs with formatting: `(999) 999-9999` (US) or `+0 (999) 999-9999` (International)
- Social Security Numbers: `999-99-9999`
- Credit card numbers: `9999 9999 9999 9999`
- Dates: `99/99/9999`
- ZIP codes: `99999`
- Custom patterns for consistent data entry

## 🔧 Configuration Options

| Setting        | Type    | Description                                   |
| -------------- | ------- | --------------------------------------------- |
| Field          | String  | Data field binding                            |
| Label          | String  | Display label text                            |
| Placeholder    | String  | Input placeholder text                        |
| Default Value  | String  | Initial value for the field                   |
| Help Text      | String  | Additional help text below the field          |
| Mask Preset    | Select  | Pre-built mask patterns                       |
| Input Mask     | String  | Custom mask pattern (when preset is "Custom") |
| Validation     | Rules   | Input validation rules                        |
| On Change      | Event   | Triggered when input loses focus              |
| Alignment      | Select  | Text alignment (Left/Center/Right)            |
| Grab Focus     | Boolean | Auto-focus on load                            |
| Disabled       | Boolean | Disable input interaction                     |
| Read Only      | Boolean | Read-only mode                                |
| Invisible      | Boolean | Hide the component                            |
| Inline Icon    | Icon    | Icon to display inside the input              |
| Buttons        | Config  | Additional buttons configuration              |
| Field Mode     | Select  | Form Input or Inline Input mode               |
| Label Position | Select  | Label positioning (Auto/Above/Left/Disabled)  |
| Size           | Number  | Component width (2-12 columns)                |

### Mask Presets Available

- **None**: No masking applied
- **Phone (US)**: `(000) 000-0000`
- **Phone (International)**: `+0 (000) 000-0000`
- **Social Security Number**: `000-00-0000`
- **Credit Card**: `0000 0000 0000 0000`
- **Date (MM/DD/YYYY)**: `00/00/0000`
- **ZIP Code**: `00000`
- **Custom**: Define your own mask pattern

## 📋 Events

### On Change

Triggered when the input loses focus (not on every keystroke). Provides:

- `value`: The current masked/unmasked value
- `field`: Field context information

## 🎨 Styling

Supports Budibase's theming system:

- Size variants and responsive design
- Custom colors and backgrounds
- Font weights and text alignment
- Custom CSS classes
- Visual states (error, complete, dirty)

## 🔍 Best Practices

- Choose appropriate mask presets for common data types
- Test custom mask patterns thoroughly with real data
- Use validation rules to ensure data integrity
- Consider user experience when designing mask patterns
- Enable auto-focus for single-field forms
- Use help text for complex mask requirements

## 📋 Mask Pattern Syntax

The component uses IMask library patterns:

- `0`: Number (0-9) - Required
- `9`: Number (0-9) - Optional
- `a`: Letter (a-z, A-Z) - Required
- `A`: Letter (a-z, A-Z) - Optional
- `*`: Alphanumeric (a-z, A-Z, 0-9) - Required
- Other characters are treated as literals (fixed text)

### Examples:

- Phone: `(000) 000-0000`
- SSN: `000-00-0000`
- Date: `00/00/0000`
- Custom: `AAA-000`

## 🔧 Technical Details

- **State Management**: Uses svelte-fsm for robust state handling
- **Masking Engine**: IMask library for advanced input masking
- **Validation**: Integrated with Budibase's validation system
- **Accessibility**: Keyboard navigation and screen reader support
- **Performance**: Optimized with focus-based updates (no debounce)
