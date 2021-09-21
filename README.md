<img src="https://i.imgur.com/Yysn5IA.png">

# Intro to React Hooks
---

## Learning Objectives

| Students Will Be Able To: |
| --- |
| Explain the use case (what & why) of hooks |
| Use the `useState` hook to use state in function components |
| Use the `useEffect` hook to perform "side effects" in function components |

## Road Map

- Intro
- Set Up
- Review the Starter Code
- Why use React Hooks?
- What are Hooks?
- Using Hooks
- Rules of Using Hooks
- Essential Questions
- Custom Hooks

## Intro

[Hooks](https://reactjs.org/docs/hooks-intro.html) are an exciting addition to React versions 16.8+.

This lesson will cover the use case of hooks and demonstrate how to use the two most common React hooks.

## Set Up

Set up for this lesson will be a little more involved. First we'll create a new branch and pull down the solution code. After that branch has been created we can then create a second branch to practice using hooks by refactoring one of the gamelib components. 

*Note: You will need to start up your gamelib-api so your react frontend can request data.*
```
$ mkdir react-hooks
$ cd react-hooks
$ git clone https://github.com/SEIR-7-06/mern-api-starter-code
$ git clone https://github.com/SEIR-7-06/mern-react-starter-code
$ cd mern-api-starter-code
$ npm install
$ code .

(in VS Code terminal) 
$ npm start

(return to regular terminal)

$ cd ..
$ cd mern-react-starter-code
$ npm install
$ code .

(in VS Code terminal) 
$ npm start
```

## Why use React Hooks?

Fact: Hooks don't add any additional functionality to React applications.

So, if hooks don't bring any new features to React apps, one might wonder why they've been added to the library.

The short answer is that hooks enable functional components to use the capabilities of class components.

Capabilities such as the ability to:

- Store and update state.
- Run code in a way similar to using lifecycle methods.

> IMPORTANT:  Hooks are backward-compatible and can be used side-by-side with classes and existing code so you can start using them gradually when you feel the urge to do so.

### So, what's wrong with class components?

There's nothing "wrong" with class components. However, consider that:

- Most developers find class components more complex to write and grasp than function components.
- With function components, there's no need to be concerned about the proper binding of `this` in event handlers, etc.
- Equivalent function components are more concise than their class counterparts.
- Functions aren't just easier on human developers, they're also easier for tooling and testing.

### Additional benefits of hooks

Just a few of the additional benefits hooks include:

- Hooks allow complex components to be split into smaller functions.
- The functionality in hooks are reusable across multiple components.
- Hooks allow related, stateful behavior to be kept together instead of being spread across multiple class methods such as `componentDidMount`, `componentDidUpdate` & `componentWillUnmount`.
- Custom hooks can be written and shared across projects and with the community.

## What are Hooks?

Specifically, a hook:

- Is a JavaScript function.
- Can only be used within function components.
- Allows function components to "remember" state and stateful behavior between renders.

### Built-in Hooks and their use cases:

| Hook | Use Case |
|---|---|
|[`useState()`](https://reactjs.org/docs/hooks-reference.html#usestate)|Used to implement class components' `this.state` and `setState()`.|
|[`useEffect()`](https://reactjs.org/docs/hooks-reference.html#useeffect)|Used to implement "side effects", e.g., fetching data, using timers, subscriptions, etc.<br>`useEffect()` implements the functionality of `componentDidMount`, `componentDidUpdate` & `componentWillUnmount` with a single hook!|
|[`useRef()`](https://reactjs.org/docs/hooks-reference.html#useref)|In addition to how refs are used to access DOM elements in class components, `useRef()` can be used more generally to "remember" any non-state data that needs to be persisted between renders similar to how we use instance properties in class components. |
|[`useReducer()`](https://reactjs.org/docs/hooks-reference.html#usereducer)|An alternative to `useState()` for when the state is more complex.  It uses a reducer function and "actions" to update state - similar to how Redux does (but not as comprehensive).|
| Other built-in hooks|[`useContext()`](https://reactjs.org/docs/hooks-reference.html#usecontext)<br>[`useMemo()`](https://reactjs.org/docs/hooks-reference.html#usememo)<br>[and other less common hooks here...](https://reactjs.org/docs/hooks-reference.html#useimperativehandle)|

## Using Hooks

We are going to be refactoring our existing gamelib client from class based components into functional components practice hooks.

### 💪 Practice Refactoring GamesListPage.jsx

Jump into GamesListPage.jsx and remove the current class based component and frame up a functional component. For now we will just return a div with the h1 saying "All Games".

In: `src/pages/GamesListPage.jsx`

```js
import React from "react";
import { Link } from "react-router-dom";

import Game from "../components/Game";

function GameList(props) {
  return (
    <div>
      <h1>All Games</h1>
    </div>
  );
}

export default GameList;
```

### Adding State Using `useState()`

We use the `useState()` hook to add a piece of state to a function component.

We invoke `useState()` and provide the initial value for the piece of state as an argument.

`useState()` returns an array with two elements, the value and a setter function used to update the value.

We know that we need to store all of the games from our database into state to render our list. Let's get to it. 

In: `src/pages/GamesListPage.jsx`

```js
// Update the import to include the useState hook
import React, {useState} from "react";
import { Link } from "react-router-dom";

import Game from "../components/Game";

function GameList(props) {
  // useState always returns an array of two elements
  const [games, setGames] = useState([]);
  // First element "games" is the value of the state returned
  // Second element "setGames" is a setter function for updating the state
  
  return (
    <div>
      <h1>All Games</h1>
      {/* we will display the length of the state 
      to see how many games are in state */}
      <h2>{ games.length }</h2>
    </div>
  );
}

export default GameList;
```

> Key Point: The names of the variables that hold the state value and setter function are up to us. However, it's convention to name the setter function by prepending `set` to the name of the state variable as we did with `games` & `setGames`.

Now let's create a button to add a hard coded game to our page to test out the setGames function. 

1. Create a function called fetchGames inside the GameList component that will add a hardcoded game object.
2. Add a button that onClick calls the function fetchGames.

In: `src/pages/GamesListPage.jsx`

```js
import React, {useState} from "react";
import { Link } from "react-router-dom";

import Game from "../components/Game";

function GameList(props) {
  const [games, setGames] = useState([]);
  
  // function to simulate calling the db
  function fetchGames(){
      const gamesArr = [{
      "_id": "5f9047e85455fbe527c997c2",
      "title": "Half-Life 2",
      "publisher": "Valve",
      "coverArtUrl": "https://cdn.mos.cms.futurecdn.net/yeLLJgaqz2UC6dDELTmf3S.jpg",
      "completed": true,
      "__v": 0
    }]
    
    // now we can call on setGames and pass the data into state
    setGames(gamesArr);
  }
  
  return (
    <div>
      <h1>All Games</h1>
      <h2>{ games.length }</h2>
      <button onClick={fetchGames} >Get Games</button>
    </div>
  );
}

export default GameList;
```

Now click that button and watch our state update! You should see the zero turn into 1.

*Open the react dev tools and select the GameList component from the available components to view your game object in action!*

### Rendering our new state

Time to get the state games onto the page! 

1. Create a generateList function inside our component that will return an array of Game components.
2. Inside the GameList render, write a conditional statement to check for existing games in our state, and invoke the generateList function if they exist. 

In: `src/pages/GamesListPage.jsx`

```js
import React, {useState} from "react";
import { Link } from "react-router-dom";

import Game from "../components/Game";

function GameList(props) {
  const [games, setGames] = useState([]);
  
  function fetchGames(){
      const gamesArr = [{
      "_id": "5f9047e85455fbe527c997c2",
      "title": "Half-Life 2",
      "publisher": "Valve",
      "coverArtUrl": "https://cdn.mos.cms.futurecdn.net/yeLLJgaqz2UC6dDELTmf3S.jpg",
      "completed": true,
      "__v": 0
    }]
    
    setGames(gamesArr);
  }
  
  // generate list will return an array of game card components 
  function generateList(games) {
    return games.map((game, index) => (
      <Link to={`/games/${game._id}`} key={index}>
        <Game {...game} />
      </Link>
    ));
  }
  
  return (
    <div>
      <h1>All Games</h1>
      <h2>{ games.length }</h2>
      {/* will return loading while games is empty
       and the componets once there is a game in the list */}
      {games.length ? generateList(games) : "Loading..."}
      <button onClick={fetchGames} >Get Games</button>
    </div>
  );
}

export default GameList;
```

Since we are pulling our data the const we declared in GamesListPage.jsx, we also need to modify our Game.jsx starter code to be:

```
function Game(props) {
  console.log(props);

  return (
    <div>
      <h3>{props.title}</h3>
      {/* <h3>{props.gameObj.title}</h3> */}
      <img 
        className="game-img" 
        src={props.coverArtUrl} 
        // src={props.gameObj.coverArtUrl} 
        alt="the art for a game" 
      />
    </div>
  )
}

export default Game;
```

Now click that button and witness the game card magically appear! 

So this is cool but now it's time to get data from our api. 

### Performing "Side Effects" Using `useEffect()`

"Side effects" include performing tasks such as:

- Fetching data
- Using timers
- Manually updating the DOM

> Key Point: When performing a task in a class component that requires overriding `componentDidMount`, `componentDidUpdate`, and/or `componentWillUnmount`, an equivalent function component will require a `useEffect()` hook.  Amazingly though, a single `useEffect()` hook can replace those three lifecycle methods combined!

#### Adding the `useEffect()` Hook

Like `useState()` and other hooks, because they are functions, we just invoke them from the top-level of the function component:

In: `src/pages/GamesListPage.jsx`

```js
// Update the import to include useEffect
import React, {useState, useEffect} from "react";
import { Link } from "react-router-dom";

import Game from "../components/Game";

function GameList(props) {
  const [games, setGames] = useState([]);
  
  // takes in a callback function as first required argument
  useEffect(function(){
      console.log('useEffect was called');
  })
  
  function fetchGames(){
      const gamesArr = [{
      "_id": "5f9047e85455fbe527c997c2",
      "title": "Half-Life 2",
      "publisher": "Valve",
      "coverArtUrl": "https://cdn.mos.cms.futurecdn.net/yeLLJgaqz2UC6dDELTmf3S.jpg",
      "completed": true,
      "__v": 0
    }]
    
    setGames(gamesArr);
  }
  
  
  function generateList(games) {
    return games.map((game, index) => (
      <Link to={`/games/${game._id}`} key={index}>
        <Game {...game} />
      </Link>
    ));
  }
  
  return (
    <div>
      <h1>All Games</h1>
      <h2>{ games.length }</h2>
      {games.length ? generateList(games) : "Loading..."}
      <button onClick={fetchGames} >Get Games</button>
    </div>
  );
}

export default GameList;
```

`useEffect()` takes a callback function as its first and only _required_ argument.

**By default, the effect's callback function is going be invoked after every render of the component.**

> Note: In class components, we would have to implement the `componentDidUpdate()` lifecycle method to run a side effect every render after the initial render.

You can test the above statement by clicking the button and updating state. 
As you can see the log will run with every update.

#### Preventing Side Effects from Running

In many cases, you will want to optimize the component so that side effects only run if:

- Certain data changes (typically a prop or state variable).
- After the initial render, but not after subsequent renders (as with `componentDidMount` in class components). 

The `useEffect()` hook provides for these scenarios by accepting an array as a second argument.

The array is designed to hold a list of dependencies, that is, a list of variables and/or object properties that causes the side effect to run only if at least one of the dependencies change their value.

Providing an empty array (`[]`), will result in the side effect only running after the **initial** render.  Let's check this out:

```js
  // Add the [] as a 2nd argument
  useEffect(() => {
    console.log('useEffect was called');
  }, []);
```

Clear the console and refresh. The "useEffect was called" message will be logged.  However, unlike without the `[]` arg, clicking the button will no longer run the side effect!

#### Time to fetch data from our database!

1. Import GameModel at the top of the file handle our request.
2. Update our fetchGames function to handle our db call instead of the hardcoded data.

In: `src/pages/GamesListPage.jsx`

```js
import React, {useState, useEffect} from "react";
import { Link } from "react-router-dom";

// add import for game model
import GameModel from "../models/game";
import Game from "../components/Game";

function GameList(props) {
  const [games, setGames] = useState([]);
  
  
  useEffect(function(){
      console.log('useEffect was called');
  },[])
  
  function fetchGames(){
      // call the .all method on GameModel
      // set the returned data to the games state with setGames()
      
      GameModel.all().then((data) => {
        setGames(data);
      });
  }
  
  
  function generateList(games) {
    return games.map((game, index) => (
      <Link to={`/games/${game._id}`} key={index}>
        <Game {...game} />
      </Link>
    ));
  }
  
  return (
    <div>
      <h1>All Games</h1>
      <h2>{ games.length }</h2>
      {games.length ? generateList(games) : "Loading..."}
      <button onClick={fetchGames} >Get Games</button>
    </div>
  );
}

export default GameList;
```


Click that button and watch all your games flood the page! 

#### Leverage useEffect to fetch on component mount

To have our component load the games on mount we must call our fetchGames function inside useEffect. 

In: `src/pages/GamesListPage.jsx`

```js
import React, {useState, useEffect} from "react";
import { Link } from "react-router-dom";

import GameModel from "../models/game";
import Game from "../components/Game";

function GameList(props) {
  const [games, setGames] = useState([]);
  
  
  useEffect(function(){
      console.log('useEffect was called');
      // on mount fetch data
      fetchGames();
  },[])
  
  function fetchGames(){
      GameModel.all().then((data) => {
        setGames(data);
      });
  }
  
  
  function generateList(games) {
    return games.map((game, index) => (
      <Link to={`/games/${game._id}`} key={index}>
        <Game {...game} />
      </Link>
    ));
  }
  
  return (
    <div>
      <h1>All Games</h1>
      <h2>{ games.length }</h2>
      {games.length ? generateList(games) : "Loading..."}
      <button onClick={fetchGames} >Get Games</button>
    </div>
  );
}

export default GameList;
```

Now our page will fill with data on load! 

#### Cleaning Up Side Effects

Certain side effects need to "clean up" after themselves and others don't.

For example, a side effect that fetches data, has nothing to clean up - it fetches data, updates state with it, and that's it.

However, in the case of say, creating a timer, or using subscriptions, you'll want to clear that timer or unsubscribe when the component that created it no longer exists.

Cleaning up is a good development practice because it prevents memory leaks.

In a class component, we use the `componentWillUnmount()` lifecycle method to clean up side effects.

However, a `useEffect()` hook can also run a cleanup process by returning a "cleanup" function from the hook's function:

```js
  useEffect(function(){
      console.log('useEffect was called');
      fetchGames();
      // Return a "cleanup" function
      return function(){
          console.log("runs on unmount");
      }
  },[])
```

Using that empty array as a second argument results in the cleanup function running once - when the component unmounts.

Here's a summary of when side effects and their cleanup functions run according to what is passed as a second argument:

|Second Arg| When **Effect** and **Cleanup** Functions Run|
|---|---|
|none|**Effect fn:** Runs after every render.<br>**Cleanup fn:** Runs just before then next render to cleanup the previous render's effects.|
|`[]`|**Effect fn:** Runs once after initial render (mounting).<br>**Cleanup fn:** Runs once before unmounting.|
|`[depVar, obj.depProp, ...]`|**Effect fn:** Runs once after initial render, then only when either `depVar`, `obj.depProp`, etc. changes.<br>**Cleanup fn:** Runs before next render if previous render ran the effect and also when unmounting.|

#### Summary

Both the functional version and Class version of GameList are functionally equivalent.

Comparing the code may tempt you to write only function components in future projects 😊

## Rules of Using Hooks

Using hooks requires that we always follow these two rules:

- Only call hooks at the top-level of a function component. Calling hooks inside loops, conditions, or nested functions is not allowed.

- Only call hooks from within:
	- React function components (they don't work with class components)
	- Or, your own custom Hooks

## ❓ Essential Questions

1. **True or False: React hooks allow function components to use React features previously possible only in class components?**

2. **The ________ hook allows function components to manage state.**

3. **Give one example of when to use the `useEffect()` hook.**

## Additional Practice! 

Time to refactor our other class based components into hooks! 

1. Refactor GamesShowPage.jsx into a functional component and leverage useState and useEffect to get the same functionality. 


<details>
  <summary>Solution</summary>

In: `src/pages/GamesShowPage.jsx`

```js
import React, {useState, useEffect} from 'react';
import GameModel from '../models/GameModel';
import Game from '../components/Game'

function GamesShowPage(props){
  const [game, setGames] = useState(null)

  useEffect(function(){
    fetchGame()
  })

  function fetchGame(){
    GameModel.show(props.match.params.id).then((data) => {
      setGames(data)
    })
  }
    return (
      <div>
        <Game {...game} />
      </div>
    )
}

export default GamesShowPage;        
```
</details>

</br>

2. **(HARD MODE)** Refactor NewGame.js into a functional component and leverage useState and useEffect to get the same functionality.

<details>
  <summary>Solution</summary>

In: `src/pages/NewGame.js`

```js
import React, { useState } from "react";
import GameModel from "../models/game";

function NewGame(props) {
  const [title, setTitle] = useState("");
  const [publisher, setPublisher] = useState("");
  const [coverArtUrl, setCoverArtUrl] = useState("");
  const [completed, setCompleted] = useState(false);

  function handleSubmit(event) {
    event.preventDefault();

    GameModel.create({ title, publisher, coverArtUrl, completed }).then(
      (data) => {
        props.history.push("/games");
      }
    );
  }

  return (
    <div>
      <h2>New Game</h2>
      <form onSubmit={handleSubmit}>
        <div className='form-input'>
          <label htmlFor='title'>Title</label>
          <input
            type='text'
            name='title'
            onChange={(e) => setTitle(e.target.value)}
            value={title}
          />
        </div>
        <div className='form-input'>
          <label htmlFor='publisher'>Publisher</label>
          <input
            type='text'
            name='publisher'
            onChange={(e) => setPublisher(e.target.value)}
            value={publisher}
          />
        </div>
        <div className='form-input'>
          <label htmlFor='coverArtUrl'>Image URL</label>
          <input
            type='text'
            name='coverArtUrl'
            onChange={(e) => setCoverArtUrl(e.target.value)}
            value={coverArtUrl}
          />
        </div>
        <div className='form-input'>
          <label htmlFor='completed'>Completed</label>
          <input
            type='checkbox'
            id='completed'
            checked={completed}
            onChange={(e) => setCompleted(!completed)}
          />
        </div>

        <input type='submit' value='Save!' />
      </form>
    </div>
  );
}

export default NewGame;
```
</details>

## Custom Hooks

Building your own custom hooks enables extracting logic out of function components and into reusable functions (hooks) - just like what we saw with `useState()` and `useEffect()`.

The key word in the above description is **reusable**.  If you find that you've written components that include duplicate logic, a custom hook can be used to make components more DRY!

A custom hook is a JavaScript function whose name starts with `use` (by highly recommended convention).

Custom hooks may call other hooks such as `useState()`, etc.

For an example, and to learn more, start by checking out [Building Your Own Hooks](https://reactjs.org/docs/hooks-custom.html) in the docs.

## Creating our own useGames hook.

As we discussed at the moment we have repeating logic in our GamesListPage.jsx and GamesShowPage.jsx that is our fetch functions. Let's convert this into a custom hook and dry up our code! 

1. Create a new directory called hooks inside src `$ mkdir src/hooks`
2. Create a file called useGames.js inside the hooks directory `$ touch src/hooks/useGames.js`

Now let's set up the base of our custom hook.

In: `src/hooks/useGames.js`

```js
// notice we did not import React. 
// This is not a component so we only need the useState and useEffect functionality
import { useState, useEffect } from "react";

import GameModel from "../models/game";

// we will define our custom hook with the use naming convention
function useGames() {
  const [games, setGames] = useState([]);
  
  // fetch games now exists inside of our custom hook to handle fetching data. 
  function fetchGames() {
      GameModel.all().then((data) => {
        setGames(data);
      });
  }

  // when this hook is ran we will run by default the fetchGames function to update our state
  useEffect(
    function () {
      fetchGames();
    },
    []
  );
  
  // we will return our games state and function to refetch in case we need it
  return [games, fetchGames];
}

export default useGames;
```

> Key Point: just like how useState returns an array of a state in index one and a function in index two our function will the same. Now we are only requesting data so we will return a custom function that will only request new data when needed. 

### Using our custom hook.

Let's use our useGames hook inside our GamesList component. 

1. Remove useState and useEffect from our component. Our custom hook will handle this for us now. 
2. Remove the GameModel import. 
3. Remove the fetchGames function.
4. Import our useGames hook.
5. Replace our useState call inside our component with our useGames hook.

In: `src/pages/GamesListPage.jsx`

```js
import React from "react";
import { Link } from "react-router-dom";

// import useGames hook
import useGames from "../hooks/useGames";
import Game from "../components/Game";

function GameList(props) {
  // replace useState with our hook replacing the setGames with fetchGames
  const [games, fetchGames] = useGames();
  
  function generateList(games) {
    return games.map((game, index) => (
      <Link to={`/games/${game._id}`} key={index}>
        <Game {...game} />
      </Link>
    ));
  }
  
  return (
    <div>
      <h1>All Games</h1>
      <h2>{ games.length }</h2>
      {games.length ? generateList(games) : "Loading..."}
      <button onClick={fetchGames} >Get Games</button>
    </div>
  );
}

export default GameList;
```

And just like that we have replaced a large chuck of code in our component with a custom hook! So satisfying... 

<img src="https://media.giphy.com/media/jQDozgWeDXUoQZ1htF/giphy.gif" />

### But wait! What about the GameShow?

At the moment our hook only handles getting all games. Let's modify our hook so that IF it receives an id then it will fetch and return a single game from the api.  

1. Add a param of gameId to the hook. 
2. We will now use this gameId in our fetchGames function. Add an id param to fetchGames.
3. In fetchGames add an `if else` statement that IF the id is provided we will update state with .show vs .all
4. In our useEffect we must now add the gameId as a dependency so our hook will auto fetch new info on id change. 
5. Pass the id into fetchGames within our useEffect and we are all set! 

In: `src/hooks/useGames.js`

```js
import { useState, useEffect } from "react";

import GameModel from "../models/game";

// add a param of gameId 
function useGames(gameId) {
  const [games, setGames] = useState([]);

  // allow the fetchGames function to recieve an id
  function fetchGames(id) {
    // if id is provided get one game by id
    if (id) {
      GameModel.show(id).then((data) => {
        setGames(data.game);
      });
      // id not provided get all games
    } else {
      GameModel.all().then((data) => {
        setGames(data);
      });
    }
  }

  useEffect(
    function () {
      fetchGames(gameId);
    },
    [gameId]
  );

  return [games, fetchGames];
}

export default useGames;
```

Our custom hook is now ready to be used in the GameShow component. 
Time to refactor! 

1. Remove useState and useEffect from our component. Our custom hook will handle this for us now. 
2. Remove the GameModel import. 
3. Remove the fetchGame function.
4. Import our useGames hook passing in the id from the url params.
5. Replace our useState call inside our component with our useGames hook.

In: `src/pages/GamesShowPage.jsx`

```js
import React from "react";
import useGames from "../hooks/useGames";

import Game from "../components/Game";

function GameShow(props) {
  const [game] = useGames(props.match.params.id);

  return (
    <div>
      <Game {...game} />
    </div>
  );
}

export default GameShow;
```

Look how small our component is now!

<img src="https://media.giphy.com/media/1yiPWNsQ1vq7V90fRY/giphy.gif" />


## References

[React Docs - Introducing Hooks](https://reactjs.org/docs/hooks-intro.html)

[React Docs - Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)

[React Docs - Building Your Own Hooks](https://reactjs.org/docs/hooks-custom.html)

[MDN - Battery Status API](https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API)

[Observer Design Pattern](https://en.wikipedia.org/wiki/Observer_pattern)
