______________________________________________________________________________

### `ComboInput.props.attr`

```html
<ComboInput attr={...}>
    <div className='combo-input-container' {...props.attr.container}>
        <div className='input-container' {...props.attr.textbox}>
            <input className='input' {...props.attr.input} />
            <button className='cancel' {...props.attr.clearButton} />
            <span className='chevron' {...props.attr.chevron} />
            <div className='dropdown' {...props.attr.dropdown}>
                <button className='option' {...props.attr.option} />
                ...
                <button className='option' {...props.attr.option} />                
            </div>
        </div>
    </div>
</ComboInput>
```

______________________________________________________________________________

### Examples

#### Default

```jsx
const initialState = {
    value: '',
    options: [
        {label: 'Label 1', value: 'Option 1'},
        {label: 'Label 2', value: 'Option 2'},
        {label: 'Label 3', value: 'Option 3'},
        {label: 'Label 4', value: 'Option 4', hidden: true},
        {label: 'Label 5', value: 'Option 5', disabled: true},
        {label: 'Label 6', value: 'Option 6'},
        {label: 'Label 7', value: 'Option 7'},
        {label: 'Label 8', value: 'Option 8'},
        {label: 'Label 9', value: 'Option 9'},
    ]
};

<div>
    <div style={{marginBottom: '20px'}}>
        Current value:  {
            typeof(state.value) === 'string' ? `'${state.value}'`
                : <pre>{JSON.stringify(state.value, null, 2)}</pre>
        }
    </div>
    <ComboInput
        name='combo-input'
        value={state.value}
        onChange={newValue => setState({value: newValue})}
        options={state.options}
        placeholder='Example placeholder'
        attr={{input: {'data-test-hook': 'combo1-input'}}}
    />
</div>
```

#### Using an optionMap callback

`ComboInput` uses four callback properties in order to filter through this list of options and decide how to display in the dropdown. If you have a list of objects and all you need is default behavior, you just have to provide the property `optionMap`, which maps a `FormOption` to a string that is then used by the other callbacks.

In this example, all of the `FormOption.value` value are Javascript objects so `optionMap` will instead return the label string value. Regardless of how `optionMap` transforms the options, the `onChange` callback will be called with `FormOption.value` when possible (if `optionSelect` recognizes the text field value as a FormOption or the user clicks in the dropdown).

```jsx
const optionMap = option => option.label;

const initialState = {
    value: '',
    options: [
        {label: 'Label 1', value: {str: 'Option 1'}},
        {label: 'Label 2', value: {str: 'Option 2'}},
        {label: 'Label 3', value: {str: 'Option 3'}},
        {label: 'Label 4', value: {str: 'Option 4'}},
        {label: 'Label 5', value: {str: 'Option 5'}},
    ]
};

<div>
    <div style={{marginBottom: '20px', height: '64px', overflowY: 'scroll'}}>
        Current value:  {
            typeof(state.value) === 'string' ? `'${state.value}'`
                : <pre>{JSON.stringify(state.value, null, 2)}</pre>
        }
    </div>
    <ComboInput
        name='combo-input'
        options={state.options}
        value={state.value}
        onChange={newValue => setState({value: newValue})}
        optionMap={optionMap}
        attr={{input: {'data-test-hook': 'combo2-input'}}}
    />
</div>
```

#### Using optionFilter and optionLabel callbacks

By default, `optionFilter` returns true as long as `FormOption.hidden` is false so all visible options will be displayed in the dropdown with `FormOption.label` as the contents of the dropdown items (returned by the default `optionLabel` callback).

In this example, a custom `optionFilter` hides any `FormOption`s that don't contain the value typed into the text input (except when the value is empty) and a custom `optionLabel` creates HTML text with the value string bolded.

**Note: that `ComboInput` will ignore `optionFilter` if an option from the dropdown has been selected, at least until the user starts to type into the text box again.**

