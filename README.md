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
3. [Coding](#coding)
4. [Formatting](#formatting)
5. [Comments](#comments)
7. [CSS](#css)
8. [GIT](#git)
9. [Accessibility](#accessibility)


#nuxtjs-structure
# NuxtJS Structure/ Coding standards

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

# Components standards <Component>

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

### All components with no content should be self-closing

Components that self-close communicate that they not only have no content, but they are meant to have no content

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
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">\
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
    

# References

- [https://www.cataline.io/](https://www.cataline.io/)
- [https://www.w3.org/wiki/JavaScript_best_practices](https://www.w3.org/wiki/JavaScript_best_practices)
- [https://blog.bitsrc.io/4-best-practices-for-large-scale-vue-js-projects-9a533450bdb2](https://blog.bitsrc.io/4-best-practices-for-large-scale-vue-js-projects-9a533450bdb2)
- [https://v2.vuejs.org/v2/style-guide/](https://v2.vuejs.org/v2/style-guide/?redirect=true#Prop-name-casing-strongly-recommended)
- [https://vueschool.io/articles/vuejs-tutorials/how-to-structure-a-large-scale-vue-js-application/](https://vueschool.io/articles/vuejs-tutorials/how-to-structure-a-large-scale-vue-js-application/)
- [https://www.telerik.com/blogs/10-good-practices-building-maintaining-large-vuejs-projects](https://www.telerik.com/blogs/10-good-practices-building-maintaining-large-vuejs-projects)