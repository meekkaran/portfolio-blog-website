---
title: Redux vs. Redux Toolkit- Which is Right for Your React App?
date: '2023-04-11'
tags: ['React', 'Redux', 'Redux-Toolkit']
draft: false
summary: 'A beginner guide to exploring Redux and Redux Toolkit'
---


![Alt Text](/static/images/blog/techcareer/redux.jpeg)


# Redux vs. Redux Toolkit

## What is Redux

Redux is a way for a React app to keep track of its state, or data. 

Imagine you're playing with a toy car and you want to know where it is at all times. You might put it in a special box and check the box whenever you want to know where the car is. 

That's what Redux does for a React app -- it puts all of the app's important information in a special box (called a "store") and lets the app check the box whenever it needs to know what's going on.


## What is Redux Toolkit

Redux Toolkit is like a set of tools that makes using Redux easier. 

Imagine you're building a tower out of blocks, but the blocks keep falling over because they're not stacked properly. You might ask someone for help, and they could give you a special tool that helps you stack the blocks more easily.

That's what Redux Toolkit does -- it gives you tools to help you use Redux more easily.

## Redux or Redux Toolkit
Now, you might be wondering which is better -- Redux or Redux Toolkit.

The truth is, they're both great! Redux is more powerful and flexible, but it can be a little bit harder to use.

Redux Toolkit is easier to use, but it's not as powerful or flexible as Redux.

It's like comparing a grown-up's tools to a kid's tools -- the grown-up tools can do more things, but the kid's tools are easier to use.

## Practical example 

Here's an example of how you might use Redux and Redux Toolkit in a React app. Let's say you're building a simple app that lets you keep track of your favorite colors. You might start by creating a "store" using Redux:


```Javascript
import { createStore } from 'redux';

const initialState = {
  favoriteColor: 'unknown',
};

function colorReducer(state = initialState, action) {
  switch (action.type) {
    case 'SET_FAVORITE_COLOR':
      return {
        ...state,
        favoriteColor: action.payload,
      };
    default:
      return state;
  }
}

const store = createStore(colorReducer);
```

This code creates a store that keeps track of your favorite color. When you first start the app, your favorite color is "unknown". When you tell the app what your favorite color is, it uses a "reducer" (the colorReducer function) to update the store.

Now, let's say you want to use Redux Toolkit to make this code a little bit easier to use:


```Javascript
import { createSlice, configureStore } from '@reduxjs/toolkit';

const colorSlice = createSlice({
  name: 'color',
  initialState: {
    favoriteColor: 'unknown',
  },
  reducers: {
    setFavoriteColor: (state, action) => {
      state.favoriteColor = action.payload;
    },
  },
});

const store = configureStore({
  reducer: colorSlice.reducer,
});
```


This code does the same thing as the previous code, but it uses Redux Toolkit to make it a little bit easier to read and write. 

Instead of defining a reducer function, we use the createSlice function to create a "slice" of the store that handles the setFavoriteColor action. We also use the configureStore function to create the store itself.

# Conclusion
And that's it! Now you know a little bit about Redux, Redux Toolkit, and how they're used in a React app. 

Remember, Redux is like a special box that helps the app keep track of its state, 

and 

Redux Toolkit is like a set of tools that make using Redux easier.




​


