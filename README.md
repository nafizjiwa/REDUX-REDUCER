# REDUX-REDUCER

- IN THE STORE ACTION CREATORS, AND REDUCERS.
- state = {
      feature1: [ { array },{ of },{ objects } ],
      feature2: 'string'
  }
- The initialState provides how many features or slices are in the apps state.
- Create InitialState object which includes properties/features/slices and their initial values.  ‘sliceName/actionName’

         const initialStateObject = {
             property1: property1InitialValue,
             porperty2: property2InitialValue,
         }
- Dertermine types of action to dispatch
- How to trigger changes to these slice with actions
- Actions are js objects with a type: 'sliceName/actionDescriptor' and payload: action.payload
- They are dispatched with store.dispatch( store.dispatch({ type: 'searchTerm/setSearchTerm', payload: 'Spaghetti' });)
- Remember, action creators are functions that return a formatted action object. We use a function so the action can be re-used whenever action is required and sent to STORE.

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
- The stores reducer receives the initialState and will execute all the possible changes that can occur to the state.

      const reciperReducer = (state-initialState, action) => {
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
            return {..state, favoriteRecipes: state.favoriteRecipes.filter(favorite=>favorite.id !== action.payload.id)}
          default:
            return state;
        }
      }

- First up is the searchTerm/setSearchTerm action. This action will be dispatched with a payload whose value is the term to be set as the new value for state.searchTerm.

- Within the switch statement of recipesReducer(), fix the case that handles the 'searchTerm/setSearchTerm' action type.

- For this case, the reducer should return a new state object with an updated searchTerm slice set to the new term provide- d by action.payload.

- Take a look at the setSearchTerm() action creator and you’ll see that the new term is passed to the recipesReducer in the action.payload property. Your code should look something like this:

      case 'sliceName/actionType':
        return {
          ...state,
          sliceName: action.payload
        };
- Make sure that you put ...state BEFORE you update the slice. Reversing the order will overwrite any changes you made.

-
- This action will be dispatched with a payload whose value is the recipe object to be added to the state.favoriteRecipes array.
- For this action type, the reducer should return a new state object with an updated favoriteRecipes slice.

- The new value should be a new array that includes all the previously added values in addition to the new recipe (from action.payload) added to the end.
- Remember, you must not mutate the incoming state object or the original state.favoriteRecipes array!

- To add a new value to a slice that is an array without mutating the original state, you can use the spread operator (...) to return an object like this:

        case 'sliceName/actionType':
        return    {
                ...state,
                slice: [...state.slice, action.payload]
              }

This action will be dispatched with a payload whose value is the recipe object to be removed from the state.favoriteRecipes array.

For this case, the reducer should return a new state object with an updated favoriteRecipes slice.
The favoriteRecipes slice should be a new array that includes all the existing values from state.favoriteRecipes except for the recipe from action.payload.

We recommend that you use the .filter() array method and filter out the element whose 'id' matches the recipe from action.payload.

To remove a value from a slice that is an array without mutating the original state, you can use the .filter() method:

      case 'sliceName/actionType':
      return  {
              ...state,
              sliceName: state.sliceName.filter(element => element.id !== elementToRemove.id)
            }
In this case, the elementToRemove is the action.payload value.

Remember, you must not mutate the incoming state object or the original state.favoriteRecipes array!
