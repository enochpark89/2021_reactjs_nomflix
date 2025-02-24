# Nomflix

-  An app that introduces movies to users.
-  This app is created with react.js.

# Project plan

## Screens

-  [ ] Home
-  [ ] TV shows
-  [ ] Search
-  [ ] Detail - click in the movies to see details.

## Prerequisite Knowledge:

1. Arrow Function
2. Template Literal
3. Object Destructuring
4. Spread Operator
5. Classes
6. Array map
7. Array filter

# Progress Report

# 1.0 Project Setup

-  Use npx because it doesn't require you to create reactjs in your computer.

1. run _npm install npx_ to install npx. With npx, this is going to install everything that we need to utilize react.js and going to delete when the app is finished.
2. run _npx create-react-app ._ to create a react app in the current directory. Otherwise, you can specify that path.
3. create .env file > write NODE_PATH=src
4. run _install prop-types_ to install prop-types

# 1.1 Router

1. set up .env file and write "NODE_MODULE=src" and React will search app to find files.
2. Create a _Component_ folder and paste App.js
3. Create a _Router_ folder and set up routes.
4. Create files Home.js, Search.js, TV.js, Detail.js
5. Install react-router to route users into different files or pages. > Run _npm install react-router-dom_
6. After installing, you have to import the router. There are HashRouter and BrowserRouter.
7. When you implement the Router, you have include the component that you want users to be routed to.
8. Also, in App.js, you have to return components. Since you can't import more than one component, you can use <></> as below - I am not doing this for now.
9. In Router.js, include three more routes. There should be four routing paths to Home.js,Detail.js, Search.js, and TV.js
10.   _HashRouter_ will create # inside the URL. Therefore, it is better to use **BrowserRouter** if you do not want to see this Hashtag symbol.
      router.js:

```
export default () => (
  <Router>
    <Route path="/" exact component={Home} />
    <Route path="/tv" exact component={TV} />
    <Route path="/search" exact component={Search} />
  </Router>
);
```

11. **Composition**: Composition is a way to render two or more routes at the same time.
12. As long as you have the matching URL, React will render them in the same page.
    For example, below code will render both /tv component and /tv/popular components in the page "/tv/popular.
    Router.js:

```
  <Route path="/tv" component={TV} />
  <Route path="/tv/popular" render={() => <h1>Popular</h1>} />
```

13. Now, create Components/Header.js
14. This Header will have few links that routes users to different pages like a menu bar.
15. IF you set the Redirect at the end of your Routes, it will route the user to Redirect's destination if users are not routed to any of the Routes.
16. Redirect must be used along with Switch.
17. This way, when people try to input URL that is not within the route such as "/giberish", they will be routed to HOME instead.

# 1.2 Styles

## 1.2a CSS

There are generally three way to style Header.js component.

1. Create style.css and import.
   or Make a folder in the Components folder > Create .css, .js, and .index.
2. Use **CSS module**: you can use the CSS module in React but it will make the className random.
   Header.js:
3. Styled Component

#2: CSS module

App.js:

```javascript
import React from "react";
import styles from "./Header.module.css";

export default () => (

  <header>
    <ul className={styles.navList}>
      <li>
        <a href="/">Movies</a>
      </li>
  <header>
```

Header.module.css:

```css
.navList {
   display: flex;
}

.navList:hover {
   background-color: blue;
}
```

-  The CSS name has to be <name>.module.css and then, import it in .js file.
-  Cons: you have to memorize the className; the ClassName is going to be randomized. You always have to write className.

-  This is perfect for tiny projects.

#3. Styled Components

Steps:
a. Run _npm install styled-components_ to install styled-component package.
b.Import styled-component in Header.js.
c. you can declare a style within the component and use that element as below:

```javascript
import styled from "styled-components";

// initiate a styled-component named "List"
const List = styled.ul`
   display: flex;
   &:hover {
      background-color: blue;
   }
`;

// use the <List> as if you would use for HTML element and it will contain styled that you declared in it.
export default () => (
   <header>
      <List>
         <li>
            <a href="/">Movies</a>
         </li>
         <li>
            <a href="/tv">TV</a>
         </li>
         <li>
            <a href="/search">Search</a>
         </li>
      </List>
   </header>
);
```

e. you can create as much components you want with different syles in it.
f. you can use {Link} from "react-router-dom" to replace anchor.
g. Once you create bunch of components and use it, it will create random className.

_Tip:Since you can't use "Link" outside the Router, include "Header" inside the router._

## 1.3b GlobalStyles

-  Styled-Components are local. We need to set up a Global-Components for later uses.

Steps:

