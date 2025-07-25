# REDUX-REDUCER

- THE store.js file structure

        - import createSlice -
        - import any data - 
              - InitialState Object-
              - Action Creator Functions -
                    - Contain the Actions
              - Reducer -
                    - Passed the current state and action
                    - Returns the store's next state
        - Export Action Creators -
        - Export Reducer -
        - Export Store -
- IN THE STORE fileACTION CREATORS, AND REDUCERS.
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
            return {..state, favoriteRecipes: state.favoriteRecipes.filter(favorite=>favorite.id !== action.payload.id)}
          default:
            return state;
        }
      }
 //Within the switch statement of recipesReducer()
- The searchTerm/setSearchTerm action. This action is dispatched with a payload whose value = term which is set as the new value for state.searchTerm.

- Within the switch statement of recipesReducer(), fix the case that handles the 'searchTerm/setSearchTerm' action type.

- The reducer should return a new state object with searchTerm slice set to action.payload.

- The setSearchTerm() action creator passes the new term to the recipesReducer in the action.payload property. 

      case 'sliceName/actionType':
        return {
          ...state, sliceName: action.payload
              // Reversing the order will overwrite any changes
        };

- The 'favoriteRecipes/addRecipe' action type is dispatched with a payload.
- Payloads Value = recipe object to be added to the state.favoriteRecipes array.
- For this action type, the reducer should return a new state object with an updated favoriteRecipes slice.

- The new value is a new array that includes all previous values with the new recipe (action.payload) at the end.
- NOTE, The incoming state object or the original state.favoriteRecipes array can't be mutated!

- To add a new value to a slice without mutating the original state, use the spread operator (...):

        case 'sliceName/actionType':
              return    { ...state, slice: [...state.slice, action.payload]
              }

- The 'favoriteRecipes/removeRecipe' action type is dispatched with a payload.
- Payload value = recipe object to be removed from the state.favoriteRecipes array.
- The reducer should return a new state object with an updated favoriteRecipes slice.
- The favoriteRecipes slice new array includes all the existing values from state.favoriteRecipes except for the recipe from action.payload.
- Use .filter() array method. Filter out the element whose 'id' matches the recipe = action.payload
      - This will not mutate the original state.

      case 'sliceName/actionType':
      return  { ...state,
              sliceName: state.sliceName.filter(element => element.id !== elementToRemove.id)
            }
- The elementToRemove value = action.payload.
- NOTE: Don't mutate the state object or state.favoriteRecipes array!
