<h2>Explain what the simple List component does.</h2>
<p>List Component displays List of items with some texts.The wrappedListComponent returns the components list. It takes an array  of strings as props. For each items in the array, it maps through all and displays their content on the screen with a red background color. If one items is selected, then it displays it with a different background color (Green).
 

What problems / warnings are there with code?

errors:
1.Syntax error of PropTypes in WrappedListComponent. It should be arrayOf instead of shapeOf.
<code>WrappedListComponent.propTypes = {
    items: PropTypes.arrayOf(
        PropTypes.shape({
            text: PropTypes.string.isRequired
        })
    )
};</code>


1.TypeError: prop_types__WEBPACK_IMPORTED_MODULE_2___default(...).shapeOf is not a function
2.Uncaught TypeError: Cannot read properties of null (reading 'map')
3.List.js:38 Uncaught TypeError: setSelectedIndex is not a function
4.
5.
6.




Warnings:
1.Uncaught Invariant Violation: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them.


Please fix, optimize, and/or modify the component as much as you think is necessary.