1. Create a new file name _GlobalStyles.js_ in a folder Components.
2. Import style reset (reset all styles to default) - Run _npm install styled-reset_
3. Style a{}, \*{} and body{}
   GlobalStyle.js:

```javascript
import { createGlobalStyle } from "styled-components";
import reset from "styled-reset";

const globalStyles = createGlobalStyle`
    ${reset};
    a{
        text-decoration:none;
        color:inherit;
    }
    *{
        box-sizing:border-box;
    }
    body{
        font-family:-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        font-size:12px;
        background-color:rgba(20, 20, 20, 1);
        color:white;
        padding-top:50px;
    }
`;

export default globalStyles;
```

4. Import the globalstyle in the App.js.
5. the Globalstyle is now applied on the website and the background is black.
6. Change the CSS of the header.js

## 1.3c Location aware Header.

-  Give Header Props:

Step:

1. Give Header a prop.
2. On Styled-Component, you can give props to your components.

```javascript
const Item = styled.li`
   width: 80px;
   height: 50px;
   text-align: center;
    // pass props and if the props is TRUE, change the color of the border-bottom.
   border-bottom: 5px solid
      ${(props) => (props.current ? "#3498db" : "transparent")};
`;
<Item current={true}>
```

-  Make Header aware of the routes.

Step:

1. Import _withRouter_ from "react-router-dom" package.
2. Thanks to withRouter, the Router is sending props to the Header.js. The props contain information regarding the current URL.

In Header.js

```javascript
// Use withRouter to get the props and a variable called pathname that shows the current URL.
export default withRouter(({ location: { pathname } }) => (
   <Header>
      <List>
         // Compare the current pathname with hypothetical path > set current
         variable to true > Change border-bottom color.
         <Item current={pathname === "/"}>
            <SLink to="/">Movies</SLink>
         </Item>
         <Item current={pathname === "/tv"}>
            <SLink to="/tv">TV</SLink>
         </Item>
         <Item current={pathname === "/search"}>
            <SLink to="/search">Search</SLink>
         </Item>
      </List>
   </Header>
));
```

3. As a result, when the user click on the list, it will change the border-bottom color and indicate which page that they are in.

# 1.4 Network

-  Data is going to be coming from Movie API database.
-  The Movie DB has an API.
-  Once you have the authentication key, using that key, you can get the Data from the websites using API.
-  Data is offered as JSON file which is _easy to navigate_ unlike HTML tags.

## 1.4a Axios

### API verbs

-  [ ] Now playing (Movie)
-  [ ] Upcoming (Movie)
-  [ ] Top Rated (TV, Movie)
-  [ ] Popular (TV, Movie)
-  [ ] Airing Today (TV)

-  Normally, we use fetch the get the data from the website.
-  Fetch is not efficient because some of the URL remains the same for getting different data.
-  Install axios: run _npm install axios_
-  you can set the baseURL so that you don't have to type that everytime.
-  parameter is going to be the rest of the URL.

-  Create a directory src/api.js and import axios and create one API request as below.

api.js:

```javascript
import axios from "axios";

const api = axios.create({
   baseURL: "https://api.themoviedb.org/3/",
   params: {
      api_key: "10923b261ba94d897ac6b81148314a3f",
      language: "en-US",
   },
});

export default api;
```

## 1.4b. API

-  once **api** constant is created with axios, you can create a new const to get datas as below. Please notice that you do not have to put in the whole URL for different datas. you just need to state the different part thanks to axios :)

```js
// movieApi hold three properties that receives data from movieDB API using axios api constant.
export const moviesApi = {
   nowPlaying: () => api.get("movie/now_playing"),
   upcoming: () => api.get("movie/upcoming"),
   popular: () => api.get("movie/popular"),
};
```

-  Some API requires putting the id.

```

```

-  Some API comes with append_to_response. It demands a response of videos or images.

   -  trailer video
   -  posters
   -  images.
      api.js:

```js
   movieDetail: (id) =>
      api.get(`movie/${id}`, {
         params: {
            append_to_response: "videos",
         },
      }),
```

-  For this particular example, it gives a key of a youtube video that shows the trailer video.
-  MovieDB also has search API. It could search TV and Movies.

```js
   search: (term) =>
      api.get("search/movie", {
         params: {
            query: encodeURIComponent(term),
         },
      }),
```

# 1.5 Containers

## 1.5a Container Presenter Pattern

-  We need to have three pages displaying different API data results.
-  Normally, you use class component and state to render API data. This works when your application is tiny.
   -  Class compoenent and State > Mount > render data.
