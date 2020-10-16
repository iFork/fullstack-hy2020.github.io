---
mainImage: ../../../images/part-2.svg
part: 2
letter: a
lang: en
---

# a. Rendering a collection, modules

Before starting a new topic, let's recap some of the topics that proved difficult last year.

## console.log

***What's the difference between an experienced JavaScript programmer and a rookie? The experienced one uses console.log 10-100 times more.***

Paradoxically, this seems to be true even though a rookie programmer would need console.log (or any debugging method) more than an experienced one.

When something does not work, don't just guess what's wrong. Instead, log or use some other way of debugging. 

**NB** As explained in part 1, when you use the command _console.log_ for debugging, don't concatenate things 'the Java way' with a plus. Instead of writing:

```js
console.log('props value is' + props)
```

separate the things to be printed with a comma:

```js
console.log('props value is', props)
```

If you concatenate an object with a string and log it to the console (like in our first example), the result will be pretty useless: 

```js
props value is [Object object]
```

On the contrary, when you pass objects as distinct arguments separated by commas to _console.log_, like in our second example above, the content of the object is printed to the developer console as strings that are insightful.
If necessary, read more about debugging React-applications [here](/en/part1/a_more_complex_state_debugging_react_apps#debugging-react-applications).

## Protip: Visual Studio Code snippets

With Visual studio code it's easy to create 'snippets', i.e. shortcuts for quickly generating commonly re-used portions of code, much like how 'sout' works in Netbeans. 
Instructions for creating snippets can be found [here](https://code.visualstudio.com/docs/editor/userdefinedsnippets#_creating-your-own-snippets).

Useful, ready-made snippets can also be found as VS Code plugins, for example [here](https://marketplace.visualstudio.com/items?itemName=xabikos.ReactSnippets).

The most important snippet is the one for the `console.log()` command, for example `clog`. This can be created like so: 
```js
{
  "console.log": {
    "prefix": "clog",
    "body": [
      "console.log('$1')",
    ],
    "description": "Log output to console"
  }
}
```

## JavaScript Arrays

From here on out, we will be using the functional programming methods of the JavaScript [array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array), such as  _find_, _filter_, and _map_ - all of the time. They operate on the same general principles as streams do in Java 8, which have been used during the last few years in both the 'Ohjelmoinnin perusteet' and 'Ohjelmoinnin jatkokurssi' courses at the university's department of Computer Science, and also in the programming MOOC. 

If functional programming with arrays feels foreign to you, it is worth watching at least the first three parts of the YouTube video series [Functional Programming in JavaScript](https://www.youtube.com/playlist?list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84):

- [Higher-order functions](https://www.youtube.com/watch?v=BMUiFMZr7vk&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84)
- [Map](https://www.youtube.com/watch?v=bCqtb-Z5YGQ&list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84&index=2)
- [Reduce basics](https://www.youtube.com/watch?v=Wl98eZpkp-c&t=31s)

## Event handlers revisited

Based on last year's course, event handling has proven to be difficult. 
It's worth reading the revision chapter at the end of the previous part [event handlers revisited](/en/part1/a_more_complex_state_debugging_react_apps#event-handling-revisited), if it feels like your own knowledge on the topic needs some brushing up. 

Passing event handlers to the child components of the *App* component has raised some questions. A small revision on the topic can be found [here](/en/part1/a_more_complex_state_debugging_react_apps#passing-event-handlers-to-child-components).

## Rendering collections

We will now do the 'frontend', or the browser-side application logic, in React for an application that's similar to the example application from [part 0](/en/part0)

Let's start with the following:

```js
import React from 'react'
import ReactDOM from 'react-dom'

const notes = [
  {
    id: 1,
    content: 'HTML is easy',
    date: '2019-05-30T17:30:31.098Z',
    important: true
  },
  {
    id: 2,
    content: 'Browser can execute only Javascript',
    date: '2019-05-30T18:39:34.091Z',
    important: false
  },
  {
    id: 3,
    content: 'GET and POST are the most important methods of HTTP protocol',
    date: '2019-05-30T19:20:14.298Z',
    important: true
  }
]

const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        <li>{notes[0].content}</li>
        <li>{notes[1].content}</li>
        <li>{notes[2].content}</li>
      </ul>
    </div>
  )
}

ReactDOM.render(
  <App notes={notes} />,
  document.getElementById('root')
)
```

Every note contains its textual content and a timestamp as well as a _boolean_ value for marking whether the note has been categorized as important or not, and also a unique *id*.


The example above works due to the fact that there are exactly three notes in the array. 
A single note is rendered by accessing the objects in the array by referring to a hard-coded index number:

```js
<li>{note[1].content}</li>
```

This is, of course, not practical. We can improve on this by generating React elements from the array objects using the [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) function.

```js
notes.map(note => <li>{note.content}</li>)
```

The result is an array of *li* elements.

```js
[
  <li>HTML is easy</li>,
  <li>Browser can execute only Javascript</li>,
  <li>GET and POST are the most important methods of HTTP protocol</li>,
]
```


Which can then be placed inside *ul* tags:

```js
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
// highlight-start
      <ul>
        {notes.map(note => <li>{note.content}</li>)}
      </ul>
// highlight-end      
    </div>
  )
}
```

Because the code generating the *li* tags is JavaScript, it must be wrapped in curly braces in a JSX template just like all other JavaScript code. 

<!-- Parannetaan koodin luetteloa vielä jakamalla nuolifunktion määrittely useammalle riville: -->
We will also make the code more readable by separating the arrow function's declaration across multiple lines:

```js
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
        // highlight-start
          <li>
            {note.content}
          </li>
        // highlight-end   
        )}
      </ul>
    </div>
  )
}
```

## Key-attribute

Even though the application seems to be working, there is a nasty warning in the console: 

![](../../images/2/1a.png)

As the linked [page](https://reactjs.org/docs/lists-and-keys.html#keys) in the error message instructs, the list items, i.e. the **elements generated by the _map_ method, must each have a unique key value:  an attribute called *key*.**

Let's add the keys:

```js
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
        // highlight-start
          <li key={note.id}>
            {note.content}
          </li>
          // highlight-end
        )}
      </ul>
    </div>
  )
}
```

And the error message disappears. 

React uses the key attributes of objects in an array to determine how to update the view generated by a component when the component is re-rendered. More about this [here](https://reactjs.org/docs/reconciliation.html#recursing-on-children).

## Map

Understanding how the array method [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) works is crucial for the rest of the course. 

The application contains an array called _notes_:

```js
const notes = [
  {
    id: 1,
    content: 'HTML is easy',
    date: '2019-05-30T17:30:31.098Z',
    important: true
  },
  {
    id: 2,
    content: 'Browser can execute only Javascript',
    date: '2019-05-30T18:39:34.091Z',
    important: false
  },
  {
    id: 3,
    content: 'GET and POST are the most important methods of HTTP protocol',
    date: '2019-05-30T19:20:14.298Z',
    important: true
  }
]
```

Let's pause for a moment and examine how _map_ works.


If the following code is added to, let's say, the end of the file:

```js
const result = notes.map(note => note.id)
console.log(result)
```

*[1, 2, 3]*  will be printed to the console.
 _map_ always creates a new array, the elements of which have been created from the elements of the original array by *mapping*: using the function given as a parameter to the _map_ method. 


The function is

```js
note => note.id
```

Which is an arrow function written in compact form. The full form would be: 

```js
(note) => {
  return note.id
}
```

The function gets a note object as a parameter, and *returns* the value of its *id* field.

Changing the command to:

```js
const result = notes.map(note => note.content)
```

results in an array containing the contents of the notes.

This is already pretty close to the React code we used:

```js
notes.map(note =>
  <li key={note.id}>{note.content}</li>
)
```

which generates a *li* tag containing the contents of the note from each note object. 

Because the function parameter passed to the _map_ method - 

```js
note => <li key={note.id}>{note.content}</li>
```

&nbsp;- is used to create view elements, the value of the variable must be rendered inside of curly braces. Try to see what happens if the braces are removed. 

The use of curly braces will cause some headache in the beginning, but you will get used to them soon enough. The visual feedback from React is immediate.

## Anti-pattern: array indexes as keys

We could have made the error message on our console disappear by using the array indexes as keys. The indexes can be retrieved by passing a second parameter to the callback function of the map-method: 

```js
notes.map((note, i) => ...)
```

When called like this, _i_ is assigned the value of the index of the position in the array where the *Note* resides.

As such, one way to define the row generation without getting errors is:

```js
<ul>
  {notes.map((note, i) => 
    <li key={i}>
      {note.content}
    </li>
  )}
</ul>
```

This is, however, **not recommended** and can cause undesired problems even if it seems to be working just fine. 
Read more about this [here](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318).

## Refactoring modules

Let's tidy the code up a bit. We are only interested in the field _notes_ of the props, so let's retrieve that directly using [destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment): 

```js
const App = ({ notes }) => { //highlight-line
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>
            {note.content}
          </li>
        )}
      </ul>
    </div>
  )
}
```

If you have forgotten what destructuring means and how it works, review [this](/en/part1/component_state_event_handlers#destructuring).


We'll separate displaying a single note into its own component *Note*: 

```js
// highlight-start
const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  )
}
// highlight-end

const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        // highlight-start
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
         // highlight-end
      </ul>
    </div>
  )
}
```

**Note that the *key* attribute must now be defined for the *Note* components, and not for the *li* tags like before.** 

A whole React application can be written in a single file. Although that is, of course, not very practical. Common practice is to declare each component in their own file as an *ES6-module*.

We have been using modules the whole time. The first few lines of the file:

```js
import React from 'react'
import ReactDOM from 'react-dom'
```

[import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) two modules, enabling them to be used in that file. The *react* module is placed into a variable called _React_ and *react-dom* to variable _ReactDOM_.


Let's move our *Note* component into its own module. 

In smaller applications, components are usually placed in a directory called *components* , which is in turn placed within the *src* directory. The convention is to name the file after the component. 

Now we'll create a directory called *components* for our application and place a file named *Note.js* inside. 
The contents of the Note.js file are as follows: 

```js
import React from 'react'

const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  )
}

export default Note
```

Because this is a React-component, we must import React. 

The last line of the module [exports](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) the declared module, the variable *Note*.

Now the file that is using the component - *index.js* - can [import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) the module: 

```js
import React from 'react'
import ReactDOM from 'react-dom'
import Note from './components/Note' // highlight-line

const App = ({ notes }) => {
  // ...
}
```

The component exported by the module is now available for use in the variable *Note*, just as it was earlier. 

Note that when importing our own components, their location must be given *in relation to the importing file*:

```js
'./components/Note'
```

The period - *.* - in the beginning refers to the current directory, so the module's location is a file called *Note.js* in the *components* sub-directory of the current directory. The filename extension - _.js_ - can be omitted.

*App* is a component as well, so let's declare it in its own module as well. Since it is the root component of the application, we'll place it in the *src* directory. The contents of the file are as follows: 

```js
import React from 'react'
import Note from './components/Note'

const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map((note) => 
          <Note key={note.id} note={note} />
        )}
      </ul>
    </div>
  )
}

export default App // highlight-line
```

What's left in the *index.js* file is: 

```js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'  // highlight-line

const notes = [
  // ...
]

ReactDOM.render(
  <App notes={notes} />,
  document.getElementById('root')
)
```

Modules have plenty of other uses other than enabling component declarations to be separated into their own files. We will get back to them later in this course. 


The current code of the application can be found on [GitHub](https://github.com/fullstack-hy2020/part2-notes/tree/part2-1).


Note that the *master* branch of the repository contains the code for a later version of the application. The current code is in the branch [part2-1](https://github.com/fullstack-hy2020/part2-notes/tree/part2-1):

![](../../images/2/2e.png)

**If you clone the project, run the command _npm install_ before starting the application with _npm start_.**

## When the application breaks

Early in your programming career (and even after 30 years of coding like yours truly), what often happens is that the application just completely breaks down. This is even more the case with dynamically typed languages, such as JavaScript, where the compiler does not check the data type of, for instance, function variables or return values. 


A "React explosion" can for example look like this:

![](../../images/2/3b.png)


In these situations your best way out is the `console.log`.
The piece of code causing the explosion is this: 

```js
const Course = ({ course }) => (
  <div>
   <Header course={course} />
  </div>
)

const App = () => {
  const course = {
    // ...
  }

  return (
    <div>
      <Course course={course} />
    </div>
  )
}
```


We'll hone in on the reason for the breakdown by adding `console.log` commands to the code. Because the first thing to be rendered is the *App* component, it's worth putting the first `console.log` there: 

```js
const App = () => {
  const course = {
    // ...
  }

  console.log('App works...') // highlight-line

  return (
    // ..
  )
}
```

To see the printing on the console, we must scroll up over the long red wall of errors.

![](../../images/2/4b.png)

When one thing is found to be working, it's time to log deeper. If the component has been declared as a single statement, or a function without a return, it makes printing to the console harder. 

```js
const Course = ({ course }) => (
  <div>
   <Header course={course} />
  </div>
)
```

The component should be changed to its longer form in order for us to add the printing: 

```js
const Course = ({ course }) => { 
  console.log(course) // highlight-line
  return (
    <div>
    <Header course={course} />
    </div>
  )
}
```

Quite often the root of the problem is that the props are expected to be of a different type, or called with a different name than they actually are, and destructuring fails as a result. The problem often begins to solve itself when destructuring is removed and we see what the `props` actually contains. 

```js
const Course = (props) => { // highlight-line
  console.log(props)  // highlight-line
  const { course } = props
  return (
    <div>
    <Header course={course} />
    </div>
  )
}
```

If the problem has still not been resolved, there really isn't much to do apart from continuing to bug-hunt by sprinkling more _console.log_ statements around your code. 

I added this chapter to the material after the model answer for the next question exploded completely (due to props being of the wrong type), and I had to debug it using `console.log`.


# Exercises 2.1.-2.5.

The exercises are submitted via GitHub, and by marking the exercises as done in the [submission system](https://studies.cs.helsinki.fi/stats/courses/fullstackopen).

You can submit all of the exercises into the same repository, or use multiple different repositories. If you submit exercises from different parts into the same repository, name your directories well.

The exercises are submitted **One part at a time**. When you have submitted the exercises for a part, you can no longer submit any missed exercises for that part.

Note that this part has more exercises than the ones before, so *do not submit* before you have done all exercises from this part you want to submit. 

**WARNING** create-react-app makes the project automatically into a git-repository, if the project is not created inside of an already existing repository. You probably **do not** want the project to become a repository, so run the command  _rm -rf .git_ from its root. 

## 2.1: course contents step6


Let's finish the code for rendering course contents from exercises 1.1 - 1.5. You can start with the code from the model answers. The model answers for part 1 can be found by going to the [submission system](https://studies.cs.helsinki.fi/stats/courses/fullstackopen), click on *my submissions* at the top, and in the row corresponding to part 1 under the *solutions* column click on *show*. To see the solution to the *course info* exercise, click on _index.js_ under *kurssitiedot* ("kurssitiedot" means "course info").


**Note that if you copy a project from one place to another, you might have to delete the *node_modules* directory and install the dependencies again with the command `npm install` before you can start the application.**
Generally, it's not recommeneded that you copy a project's whole contents and/or add the *node_modules* directory to the version control system. 

Let's change the *App* component like so: 

```js
const App = () => {
  const course = {
    id: 1,
    name: 'Half Stack application development',
    parts: [
      {
        name: 'Fundamentals of React',
        exercises: 10,
        id: 1
      },
      {
        name: 'Using props to pass data',
        exercises: 7,
        id: 2
      },
      {
        name: 'State of a component',
        exercises: 14,
        id: 3
      }
    ]
  }

  return <Course course={course} />
}
```

Define a component responsible for formatting a single course called *Course*. 

The component structure of the application can be, for example, the following: 

<pre>
App
  Course
    Header
    Content
      Part
      Part
      ...
</pre>

Hence, the *Course* component contains the components defined in the previous part, which are responsible for rendering the course name and its parts. 

The rendered page can, for example, look as follows: 

![](../../images/teht/8e.png)

You don't need the sum of the exercises yet. 

The application must work *regardless of the number of parts a course has*, so make sure the application works if you add or remove parts of a course. 

Ensure that the console shows no errors!

## 2.2: Course contents step7

Show also the sum of the exercises of the course. 

![](../../images/teht/9e.png)

## 2.3*: Course contents step8

If you haven't done so already, calculate the sum of exercises with the array method [reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce).

**Pro tip:** when your code looks as follows:

```js
const total = 
  parts.reduce((s, p) => someMagicHere)
```

and does not work, it's worth to use *console.log*, which requires the arrow function to be written in its longer form:

```js
const total = parts.reduce((s, p) => {
  console.log('what is happening', s, p)
  return someMagicHere 
})
```

**Pro tip2:** There is a [plugin for VS code](https://marketplace.visualstudio.com/items?itemName=cmstead.jsrefactor) that automatically changes short form arrow functions into their longer form, and vice versa. 

![](../../images/2/5b.png)

## 2.4: Course contents step9


Let's extend our application to allow for an *arbitrary number* of courses:

```js
const App = () => {
  const courses = [
    {
      name: 'Half Stack application development',
      id: 1,
      parts: [
        {
          name: 'Fundamentals of React',
          exercises: 10,
          id: 1
        },
        {
          name: 'Using props to pass data',
          exercises: 7,
          id: 2
        },
        {
          name: 'State of a component',
          exercises: 14,
          id: 3
        },
        {
          name: 'Redux',
          exercises: 11,
          id: 4
        }
      ]
    }, 
    {
      name: 'Node.js',
      id: 2,
      parts: [
        {
          name: 'Routing',
          exercises: 3,
          id: 1
        },
        {
          name: 'Middlewares',
          exercises: 7,
          id: 2
        }
      ]
    }
  ]

  return (
    <div>
      // ...
    </div>
  )
}
```

The application can, for example, look like this: 

![](../../images/teht/10e.png)

## 2.5: separate module

Declare the *Course* component as a separate module, which is imported by the *App* component. You can include all subcomponents of the course into the same module. 

