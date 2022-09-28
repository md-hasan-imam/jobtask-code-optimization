<h3>Question 1: Explain what the simple List component does.</h3>
<p>List Component displays List of items with some texts.The wrappedListComponent returns the components list. It takes an array  of strings as props. For each items in the array, it maps through all and displays their content on the screen with a red background color. If one items is selected, then it displays it with a different background color (Green).
 

<h3>Question 2: What problems / warnings are there with code?</h3>

1.Syntax error of PropTypes in WrappedListComponent. It should be arrayOf instead of array and shape instead of shapeOf.
<code>
WrappedListComponent.propTypes = { <br>
    items: PropTypes.array(<br>
        PropTypes.shapeOf({<br>
            text: PropTypes.string.isRequired<br>
        })<br>
    )<br>
};</code>
2. syntax error when initializing useState
<code>const [setSelectedIndex, selectedIndex] = useState();</code>

2.Uncaught TypeError: Cannot read properties of null (reading 'map')
WrappedListComponent.defaultProps has items: null. Therefore there is no item to be mapped.
<code>WrappedListComponent.defaultProps = {
  items: null,
};</code>

3.List.js:38 Uncaught TypeError: setSelectedIndex is not a function
4.
5.
6.




Warnings:
1.Uncaught Invariant Violation: Calling PropTypes validators directly is not supported by the `prop-types` package. Use `PropTypes.checkPropTypes()` to call them.


<h3> Please fix, optimize, and/or modify the component as much as you think is necessary.</h3>
<p>optimized code given below:</p>
<snippet> 
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red', listStyleType:'none',padding:'10px'}}
      onClick={()=>onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes= {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items}) => {
  const [selectedIndex, setSelectedIndex ] = useState(0);

  const handleClick = (index) => {
    setSelectedIndex(index);
    items[index].isSelected = !items[index].isSelected;

  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={item.isSelected}
          key={index}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
    isSelected: PropTypes.bool,
  })),
};

WrappedListComponent.defaultProps = {
  items: [{text:'Frontend',isSelected:false},{text:'Backend',isSelected:false},{text:'FullStack',isSelected:false},{text:'MERNstack',isSelected:false}],
};

const List = memo(WrappedListComponent);

export default List;</snippet>
