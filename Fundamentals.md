1. What is SPA
2. Why should we use React, what are the main features React provides- composability, reusable components
3. What react lacks due to which we call it a library, not a framework
4. what is the role of transpiler like babel?
5. Why does vite recommend to use jsx extension for any js file which has jsx code inside it? How does it help bundler like vite?
6. What is the role of bundler like vite, webpack, parcel in front end ?
7. why do we need to wrap the elements inside one single parent element in react components?
8. what is composable means in react? We have small pieces that we can put together to make something
larger or greater than the individual pieces themselves.
9. Declarative vs Imperative - *Imperative* means we need to give specific step-by-step instructions on how to
accomplish a task.
*Declarative* means we can write our code to simply "describe" *what* should show up
on the page and allow the tool (React, e.g.) to handle the details on *how* to 
put those things on the page.
10. what is react component vs react element? React component is a function that returns react elements.
11. lazy state initialization :  const [dice, setDice] = useState(() => generateAllNewDice())
12. diff between props vs compound component pattern- In the normal props based components, the philosophy is you(parent component) define the configuration via props and I will decide the UI structure. compound components take the opposite approach, It says you (parent component) decide the structure and I(child component) will provide the shared behaviour.
13. props - how to send data from parent to child - via props, how to send data from child to parent - define a function in the parent that receives data, pass that function as prop to child component, call that function inside the child component and pass the data. how to call child method from parent- useImperativeHandle / React.forwardRef
14. when the prop changes, the consuming component rerenders and you can detect which prop has changed by having separate useEffect hooks 
15. when a state variable changes, the parent component re renders and if there is any child component inside the parent then that child also re renders. This re render is independent of whether child receives a prop or not. Incase you don't want to rerender the child component , then a lame solution can be to memoize the child component using React.memo , better solution is to use children prop pattern.
 props are immutable, only i development mode and in production mode, react does not restrict.
16. children prop pattern or composition pattern- use case polymorphic component, high reusability , high customization, prevent re renders , alternative of memo in some cases
17. In compound components, we flatten the structure and it helps us avoid the problems of prop drilling. It helps us pass props easily to the nested components.
18. In compound components we create several sub components which work together to achieve a functionality rather than relying on one single component. In react compound component use children props. Have dedicated functionality or styling, it makes the component structure more transparent, and give more control to the user of the component.
19. Compound component "flatten" the hierarchy that I would otherwise need to pass props through. Since I need to provide the children to render, the parent-most component has direct access to those "grandchild" components, to which it can pass whatever props it needs to pass directly. In compound component, composed components work together and usually share state or behavior.
20. what is component composition in react? - Instead of creating one component that does everything, build small components with clear responsibilities and combine them to build more complex UI.
And children, props containing JSX, and compound components are simply different ways to achieve that composition.
21. Headless component in react - Headless component in react does not have any styled UI to display , they are purely intended for providing functionality
