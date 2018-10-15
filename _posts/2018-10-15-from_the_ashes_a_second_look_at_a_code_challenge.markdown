---
layout: post
title:      "From the Ashes. A second look at a code challenge"
date:       2018-10-15 13:49:07 -0400
permalink:  from_the_ashes_a_second_look_at_a_code_challenge
---


The job hunt has been quite hard for me. Overcoming lots of anxiety, imposter syndrome and trying to convince myself that I am indeed a badass programmer is not easy. Add on top of that some of the most grueling code challenges and anyone would feel like a complete failure.

Recently I was given a code challenge that wasn't quite like the others. Previous code challenges had me solving complex problems and one had me use broken drag and drop software to develop an app. This one however was actually whithin my ability.

Hit a github endpoint, and print its data.

I was confident. So very *confident*, just over the moon! 

It was pretty open ended. So I had many choices to make on how to do it.
Javascript or Ruby? I could use Ruby. Bust out the Faraday gem and feel smart for doing something thats more obviously better suited for Javascript. Or...just use Javascript.

VanillaJS or Use a fancy library?
I didnt wan't to blow this. I have overly complicated things before, which led to failure. K.I.S.S (keep it simple sweetie) 
This program should be easily extendable. What if we pair on it? The less breaking points the better. Lets keep it vanilla and I can show my knowledge of javascript as well. Sound logic! I was going to test the entire program. I had been learning how to write tests in ruby. Ill do the same in Javascript. (Also sound logic?)

Lets first say, testing in Javascript and Ruby are nothing alike and I still havent figured out how to do it. That will be for another day. But I did try! Unfortunately this made my simple program even more simple.

So I started my project the same way any great project starts. With create-react-app!
Then deleted all the react code. This isnt a react app silly! We are keeping it simple!

```
import fetch from "isomorphic-fetch"

export const getRepos = (name) => {
    return fetch(`http://api.github.com/users/${name}`)
        .then(res => res.json())
}

export const renderResult = (data) => {
    let html = ""
    html += `<h1>User: ${data.login}</h1>
            <h2>Name: ${data.name}</h2>`
    Object.keys(data).forEach(key => {
        if (data[key] && key !== 'login' && key !== 'name')
           html += `<p>${key} : ${data[key]}</p>`
    })
    document.getElementById("root").innerHTML = html
}

export const getReposAndRender = (name) => {
    getRepos(name)
        .then(data => renderResult(data))
}

export const searchForRepos = () =>{
    const search = document.getElementById("query").value
    getReposAndRender(search);   
}

document.addEventListener('DOMContentLoaded', function(){
    document.getElementById("search-box").addEventListener("submit", function(e){
    e.preventDefault(); 
    searchForRepos();
    })
})



```
Thats it! Thats my code. That weirdly wrapped event listener at the bottom is because I have no idea how to write tests in javascript and my 1 test I was proud of wouldnt run without it. I wasnt going to delete that test though!

Did I mention how proud I was of this code? It worked. I was super happy! Look at it working!

![Imgur](https://i.imgur.com/9QNE3bi.png)

Okay, yea...thats pretty simple. Also look at the code. Im not even hitting the repo endpoint. I labled these functions completely wrong. Also despite testing this code and realizing there was an issue when users couldnt be found, I forgot to handle that particular problem. 

But it did do what it was supposed to do! I obviously had it in the bag, I am currently sitting in my cushy dev job now right?

**Of Course Not**
This code showed nothing. It showed I can fetch data and display it, but it showed nothing of my true skills and naturally they picked up on that. It was pretty gut wrenching because I made a strategic choice and it was an incredibly bad strategy. I AM a good programmer but I didnt show it that day. So I want to show off the program, I wish I submitted.
![Imgur](https://i.imgur.com/LOpVcz5.jpg)

So how did I make these changes?
First off I had a little help from my [codepen.](https://codepen.io/Saturn226/)
My codepen is like my little scratch pad. I had been practicing a lot of react snippets lately. Mostly I have been experimenting with styled-components and css-grid which I used in the new project.

I created 3 components. A search component. A repos component and a results page component.
```
import React, {Component} from "react";
import styled from "styled-components"
import {ResultsPageComponent} from "./resultPageComponent"

export class SearchPageComponent extends Component {

    getInitialState = () => {
        return {
            searchUser: "",
            error: null,
        }
    }

    state = this.getInitialState();
    
    handleErrors = (response) => {
        if(!response.ok){
            throw Error(response.statusText);
        }
        return response.json()
    }

    

    searchUser = () => {
        if (this.state.searchUser) {
            return fetch(`http://api.github.com/users/` + this.state.searchUser) 
                .then(res => this.handleErrors(res))
                .then(user => this.setState({user}))
                .then(()=> this.setState(this.getInitialState())) //fixes an issue where child components may not rerender

                .catch((e) => this.setState({error: e.message}))
        } 
    }