-  **Container-Presenter pattern**: Container contains data, state, API, process data. Presenter shows and presents the data that has been processed but it does not have any information about the API and data. It simply receives the data and present it.

**Folder structure**:

-  Routes
   -  Home
      -  HomePresenter.js
      -  HomeContainer.js

HomeContainer.js:

Steps:

1. import React and HomePresenter.
2. State creates 5 different properties that will be used in the React.Component.

```js
export default class extends
React.Component {
   state = {
      nowPlaying: null,
      upcoming: null,
      popular: null,
      error: null,
      loading: true,
   };
```

3. render() takes 5 properties in this.state and use them to assign values that we received from the API.

```js
   render() {
      const { nowPlaying, upcoming, popular, error, loading } = this.state;
      return (
         <HomePresenter
            nowPlaying={nowPlaying}
            upcoming={upcoming}
            popular={popular}
            error={error}
            loading={loading}
         />
      );
   }
```

-  Do the same for the rest of the pages

   -  Detail
   -  Search
   -  TV

-  Search is a bit trickier because it requires some interactivity.
   -  loading false > Search enter > loading true > put the results.

## 1.5b Home Container

-  We can now work on Home Container in detail.
-  Between state and render, you create componentDidMount() that transfer the API data to the Container.
-  try...catch...finally.

HomeContainer.js:

```js
   async componentDidMount() {
      try {
         const {
            data: { results: nowPlaying },
         } = await moviesApi.nowPlaying();
         // setState with assigning nowPlaying variable with API nowPlaying data.
         this.setState({
            nowPlaying,
         });
      } catch {
         this.setState({
            error: "Can't find movies information",
         });
      } finally {
         this.setState({
            loading: false,
         });
      }
   }
```

-  **async...await**: needs to be used because it might take some time to get the data. If React continues on to the next code, it might ignore the data even if the data is retrieved later.

-  _Async needs to be used along with await or it is not going to work._

## 1.5c TV Container.

-  TV container is similar with the Home container. Therefore, we will skip the details. Please refer to the code below:
   -  Import tvApi from api.js
   -  Create compoenetDidMount() to retrieve the data from API.
   -  setState()

```js
   async componentDidMount() {
      try {
         const {
            data: { results: topRated },
         } = await tvApi.topRated();
         const {
            data: { results: popular },
         } = await tvApi.popular();
         const {
            data: { results: airingToday },
         } = await tvApi.airingToday();
         this.setState({ topRated, popular, airingToday });
      } catch {
         this.setState({
            error: "Can't find TV information.",
         });
      } finally {
         this.setState({ loading: false });
      }
   }
```

## 1.5d Search Container

**Search container logic:**

1. Handle submit > Check the search term is not blank

```js
const { searchTerm } = this.state;
if (searchTerm !== "") {
   this.searchByTerm();
}
```

2. Get the searchTerm > Find related information > Display the information retrieved.

```js
  searchByTerm = async () => {
      const { searchTerm } = this.state;
      this.setState({ loading: true });
      try {
         // Use the searchTerm to request for Search API grab the results.
         const {
            data: { results: movieResults },
         } = await moviesApi.search(searchTerm);
         const {
            data: { results: tvResults },
         } = await tvApi.search(searchTerm);

         // Set the State and assign API data to currentState variables.
         this.setState({
            movieResults,
            tvResults,
         });
      } catch {
         this.setState({ error: "Can't find results." });
      } finally {
         this.setState({ loading: false });
      }
   };
   render() {
      const { movieResults, tvResults, searchTerm, loading, error } =
         this.state;
      return (
         <SearchPresenter
            movieResults={movieResults}
            tvResults={tvResults}
            searchTerm={searchTerm}
            loading={loading}
            error={error}
            handleSubmit={this.handleSubmit}
         />
      );
   }
```

-  Set the route for the Detail pages. The user will have options to choose either a movie or a TV show. Once they make a selection, we will route them to different detail pages such as /movie or /show and create a unique pages based on the movie ID that they clicked.

Router.js

```js
<Route path="/movie/:id" component={Detail} />
<Route path="/show/:id" component={Detail} />
```

## 1.5e Detail Container

-  Header component was aware of the location of our router because we were decorating it with withRouter funciton.
-  We do not have to do this with detail because by deafult, React router is going to give all the information to the routes by giving props.
   -  Test: When you console.log(this.props) from the Detail Container, we may view the information that was sent from the router.

Set up Detail Container:

Steps:

1. Find out whether you are in /movie or /show because both goes to the same component.
2. We need to know the numbers in the props.

-  props.match.params contains id that we can extract.

-  With props, you can do goBack, goForward etc.

