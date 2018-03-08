# Remaining tasks for the Film App

### TASK 1: Update `Faves` counter

Currently in the browser, the `faves` counter the user sees is always 0. You'll update the counter in the `FilmListing` to accurately show the number of faves in the array.

If you look at what's rendered in the `FilmListing` component, right now the faves counter is hardcoded to `0`. Replace that with the length of the `faves` array that is received through the props.

You now have favorites properly stored and available to all components, and you have a counter that accurately reflects that to the user.

Great job! Check it out in your browser.

### TASK 2: Move the details event handler up component tree from `FilmRow`

In `FilmRow`, there's still the function to handle when a user clicks a row for more details.

Following the same steps as you did for the `Fave` event handler, move the `handleDetailsClick` definition to the `App` component.

For `handleDetailsClick` in the `App` component, just log to the console and set the `current` state to the passed film for now. You'll handle looking up film details later. Make sure you pass the `current` state as a `film` prop to the `FilmDetails` component.


#### Step 1: Move `handleDetailsClick` to the `App` component

- In the `App` component, create a `handleDetailsClick()` function. It doesn't need to do anything yet, but soon you will display film details when a FilmRow is clicked on. The `handleDetailsClick` function should accept a film object as an argument (this will be the film that the user is clicking on).


#### Step 2: Bind the handler to the component

As you saw previously, you need to bind your custom component methods to ensure `this` refers to the component within the body of the method.

Add the following to the `App` component's constructor:

```js
this.handleDetailsClick = this.handleDetailsClick.bind(this)
```

#### Step 3: Update the `current` state

This click event will change the value of `current` in the App's state. Since we are having the clicked film passed in as an argument, we can use that to set the new state.

To do this, you need to call `setState` and give it the updated film (you can't just update it directly; otherwise React won't know to re-render the components to reflect the changes). Set the `current` in the state to be the `film` passed into the function.

#### Step 4: Pass the `handleDetailsClick` function to `FilmListing` through props

Now that the `handleDetailsClick` method lives on the `App` component, you want to pass it all the way down the tree so that you can call it when the `FilmRow` is clicked.

In the `App` component's `render` method, add a new prop to the `FilmListing` component called `onDetailsClick`. Its value should be a reference to the `handleDetailsClick` method you just finished writing.

#### Step 5: Pass the `onDetailsClick` function to `FilmRow` through props

In the `FilmListing` component, you render one `FilmRow` component for each film in the `films` prop. You need to pass the `onDetailsClick` function down to each `FilmRow`, but you want to ensure that it passes the current film up to the `handleDetailsClick` method in the `App` component when called.

To make this happen, you won't simply pass the function down to `FilmRow` as a prop as-is; you'll wrap it in another function that simply calls the `onDetailsClick` function passed down from App through props (remember, `onDetailsClick` in `FilmListing` is just a reference to `handleDetailsClick` in the `App` component).

In the `FilmListing` component's `render` method, add the `onFaveToggle` variable. Replace your existing `map` function with this:

```js
let allFilms = films.map( (film,index) => {
  return(
    <FilmRow onFaveToggle={() => this.props.onFaveToggle(film)}
      onDetailsClick={() => this.props.onDetailsClick(film)}
      title={film.title}
      date={film.release_date}
      key={film.id}
      url={film.poster_path}
      isFave={ faves.includes(film) } />
  )
})
```

#### Step 6: Update `FilmRow` to call the new handler `onDetailsClick` passed via props

In the first line of the JSX in our `FilmRow`, we are calling the handler but we are still trying to find it in the current file.

Update the line to let it call the `onDetailsClick` that was passed into it via props.


### TASK 3: Make the filter work on `FilmListing`

You have the `filter` state on `FilmListing`, but you still need to make it actually change the UI. You're **not** going to move the `filter` state because this filter only affects the `FilmListing`, not any other parts of the app.

Add a conditional in `FilmListing` so that if the `filter` state is set to `faves`, the listing only shows films in the faves array. Otherwise, it shows all films.

Try it out - you should be able to add films to your favorites and view just your favorites list by clicking that tab.
