# REDUX-REDUCER

- THE store.js file structure

        - import createSlice -
        - import any data - 
              - InitialState Object-
              - Action Creator Functions -
                    - Contain the Actions
              - Single Reducer -
                    - Passed the current state and action
                    - Returns the store's next state
        - Export Action Creators -
        - Export Reducer -
        - Export Store -
### The STATE
- state = {
      feature1: [ { array },{ of },{ objects } ],
      feature2: 'string'
  }
### The InitialState Object
- The initialState provides how many features or slices are in the apps state.
- Create InitialState object which includes properties/features/slices and their initial values.  ‘sliceName/actionName’

         const initialStateObject = {
             property1: property1InitialValue,
             porperty2: property2InitialValue,
         }
### Actions
- Dertermine types of action to dispatch
- How to trigger changes to these slice with actions
- Actions are js objects with a type: 'sliceName/actionDescriptor' and payload: action.payload
- They are dispatched with store.dispatch( store.dispatch({ type: 'searchTerm/setSearchTerm', payload: 'Spaghetti' });)
### Action Creators
- Remember, action creators functions return an action object. We use a function so the action can be re-used whenever action is required.
- AC are dispatched by user interaction and the action is sent to STORE

        const actionName = (data) {
              return {
                   type: 'sliceName/actionName',
                   payload: data
                 }
        }
- Action creator passes parameters to store as payload

        // Dispatched when the user types in the search input.
        // Sends the search term to the store.
        export const setSearchTerm = (term) => {
           return { 
               type: 'searchTerm/setSearchTerm', 
               payload: term 
             };
        }
### Reducer
- The stores reducer receives the initialState and will execute all the possible changes that can occur to the state.

      const reciperReducer = (state=initialState(default), action) => {
       
        switch(action.type){
          case 'allRecipes/loadData':
            return{...state, allRecipes: action.payload }
          case 'searchTerm/clearSearchTerm':
            return{...state, searchTerm:''}
          case 'searchTerm/setSearchTerm':
            return {...state, searchTerm: action.payload}
          case 'favoriteRecipes/addRecipe':
            return {...state, favoriteRecipes: [...state, action.payload] }
          case 'favoriteRecipes/removeRecipe':
            return {..state, favoriteRecipes: state.favoriteRecipes.filter(favorite = >
                                                  favorite.id !== action.payload.id)}
          default:
            return state;
        }
      }
 //Within the switch statement of recipesReducer( )
 ##### Action Types
- `searchTerm/setSearchTerm` action type (action).</br>
          - Dispatched with a payload value = the new term value for `state.searchTerm`.
          - Action: The reducer returns a new state object searchTerm slice = action.payload new Term.
- `setSearchTerm() action creator` passes the new term to the recipesReducer as action.payload property. 

      case 'sliceName/actionType':
        return {
          ...state, sliceName: action.payload
              // ...state 1st Reversing the order will overwrite any changes
        };

- 'favoriteRecipes/addRecipe' action type (action)
          - Dispatched with a payload Value = recipe object added to state.favoriteRecipes array.
          - Action: reducer returns a new state object with an updated favoriteRecipes slice array.
          - New array of `favoriteRecipes` previous values and the new recipe (action.payload) at end
- NOTE: Don't mutate 1. The incoming state object or 2. The original state.favoriteRecipes array.

- Add a new value to a slice without mutating the original state, use the spread operator (...):

        case 'sliceName/actionType':
              return    { ...state, slice: [...state.slice, action.payload]
              }

- 'favoriteRecipes/removeRecipe' action type
          - Dispatched with a payload Value = recipe object to remove from state.favoriteRecipes array.
          - Action: Reducer returns a new state object with an updated favoriteRecipes slice.
          - The new `favoriteRecipes` slice array includes existing values of state.favoriteRecipes the 1 recipe = action.payload.
          - To prevent state mutation use .filter() array method.
          - Filter out the element whose 'id' matches the recipe = action.payload

      case 'sliceName/actionType':
      return  { ...state,
              sliceName: state.sliceName.filter(element => element.id !== elementToRemove.id)
            }
- The elementToRemove value = action.payload.
  #### - So action.payload.id
- NOTE: Don't mutate the state object or state.favoriteRecipes array!

### For complex states with multiple reducers. Solution = Reducer Composition Pattern

#### When action dispatched to store
- rootReducer calls slice reducer with the action and slice of state
- Slice reducers decide if they need updating or return their slice unchanged
- RootReducer reassembles the updated slice values in a new object
- PROS: Each slice reducer only receives its slice of the entire application’s state
- Resctructe the above Reducer 3 slice reducer
- Replace initialState object with initialSliceName variables
- Variable is the default value for each slice reducer's slice of state.
        - eg. const initialAllRecipes = []
        - (param = 'default value') => {}
        - eg. const allRecipesReducer = (allrecipes = initialAllRecipes) => {
                  switch statement}
- Create Individual _**slice reducers**_ responsible for only one slice of the store's state

        - Reducer 1 = const allRecipesReducer(state,action)=> {switch cases}
        - Reducer 2 = const searchTermReducer(state,action)=> {switch cases}
        - Reducer 3 = const favoriteRecipesReducer(state,action)=> {switch cases}
- rootReducer calls each slice reducer to update their slices of state
- Each slice of state only recieves its slice of state not the state object
- So the each reducers()=> returned value = new array with the prior slice coped plus any changes.
        - return [newArray-->priorSlice, changes]
        - return [...sliceNameStatePriorValue, action.payload]
