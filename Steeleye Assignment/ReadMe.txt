Question-1. Explain what the simple List component does?

- Simple list component accepts various props and renders a list item on the page and background color of list item
  is determined by is Selected prop. If item is selected  then background color of item is green else background color
  is red by default.

---------------------------------------------------------------------------------------------------------------------------

Questions-2. What problems / warnings are there with code?

- Defining a default prop as null is not recommended.

WrappedListComponent.defaultProps = {
  items: null,
};

- Unique key prop is missing for List items.

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

- Syntax errors in the following code. ShapeOf would be shape and array should be arrayOf.

 WrappedListComponent.propTypes = {
   items: PropTypes.array(
     PropTypes.shapeOf({
       text: PropTypes.string.isRequired,
     })
   ),
 };

- Syntax error in useState() hook.

 const [setSelectedIndex, selectedIndex] = useState();

- In list item's onClick method there is a function call but onClick accepts function's reference.

 <li style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler(index)}>
       {text}
 </li>

- Passing a number selectedIndex to isSelected which should be a bool

<ul style={{ textAlign: "left" }}>
       {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
</ul>

---------------------------------------------------------------------------------------------------------------------------

Questions-3. Please fix, optimize, and/or modify the component as much as you think is necessary.

import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

const WrappedSingleListItem = ({
    index,
    isSelected,
    onClickHandler,
    text,
}) => {
    return (
        <li
            style={{ backgroundColor: isSelected ? 'green' : 'red' }}
            // onClick={onClickHandler(index)}
            onClick={() => onClickHandler(index)}
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

const WrappedListComponent = ({ items, }) => {

    const [selectedIndex, setSelectedIndex] = useState();

    useEffect(() => {
        setSelectedIndex(null);
    }, [items]);

    const handleClick = index => {
        setSelectedIndex(index);
    };

    return (
        <ul style={{ textAlign: 'left' }}>
            {items.map((item, index) => (
                <SingleListItem
                    key={index}
                    onClickHandler={() => handleClick(index)}
                    text={item.text}
                    index={index}
                    // isSelected={selectedIndex}
                    isSelected={Boolean(selectedIndex)}
                />
            ))}
        </ul>
    )
};

WrappedListComponent.propTypes = {
    items: PropTypes.arrayOf(PropTypes.shape({
        text: PropTypes.string.isRequired,
    }).isRequired
    ).isRequired
};

WrappedListComponent.defaultProps = {
    items: undefined
};

const List = memo(WrappedListComponent);

export default List;