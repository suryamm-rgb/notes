# Rules of Hooks (React)

## 1. Call Hooks at the Top Level

Hooks should always be called at the top level of a React component.

Do not place hooks inside conditions, loops, or nested functions.

------------------------------------------------------------------------

## 2. Call Hooks Before Any Early Returns

Hooks must run before any return statement.

Wrong:

``` jsx
if(!user){
 return <Login />
}

const [name,setName] = useState("");
```

Correct:

``` jsx
const [name,setName] = useState("");

if(!user){
 return <Login />
}
```

------------------------------------------------------------------------

## 3. Do Not Call Hooks Inside Loops

Never call hooks inside for loops, while loops, or array methods like
map.

------------------------------------------------------------------------

## 4. Do Not Call Hooks Inside Conditions

Do not use hooks inside if, else, or switch blocks.

------------------------------------------------------------------------

## 5. Do Not Call Hooks Inside Nested Functions

Hooks should not be inside event handlers or inner functions.

------------------------------------------------------------------------

## 6. Only Call Hooks From React Functions

Hooks can only be used inside:

-   Functional components
-   Custom hooks

------------------------------------------------------------------------

## 7. Custom Hooks Must Start With "use"

Example:

``` jsx
function useAuth(){

}
```

React recognizes custom hooks using the use prefix.

------------------------------------------------------------------------

## 8. Keep Hook Order Same Between Renders

React tracks hooks by their order.

Do not add or remove hooks conditionally.

------------------------------------------------------------------------

## 9. Do Not Call Hooks Based on Props or State

Wrong:

``` jsx
if(isLoggedIn){
 useEffect(()=>{})
}
```

Correct:

``` jsx
useEffect(()=>{

 if(isLoggedIn){

 }

},[isLoggedIn])
```

------------------------------------------------------------------------

## 10. Keep Hooks Easy to Find

Prefer keeping hooks near the top of the component.

Example:

``` jsx
function App(){

 const [user,setUser] = useState(null);
 const [loading,setLoading] = useState(false);

 useEffect(()=>{

 },[])

 return <div />

}
```

------------------------------------------------------------------------

# Summary

  Rule                           Reason
  ------------------------------ --------------------------------
  Top level only                 Keeps hook order stable
  Before return                  Hooks run every render
  No loops                       Prevents different hook counts
  No conditions                  Prevents changing order
  No nested functions            Keeps React lifecycle control
  Only components/custom hooks   React manages hooks
  Custom hooks use use prefix    React identifies hooks
  Same order every render        React tracks state correctly
