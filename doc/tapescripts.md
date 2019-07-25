For a written version of this course, check out the [Gatsby docs](https://www.gatsbyjs.org/tutorial/building-a-theme/).

## Transcript 01

Instructor: 00:01 In an empty directory, create a package.json. Inside the package.json, you're going to set private to true. Then you're going to set up your workspaces. This is an array of folder names. We're going to have two packages in this workspace. The first one is going to be called gatsby-theme-events. The second one will be called site.

00:31 Next, we're going to create folders for our packages. The first one is going to be called gatsby-theme-events. It's going to need its own package.json. We'll also need site with a package.json.

00:48 Inside the package.json, we're going to set up our theme. We need a name. This one is going to call it gatsby-theme-events. It's going to have a version. That version will be 1.00Then it's going to need a license. We'll set the license. Use the MIT license for now.

01:12 Because this will be installed as a package, we have to provide a main file, which we'll mark as index.js. In order to resolve properly, that file does need to exist. For now, we're going to create an index.js. We just leave a little comment to let people know that it was left blank on purpose.

01:34 In the site package.json, we're going to set it to private because we're not actually going to publish this site as an npm package. We're also going to give it a name, which is going to be site. This is what we refer to when we use a workspace.

01:48 Next, we're going to set a version. That version will again be 1.00Then we will set a license. That license will be the MIT license. We're also going to want scripts here. For our scripts, we want to have a build script. This is going to run the gatsby build command. We want a develop script. This will run gatsby develop. Finally, a clean script. This runs gatsby clean.

02:23 We want these to be available in both our theme and our site. We can copy-paste this over to our theme. Next, we can open up the terminal. We're going to install dependencies. To do this, instead of using the standard yarn add, we're going to use yarn workspace and then the package name, site, and add.

02:50 We want Gatsby, React, and ReactDOM, as well as our theme. We're going to install gatsby-theme-events. The reason that we use the `@*` is because we want the workspace to pick up an unpublished theme.

````
yarn workspace site add gatsby react react-dom gatsby-theme-events@*
````

03:14 Now that the installation is complete, we can see that it is referencing gatsby-theme-events. If we run yarn workspaces info, we can see that the site is using the gatsby-theme-events from the workspace.

````
yarn workspaces info
yarn workspaces v1.17.3
{
  "gatsby-theme-events": {
    "location": "gatsby-theme-events",
    "workspaceDependencies": [],
    "mismatchedWorkspaceDependencies": []
  },
  "site": {
    "location": "site",
    "workspaceDependencies": [
      "gatsby-theme-events"
    ],
    "mismatchedWorkspaceDependencies": []
  }
}
Done in 0.11s.
````

03:33 Our theme needs to have Gatsby, React, and ReactDOM added as peer dependencies. We'll do yarn workspace gatsby-theme-events. Add with the -P to mark it as a peer dependency. React, ReactDOM, and Gatsby.

````
yarn workspace gatsby-theme-events add -P react react-dom gatsby
````

04:01 During development, we're going to use our theme as a regular Gatsby site. We'll also set it up as a development dependency using -D. Once these finish installing, we can check our package.json and see that we have Gatsby, React, and ReactROM set up as both peer dependencies and dev dependencies.

````
yarn workspace gatsby-theme-events add -D react react-dom gatsby
````

04:27 To make sure that everything's working, we can just go ahead and start up Gatsby in development mode. We'll start with the site, yarn workspace site develop. This will start our Gatsby server in develop.

````
yarn workspace site develop -H 0.0.0.0 -p 6206
````

04:42 We don't have any content here. It's going to be an empty site, but we can grab out localhost 8000. We can go out to our browser and paste this up. We can see that Gatsby's running. There's no pages yet. Again, it's just going to give us this 404, but we can see that it's working.

04:59 Let's do the same thing with the theme, so gatsby-theme-events run develop. Now we can see here's localhost 8000 again. If we come back out and refresh the page, again we get that 404. Gatsby's running. It just needs some content.

````
yarn workspace gatsby-theme-events develop -H 0.0.0.0 -p 6207
````

## Transcript 02

Add Static Data to a Gatsby Theme
Jason Lengstorf

gatsbyGatsby
>=2.13.1

Learn how to load data from MDX (or any data source) and ensure the necessary folders exist.

To do this, we'll need to add the data in a folder in the project and install two plugins: gatsby-source-filesystem and gatsby-transformer-yaml

In gatsby-config.js, both of those plugins will be defined in the plugin array and the data will be exposed through out our application through GraphQL.

For a written version of this course, check out the [Gatsby docs](https://www.gatsbyjs.org/tutorial/building-a-theme/).

Instructor: 00:00 To get started on our theme, we need some data. We're going to create a folder called Data, and inside of it, we're going to create events.yml. Inside here, we're going to add information about conferences. Each of these conferences has a name, location, a start date and end date, and a URL.

00:19 To read these, we need to install a few more dependencies. In the terminal, we're going to run yarn-workspace-gatsby-theme-events add-gatsby-source-file-system. This is so that we can load this events.yml. Then we're going to install gatsby-transformer-yaml so that we can turn that yaml into something that Gatsby can use.

````
yarn workspace gatsby-theme-events add gatsby-source-filesystem gatsby-transformer-yaml
````

00:50 As we are waiting for that install, we're going to create a gatsby-config.js. In this, we're going to set up module.exports. This is our gatsby-config object.

01:04 In the plug-ins array, we're going to add gatsby-source-file-system. For it's options, we want to point to this data folder that we just created. Next, we add gatsby-transformer-yaml, and we're going to pass in an option to set the type name to event.

01:35 With our plug-ins configured, we can start up a Gatsby development server and see the data that's been loaded. Start this with yarn-workspace-gatsby-theme-events-develop. This starts up the Gatsby development server. While it hasn't added anything to the page, it is going to give us our GraphQL.

01:55 When we open localhost 8000/underscore-underscore-underscore-graphql, we're looking at the graphical explorer. In here, we can come in and take a look. We can see that we've got an all event query. Let's grab that and the nodes, and for now, let's just get the name.

02:14 We can see that the data from our yaml file has now been loaded. If we continue checking boxes, we can see that we have all of that data coming out which is great. We're now ready to start building pages.

## Transcript 03

Create a Data Directory in Gatsby using the onPreBootstrap lifecycle

Learn how to architect the React components in your Gatsby theme to maximize reusability and extendability while minimizing maintenance and hassle.

The first step is to make sure the data directory exists so because gatsby-source-filesystem will throw an error if it doesn't.

This lesson will cover that step.

Instructor: 00:00 To create pages, we need to create a file called gatsbynode.js. Inside our Gatsby node, we need to do several things. First, we need to make sure the data directory exists. The reason for that is that when we first fire up our theme, if that data directory doesn't exist, Gatsby's source file system is going to throw an error.

00:23 Second, we need to define the event type. The reason for that is that if we don't have any events to find, we should get an empty array not an error. Once that's done, we need to define resolvers for any custom fields. In this case, we're going to have a slug field.

00:45 Finally, we need to query for events and create pages. Our first step is going to be to make sure the data directory exists. To do this, we're going to use the onPreBootstrap API hook from Gatsby.

01:03 We're going to grab the reporter out to let people know that things are happening. Inside of this, we're going to decide where our content lives. We'll call this content path. This is going to be data. That's the data directory that we created over here.

01:20 Next, we're going to use the FS dependency from Node. This is a built-in. We're just going to get FS by requiring FS. Then we want to check if the content path doesn't exist, we want to create it.

01:43 We'll start by sending out a message. We'll say reporter.info, and we are creating the content path directory. Then we use FS. We're going to use makedersync to create the content path.

02:04 Now whenever we fire up a site using this theme, the first thing that'll happen is it's going to check if the data directory exists, and if not, it'll create it.

## Tapescript 04

Set up to Create Data Driven Pages in Gatsby

To actually create pages, we'll need to:

    define the event type
    define resolvers for custom fields
    query for events

Once we do this, we'll be ready to finally display the data in a page which is covered in: Display Data in React Provided in Gatsby with GraphQL

Instructor: 00:00 Our next step is to use the sourceNodes API hook, which will give us an actions object. We're going to define our node type or our event node. We do that using the createTypes action. Inside of this, we need to set up an event type that's going to implement the node interface.

00:26 We don't want Gatsby to infer any fields. We want to make sure that each one of them is explicitly defined. We're going to need an ID. We're also going to need the name, which is going to be a string. We need a location, which is going to be a string. We need the start date.

00:47 You'll note if we look at the events, start date doesn't actually line up with startDate, but we do want to use that camelCase style. We'll deal with that in just a second. Because the start date is a date, we want to use Gatsby's date built-in. We're going to use date format like this.

01:07 To make sure that we can actually load the start date into this field despite the fields not lining up, we're going to use the proxy directive, which will let us say where to pull it from. In this case, it's going to be from start date.

01:22 We can do exactly the same thing for the end date by duplicating that whole line and swapping it out for end date. Finally, we want to get the URL, which is this string. This URL is the live website. We also need to be able to create a URL on our own website.

01:41 We'll do that using a slug. The slug is going to be a string. It is a required string, but you'll notice that we don't have a slug to find in this object. We're going to need to define one ourselves, which we'll do with this custom resolver.

01:57 Gatsby gives us a createResolvers API hook. That gives us a function called createResolvers. Inside this function, we are going to set up a base path. This is the URL base. In our case, it's going to be the site index.

02:17 Then we need to set up a function to actually create our slugs. We're going to call this slugify. This is going to accept a string, which will be in our case the name of the event. That is going to need to return a slugified string.

02:35 Our first step is to turn the name into a slug, which we'll do by creating a variable called slug. That's going to get the string, which we will then turn into a lowercase string. After we have the name converted to lowercase, we need to change any character that's not a letter or a number into a hyphen. We'll use a regular expression for that with the replace function.

02:59 Let's set up a character set using square brackets and say anything that's not -- that's what this caret at the beginning means -- a lowercase A through Z or the number zero through nine. We can take one or more of those. We're going to take all of them that we find, not just the first occurrence, but every occurrence in the string. We're going to turn it into a hyphen.

03:23 This gives us a lowercase hyphenated slug, but it might still have a leading or a trailing hyphen if it started with, say, a quote. We want to add another regular expression to correct that.

03:36 We'll check if it starts with a hyphen. That's what the caret hyphen here means or with a pipe if it ends with a hyphen, which is what hyphen dollar sign means. Again, we'll just do a plus sign in case there are two hyphens or something at the end. We'll find every occurrence in the string and replace that with an empty string, so basically remove it.

04:02 Now that we've got a slug, we can return that. We're going to include the base path first, add a slash and then the slug. You probably just noticed that the base path has a slash in it. We're leading with a slash and we're adding another slash here. That actually means this is going to be three slashes.

04:22 We're going to do one last thing to prevent over-slashing. We're going to look for a forward slash, any sequence of two or more forward slashes, which we're doing by escaping a forward slash here, escaping another one, and then checking for one or more of those. This is going to be two-plus forward slashes. We'll replace those with a single forward slash.

04:50 The reason we're doing that is that if we decide to change this base path or we set up from an option, which we'll do in another lesson in this course, you want to make sure that you can't accidentally break your slugs by adding too many slashes. This is just a real quick check to make sure that you're returning a properly slashed slug.

05:12 With our slugify function defined, we can create our resolvers, which we do with the createResolvers function. Pass that an object. We get into the event type, the slug field. Then we want to set a function to resolve it. That function gets the source, which would be in this case the event node, so the name, location, start date, etc. We want to return the slugified version of the event name.

05:47 We can test that this is working by running yarn workspace gatsby theme events develop. This will give us our GraphQL layer again, which we can see here at 8000/graphql. If we open up GraphQL again, we can go back to all event, look into our nodes. Now we can see the start date and end date have been switched up to camelCase and we have a slug available.

06:22 Let's open these up. Because we set the end date and start date to be date types, we also get these helper functions, where we can do things like say fromNow. Using fromNow, we're able to do things like relative time, like how far away an event is, or we can format the string with something like this, where it'll give us the long date, the day, and the long year or a short date.

````
yarn workspace gatsby-theme-events develop -H 0.0.0.0 -p 6207

http://0.0.0.0:6207/___graphql

query MyQuery {
  allEvent {
    edges {
      node {
        name
        endDate(formatString: "MMMM DD, YYYY")
      }
    }
  }
}
````

## Transcript 05

Create Data Driven Pages in Gatsby with GraphQL and createPages

To create pages in Gatsby, there are 4 steps needed before actually doing the thing:

* ensure `data` directory exists
* define the event type
* define resolvers for custom fields
* query for events

Now that these are done (see lesson 03 and lesson 04), let's create the `allEvent` query and loop through the result with `createPages`.

Instructor: 00:00 Our last step in Gatsby Node is to create pages both for the even previews and individual event pages. We're going to do that using the createPages API hook. This one, because we're going to be running a GraphQL query, is going to be async.

00:17 We're going to start up by getting the actions. We want GraphQL. Let's also grab that reporter. Set up a base path, which we will set up to be just a / right now. We'll build the pages at root. Let's set up our first page, which we'll do using the createPage action.

00:42 This one is going to live at the base path. The component we're going to use doesn't exist yet, but it's going to be called event. We'll put it at source/template/events.js. We'll create that component in just a second.

01:01 Next, we'll set up our GraphQL query for individual events, which will store and result. Result is going to be the result of the GraphQL query, which will await because it returns a promise. That query is going to be all event and we want to sort these by the start date. We're going to sort them in ascending order.

01:24 We're going to get the fields of start date and the order is going to be ascending. All we need back from this is for each event node, we're going to get the id and the slug. If anything goes wrong, it will show up in result.errors. We can check for that here.

01:49 If something does go wrong, we'll run reporter.panic, which will cause a builder and put out an error message. We'll say there was an error loading events, and show the actual error that showed up, so reporter.errors.

02:07 Just to make sure that nothing else happens, we'll return. If everything goes well, we're able to grab at our events, which we will extract by going to result.data.allEvent.nodes. It will loop through each event.

02:27 We're going to get the slug out, which is going to be at event.slug, and call the createPage action. The path will be the slug. The component doesn't exist yet, but it's going to live in the source/templates/event.js file.

02:54 To make sure that we can identify which event is which, we're going to send in the event id as context. That is going to be pulled out of the event object at .id.

03:06 Our last step to make sure that these pages build is to create the components. We need to create the event and the events components in source/template.

03:19 Let's create the source/template and we'll start with events. These aren't going to show any data yet. They're just going to ensure the things don't break. We'll call this one events template. It's going to give us what to do.

03:46 We'll export that as the default. Copy that. We're going to create another placeholder. This one is going to be called event template. This is going to be for single events. To test this, we can run yarn workspace Gatsby theme events develop. That will give us a local host 8000, or we can go check to see that we've got our event's page.

04:28 If we trigger for 404 to see what's being created, we can see the pages for dinosaur.js, React Rally delete developer and JSHeroes have all been created. Each of those says there will be event page.

````
yarn workspace gatsby-theme-events develop -H 0.0.0.0 -p 6207

http://localhost:6207/404
````

## Transcript 06

Display Sorted Data with useStaticQuery in Gatsby

Now that the event list data is available to us and the subsequent pages created, we need to create a template in Gatsby to render this list too.

In this lesson we'll use `useStaticQuery` provided to us from Gatsby and query for the data available. Then a `EventList` component will be created for that data to be passed into and displayed properly.

A `Layout` Component will also be created so there is a consistent look throughout our application.

Instructor: 00:01 To show event data, we're going to import GraphQL and use static query from Gatsby in our events.js component. We need to make sure that we're able to actually run things inside of this, so we'll refactor it. We're going to set up a data constant. That's going to be the result of the useStaticQuery hook.

00:33 The query that we're going to run is going to be allEvent. We're going to sort by the start date field. We want to get that back in ascending order. For each node or each event in the event database, we want to get the ID, the name, the start date, the end date, the location, the URL, and the slug. That's every piece of data that we have.

01:07 When that comes back, we're going to store that in events. This is just going to be an alias for data.allEvent.nodes to make it a little easier to read. We want to pass that into a component.

01:25 We want to do two things here. First, we want to create a general layout that we'll reuse across all of our sip pages. Then we want to create an event list. Let's show what this will look like and then create the components that we need.

01:39 We'll have a layout. Then we'll have an event list. That will take an events prop that will get the value of the events from the database. Let's go in and create this. First, we're going to create a components folder and a layout.js. For now, this is going to be pretty basic. We'll call it layout.

02:12 It's going to have a children prop, which is whatever's inside of it. It will return just a div with an h1. It'll say gatsby-events-theme. How about that? Then it will return the children. We'll export default layout. That'll be that for now.

02:42 Next, in the components directory, we're going to create event-list.js. This one is also going to import React from react. It's going to be called event-list. It takes one prop, which is events. Inside of it, for now, let's just dump that data. We'll do a json.stringify of the events. We'll use a little bit of formatting to make sure that looks OK.

03:17 We'll export default event list. Then let's go back to events.js and make sure that we've imported these components. We'll import Layout from components/layout. We'll import EventList from components/event-list. Then we'll save that and start up our server again, so yarn workspace gatsby-theme-events develop.

03:53 When we open localhost 8000 in our browser, we can see that our layout shows up. We have gatsby-events-theme. We have the dump of all of the event objects. This is an array with each event in it. We can see we've got the ID, name, and all the other data for these events.

04:11 To display these, we're going to update our event list. The nice thing about this is that when we look at this, we've created what I like to call the layer cake. We have our events template. This fires off a query and then uses that query into a component that takes the event as a prop.

04:34 Because we've now sent these in as a prop, event list doesn't have to worry about data. It just has to worry about props, which makes it much easier, as we get into things like component shadowing, to allow people to do what I like to call the progressive disclosure of complexity, where they can peel back one layer of abstraction at a time.

04:54 They can update the way that this component works without having to care about GraphQL. When they need to go deeper, they can jump in and start messing with the GraphQL query or even go a little bit deeper than that and start playing with the Gatsby node.

05:09 Let's start by getting rid of this pre tag. Then we're going to set up a fragment because we don't want to generate unnecessary markup here. We'll put a heading. That heading is going to be, for now I guess, just upcoming events. How about that?

05:31 Then we're going to set up an unordered list. We'll do an events.map through each event. That's going to return a list item. Each list item needs a key. We'll use the event ID for that. Then we're going to set up a strong tag that's going to be a linked name of the header.

05:56 To link internally, since we're linking to a page on the same site, we're going to import the link component from Gatsby. Then we're going to use that down here. We're going to go link to and, because we've got the slug, we can go to event.slug and then event.name.

06:21 Outside of the strong tag, we're just going to add a line break. On the next page, we will set up the date. We're going to use the built-in date handling. We'll do event.startDate. Create a JavaScript date object out of that. Then we're going to use toLocaleDateString.

06:42 I'm going to use en-US as the locale. That's American English. We want to use the long version of the month. We want to use the numeric version of the day. We also want to use the numeric version of the year.

07:02 After that, we're going to add where it is. It's going to be on that date in event.location. If we save that, we can come back out and take a look. Now we can see we've got dinosaur.js, React Rally. If we click through, it takes us to the appropriate page for each event.

## Transcript 07

Display and Query Data by id with Gatsby context and useStaticQuery

Similar to `EventList`, we'll need to create a template React component for each individual Event page.

To do this, we'll need to add a page query, to access GraphQL variables, to query for individual events by id. Once that data is available, it can be passed into an `Event` component that will display the data for us.

Instructor: 00:00 Next up, we can do the same thing but for individual event pages, so this is going to be a little bit different. We're going to import GraphQL from Gatsby, and we're going to then export a page query. The reason that we're doing a page query here is that we need access to GraphQL variables.

00:21 In pages, when we create pages using context, anything that we add to the context becomes available as a GraphQL query. In here, I can set up my query and then use the event ID which will be a string. I'm going to get an event with an ID that is equal to the event ID that we passed in context.

00:50 Then I want the name, the URL, the start date, and I will format that event string as long-month-name and then the day. Here, we'll also do the same thing with the end date. We want the location and the slug.

01:19 What's going to happen? That will be passed in the data prop. Anything that goes into page query gets injected into this component as the data prop. Then we can just destructure that to get to the event.

01:37 With this, we want to refactor to use that layout component. We'll do layout, and we can have it auto-import for us. Then we need to create a new component called Layout. We're actually going to spread the event right into it. What that means is that this component is going to get each of these pieces of data as a prop.

02:06 We can go into components and create event.js. If we import React from React, we can then set up event and we'll start with props. Let's just spread that so we can see.

02:23 We've got the pre-tag. We will stringify our props. Let's see what comes out. We'll export that as our default. If I import this from event then we jump back out.

02:43 We can see that it is giving us the name, URL, start date, end date, location, and slug as props. I can in this event component now turn it into actual markup. For this markup, we can delete that dump.

03:04 We'll set up a div, then we're going to set up a heading. We can destructure here so that we've got it. We're going to have the name, location, URL, start date. End date is what we're going to use today. We'll grab that name and say where it is. It's going to be on location.

03:27 Then we want to show off the date. Now the date is going to be a little funky, I'll show you why. If we start with the start date and then go to end date, that'll look OK instead of the website.

03:47 The website is an external link so we're not going to use the Gatsby link component. We'll set that to URL, and we'll just show that URL so that you know what your'e clicking on. If we save that and come back out here then we can see this looks OK.

## Transcript 08

Style and Format Dates in React

The `Event` Page is looking pretty good. But we can do better.

The date currently comes from a query that gets the start and end date as props and dumps that data out in a `<p>` tag.

In this lesson, we'll add some custom logic to display the date of an Event how you would expect it.

Instructor: 00:00 Right now, the date, but it doesn't look great. The date currently comes from a query that looks for the start date and the end date. This is then passed into our Event component. Inside the Event component, we get the start date and end date as props which we then just set next to each other, so the start date and en dash and then the end date.

00:21 While this is fine, it's not ideal. We're going to refactor this component with a little bit of business logic to read those dates and display them in a way that's a little more human-readable.

00:31 We'll start out by saying let's create a new component called EventDate. This one is going to take a start date and an end date. First, we're going to turn in each of these into a new Date object. We'll send in start date. We'll get the end. We'll turn that into a Date object.

00:57 Then we're going to do a check to see if it's a one-day event, so if it happens on the same day. One example of this is -- let's see -- this one here. This one happens on the same day. We can go say if it's one day, we'll say start.toDateString equals end.toDateString.

01:19 Then we can return a component. We'll use a fragment to make sure that we're not generating any junky markup. We're going to use the Time component with datetime so that it's machine-parsable. That's going to need to contain something. Then this one is going to need to contain the actual date.

01:41 The way that we want this to work is, in pseudocode, if it's two days or multiple days, we want it to look something like this, June 21st to 23rd, 2019. It'll look something like that. If it's one day, we want it to look like this. If it crosses month boundary, so if it was June 30th to July 2nd, we want it to look like that.

02:12 We need to add all of that logic. The way that we're going to do that is first we're going to create a little helper function to get our date so that we don't have to have a bunch of duplicate code. We'll do getDate. That's going to accept our date.

02:28 Then we're going to say whether we want the day, which will default to true, the month, which will also default to true, and the year, which will default to true. We'll also make sure that if you don't pass in an argument, it'll default to an empty object, which means everything will be true.

02:48 What we're going to give back is the toLocaleDateString. That's going to be in the US locale. We'll say the day is going to be day. It's either numeric, or if day is false, it'll be undefined. We'll do the same thing for the year. For the month, it's going to be long if it's not undefined.

03:26 Then, down in this time, we can set it up to say we want to get date using start. We will say only show the year if it is one day. For the datetime, we're going to use the start.toISOString. To start, let's use this event date down here. We'll just say event date with a start date of start date and an end date of end date.

04:17 This gives us a time datetime. We can see that ISO string comes in here. Because it is one day, this shows us the year. If we go back and look at one that's multi-day, it doesn't show us the year. We want to finish that out now by getting the rest of these.

04:39 We'll say up here that if it's not one day, then we will use another fragment. Add that en dash. Add another time, the datetime. This one is going to be the end.toISOString. We will getDate using the end. The month will be if the start.getMonth does not the end.getMonth, then we want to show the month.

05:29 This basically says only show the month if the start month and the end month are different. I need to add in my last fragment. If I save that, then when I come back out, I can see we've now got June 20th to 21st. Perfect.

05:51 If we go back to the main list, we can see that that is working properly. For a one-day conference, it gives us what we want. That puts us in good shape. We're looking pretty good here.