```jsx
const optionFilter = (newValue, option) => {
    return (
        option.label.toLowerCase().indexOf(newValue.toLowerCase()) > -1 ||
        newValue.length === 0
    );
};

const optionLabel = (newValue, option) => {
    const str = option.label;
    const index = str.toLowerCase().indexOf(newValue.toLowerCase());
    if (index > -1) {
        const prefix = str.substr(0, index);
        const body = str.substr(index, newValue.length);
        const suffix = str.substr(index + newValue.length);
        return (
            <span>
                {prefix}
                <span style={{fontWeight: 'bold'}}>{body}</span>
                {suffix}
            </span>
        )
    }
    return option.label;
};

const initialState = {
    value: '',
    options: [
        {label: 'Alaska', value: {str: 'AK'}},
        {label: 'California', value: {str: 'CA'}},
        {label: 'Florida', value: {str: 'FL'}},
        {label: 'New York', value: {str: 'NY'}},
        {label: 'Washington', value: {str: 'WA'}},
    ]
};

<div>
    <div style={{marginBottom: '20px', height: '64px', overflowY: 'scroll'}}>
        Current value:  {
            typeof(state.value) === 'string' ? `'${state.value}'`
                : <pre>{JSON.stringify(state.value, null, 2)}</pre>
        }
    </div>
    <ComboInput
        name='combo-input'
        options={state.options}
        value={state.value}
        onChange={newValue => setState({value: newValue})}
        optionMap={opt => opt.label.toString()}
        optionFilter={optionFilter}
        optionLabel={optionLabel}
        showLabel={false}
        attr={{input: {'data-test-hook': 'combo3-input'}}}
    />
</div>
```

#### Disabled Combo Input Example

```jsx
const initialState = {
    value: '',
    options: [
        {label: 'Label 1', value: 'Option 1'},
        {label: 'Label 2', value: 'Option 2'},
        {label: 'Label 3', value: 'Option 3'},
        {label: 'Label 4', value: 'Option 4', hidden: true},
        {label: 'Label 5', value: 'Option 5', disabled: true},
    ]
};

<div>
    <div style={{marginBottom: '20px', height: '64px', overflowY: 'scroll'}}>
        Current value:  {
            typeof(state.value) === 'string' ? `'${state.value}'`
                : <pre>{JSON.stringify(state.value, null, 2)}</pre>
        }
    </div>
    <ComboInput
        name='combo-input'
        value={state.options[0].value}
        onChange={newValue => setState({value: newValue})}
        options={state.options}
        disabled
        attr={{input: {'data-test-hook': 'combo4-input'}}}
    />
</div>
```


#### Error Example

```jsx
class ComboError extends React.Component {
    constructor(props) {
        super(props);
        this.options = [
            {label: 'Label 1', value: 'Option 1'},
            {label: 'Label 2', value: 'Option 2'},
            {label: 'Label 3', value: 'Option 3'},
            {label: 'Label 4', value: 'Option 4', hidden: true},
            {label: 'Label 5', value: 'Option 5', disabled: true},
        ];

        this.state = {value: 'text'};
    }
    render() {

        return <div>
            <div style={{marginBottom: '20px'}}>
                Current value:  {
                    typeof(this.state.value) === 'string' ? `"${this.state.value}"`
                        : <pre>{JSON.stringify(this.state.value, null, 2)}</pre>
                }
            </div>
            <ComboInput
                name="combo-input"
                value={this.state.value}
                onChange={newValue => this.setState({value: newValue})}
                options={this.options}
                placeholder='Example placeholder'
                error
                attr={{input: {'data-test-hook': 'combo5-input'}}}
                error
            />
        </div>;
    }
}
<ComboError />
```

#### No FormOptions

```jsx
const initialState = {
    value: '',
    options: []
};

<div>
    <div style={{marginBottom: '20px'}}>
        Current value:  {
            typeof(state.value) === 'string' ? `'${state.value}'`
                : <pre>{JSON.stringify(state.value, null, 2)}</pre>
        }
    </div>
    <ComboInput
        name='combo-input'
        value={state.value}
        onChange={newValue => setState({value: newValue})}
        options={state.options}
        placeholder='Example placeholder'
        attr={{input: {'data-test-hook': 'combo1-input'}}}
    />
</div>
```