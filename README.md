# Balance of Nature - Front-end Coding Standards and Best Practices

## Overview

These principles, guidelines and standards allow us to:

- the code must be readable and understandable by all team members. 
- produce code of a consistent quality across all projects
- work simultaneously with multiple developers on the same codebase at the same time in the same way
- produce code less prone to bugs and regressions, easier to understand and debug
- write code that supports re-use.
- assistance in onboarding new development team members for new and existing projects.

### Contributing
- Can anyone who has access to the repository contribute?

Table of Contents
1. [Default Setup for Frontend](#setup-default)
2. [NuxtJS Structure](#nuxtjs-structure)
3. [Coding standards](#coding-standards)
4. [QAs Patterns](#qas-patterns)
5. [CSS](#css)
6. [GIT](#git)
7. [Accessibility](#accessibility)

# NuxtJS Structure/ Coding 
# Folder structure

For the most part we're using the default nuxt folder structure, there’s just a couple of places where it really differs from project to project, standard to standard: the components and the pages folders. 

For the components folder there are two widely adopted standards:

### Flat component directory/no subfolders on the components folder

The advantages of this standard, when used with the standards for naming related components are:

- Use your IDE's quick find or file jumping feature to filter files based on their most general attribute down to the more specific
- Remove analysis paralysis when it comes to deciding how to organize components into sub directories
- Be able to see all your components at once in a single list
- Get rid of the redundancy of keywords in filenames AND in the directory (that is if you're following the style guide (and you should be) and you're using nested directories) (ie. `post/PostList.vue`, `post/PostFeature.vue`, etc)
- Remove the temptation to use short one word component names which is easier to do with nested directories (ie. `post/List.vue`, `post/Feature.vue` ) and violates the style guide
- Eliminate surfing the file structure in and out of directories to find a component
- Simplify importing components (will always be `import SomeComponent from "@/SomeComponent"`)

On this structure the components folder looks like something like this

```jsx
/components
|-AppButton.vue
|-AppLink.vue
|-AppLoginForm.vue
|-AppRegisterForm.vue
|-ProductsList.vue
|-ProductsListItem.vue
|-ProductsListDetails.vue
|-TheSideBar.vue
|-TheHeader.vue
|-TodoList.vue
|-TodoListItem.vue
|-TodoListButton.vue
```

### Having a folder structure inside the components directory

The advantages of this standard is:

- Reducing the list size of components on big projects
- Having all related components on the same folder
- Better understanding where a component is used and if whether it’s a global or a page specific component

On this structure the components folder looks like something like this

```jsx
components/
|-Common/
	|-Actions
		|-AppButton.vue
		|-AppLink.vue
	|-Structure
		|-TheSidebar.vue
		|-TheHeader.vue
	|-Forms
		|-AppLoginForm.vue
		|-AppRegisterForm.vue
|-Todo/
	|-TodoList.vue
	|-TodoListItem.vue
	|-TodoListButton.vue
|-Products/
	|-ProductsList.vue
	|-ProductsListItem.vue
	|-ProductsListDetails.vue
```

For the pages folder there are also two common standards:

### Placing all page files on a folder and naming the file index.vue

This approach is prefered to have more consistency between pages that don’t have children and pages that do. It places all files inside a folder with the pages name and a index.vue file for the parent, then subfolders for its children. It looks something like this:

```jsx
pages/
|-index.vue
|-cart/
	|-index.vue
	|-empty/
		|-index.vue
|-products/
	|-index.vue
	|-product1
		|-index.vue
	|-product2
		|-index.vue
|-about
	|-index.vue
```

### Creating folders only when necessary

This approach results in a cleaner folder structure, with less folders and subfolders. This is an example of this standard in use:

```jsx
pages/
|-index.vue
|-cart/
	|-index.vue
	|-empty.vue
|-products/
	|-index.vue
	|-product1.vue
	|-product2.vue
|-about.vue
```

#Coding standards #coding-standards

### Handling dynamic routes
For routes that have a value that is dynamic and can be basically anything, the best approach is to:
- **If there is a parent to the dynamic route:** create a folder with the parent route name, then placing a index.vue and a ${dynamicRouteName}.vue files inside it
```jsx
pages/
|-product/
  |-index.vue
  |-_id.vue
|-user/
  |-index.vue
  |-_name.vue
```
- **If there isn't a parent to the dynamic route:** follow the choosen standard for the folder structure
```jsx
//If you are using the standard of creating a folder for each page
pages/
|-_slug/
  |-index.vue

//If you are using the standard of only creating a folder when it's necessary
pages/
|-_slug.vue
```

# Components standards <Component>


### All components and page files must be in PascalCase

This is the best casing for IDE's autocompletion and maintains a consistence between components in JS(X) and templates

```jsx
//Don't
components/
|-mycomponent.vue
|-myOtherComponent.vue

//Do
|-MyComponent.vue
|-MyOtherComponent.vue
```

### Components that will be used on the whole project, base components, or components from the design system must have the specific 'App' prefix

```jsx
//Don't
components/
|-MyButton.vue
|-Icon.vue

//Do
components/
|-AppButton.vue
|-AppIcon.vue
```

### Components that are to be used only once *per page* must have the 'The' prefix

Components that are to be used only once <i>per page</i> must have the 'The' prefix

```markdown
//Don't
components/
|-Heading.vue
|-Sidebar.vue
|-Footer.vue
 
 //Do
 components/
 |-TheHeading.vue
 |-TheSidebar.vue
 |-TheFooter.vue
```

### Components that are related should include it's parent component as a prefix

This makes the relationship between components evident and makes sure related files are next to each other on the IDE's file explorer

```jsx
//Don't
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue

//Do
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue
```

### Component names should start with the most general word and end with the most descriptive one

This standard makes it easier to find related components and to identify them by its function

```jsx
//Don't
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue

//Do
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```
### Use multi-worded component names

Component names should always be multi-word to prevent conflicts with existing and future HTML elements

```jsx
//Don't
export  default {
    name: 'Todo',
    // ...
}

//Do
 export  default {
    name: 'TodoItem',
    // ...
}
```

### All components with no content should be self-closing

Components that selfeu -close communicate that they not only have no content, but they are meant to have no content

```jsx
//Don't
<MyComponent></MyComponent>

//Do
<Mycomponent/>
```

### Components must use PascalCase on the HTML

This assures that the IDE highlights then different from the default HTML tags 

```jsx
//Don't
<my-component>
	Component content
<my-component>
<my-other-component/>

//Do
<MyComponent>
	Component content
<MyComponent>
<MyOtherComponent/>
```

### Always use directive shorthands

Use : for v-bind, @ for v-on and # for v-slot, making the code less verbose

```jsx
//Don't
<input
	v-on:focus="doSomething"
	v-bind:placeholder="placeholderVariable"
/>

//Do
<input
	@focus="doSomething"
	:placeholder="placeholderVariable"
/>
```

# Other HTML related standards

### Use a unique key attribute in every v-for. Avoid using index

This helps Vue/Nuxt optimize the rendering proccess

```jsx
//Don't
<ul>
    <li v-for="(todo, index) in todos" :key="index">
    {{ todo.text }}
    </li>
</ul>
//Even worse
<ul>
    <li v-for="todo in todos">
    {{ todo.text }}
    </li>
</ul>

//Do
<ul>
    <li
     v-for="todo in todos"
     :key="todo.id"
    >
    {{ todo.text }}
    </li>
</ul>
```

### Non-empty HTML attributes should always be inside double quotes

While attribute values without any spaces are not required to have quotes in HTML, this practice often leads to *avoiding* spaces, making attribute values less readable.

```jsx
//Don't
<AppSidebar :style={width:sidebarWidth+'px'}>

//Do
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```

### Add line-breaks on multi-attribute elements

In JavaScript, splitting objects with multiple properties over multiple lines is widely considered a good convention, because it’s much easier to read. Our templates and JSX deserve the same consideration.

```jsx
//Don't
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">
<MyComponent foo="a" bar="b" baz="c"/>

//Do
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

### Attributes of elements should be ordered consistently

This is the default order for elements attributes

```jsx
<Element
	v-for="el in els"
	v-if/v-else-if/v-else/v-show
	id="elementID"
	ref="elementRef"
	:key="el.id"
	v-model="elementModel"
	@click="doSomething"/any other v-on listeners
	v-html/v-text="myVariable"
/>
```

### Use the key attribute on v-if + v-else on the same HTML elements

This helps Nuxt optimize the rendering proccess

```jsx
//Don't
<div v-if="error">
  Error: {{ error }}
</div>
<div v-else>
  {{ results }}
</div>

//Do
<div
  v-if="error"
  key="search-status"
>
  Error: {{ error }}
</div>
<div
  v-else
  key="search-results"
>
  {{ results }}
</div>
```

### All logic must be **encapsulated inside the script tag**

Avoid emitting events or changing variable values directly inside the <template> tag. Create a method do to so.

```jsx
//Don't
<template>
	<button 
		@click="showPopUp = false"
	>
		Close
	</button>
	<button 
		@click="$emit('userCancelledAction')"
	>
		Cancel
	</button>
</template>

//Do
<template>
	<button 
		@click="closePopUp"
	>
		Close
	</button>
	<button 
		@click="emitCancellation"
	>
		Cancel
	</button>
</template>
<script>
	.
	.
	.
	methods: {
		closePopUp() {
			this.showPopUp = false
		},
		emitCancellation() {
			this.$emit('userCancelledAction')
		}
	}
</script>
```

### **Component templates should only include simple expressions, with more complex expressions refactored into computed properties or methods.**

Complex expressions in your templates make them less declarative. We should strive to describe *what* should appear, not *how* we’re computing that value. Computed properties and methods also allow the code to be reused.

```jsx
//Don't
<p>
{{
  fullName.split(' ').map(function (word) {
    return word[0].toUpperCase() + word.slice(1)
  }).join(' ')
}}
</p>

//Do
<p> {{ normalizedFullName }} </p>
.
.
.
computed: {
	normalizedFullName() {
		return this.fullName.split(' ').map(function (word) {
      return word[0].toUpperCase() + word.slice(1)
    }).join(' ')
	}
}
```

# Variables standards

Besides making sure to ***always*** making sure to use meaningfull names for variables, having a default casing standard assure that by quick looking at its name, you know what it represents

> Make sure to never use generic variable names like ‘x’, ‘a’ or ‘el’. Always use names that are easy to understand and that represent what this variable stores
> 
- **Props**
    
    Props should use *KebabCase*
    
    ```jsx
    props: {
    	SomeProp: {
    		type: String,
    		default: 'Default value'
    	}
    }
    
    <MyComponent :SomeProp="someVariable"
    ```
    
- **Data variables**
    
    Variables declared on the data function should be declared using *camelCase*
    
    ```jsx
    data() {
    	return {
    		someVariable: '',
    		anotherVariable: true
    	}
    }
    
    <input v-model="someVariable"/>
    ```
    
- **Computed variables**
    
    Computed variables should be declared using *snake_case*
    
    ```jsx
    computed: {
    	my_computed() {
    		return someValue
    	}
    }
    ```
    
- **Methods**
    
    standard for declaring methods
    
    ```jsx
    //Don't
    methods: {
    	myMethod: function () {...},
    	myOtherMethod: () => {...}
    }
    
    //Do
    methods: {
    	myMethod() {...},
    	myOtherMethod() {...}
    }
    ```
    

# JS/ script tag standards <script>

### **Component/instance options should be ordered consistently**

This is the default order for component options, so they are easy to find and are consistent between projects and files

```jsx
export default {
	name: "ComponentName",
	components: {
		MyComponent: () => import('@/components/Mycomponent.vue')
	},
	filters: {
	  capitalize: function (value) {
	    if (!value) return ''
	    value = value.toString()
	    return value.charAt(0).toUpperCase() + value.slice(1)
	  }
	},
	mixins: [global],
	props: {
		MyProp: {
			type: String,
			required: true
		}
	},
	data() {
		return {
			myVariable: 0
		}
	},
	computed: {
		myComputedVariable() {
			return this.myVariable * 40
		}
	},
	watch: {
		myVariable(newVal, oldVal) {
			if(newVal > oldVal) {
				this.doSomething()
			}
		}
	},
	beforeCreate() {...},
	created() {...},
	beforeMount() {...},
	mounted() {...},
	beforeUpdate() {...},
	updated() {...},
	activated() {...},
	deactivated() {...},
	beforeDestroy() {...},
	destroyed() {...},
	methods: {
		doSomething() {...}
	},
	
}
```

### Use the fat arrow to declare one-time-use functions

Functions inside maps, filters and other JS methods, or that are only executed inside a lifecycle must be declared using ES6’s arrow function

```jsx
mounted() {
	const myFunction = () => {...}
	myFunction()
},
methods: {
	myFunction(id) {
		this.arr = this.arr.map(item => item.active = !item.active)
		this.selected = this.arr.find(item => item.id === id)
	}
}
```

### Props must have at least a type and a default/required properties defined on declaration

Defining those properties helps us catch error that might pass through without them

```jsx
//Don't
     props: ['status']

//Do
props: {
  Status: {
   type: String,
   required: true
  }
 }

//Even better
props: {
  Status: {
   type: String,
   required: true,
   validator: function (value) {
     return [
     'syncing',
     'synced',
     'version-conflict',
     'error'
     ].indexOf(value) !== -1
    }
   }
  }
```

### Components should be imported using a arrow function directly on the components object

This decreases the bundle size and optimizes imports for better performance in general

```jsx
//Don't
import MyComponent from '@/components/MyComponent'
components: {
	MyComponent
}

//Do
components: {
	MyComponent: () => import('@/components/MyComponent')
}
```

### Use props and events to communicate between parent and child

Avoid the use of this.$parent, because it makes the code harder to understand and maintain

```jsx
//Don't
methods: {
  removeTodo (id) {
    this.$parent.todos = this.$parent.todos.filter(function (todo) {
      return todo.id !== id
    })
  }
}

//Do
methods: {
  removeTodo (id) {
    this.$emit('delete', id)
  }
}
```

### **Complex computed properties should be split into as many simpler properties as possible.**

This way they are easier to test, easier to read and more adaptable to changing requirements

```jsx
//Don't
computed: {
  price: function () {
    var basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}

//Do
computed: {
  basePrice: function () {
    return this.manufactureCost / (1 - this.profitMargin)
  },
  discount: function () {
    return this.basePrice * (this.discountPercent || 0)
  },
  finalPrice: function () {
    return this.basePrice - this.discount
  }
}
```

### Don’t use var for variable declaration, use const when possible and let when not

Avoid imprevisible hoisting and the bugs that come with it

```jsx
//Don't
myFunction(id) {
	var index = this.arr.findIndex(el => el.id === id)
	var value = this.arr[index].counter
	if(value > 0) value = 0
	this.arr[index].counter= value
}

//Do
myFunction(id) {
	const index = this.arr.findIndex(el => el.id === id)
	let value = this.arr[index].counter
	if(value > 0) value = 0
	this.arr[index].counter= value
}
```

# VueX standards

The VueX comes bundled with NuxtJs and it’s the go to for state control at BoN. It’s a great tool, but should be use with discretion. Every value stored in the VueX is available in the entire project, which means that having a lot of information on its results in a very heavy and slow application. So make sure you only store on the VueX what makes sense, and is an information that is needed everywhere in the project.

There are some best practices that must be followed when using the store:

- Never access the state directly, use getters instead
    
    ```jsx
    //Don't
    computed: {
    	this.$store.state.storeName.someState.someProperty
    }
    
    //Do
    computed: {
    	this.$store.getters['storeName/$someState'].someProperty
    }
    ```
    
- Use mapGetters/mapMutations/mapActions when it makes sense (when you need a bunch of information of the store on the file)
    
    ```jsx
    computed: {
      ...mapGetters("products", [
        "$getName",
        // Here you can import other getters from the products.js
      ])
    }
    ```
    
- Never directly commit mutations, always call an action
    
    ```jsx
    //Don't
    this.$store.commit('storeName/MUTATION_NAME', value)
    
    //Do
    this.$store.dispatch('storeName/actionThatCommitsMyMutationName', value)
    ```
    
- Destructure what you need from context
    
    ```jsx
    //Don't
    someAction(context) {
    	context.commit('SOME_MUTATION')
    	context.dispatch('someOtherAction')
    }
    
    //Do
    someAction({commit, dispatch}) {
    	commit('SOME_MUTATION')
    	dispatch('someOtherAction')
    }
    ```
    

There are some standards for naming actions, mutations and getters on the VueX store, to better differenciate between them and to reduce redundancy

- Actions names use camelCase and describe what they do. If they make a http request, use the http method on the name
    
    ```jsx
    getSomething() {
    	const response = this.axios.get(apiUrl)
    },
    postSomething(data){
    	const response = this.axios.post(apiUrl, data)
    }
    ```
    
- Mutations name use snake_case with all uppercase letters and describe its functionality. They should start with a verb that specifies what they do
    
    ```jsx
    SET_DATA(state, newData) {
    	state.data = newData
    },
    CHANGE_ACTIVE_STATUS_BY_ID(state, {id, newStatus}) {
    	state.data[id].status = newStatus
    },
    FORMAT_DATA(state) {
    	state.data = state.data.map(currentData => currentData.sort((a, b) => a + b))
    }
    ```
    
- Getters use camelCase and start with a $. If the getter return a state with the same name as the store, it should omit it
    
    ```jsx
    //@requests store
    getters: {
    	$byId(state) {
    		return (id) => {
    			return state.requests.find(request => request.id === id)
    		}
    	},
    	$all(state) {
    		return state.requests
    	}
    }
    
    //@component level
    this.$store.getters['requests/$all']
    this.$store.getters['requests/$byId', requestId]
    ```
#CSS   
--> https://blog.logrocket.com/bem-vs-smacss-comparing-css-methodologies/
https://www.devbridge.com/articles/implementing-clean-css-bem-method/
https://medium.com/@GreenXIII/bem-vs-smacss-war-till-death-6e035b87d6c6
https://getbem.com/introduction/
https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/
#qas-patterns

# Git Structure / Standards

## Issues

### Title of the Issue
The title should always briefly describe what is going to be worked on that Issue. Since the [Merge Request](##Creating-a-Merge-Request) and the branch are going to inherit its name, longer names could be problematic. Optionally, a prefix describing the type of task being performed on that Issue can be beneficial.

#### The proposed formats for bug titles are:
```jsx
1. "person/type of user" can’t "perform action/get result they should be able to" (e.g. New User can’t view home screen)

2. When "performing some action/event occurs", the "system feature" doesn’t work

3. When "persona/type of user"  "performs some action", the "system feature" doesn’t work

4. "quick name". "one of the formats above" (e.g. “Broken button. New User can’t click the Next button on Step 2 of the Wizard”).
```

#### For example
```jsx
✅ Do

Title: [Fix] "Select All" should "select all CSWs" but doesn't

🚫 Don't

Title: THIS ISSUE RESOLVES ALL THE PROBLEMS FOR THE COMM PROGRAM
Title: resolving all problems on comm program
Title: select all not working
Title: Implementing all the possible ways to interpret the different columns of users related to the list of users [...]
```

The Issue problematic should always be clear and concise to what is expected to be developed.
```jsx
✅ Do

Description:
	The "Select All" feature is not working as expected, instead of selecting all CSWs it is not selecting anything at all.
	
🚫 Don't

Description:
	Fix select all bug
```

Additionally, add a few elements to better organize the topics of the description:

#### Example:
## 🎯 Objectives
- [x] Get the list of available users on the company
- [ ] Update the design on the list of users

## 🔗 API
### Get list of Avaliable Users
| API | TYPE | DESCRIPTION |
| --- | --- | --- |
| {{url}}/list-users | GET | Returns the list of the available Users |
 
## 🖼️ Reference
An Image that refers to the design of the discussed topic

## Merge Requests

>**Important:** If the 'dev' branch is not the default branch on the repository, when creating a new Merge Request with a branch, the Source should be set to 'dev' or any other branch that represents the dev environment

The Merge Request should be created via the button "Create merge request" available on the Issue.  That will create a branch with the name of the functionality and further improving the way branches are merged into the same branch (dev). 

**i.e.:** There's an issue to "Implement new design on Comm Program", the branch will automatically be: 149-implement-new-design-on-comm-program. And any work related to this implementation should be on this new branch.

When creating, you can optionally assign yourself or the person who will work on that issue. You can also assign a Reviewer.

Once the new feature has been implemented, you can "Mark as Ready". This will notify any Reviewer assigned to that Merge Request.

## Commits
Commit messages should sum what has been worked on in the commiting files. The commit message should contain the following:

#### Commit message format
```jsx
<header>
<BLANK LINE>
<body> (Optional)
<BLANK LINE>
<footer> (Optional)
```


#### Commit message header 
```jsx
<type>(<scope>): <short summary>
  │       │             │
  │       │             └─→ Summary in present tense. Not capitalized. No period at the end.
  │       │
  │       └─→ Commit Scope: animations|bazel|benchpress|common|compiler|compiler-cli|core|
  │                          elements|forms|http|language-service|localize|platform-browser|
  │                          platform-browser-dynamic|platform-server|router|service-worker|
  │                          upgrade|zone.js|packaging|changelog|docs-infra|migrations|ngcc|ve|
  │                          devtools
  │
  └─→ Commit Type: build|ci|docs|feat|fix|perf|refactor|test

```

The type of the Commit Type can be replaced by an [gitmoji](https://gitmoji.dev/) that will represent that type of commit

#### Commit message body (Optional)

Just as in the summary, use the imperative, present tense: "fix" not "fixed" nor "fixes".

Explain the motivation for the change in the commit message body. This commit message should explain  _why_  you are making the change. You can include a comparison of the previous behavior with the new behavior in order to illustrate the impact of the change.

#### Commit message footer (Optional)
> Since the Gitlab always refers to the Issue, this is very unnecessary

The footer can contain information about breaking changes and deprecations and is also the place to reference GitHub issues, Jira tickets, and other PRs that this commit closes or is related to.

#### Examples of Commit
```
📱: Fix main page responsivity
<BLANK LINE>
Fix the issue where the main page is not responsive to Tablet Views
<BLANK LINE>
<BLANK LINE>
Fixes #149
```
Or
```
🚑️: Fix "Purchase" feature on Landing Page
<BLANK LINE>
Fix the issue the user can not finish the purchase because the "Purchase" button did not have a proper function working.
<BLANK LINE>
<BLANK LINE>
Fixes #150
```

# Making the QAs job easier

The frontend developers can make the QAs job of creating automated testing a lot easier by using some patterns and good practices

- **Use the data-qa attribute**
    
    This attributes makes sure that there is a consistent and imutable way to select a HTML element when writting automated testing. So, make sure to implement this attribute on important buttons and inputs. There’s also a set of rules that we must follow when giving it a value:
    
    - For confirmation buttons use the value ‘confirm’ for the data-qa
        
        ```jsx
        <button data-qa="confirm">Confirm</button>
        ```
        
    - For submit buttons use the value ‘submit’ for the data-qa
        
        ```jsx
        <button data-qa="submit" type="submit">Submit</button>
        ```
        
    - For cancellation buttons use the value ‘cancel’ for the data-qa
        
        ```jsx
        <button data-qa="cancel">Cancel</button>
        ```
        
    - For navigation links (that change the page view/url) use its inner text, on camelCase, as a value for the data-qa
        
        ```jsx
        <a href="https://www.google.com.br" data-qa="google">Google</a>
        <nuxt-link to="/products" data-qa="productsPage">ProductsPage</nuxt-link>
        ```
        
    - For inputs, use its label (on camelCase, if there are spaces on it) as a value for the data-qa
        
        ```jsx
        <label for="name">Name</label>
        <input 
        	type="text" 
        	data-qa="name" 
        	id="name" 
        	name="name"
        />
        <label for="password">Password</label>
        <input 
        	type="text" 
        	data-qa="password" 
        	id="password" 
        	name="password"
        />
        <label for="confirmPassword">Confirm password</label>
        <input 
        	type="text" 
        	data-qa="confirmPassword" 
        	id="confirmPassword" 
        	name="confirmPassword"
        />
        ```
        
- **Use meaningfull and unique names for CSS classes**
    
    This way it’s easier to check if a certain element is on the screen, or get its css properties. If the class name it’s not specific or unique (in the page’s context, at least) enough, it makes writting automated tests a lot harder
    
     
    
    ```jsx
    //Don't
    <div class="column">...</div>
    <div class="column">...</div>
    <div class="column">...</div>
    <button class="btn">Confirm</button>
    <button class="btn">Cancel</button>
    
    //Do
    <div class="column__first">...</div>
    <div class="column__second">...</div>
    <div class="column__third">...</div>
    <button class="btn__confirm">Confirm</button>
    <button class="btn__cancel">Cancel</button>
    ```
    
# Accessibility Standards

## Accessibility checker tool:

[https://www.accessibilitychecker.org](https://www.accessibilitychecker.org/)/

## Font size accessibility:

- Set a main font size as variable, using `em` value.
- Change the main font size value clicking on the encrease or decrease font size and using `rem` on all the others font sizes.

## Contrast:

- **Eg:** Switch the colors inside variables by re-setting the variables or changing the the css file.

## ARIA:

> **ARIA “Accessible Rich Internet Applications”: Readers and other tools to present and support interaction with objects in a way that is consistent with user expectations.**
> 
- Use ARIA roles. Eg: `role="button"` in a button tag.
- A `description` or “`name`” should be added to any `input fields` such as a `check box`, `radio buttons`, `form fields`, etc. This way, a user can recognize it and can identify its purpose.
- Add `required="true"` attributes to all mandatory fields.
- Provide proper `roles` for parent elements.
- Make sure that each focusable element on the page with an active `ID` has a unique value.
- Add single and unique ARIA `labels` to each form field.
- Add `alt` text to all meaningful images/graphics.
- Add a `<caption>` inside the `<table>` tag with a description of the information contained in the table. The caption helps screen reader users identify the content of the data table. Data table rows and columns must also be defined. The relationship between `row/column` headers and table data must be highlighted.
    - Use a `<table>` tag with its corresponding `<caption>` to provide a description about the information contained in the table.
    - Provide an attribute `scope="col"` to each of the `<th>` tags which indicates column headers if the column header is not the first one.
    - Provide an attribute `scope="row"` to each of the `<th>` tags which indicates row headers if the row header is not the first one.
    - Best practice: use the `<thead>` and `<tbody>` tags to separate table headers and table content.
    
    When data tables are coded according to required standards, screen readers are able to navigate to each cell in all directions and read the cell content together with the associated table headers.
    
    **Best practice:** Use CSS instead of layout tables.
    **Best practice:** While scope is not always required, it is recommended.
    
- Provide a valid value for `lang` attribute for respective languages on all web pages.
- Captions make video elements usable for deaf or hearing-impaired users, providing critical information such as who is talking, what they're saying, and other non-speech information. So add at least one track element to the video element with the `kind='captions'` attribute, and at least one track element to the video element with attribute `kind='descriptions`'.
- Ensure all frame and iframe elements have valid `title` attribute values.
- Change the name of a landmark if it is used more than once to be sure each is unique. Use both HTML5 and ARIA landmarks to ensure all content is contained within a navigational region. In HTML5, you should use elements like `<header>`, `<nav>`, `<main>`, and `<footer>`. Their ARIA counterparts are `role="banner"`, `role="navigation"`, `role="main"`, and `role="contentinfo"`, in that order.
- Maximum-scale on `<meta>` tag to disable zooming on mobile devices.
- Construct a modal window which should be accessible. Focus should be trapped inside modal when it is open.
    
    **For details refer:** [https://www.w3.org/TR/wai-ARIA-practices/examples/dialog-modal/dialog.html](https://www.w3.org/TR/wai-ARIA-practices/examples/dialog-modal/dialog.html)
    
- Add Skip to main Content link (which will redirect main content page) on every web page which will redirect focus to main content.
- Use ARIA-expanded state to mark expandable and collapsible regions.
    
    **Visit this link for the code:** [https://www.w3.org/WAI/GL/wiki/Using_the_WAI-ARIA_ARIA-expanded_state_to_mark_expandable_and_collapsible_regions](https://www.w3.org/WAI/GL/wiki/Using_the_WAI-ARIA_ARIA-expanded_state_to_mark_expandable_and_collapsible_regions)
    
- Tag the elements properly.
    
    **Visit this link for code:** [https://www.w3.org/WAI/tutorials/page-structure/content/](https://www.w3.org/WAI/tutorials/page-structure/content/)
    
- Use # and `id` attribute to shift focus of same page links to specified location.
- Alert messages should be announced automatically by the screen reader. Session should not terminate automatically until user chooses to close it. ****
    
    **Visit this link for reference:** [https://www.digitala11y.com/understanding-sc-2-2-1-timing-adjustable/](https://www.digitala11y.com/understanding-sc-2-2-1-timing-adjustable/)
    
- Construct an accessible Play/Pause functionality.
    
    **For more detail refer:** [https://www.w3.org/WAI/WCAG21/Understanding/pause-stop-hide.html#examples](https://www.w3.org/WAI/WCAG21/Understanding/pause-stop-hide.html#examples)
    
- For decorative images, add attribute `alt=" "` or null to avoid unnecessary tab focus.
- Use ARIA `selected` attribute for the currently active link.
    
    **Visit this link for the code:** [https://www.w3.org/TR/wai-aria/#aria-selected](https://www.w3.org/TR/wai-aria/#aria-selected)
    
- Tag the titles in heading(`h1, h2, h3, h4, h5, h6`) tags, and text in `p` tags.
- Tag the quotes in quotation tag.
    
    **Visit this link for the code:** [https://www.w3.org/TR/wai-aria/#aria-selected](https://www.w3.org/TR/wai-aria/#aria-selected)
    
- Provide tooltip for icons
    - Create a `<div>` element, give it a `role="tooltip"` and an `id="IDREF"`. Then, give it a brief description of the action.
    - This element has to be accessible by focusing as well as hovering over. Both functionalities can be provided using CSS.
    - The tooltip has to be visually hidden using CSS.
    - Associate the tooltip with its button using ARIA-labelledby.
    - After applying this keyboard-accessible tooltip, consider removing the title attribute from the element.
        
        **Refer to:** [https://inclusive-components.design/tooltips-toggletips/](https://inclusive-components.design/tooltips-toggletips/) and [https://a11yproject.com/posts/how-to-hide-content/](https://a11yproject.com/posts/how-to-hide-content/)
        
- Make the design responsive and Using `CSS media query` to adjust the size of page.
- Avoid horizontal scroll bar from the web page.
- Add an appropriate color border when elements get focus.
    
    **Visit this link for the code:** [https://www.deque.com/blog/accessible-focus-indicators/](https://www.deque.com/blog/accessible-focus-indicators/)
    
- Provide appropriate `tab indexing` so that focus does not move inside modal window.
- Visible `label` should match with link label.
- Construct an accessible video with transcript functionality.
    
    **For more detail refer:** [https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-audio-desc.html#media-equiv-audio-desc-examples-head](https://www.w3.org/TR/UNDERSTANDING-WCAG20/media-equiv-audio-desc.html#media-equiv-audio-desc-examples-head)
    
- Using kind="captions" attribute provides synchronized captions with respect to each video.
    
    **For more detail refer:** [https://dequeuniversity.com/rules/axe/3.4/video-caption?application=AxeChrome](https://dequeuniversity.com/rules/axe/3.4/video-caption?application=AxeChrome)
    
- Use "ARIA-label" attribute to create an invisible text label for screen readers to read.
- Information about that '*' symbol should be present in textual form. For eg. * means required field
- Use ARIA `hidden = true` for unnecessery read elements like symbols(`'>'`) inside links or buttons.
- User interactions must be announced by screen reader. Provide aria-live=""assertive"" or role=""alert"" and aria-atomic="true" for the changes that are made after hitting an element or a message popup. Eg: Error messages, Success message, Add to cart, Modals, Loading, etc…
    
    **Visit this link for the code:** [https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/ARIA19](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/ARIA19)
    
- Current link active.
    
    **Visit this link for the code:** [https://www.digitala11y.com/ARIA-current-state/](https://www.digitala11y.com/ARIA-current-state/)
    
- Breadcrumbs, Provides a label that describes the type of navigation provided in the nav element.
    - ARIA-label="Breadcrumb".
    - ARIA-label="Breadcrumb".
    - Applied to the last link in the set to indicate that it represents the current page.
    - ARIA-current="page”
- Input error Using javascript focus() method, keyboard/screen reader focus should automatically move to invalid edit field.
- Provide the navigation links in `footer` section.
- Tag the leaderboard content in table. [https://www.w3.org/TR/wai-ARIA-practices/examples/table/table.html](https://www.w3.org/TR/wai-ARIA-practices/examples/table/table.html)
- Remove the blank div/ span from the code.
- Disabled areas Add ARIA-disabled="true" to the <input /element>. This can also be achieved by using HTML5 "disabled" attribute. [https://www.w3.org/TR/wai-ARIA-1.1/#ARIA-disabled](https://www.w3.org/TR/wai-ARIA-1.1/#ARIA-disabled)
    
# References

- [https://www.cataline.io/](https://www.cataline.io/)
- [https://www.w3.org/wiki/JavaScript_best_practices](https://www.w3.org/wiki/JavaScript_best_practices)
- [https://blog.bitsrc.io/4-best-practices-for-large-scale-vue-js-projects-9a533450bdb2](https://blog.bitsrc.io/4-best-practices-for-large-scale-vue-js-projects-9a533450bdb2)
- [https://v2.vuejs.org/v2/style-guide/](https://v2.vuejs.org/v2/style-guide/?redirect=true#Prop-name-casing-strongly-recommended)
- [https://vueschool.io/articles/vuejs-tutorials/how-to-structure-a-large-scale-vue-js-application/](https://vueschool.io/articles/vuejs-tutorials/how-to-structure-a-large-scale-vue-js-application/)
- [https://www.telerik.com/blogs/10-good-practices-building-maintaining-large-vuejs-projects](https://www.telerik.com/blogs/10-good-practices-building-maintaining-large-vuejs-projects)