- favoritesRecipesReducer() receives favoritesRecipes slice of state
        - return [...favoriteRecipes, action.payload];
- To combine allReducersSlicesOfState() with rootReducer()
- 

        const nextState = {
          sliceA: sliceAReducer(state.sliceA, action),
          sliceB: sliceBReducer(state.sliceB, action)
        }
- favoriteRecipesReducer() updates the slice of state `state.favoriteRecipes`
- It responds to these action.type cases
- 'favoriteRecipes/addRecipe':
        - case 'favoriteRecipes/addRecipe':
                  return [...favoriteRecipes, action.payload];        
 
- 'favoriteRecipes/removeRecipe':
        - case 'favoriteRecipes/removeRecipe':
                 return [ favoriteRecipes.filter(favorite=>favorite.id !== action.payload.id)}
- 'default':
          - default:
                  return favoriteRecipes;
        
        const rootReducer = (state = {}, action) => {
          const nextState = {
            allRecipes: allRecipesReducer(state.allRecipes, action),
            searchTerm: searchTermReducer(state.searchTerm, action),
            // Add in the favoriteRecipes slice with favoriteRecipesReducer function. 
            favoriteRecipes: favoriteRecipesReducer(state.favoriteRecipes, action)
          } 
          return nextState;
        }        

### Reducer Composition Pattern: Redux has a combinReducer() function to create a rootReducer for you  
- `Create a reducer object` with the slice reducers
        - The keys are all the slices the reducer manages
        - The Key value = reducer function but don't call function
  
        const reducers = {
          sliceA: sliceAReducer, // Right.
        };

- combineReducers() function accepts the reducer object to `return a reducer function`

        const rootReducer = combineReducers(reducers);
- The reducer function is passed to createStore() to create a `store` object:

        const store = createStore(rootReducer);
- So the original boilerplate code:
        
        import { createStore } from 'redux';
        // todosReducer and filterReducer omitted
        
        const rootReducer = (state = {}, action) => {
          const nextState = {
            todos: todosReducer(state.todos, action),
            filter: filterReducer(state.filter, action)
          };
          return nextState;
        };
        
        const store = createStore(rootReducer);

### Simplifies to

        const store = createStore(combineReducers({
            todos: todosReducer,
            filter: filterReducer
        }));
- Action is dispatched to the store, rootReducer is executed by calling each slice reducer with its action and its slice of state

### Refractoring for Large Redux Files
- The store can get very big expecially when the store has 10,12 or more slices.
- Best practice is to break it up with the REDUX DUCK PATTERN
- Where Redux logic is in top level directories called src/
        - src/index.j
        - src/app
             -app/store
             -app/App.js
        - src/components
             -components/reactfileName.js
        - src/features
             - sliceFolders
                  - sliceNameSlice.js
                  - sliceNameReactComponent.js
- The sub directories contain all the code for individual slices of the stores state
- This refactor involved exporting each of the slice reducers and action creators, so that they could be imported back into store.js.
1. The slice reducers should exist in separate files.
- The store.js new structure is:
        
          --store.js--
            Section 1
            import { createStore, combineReducers } from 'redux';
          
            Section 2
            //Import the slice reducers from other files
            import { sliceNameReducer } from 'file location../featuresSliceName/sliceNameSlice.js'
        
            Section 3
            //Construct reducer object
            const reducers = { sliceName: sliceNameRducer,}
        
            Section 4
            //Declare store
            const store = createStore(combineReducers(reducersObject))
        
            Section 5
            //Export store so available
            export store;

### Pass Store Data through Top Level React Components
NOTE: React apps top level component <App/> renders feature <components/> and passes data needed by those components as prop values.
Redux passes data to feature <components/>:
1. The components slice of the store's state
2. store.dispatch method to trigger state changes after user interaction
Hence these are passed to <App/> component but not in the <App/> componenet itself like in react but in index.js
First import { store } from its file './app/store.js'
Second pass props 1. state = current state of store and 2. dispatch = store.dispatch method
Lastly
3. Subscribe render to changes to the `store` with store.subscribe(render) so <App/> re-renders with all changes
            <App 
              state={store.getState()}
              dispatch={store.dispatch}
            />
### Using the Store's Data within Features Components

Passing the current state of the store and its dispatch mehtod to the top level component <App/> allows it to pass them to each feature component.

All Redux React app with feature components follow this structure
- import the React feature component into the top level App.js file
  --App.js--
   import FavoriteRecipes from '../features/favoriteRecipes/FavoriteRecipes';
- Render each feature component passing slice of state and dispatch as props
  const function App(props){
          const {state, dispatch} = props;
       return (
                <FeaturesRecipes
                  featureRecipes={visibleFavoriteRecipes}
                  dispatch={dispatch}
                 />
  }
- Within the feature component (which has 2 jobs)
  - Extract the slice of state and disptach from props

    const FavoriteRecipes = (props) =>{
    // Extract dispatch and favoriteRecipes slice
    const { favoriteRecipes, dispatch } = props;
  - Component has access to state of slice now
  - Render the components with its states slice

    const FavoriteRecipes = (props) =>{
            //other code ommitted
          return(
    {favoriteRecipes.map(createRecipeComponent)}
    )
  - import action creators from the slice file

     import { removeRecipe }  from './favoriteRecipesSlice.js';
  - dispatch action in response to user inputs.
 
    const onRemoveRecipeHandler = (recipe) => {
    // Dispatch a removeRecipe() action.
    // with removeRecipe() action creator
      dispatch(removeRecipe(recipe));
  };