    handleOnChange = (e) => {
        this.setState({searchUser: e.target.value})
    }

    handleOnClick = (e) => {
        e.preventDefault();
        this.searchUser();
        this.setState({searchUser: ""});
    }

    render() {
        return (
            <Div>
                <SearchArea> 
                    <form onSubmit={this.handleOnClick}>
                        <strong>Search for user:</strong>
                        <input type="text" required value={this.state.searchUser} onChange={this.handleOnChange}></input>
                        <Button>Submit</Button>
                    </form>
                </SearchArea>
                <ResultsPageComponent user={this.state.user} error={this.state.error}/>

            </Div>
        );
    };
};

```



This component is pretty much the main component. It handles the get requests (now properly named) and sets the user to the current state. Which I then pass as props to the ResultsPageComponent for rendering. I had to reset the state after every request. Not doing so creates an issue where It wont show another user after a request returns a 404. I admittedly dont have a great understanding of why this is, but intend to research it more.

```

import React from 'react';
import styled from 'styled-components';
import ReposComponent from './reposComponent.js';

export const ResultsPageComponent = (props) =>  {
  

    if(props.error){
      return <h1>{props.error}</h1>
    }

    if (props.user) {
      const {
        login, avatar_url, repos_url, name = "NA", organizations_url, bio,
      html_url, company, location, blog, id} = props.user

      const shamelessPlug = login === "Saturn226" ? <h1>HIRE ME!</h1> : null
  
      return (
        <Div>
          <Img src={avatar_url} />
          <Ul>
            <a href={html_url}><li><h1>{login || "N/A"}</h1></li></a>
            <li><h2>{name || "N/A"}</h2></li>

            <li><strong>Bio:</strong>     {bio || "N/A"}</li>
            <li><strong>Company:</strong> {company || "N/A"}</li>
            <li><strong>Location:</strong>{location|| "N/A"}</li>
            <li><strong>Blog:</strong>    {blog|| "N/A"}</li>
            {shamelessPlug}
          </Ul>
          <ReposComponent repos_url={repos_url} />

        </Div>
      );
    } return (<h1>Search for a user!</h1>); // Will render the first time component mounts
  }

```

This is a lot more straight forward. Destructuring props.user and printing its data. Sometimes the data is null. Such as my own profile. So I print out "N/A" so that its at least showing something. 
I wanted to go an extra mile, so realizing that I have access to the repos_url, I can now pass that in as props to another component and output those!

```
import React, {Component} from 'react'
import styled from 'styled-components'

export default class reposComponent extends Component {

    state = {
        repos: []
    }

    componentDidMount(){
        this.getRepos()
    }

    componentDidUpdate(){
        this.getRepos()
    }

    getRepos = () => {
            return fetch(this.props.repos_url + '?sort=updated-desc') 
                .then(res => res.json())
                .then(repos => this.setState({repos}))
                .catch((e) => {this.setState({error: e.message})})
        }
    
    
    render(){
        let limit = Math.min(this.state.repos.length, 4); //creating a limit for the amount of repos to show
                                                         // for the future this can probably be passed in as a prop
                                                         //the limit is 4, or the length of the repos array whichever is smaller
                                                         //this ensures we wont go out of bounds on the slice
        
        const limitedArray  = this.state.repos.slice(0, limit) //grab the specified number or repos

        const repoList = limitedArray.map(repo =>{
            return <Item key={repo.id}>
                        <a href={repo.html_url}><p>{repo.name}</p></a>
                        <p>{repo.description}</p>
                    </Item>
        })
        return(
                <Ul>
                    <h2>Most Recently Updated Repos</h2>

                    {repoList}
                </Ul>
        
        )
    }
    
}
```

When this component mounts and updates it should automatically give back the repos data. I decided to sort this by last updated and return the first 4 repos that came up. I use Math.min to make sure I get the lowest of two numbers between my hard coded limit or the length of the repos array. This way i dont go out of bounds on the slice.

All of these components are styled with styled components and grid. Though grid isnt doing its job at being responsive right now. I plan to add responsiveness soon. There are probably better ways at doing styling in react and there are two different camps on whether styles belong in the component or css files. I like to experiment around.

To me this app looks fantastic and I am no longer proud of my previous attempt. This one however. I am in love with how it turned out and in the wise words of a dragon ball Z villain that refuses to die. "This isn't even my final form"

One thing that I would like to add. Due to having incredible difficulty typing at the moment. I had to dictate how to write this code to a person who knows nothing about javascript. Have you ever tried to tell someone how to type an arrow function curly by curly? This has simultaneously been the most fun and pain in the ass experience I had "writing" code. It provided amazing clarity however and allowed me to think more about the hows and whys of doing certain things. 






