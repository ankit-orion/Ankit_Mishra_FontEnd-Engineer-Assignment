## 1. Explain what the simple ``` List ``` component does. 

In websites, occasionally we want our data to be displayed in ordered format and this can be done by using List and the given code does the following things.
* The List component is a customizable and reusable React component that displays a list of items with selectable options. It receives an array of objects as a prop, where each object contains a "text" property that represents the text to be displayed for that item.

* The component is made up of two sub-components: the SingleListItem component and the List component itself. The SingleListItem component is responsible for rendering an individual item within the list, and receives several props including an index, isSelected flag, onClickHandler function, and text.

* The List component maps through the items array and renders a SingleListItem component for each item, passing down the necessary props. When a SingleListItem component is clicked, it triggers the handleClick function in the parent List component, which updates the selectedIndex state to the index of the clicked item. The isSelected prop passed to each SingleListItem component is based on whether its index matches the selectedIndex value in the parent List component, and this causes the background color of the SingleListItem component to change based on whether it is selected or not.

## 2. What problems / warnings are there with code?
* In the propTypes declaration for the List component, ```shapeof ``` and ``` array ``` should be changed to ```shape``` and ``` arrayOf ```.
```
  WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({
    text: PropTypes.string.isRequired,
  })),
};
```

Resolved Code:
```
  WrappedListComponent.propTypes = {
       items: PropTypes.arrayOf(PropTypes.shape({ 
           text: PropTypes.string.isRequired,
   })),
   };
```

* The items array was declared as null by default, causing a Cannot read properties of null (reading ```map```) error. To fix this, we can initialize it with some default values.<br></br>
Resolved Code:
```
WrappedListComponent.defaultProps = {
  items: [
    { text: 'Default List Name 1', index: 1 },
    { text: 'Default List Name 2', index: 2 },
    { text: 'Default List Name 3', index: 3 },
    { text: 'Default List Name 4', index: 4 },
    { text: 'Default List Name 5', index: 5 },
  ],
};
```

* Another error was in the declaration of the ```selectedIndex``` state variable, where ```setSelectedIndex``` was being assigned to the first value instead of the second. Additionally, the ```useEffect``` hook had a missing dependency, causing a warning to be thrown.<br></br>
Resolved Code:
```const [selectedIndex, setSelectedIndex] = useState(''); ```

* The ```isSelected``` prop was being passed as a string instead of a boolean, causing a Invalid prop ```isSelected``` of type string supplied to ```WrappedSingleListItem```, expected boolean error. This was fixed by passing a boolean value instead.
Error.<br></br>
```const handleClick = index => { setSelectedIndex(index); };```<br></br>
Resolved Code:<br></br>
```const handleClick = index => { setSelectedIndex(true); }; ```

## 3. Please fix, optimize, and/or modify the component as much as you think is necessary.
Resolved Code:
```

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
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
    const [selectedIndex, setSelectedIndex] = useState('');  /*Error resolved*/

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(true);  /*Error resolved*/
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({         /*Error resolved*/
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
    items: [{text: "Default List Name 1", index:1}, {text: "Default List Name 2", index:2}, {text: "Default List Name 3", index:3}, {text: "Default List Name 4", index:4}, {text: "Default List Name 5", index:5}],   /*Error resolved*/
  };

const List = memo(WrappedListComponent);


export default List;
```
![Screenshot 2023-04-22 004410](https://user-images.githubusercontent.com/72105760/233716977-c1a04c86-93c5-4593-a966-2650d9efe458.png)


