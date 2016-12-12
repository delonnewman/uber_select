# UberSelect

UberSelect is a fancy UI on top of a regular `<select>` element.

## Requirements
jQuery

## Parts
UberSelect is composed of two main parts, an UberSearch that does all the of searching and UI, and an UberSelect which
is used to connect an UberSearch to a `<select>` element.

### UberSelect
This is the object that allows an UberSearch and a `<select>` element to work together. The select element can be used
to control the state of the UberSearch, and vice-versa. This means you can programmatically change the state of the
select, and UberSearch will update. Users can interact with the UberSearch, and the select will update. This also means
you can use UberSelect to gussy up forms, without changing any of the underlying inputs.

#### Options
Options can be specified by setting the `data-uber-options` attribute on the `<select>` element. Values should be passed
as a JSON string. All options on the are passed to the underlying UberSearch, see [UberSearch options](#UberSearchOptions).

- ##### prepopulateSearchOnOpen
  Determines whether the search input starts with the selected value in it when the pane is opened.

  Default: `false`

- ##### clearSearchClearsSelect
  Determines whether the select value should be cleared when the search is cleared.

  Default: `false`

- ##### placeholder
  Placeholder to show in the selected text area.

  Default: `<select>` element attributes `<select placeholder="my placeholder" data-placeholder="my placeholder">`

- ##### dataUrl
  A url to pre-fetch select options from. JSON response should be of the form
  `[{text:'option with explicit value', value: 'some value'}, {text:'option with implicit value'}]`.

  Default: `null`

- ##### value
  Initialize the UberSearch with this selected value

  Default: `<select>` element `value` property

#### Events Triggered
- ##### uber-select:ready
  This fires when the UberSelect has initialized and is ready for user interaction

#### Events Observed
The `<select>` element observes the following events:

- ##### uber-select:refreshOptions
  The UberSearch options list will be updated to match the `<select>` element's `<option>` list.

- ##### uber-select:refresh
  The UberSearch will update its selected value to match the `<select>` element's. This handler also runs when the
  `<select>` element triggers a `change` event.

### UberSearch
The UberSearch performs all of the heavy lifting. It creates the UI views, maintains state, and performs the searching.
It can be instantiated without the use of an UberSelect, which can be useful for situations where the selected value is
being used in purely in JS, and not being linked to a `<select>` element in a form.

#### Options <a name="UberSearch options"></a>
Options can be specified by setting the `data-uber-options` attribute on the `<select>` element. Values should be passed
as a JSON string.

- ##### value
  Sets the initially selected value of the UberSearch. This value should match the `value` property of the desired
  option data.

  Default: `null`

- ##### search
  Determines whether the search input be shown.

  Default: `true`

- ##### clearSearchButton
  Sets the text content of clear search button.

  Default: `✕`

- ##### selectCaret
  Sets the text content of clear select caret.

  Default: `⌄`

- ##### hideBlankOption
  Sets whether blank options should be hidden automatically.

  Default: `false`

- ##### treatBlankOptionAsPlaceholder
  Determines whether the `text` property of an option with a blank `value` property should be used as the placeholder
  text if no placeholder is specified.

  Default: `false`

- ##### minQueryLength
  Sets minimum number of characters the user must type before a search will be performed.

  Default: `0`

- ##### minQueryMessage
  Sets the message shown to the user when the query doesn't exceed the minimum length. `true` for a default message,
  `false` for none, or provide a string to set a custom message.

  Default: `true`

- ##### placeholder
  Sets the placeholder shown in the selected text area.

  Default: `null`

- ##### searchPlaceholder
  Sets the placeholder shown in the search input.

  Default: `'Type to search'`

- ##### noResultsText
  Sets the message shown when there are no results.

  Default: `'No Matches Found'`

- ##### buildResult
  A function that is used to build result elements.

  The function signature is as follows:
  ```js
    function(listOptionData) {
      return // HTML/element to insert into the the results list
    }
  ```

- ##### resultPostprocessor
  A function that is run after a result is built and can be used to decorate it. This can be useful when extending the
  functionality of an existing UberSearch implementation.

  The function signature is as follows:
  ```js
    function(resultsListElement, listOptionData) { }
  ```

  Default: No-op

- ##### onRender
  A function to run when the results container is rendered. If the result returns false, the default `select` event
  handler is not run and the event is cancelled.

  The function signature is as follows:
  ```js
  function(resultsContainer, searchResultsHTML) { }
  ```

- ##### onSelect
  A function to run when a result is selected. If the result returns false, the default `select` event handler is not
  run and the event is cancelled.

  The function signature is as follows:
  ```js
  function(listOptionData, resultsListElement, clickEvent) { }
  ```

- ##### outputContainer (Deprecated)
  An object that receives the output once a result is selected. Must respond to `setValue(value)` and `view()`. This object serves to
  attach the result list to the DOM at the desired location.

#### Events Triggered
- ##### shown
  This fires when the UberSearch pane is opened.

- ##### renderedResults
  This fires each time the list of results is updated.

- ##### clear
  This fires when the user clicks the clear search button.

- ##### select
  This fires when the user selects a result.

  The handler function signature is as follows:
  ```js
  function(event, [listOptionData, resultsContainer, originalEvent]) { }
  ```
