
# React Component Code optimization


## 

#### Q.1) Explain what the simple List component does.

Answer:List Component displays List of items with some texts.The wrappedListComponent returns the components list. It takes an array  of strings as props. For each items in the array, it maps through all and displays their content on the screen with a red background color. If one items is selected, then it displays it with a different background color (Green).

#### Q.2) What problems / warnings are there with code?

Answer:



1. Syntax error of PropTypes in WrappedListComponent. It should be arrayOf instead of array and shape instead of shapeOf.

```bash
WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};
```
solution:
```bash
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};
```


2. syntax error when initializing useState()

```bash
const [setSelectedIndex, selectedIndex] = useState();
```
solution:
```bash
const [selectedIndex, setSelectedIndex ] = useState(0);
```

3. WrappedListComponent.defaultProps has items: null. Therefore there is no item to be mapped.

```bash
WrappedListComponent.defaultProps = {
  items: null,
};
```
solution:
```bash
WrappedListComponent.defaultProps = {
  items: [{text:'Frontend',isSelected:false},{text:'Backend',isSelected:false},{text:'FullStack',isSelected:false},{text:'MERNstack',isSelected:false}],
};
```
4. onclickHandler is rendered automatically. it should be called as a call back arrow function

```bash
onClick={onClickHandler(index)}
```
solution:
```bash
onClick={()=>onClickHandler(index)}
```
5. isSelected requires boolean type but it is providing null

solution:
#### in this case i have declared isSelected key for each list items

### Q.3)  Please fix, optimize, and/or modify the component as much as you think is necessary.

fixed, optimized,and modified code is given below
```bash

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

export default List;
```