```js
// gets the id from this.props.
async componentDidMount() {
   const {
      match: {
         params: { id },
      },
      history: { push },
   } = this.props;

   // parse id from string to int.
   const parsedId = parseInt(id);
   // check for null. If null, send users to home("/").
   if (isNaN(parsedId)) {
      return push("/");
   }
}
```

-  Get the path from this.prop and check whether it includes() _movie_ or _show_.
-  We can set up a constructor for props also.

```js
  constructor(props) {
    super(props);
    const {
      location: { pathname }
    } = props;
    this.state = {
      result: null,
      error: null,
      loading: true,
      isMovie: pathname.includes("/movie/")
    };
  }
```

-  ComponentDidMount() will check the pathname and if it is a movie, it will get the data from the movieDetail API. If it is not a movie, it will get TVApi.

ComponentDidMount():

```js
let result = null;
try {
   if (isMovie) {
      const request = await moviesApi.movieDetail(parsedId);
      result = request.data;
   } else {
      const request = await tvApi.showDetail(parsedId);
      result = request.data;
   }
} catch {
   this.setState({ error: "Can't find anything." });
} finally {
   this.setState({ loading: false, result });
}
```

_Tip: result.data is same as {data: result} in JS by the same token as deconstructuring (const {value} trick)_

# 2.0 Presenter

## 2.1 Presenter Structure

-  Each presenter is imported and modified as below

TVPresenter.js

```js
import React from "react";
import PropTypes from "prop-types";
import styled from "styled-components";

const TVPresenter = ({ topRated, popular, airingToday, loading, error }) =>
   null;

TVPresenter.propTypes = {
   topRated: PropTypes.array,
   popular: PropTypes.array,
   airingToday: PropTypes.array,
   loading: PropTypes.bool.isRequired,
   error: PropTypes.string,
};

export default TVPresenter;
```

-  Other presenters follow the similar pattern. It requires the check on the props so that no wrong information is presented.

   -  Please refer to Detail, home, Search, TV presenters.js files for details.

-  Next, we have to create more componenets and start rendering movies with JSX and CSS.

## 2.2 Presenter Structure

-  We are going to create componenets for the presenters.

   -  path: src/components.

-  We create the styled component in the componenet folder and then use those .js files from the Presenters.

HomePresenter.js

```javascript
const HomePresenter = ({ nowPlaying, popular, upcoming, loading, error }) =>
   loading ? null : (
      // Three sections presents the title of the movies retrieved from API.
      <Container>
         {nowPlaying && nowPlaying.length > 0 && (
            <Section title="Now Playing">
               {nowPlaying.map((movie) => movie.title)}
            </Section>
         )}
         {upcoming && upcoming.length > 0 && (
            <Section title="Upcoming Movies">
               {upcoming.map((movie) => movie.title)}
            </Section>
         )}
         {popular && popular.length > 0 && (
            <Section title="Popular Movies">
               {popular.map((movie) => movie.title)}
            </Section>
         )}
      </Container>
   );
```

## 2.2 TVPresenter and Loader Components

-  We follow the similar style for the TVPresenter.

-  Loader is added. This Loader will show a clock until the data is retrieved.

_Tip: It is always important to make a loader because there is going to be delay until you process the data. Also, it is a good indication to the user that it is being processed_

_Please refer to the TVpresenter.js for details._

Grid is added on the constant Grid to display posters of movies and TV shows.

Section.js (Styled Component)

```js
const Grid = styled.div`
   margin-top: 25px;
   display: grid;
   grid-template-columns: repeat(auto-fill, 125px);
   grid-gap: 25px;
`;
// Output: the titles are displayed within a stated column (top to bottom)
```

## 2.3 SearchPresenter

-  Need to have container and form. This will intercept the event. Also, styled input.

## 2.4 Message components

-  Create error text and not found text.

1. Create Components/Message.js

Message.js

```js
import React from "react";
import PropTypes from "prop-types";
import styled from "styled-components";

const Container = styled.div`
   width: 100vw;
   display: flex;
   justify-content: center;
`;

const Text = styled.span`
   color: ${(props) => props.color};
`;

const Message = ({ text, color }) => (
   <Container>
      <Text color={color}>{text}</Text>
   </Container>
);

Message.propTypes = {
   text: PropTypes.string.isRequired,
   color: PropTypes.string.isRequired,
};

export default Message;
```

2. This message.js can be used in any presenters as below:
   HomePresenter.js:

```js
// suppose that there is an error props.
{
   error && <Message color="#e74c3c" text={error} />;
}
```

## 2.5 Poster components

