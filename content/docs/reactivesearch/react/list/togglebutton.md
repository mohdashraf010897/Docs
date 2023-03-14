---
title: 'ToggleButton'
meta_title: 'ToggleButton'
meta_description: 'ToggleButton creates a multiple selection based list UI component that is connected to a database field.'
keywords:
    - reactivesearch
    - togglebutton
    - appbase
    - elasticsearch
sidebar: 'docs'
nestedSidebar: 'web-reactivesearch'
---

![Image to be displayed](https://i.imgur.com/33dxDWT.png)

`ToggleButton` creates a toggle button UI component that is connected to a database field. It is used for filtering results based on a fixed set of toggle-able options.

Example uses:

-   filter movies by ratings between 1 and 5,
-   display restaurants that accept delivery and are open now,
-   show flight tickets by one way, round trip and multi-city options.

## Usage

### Basic Usage

```js
<ToggleButton
	componentId="MeetupTops"
	dataField="group_topics.topic_name.raw"
	data={[
		{ label: 'Social', value: 'Social' },
		{ label: 'Travel', value: 'Travel' },
		{ label: 'Outdoors', value: 'Outdoors' },
	]}
/>
```

### Usage (with all props)

```js
<ToggleButton
	componentId="MeetupTops"
	dataField="group_topics.topic_name.raw"
	data={[
		{ label: 'Social', value: 'Social' },
		{ label: 'Travel', value: 'Travel' },
		{ label: 'Outdoors', value: 'Outdoors' },
	]}
	title="Meetups"
	defaultValue={['Social']}
	multiSelect={true}
	showFilter={true}
	filterLabel="City"
	URLParams={false}
  	endpoint={{
    	url:"https://appbase-demo-ansible-abxiydt-arc.searchbase.io/recipes-demo/_reactivesearch.v3", //mandatory
    	headers:{
    	   // relevant headers
    	},
   	 	method: 'POST'
 	}}   
/>
```

## Props

### componentId

| Type | Optional |
|------|----------|
|  `String` |   No   |

unique identifier of the component, can be referenced in other components' `react` prop.
### endpoint

| Type | Optional |
|------|----------|
|  `Object` |   Yes   |

 
endpoint prop provides the ability to query a user-defined backend service for this component, overriding the data endpoint configured in the ReactiveBase component. 
Accepts the following properties:
-   **url** `String` [Required]
    URL where the data cluster is hosted.
-   **headers** `Object` [optional]        
    set custom headers to be sent with each server request as key/value pairs.
-   **method** `String` [optional]    
    set method of the API request.
-   **body** `Object` [optional]    
    request body of the API request. When body isn't set and method is POST, the request body is set based on the component's configured props.
 > - Overrides the endpoint property defined in ReactiveBase.
> - If required, use `transformResponse` prop to transform response in component-consumable format.
  
### dataField

| Type | Optional |
|------|----------|
|  `String` |   No   |

data field to be connected to the component's UI view.
### data

| Type | Optional |
|------|----------|
|  `Object Array` |   Yes   |

collection of UI `labels` with associated `value` to be matched against the database field.
### nestedField

| Type | Optional |
|------|----------|
|  `String` |   Yes   |


use to set the `nested` mapping field that allows arrays of objects to be indexed in a way that they can be queried independently of each other. Applicable only when dataField is a part of `nested` type.
### title

| Type | Optional |
|------|----------|
|  `String` |   Yes   |

or `JSX` [optional]
title of the component to be shown in the UI.
### defaultValue

| Type | Optional |
|------|----------|
|  `String` |   Yes   |

or `Array` [optional]
an array of default selected label(s) to pre-select one or more buttons.
### value

| Type | Optional |
|------|----------|
|  `String Array` |   Yes   |


controls the current value of the component. It selects the label (on mount and on update). Use this prop in conjunction with `onChange` function.
### multiSelect

| Type | Optional |
|------|----------|
|  `Boolean` |   Yes   |


whether multiple buttons can be selected, defaults to **true**. When set to **false**, only one button can be selected.
### showFilter

| Type | Optional |
|------|----------|
|  `Boolean` |   Yes   |


show as filter when a value is selected in a global selected filters view. Defaults to `true`.
### filterLabel

| Type | Optional |
|------|----------|
|  `String` |   Yes   |


An optional label to display for the component in the global selected filters view. This is only applicable if `showFilter` is enabled. Default value used here is `componentId`.
### URLParams

| Type | Optional |
|------|----------|
|  `Boolean` |   Yes   |


enable creating a URL query string parameter based on the selected value of the list. This is useful for sharing URLs with the component state. Defaults to `false`.
### onChange

| Type | Optional |
|------|----------|
|  `function` |   Yes   |


is a callback function which accepts component's current **value** as a parameter. It is called when you are using the `value` props and the component's value changes. This prop is used to implement the [controlled component](https://reactjs.org/docs/forms/#controlled-components) behavior.
### enableStrictSelection

| Type | Optional |
|------|----------|
|  `Boolean` |   Yes   |


When set to `true`, a selected option can't be unselected. Although, it is possible to change the selected option. Defaults to `false`.

	> Note: This only works when `multiSelect` prop is set to `false`.
## Demo

<br />

<iframe src="https://codesandbox.io/embed/github/appbaseio/reactivesearch/tree/next/packages/web/examples/ToggleButton" style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

## Styles

`ToggleButton` component supports `innerClass` prop with the following keys:

-   `title`
-   `button`

Read more about it [here](/docs/reactivesearch/react/theming/classnameinjection/).

## Extending

`ToggleButton` component can be extended to

1. customize the look and feel with `className`, `style`,
2. update the underlying DB query with `customQuery`,
3. connect with external interfaces using `beforeValueChange`, `onValueChange` and `onQueryChange`.

```js
<ToggleButton
  ...
  className="custom-class"
  style={{"paddingBottom": "10px"}}
  customQuery={
    function(value, props) {
      return {
        query: {
          match: {
            data_field: "this is a test"
          }
        }
      }
    }
  }
  beforeValueChange={
    function(value) {
      // called before the value is set
      // returns a promise
      return new Promise((resolve, reject) => {
        // update state or component props
        resolve()
        // or reject()
      })
    }
  }
  onValueChange={
    function(value) {
      console.log("current value: ", value)
      // set the state
      // use the value with other js code
    }
  }
  onQueryChange={
    function(prevQuery, nextQuery) {
      // use the query with other js code
      console.log('prevQuery', prevQuery);
      console.log('nextQuery', nextQuery);
    }
  }
/>
```

### className

| Type | Optional |
|------|----------|
|  `String` |   Yes   |

CSS class to be injected on the component container.
### style

| Type | Optional |
|------|----------|
|  `Object` |   Yes   |

CSS styles to be applied to the **ToggleButton** component.
### customQuery

| Type | Optional |
|------|----------|
|  `Function` |   Yes   |

takes **value** and **props** as parameters and **returns** the data query to be applied to the component, as defined in Elasticsearch Query DSL.
`Note:` customQuery is called on value changes in the **ToggleButton** component as long as the component is a part of `react` dependency of at least one other component.
### beforeValueChange

| Type | Optional |
|------|----------|
|  `Function` |   Yes   |

is a callback function which accepts component's future **value** as a parameter and **returns** a promise. It is called everytime before a component's value changes. The promise, if and when resolved, triggers the execution of the component's query and if rejected, kills the query execution. This method can act as a gatekeeper for query execution, since it only executes the query after the provided promise has been resolved.
 > Note:
>
> If you're using Reactivesearch version >= `3.3.7`, `beforeValueChange` can also be defined as a synchronous function. `value` is updated by default, unless you throw an `Error` to reject the update. For example:
 ```js
beforeValueChange = values => {
    // The update is accepted by default
	if (values.includes('Social')) {
		// To reject the update, throw an error
		throw Error('Selected value should not include Social.');
	}
};
```

### onValueChange

| Type | Optional |
|------|----------|
|  `Function` |   Yes   |

is a callback function which accepts component's current **value** as a parameter. It is called everytime the component's value changes. This prop is handy in cases where you want to generate a side-effect on value selection. For example: You want to show a pop-up modal with the valid discount coupon code(s) when button(s) is/are selected in a "Discounted Price" ToggleButton.
### onQueryChange

| Type | Optional |
|------|----------|
|  `Function` |   Yes   |

is a callback function which accepts component's **prevQuery** and **nextQuery** as parameters. It is called everytime the component's query changes. This prop is handy in cases where you want to generate a side-effect whenever the component's query would change.
### index

| Type | Optional |
|------|----------|
|  `String` |   Yes   |


The index prop can be used to explicitly specify an index to query against for this component. It is suitable for use-cases where you want to fetch results from more than one index in a single ReactiveSearch API request. The default value for the index is set to the `app` prop defined in the ReactiveBase component.
 
## Examples

<a href="https://opensource.appbase.io/playground/?selectedKind=Base%20components%2FToggleButton" target="_blank">ToggleButton with default props</a>