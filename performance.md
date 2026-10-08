1. What is recursive rendering in react?
Ans- React starts rendering from the root component and recursively traverses down the component/element tree. For each component, React executes it and processes the elements/components returned from it.
It continues this process for the children until it reaches the leaf/host elements such as <div>, <button> , etc
This recursive traversal allows React to build and compare the UI tree during reconcilliation.