1. Create Components/Poster.js
   -  const Poster
   -  Poster.propTypes

_Please refer to the Poster.js for detail..._

2. Add <Poster /> instead of the movie.id and movie.title.

HomePresenter.js

```js
<Poster />
```

3. Send API information to Poster from three presenters.

```js
const Poster = ({ id, imageUrl, title, rating, year, isMovie = false }) => (
   <Link to={isMovie ? `/movie/${id}` : `/show/${id}`}>
      <Container>
         <ImageContainer>
            <Image bgUrl={imageUrl} />
            <Rating>
               <span role="img" aria-label="rating">
                  ⭐️
               </span>{" "}
               {rating}/10
            </Rating>
         </ImageContainer>
         <Title>{title}</Title>
         <Year>{year}</Year>
      </Container>
   </Link>
);
```

4. Styling

a. Create the display block for the movie posters

Poster.js

```js
const Image = styled.div`
   background-image: url(${(props) => props.bgUrl});
   height: 180px;
   background-size: cover;
   border-radius: 4px;
   background-position: center center;
   transition: opacity 0.1s linear;
`;
```

b. All other components need to be stylized using your own wits.
c. In order to show the picture using the URL, we need to add the URL in front and concatenate with the props.bgURL as below:

```js
<Image
   bgUrl={
      imageUrl
         ? `https://image.tmdb.org/t/p/w300${imageUrl}`
         : require("../assets/noPosterSmall.png")
   }
/>
```

## 2.6 Detail Container.

-  There should be one container that shows the detail.
-  It has to be related to the movie specific to the one that user selected.

-  Componenets:

   -  Container
   -  Backdrop ( I left out backdrop.)
   -  Content
   -  Cover

-  For the Data, we have to check whether it is a /movie or /TVshow.
   DetailPresenter.js

```js
<Data>
   <Title>
      {result.original_title ? result.original_title : result.original_name}
   </Title>
   <ItemContainer>
      <Item>
         {result.release_date
            ? result.release_date.substring(0, 4)
            : result.first_air_date.substring(0, 4)}
      </Item>
      <Divider>•</Divider>
      <Item>{result.runtime || result.episode_run_time} min</Item>
      <Divider>•</Divider>
      <Item>
         {result.genres &&
            result.genres.map((genre, index) =>
               index === result.genres.length - 1
                  ? genre.name
                  : `${genre.name} / `
            )}
      </Item>
   </ItemContainer>
   <Overview>{result.overview}</Overview>
</Data>
```

_Tip: map() function has index that you can use._

_Please refer to the DetailPresenter for detail._

## 2.6 React Helmet

-  You can use react-Helmet to set the head of your website easily.

# 3.0 TypeScript with React

-  TypeScript is a superset of JavaScript.
-  It adds more features to JavaScript.
-  TypeScript allows developers to make less mistakes.
-  For example, in javascript, we have to write our own data validation in the function, but in typescript, we can make TS disable such inputs.

```ts
const plus = (a: number, b: number) => a + b;
console.log(plus("lalalal", 2));
// by stating the input types, uses cannot write any other types because the one that is specified. It raises an error and let the user know.
```

-  **When you mix TypeScript with JavaScript, you will be like a boss :)!**

# 3.1 Typing

**Typing**:

-  TypeScript can also make the developer make less mistake by doing below by using _Typing_

ex1: Typing example

```ts
let hello: string = "hello";
hello = 4;
// this is now allowed since you already stated the type of the const that you created.
const plus = (a: number, b: number) => a + b;
```

**Typing return value**
ex2:

```ts
const greet = (name:string, age:number):string => { return `Hello ${name} you are ${age} years old`)}
// you can also specify what you are going to return. In this particular case, it is a string.
```

**Interface**

-  interface is a shape of an object.

```ts
const enoch = {
   name: "Enoch",
   age: 18,
   hungry: true,
};

interface IHuman {
   name: string;
   age: number;
   hungry: boolean;
}

const helloToHuman = (human: IHuman) => {
   console.log(`Hello ${human.name} you are ${human.age} old`);
};

helloToHuman(enoch);

// Since TS now know what IHuman means because of the interface, it will print out the helloToHuman() function without an error.

// TypeScript does not know what human is so it will give an error.
```

-  if you put the question mark on the variable inside the interface, it will create an optional value where undefined value is allowed.

# 3.2 React State and TypeScript

**I have excluded the TypeScript incorporation to React because it seems like an extra for me now. Once I become adept in React + hook, I can move on to using TypeScript. TS will give better security on the code in general because it states the types for returned arguments. (Also, it becomes easy for developer to notice errors)**
