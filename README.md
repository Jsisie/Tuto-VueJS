# Vue JS tuto 

This is a personal tutorial to learn VueJS for beginners. I do not claim to be a pro at VueJS at all, I'm only learning and feeding this file with what I think is good. :) 
I hope this can help people learning VueJS with absolutely no skills in frontend dev (like me). 
Here are some links to some better tutos :

- https://vuejs.org/tutorial/
- https://v2.fr.vuejs.org/v2/guide/



## Intro

Vue Js is a framework for building SPA (Single Page Application). It uses MVVM (Model-View-ViewModel) architecture. It is one of the most used and easiest framework to learn in order to create SPA, in web development.

## Summary

- I - [Installation](#installation)
- II - [Data](#data) 
- III [Directives](#directives)
- IV - [Vue CLI](#cli)
- V - [Component](#component)
- VI - [Router Vue](#routerVue)
- VII - [Life Cycle](#cycleLifeVue)
- ? [Axios](#axios)

## <a name="installation">I - Installation</a>

Let's start with this simple page.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <title>My Vue App</title>
</head>
<body>
    <div id="app">
        <p>Hello!</p>
    </div>
</body>
</html>
```

In order to add the framework Vue to this application, just add the line below.

```vue
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

Now that we added the framework, we need to define where Vue can be used. For this, we need to create a new instance of Vue, that we'll save in a constant regularly named "app", as the first **\<div>** tag of the page. This instantiation needs to be done in a **\<script>** tag. 

```js
const app = new Vue()
```

At the creation of this instance, we'll define the <b>el</b> option, that Vue uses in order to mount the framework on this existing html element. We'll by convention use the id "app" from the first div tag, but it could be used on any tag at the exception of the tags **\<html>** and **\<body>**.

```js
const app = new Vue({
      el: '#app'
    })
```

We can also send some data with the option **data:{ }**, and display them with in the html content with the mustache tag **{{ }}**.

Your code should now look like that :

```vue
<!DOCTYPE html>
<html lang="fr">
<head>
    <title>My Vue App</title>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: 'Hi!',
            }
        })
    </script>
</body>
</html>
```



## <a name="data">II - Data</a>

### Data Option and Mustache syntax

As we've just seen, the option <b>data</b> can be used to send data and save data in variables. This option can be used with every type of data, such as :

```js
const app = new Vue({
      el: '#app',
      data: {
          message: 'Hi!', // String
          age: 69, // Number
          groceries: ['Apple','Pear','Banana'], // Array
          isDead: false, // Boolean
          job: { // Object
              name: 'engineer',
              salary: 4500
          }
      }
    })
```

To display the value of the variables, as we've seen earlier, we can just use the mustache syntax, with 2 opening bracket and 2 closing.

```html
<div id="#app">
  <h2>{{ message }}</h2>
  <ul>
    <li>Age: {{ age }} years</li>
    <li> {{  }}</li>
    <li> {{  }}</li>
  </ul>
</div>
```

We can also process operations with the mustache syntax.

```js
<!-- Compute the result -->
{{ (2 + 8) * 10 }}

<!-- Call methods from String -->
{{ "uppcase".toUpperCase() }}

<!-- Ternary operateur -->
{{ 2 > 0 ? 'two is bigger than zero' : 'two is lower than zero' }}
```

 ### Reactive data

One of the main goal of Vue is to store the data and change them automatically when it's needed. This simplify the utilisation of the app and doesn't need a manual modification directly in the code. An easy example of this store using what we've seen could be the below code.

```js
const app = new Vue({
  el: '#app',
  data: {
    costOfApples: 6,
    costOfBananas: 2,
    costOfCoconuts: 8
  }
})
```

```vue
<div id="app">
  <h1>Basket</h1>
  <ul>
    <li>Apple: {{ costOfApples }}€</li>
    <li>Banana: {{ costOfBananas }}€</li>
    <li>Coconut: {{ costOfCoconuts }}€</li>
  </ul>
  <p>Total: {{ costOfApples + costOfBananas + costOfCoconuts }}€</li>
</div>
```

### Computed Properties

The previous solution is convenient but can quickly become unusable with too many data. We then use the **computed properties**. They enable you to create a property that can be used to modify, manipulate, and display data within your components in a readable and efficient manner. You can use computed properties to calculate and display values based on a value or set of values in the data model.

The goal is to use a property **totalAmount** instead of computing the sum of **costOfApples** etc...

```js
const app = new Vue({
  el: '#app',
  data: {
    costOfApples: 6,
    costOfBananas: 2,
    costOfCoconuts: 8
  },
  computed: {
    totalAmount() {
      return this.costOfApples + this.costOfBananas + this.costOfCoconuts
    }
  }
})
```

```html
  <p>Total: {{ totalAmount }}€</li>
```

To test that the value are now reactive, you can type in the developer console `app.costOfApples = 4` and see the price and the basket changing dynamically.

In the method created in the **computed** option, we can use js however we want. We can also display text and value with the syntax **${}**.

```js
computed: {
    copyright() {
        const currentYear = new Date().getFullYear()
        return `Copyright ${this.restaurantName} ${currentYear}`
    }
}
```

### Arrays

To simplify the basket of our application, we can store the value in an array, and display the array easier with the directive **v-for** that we'll seen in the next chapter.

```js
const app = new Vue({
        el: '#app',
        data: {
            message: 'Hi!',
            shoppingCart: [
                { label: 'Apple', cost: 6, url: '/apple.html' },
                { label: 'Banana', cost: 2, url: '/banana.html' },
            	{ label: 'Coconut', cost: 8, url: '/coconut.html' }
            ]
        }
    })
```

We now have all the value of the basket store in the array **shoppingCart**. If you refresh the page now, nothing should be display as the variables **costOfApples**... doesn't exist anymore, but we'll see right now how to display them easier.

## <a name="directives">III - Directives</a>

A directive is a solution from Vue to deal with basics problematics. They're easy to use and very efficient. They all start with the prefix "**v-**".Here's a list of some of the most used directives in Vue applications :

- **v-if** / **v-else-if** / **v-else**
- **v-show**
- **v-for**
- **v-bind**
- **v-on**
- **v-model**

### Loop ( v-for )

The directive **v-for** can loop over a condition like a regular loop. Here we're using this directive to display the element of the shop dynamically, not one by one, with the array from the last chapter.

```html
<ul>
    <li v-for="item in shoppingCart">
        {{ item.label }} : {{ item.cost }}€
	</li>
</ul>
```

So that the total amount can still work, we need to modify the function **totalAmount()**. To do this, we can just loop on the array and return the sum.

```js
totalAmount() {
    let total = 0
    this.shoppingCart.forEach(element => {
        total+=element.cost
    })
    return total
}
```

*More information about it here : https://vuejs.org/guide/essentials/list.html*



### Condition ( v-if )

The directive **v-if** add a condition over a html element, to hide him for example. Here, we'll display the element of the shop only if their price is over 0.

```html
<ul>
    <li v-if="costOfApples > 0">Apple: {{ costOfApples }}€</li>
    <li v-if="costOfBananas > 0">Banana: {{ costOfBananas }}€</li>
	<li v-if="costOfCoconuts > 0">Coconut: {{ costOfCoconuts }}€</li>
</ul>
```

An other example of the directives conditions could be to display some information considering the status of a user.

```html
<div id="app">
    <!-- If user has the basic permission -->
    <section v-if="userPermission === 'default'">...</section>
    <!-- If not, if the user has the admin permission -->
    <section v-else-if="userPermission === 'admin'">...</section>
    <!-- Else, if the user doesn't have any permission -->
    <section v-else>...</section>
</div>
```

*More information about it here : https://vuejs.org/guide/essentials/conditional.html*



### Visible ( v-show )

Although it can seems similar to the first example of **v-if**, there's actually a major difference between this two directives. **v-if** totally remove the html element from the DOM, whereas **v-show** only change the **visibility** parameter of the element.

The code below add a button to the page. When the button is clicked, the text "hide" disappear and reappear. The code also needs the data to create a value named **showText**.

```html
<div id="app">
    <button @click="showModal = !showModal">Display Modal</button>
    <div v-show="showModal" class="modal">hide</div>
</div>
```

```vue
data: {
	showModal: false
}
```



### Link ( v-bind )

This directive can define the attribute of a html element, the **style** of a **div** tag for example, or the links of a tag **a**, like the example below. This directives is mostly used when fetching data from an API, and sending back data depending on what you got from the fetch.

```html
<a v-bind:href="item.url">{{ item.label }}</a> : {{ item.cost }}€
```



### Events ( v-on )

Probably the most used directive, it allows to listen to events on elements. An easy example is to display a text when clicking on a button. The below example call the method **alertHello** when the button is clicked. To create a method, we need to use the option **methods** from the instantiation of Vue. (Note that **@click** is an abbreviation of  **v-on:click**)

```html
 <button v-on:click="alertHello">Cliquez ici !</button>
```

```js
methods: {
    alertHello() {
    	alert('Hello')
    }
}
```

Let's see a more complex example.

```js
const app = new Vue({
    el: '#app',
    data: {
        favoriteColor: 'blue'
    },
    computed: {
        label() {
            return ' : ' + this.favoriteColor
        }
    },
    methods: {
        alertColor(colorLabel) {
            alert('My favorite color is' + colorLabel)
        },
        changeColor() {
            // Change data property 
            this.favoriteColor = 'turquoise'
            // call method alertColor() with label as parameter
            this.alertColor(this.label)
        }
    }
})
```

*More information about it here : https://vuejs.org/guide/essentials/event-handling.html*



### Data in form ( v-model )

If we want to connect a variable from the **data** option with html elements, we could use **v-on** and **v-bind**, but Vue furnish an easier way to do this, and it's the directive **v-bind**. Here's an example of a simple formular, asking for a password and a username, and printing them when they're fulfilled. 

```html
<div id="app">
    <label for="username">Nom d'utilisateur</label>
    <input id="username" type="text" v-model="username" />
    <label for="pw">Mot de passe</label>
    <input id="pw" type="password" v-model="password" />
    <p>username: {{ username }}</p>
    <p>password: {{ password }}</p>
</div>
```

```html
<script>
    const app = new Vue({
        el: '#app',
        data: {
            username: '',
            password: ''
        }
    })
</script>
```

**v-model** also provides options to simplify the uses of the directives. Here's a few of them :

- **.number** = To use number in input for example
- **.lazy**
- **.trim**

#### *More information about it here : https://vuejs.org/guide/essentials/forms.html*

Here's the full code with a few directives used :

```html
<!DOCTYPE html>
<html lang="fr">
<head>
    <title>My Vue App</title>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
        <h1>Basket</h1>
        <ul>
            <li v-if="item.cost > 0" v-for="item in shoppingCart">
                <a v-bind:href="item.url">{{ item.label }}</a> : {{ item.cost }}€
            </li>
        </ul>
        <p>Total: {{ totalAmount }}€</li>
        <br><br>
        <button @click="showText = !showText">Display Text</button>
        <div v-show="showText" class="modal">hide</div>
        <br>
        <button v-on:click="alertHello">Cliquez ici !</button>
        <br><br>
        <label for="username">Nom d'utilisateur</label>
        <input id="username" type="text" v-model="username" />
        <label for="pw">Mot de passe</label>
        <input id="pw" type="password" v-model="password" />
        <p>username: {{ username }}</p>
        <p>password: {{ password }}</p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        const app = new Vue({
            el: '#app',
            data: {
                message: 'Hi!',
                shoppingCart: [
                    { label: 'Apple', cost: 6, url: '/apple.html' },
                    { label: 'Banana', cost: 2, url: '/banana.html' },
                    { label: 'Coconut', cost: 8, url: '/coconut.html' }
                ],
                showText: false,
                favoriteColor: 'yellow',
                username: '',
                password: ''
            },
            computed: {
                totalAmount() {
                    let total = 0
                    this.shoppingCart.forEach(element => {
                        total+=element.cost
                    })
                    return total
                },
                colorLabel() {
                    return ' : ' + this.favoriteColor
                }
            },
            methods: {
                alertHello() {
                    alert('Hello')
                },
                alertColor(color) {
                    alert('My favorite color is' + color)
                },
                changeColor() {
                    console.log('I want to change my favorite color')
                }
            }
        })
    </script>
</body>
</html>
```

And another example of a small website for a bakery :

```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<meta http-equiv="X-UA-Compatible" content="ie=edge" />
		<link rel="stylesheet" href="./styles.css" />
		<title>Cafe with a Vue</title>
	</head>
	<body>
		<div id="app" class="app">
			<h1>{{ restaurantName }}</h1>
			<p class="description">
				Bienvenue dans notre café {{ restaurantName }}! Nous sommes réputés pour
				notre pain et nos merveilleuses pâtisseries. Faites vous plaisir dès le
				matin ou avec un goûter réconfortant. Mais attention, vous verrez qu'il
				est difficile de s'arrêter.
			</p>

			<section class="menu">
				<h2>Menu</h2>
				<div v-for="item in simpleMenu" class="menu-item">
					<img v-bind:src="item.image.source"
						v-bind:alt="item.image.alt"
						class="menu-item__image"
					/>
					<div>
						<h3>{{ item.name }}</h3>
						<p v-if="item.inStock">En Stock</p>
						<p v-else>En Rupture de Stock</p>		
						<p v-if="item.inStock">Quantité: {{ item.quantity }}</p>		
						<div>
							<label for="add-item-quantity">Quantité :</label>
							<input id="add-item-quantity" type="number" v-model.number="item.quantity"/>
							<button v-on:click="addToBasket(item.quantity)">Ajouter au panier</button>
						</div>
					</div>
				</div>
			</section>
            
			<aside class="shopping-cart">
				<h2>Panier d'achat: {{ totalQuantity }} items</h2>
			</aside>

			<h2>Contactez nous</h2>
			<p>Adresse : {{ address }}</p>
			<p>Téléphone : {{ phone }}</p>
			<p>Email : {{ email }}</p>
			<p>Horaires :</p>
			<ul>
				<li>L-V: 06:00 à 16:00</li>
				<li>Samedi: 07:00 à 14:00</li>
				<li>Dimanche: 07:00 à 12:00</li>
			</ul>

			<footer class="footer">
				<p>Copyright {{ restaurantName }} 2020</p>
			</footer>
		</div>

		<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
		<script>
			const app = new Vue({
				el: "#app",
				data: {
					address: "18 avenue du Beurre, Paris, France",
					email: "hello@cafewithavue.bakery",
					phone: "01 88 88 88 88",
					restaurantName: "La belle vue",
					simpleMenu: [
						{
							name: "Croissant",
							image: {
								source: "./images/croissant.jpg",
								alt: "Un croissant"
							},
							inStock: true,
							quantity: 1
						},
						{
							name: "Baguette de pain",
							image: {
								source: "./images/french-baguette.jpeg",
								alt: "Quatre baguettes de pain"
							},
							inStock: true,
							quantity: 1
						},
						{
							name: "Éclair",
							image: {
								source: "./images/eclair.jpg",
								alt: "Éclair au chocolat"
							},
							inStock: true,
							quantity: 1
						}
					],
					quantityChoose: 0,
					totalQuantity: 0
				},
				computed: {
					copyright() {
						const currentYear = new Date().getFullYear()
						return `Copyright ${this.restaurantName} ${currentYear}`
					}
				},
				methods: {
					addToBasket(amount) {
						this.totalQuantity += amount
					}
				}
			})
		</script>
	</body>
</html>
```



## <a name="cli">IV - Vue CLI</a>

Nowadays, for most projects using framework like Vue, a **CLI** (**C**ommand **L**ine **I**nterface) is necessary to build and mount the application. The CLI use building tools like *gulp*, *webpack*,... CLI allows the developer to spend more time on the creation of the application rather than its configuration. 

### Installation

To install vue cli into your project, you can just use the **npm** command in your project repository.

```shell
npm install -g @vue/cli
```

You should now be able to use vue commands, like below for example :

```shell
vue --version
```

### Create New Project

To create a new Vue project, we first use the command **create** from vue directly.

```shell
vue create my-first-vue-cli-app
```

*(note: if you're using Windows, you'll need to use the "cmd" shell from Windows, not "git bash")*

You will now need to choose preset for your project. You can choose whatever you want considering your project and needs, but here's what we preconize to start with :

![image-20220906122938612](C:\Users\leoba\AppData\Roaming\Typora\typora-user-images\image-20220906122938612.png)

To move, use the **keyboard arrows**. To check a box, press **Space**. To validate the from, press **Enter**.

For CSS Pre-processor, we'll use **Sass/SCSS (with dart-sass)**. For the Linter/Formater, we'll use**ESLint with error prevention only**. For the lint configuration, we'll choose **Lint on save**, and finally for our configuration save, we'll choose **In dedicated config files**.

Once all the downloading finished, we can now launch our Vue app for the first time.

```shell
cd my-first-vue-clir-app
npm run serve
```

By default, the app should be launched in localhost on the port 8080, unless an app is already launched on this one.  You can now type the url in a navigator, if the app hasn't been launched on its own, and see your app.

An other way to create Vue project without command line is to use the command **``vue ui``** in the console.



### Architecture

We'll now take a look at some of the files and folders created for the app.

- **node_modules** : contains all the dependencies. Handled by **npm**.

- **public** : initially contains *favicon.ino* file and *index.html*. *favicon.ino* file is the logo that appears on the tab, and *index.html* is the main file of the app that is automatically launched. The file contains a \<div> "app" where all the others files are going to be injected.

- **src** : primary repository, contains all the code files.

- **.gitignore** : contains a list of files/folders which are not attached to a repo. Like the folder */dist*, automatically generated at the build of the application, or the folder *node_modules*, generated by **npm** each time the command **``npm install``** is typed.

- **package.json** : main configuration file of the project. Contains metadata like name and version of the app, or scripts that can be used, like **serve** or **build**. **serve** launch the app on a local environment, and **build** create the final version of the app, that can be send to the client or user.

- **src/Assets** : contains all the resources like images, videos, pdfs,...

- **src/Components** : contains all the components of the project. We'll see later what a component is and how to use it.

- **main.js** : here are define all "high-level" configuration options from Vue. We'll learn how to use it later. 

- **App.vue** :  first file of the project, linked to the **main.js**, it represent the first page of the app.



*From now on, all the next chapters will use Vue CLI and will follow what we just saw.*

## <a name="component">V - Component</a>

Let's remember that Vue is a framework that uses **SPA** (**S**ingle **P**age **A**pplication), we also call it *mono-file application*. A **.vue** file is also called a component, and can be used anywhere in the code. This is one of the most interesting feature of Vue Js. To use a component, we just have to import it in the script part, and use him or display it in the template part. Components are the basic of Vue Js. They allow to encapsulate a set of html elements, in a reusable and easily maintainable way. The first component at the creation of a new CLI project is the file **App.vue**.  A **.vue** file is composed in 3 parts, represented by tags :

- #### **\<template>** : Html code

- #### **\<script>** : JavaScript cpde

- #### **\<style>** : Css code

We'll now create for the example a new file named **HomeLink.vue** in the repository **src/Components**.

```html
<template>
	<a href="/">Home</a> 
</template>

<script>
    export default {
        name: 'HomeLink'
    }
</script>

<style>
	a {
		text-decoration: none;
	}
</style>
```

We'll now have to call this component in the main file **App.vue**, by adding the lines below in the script part :

```js
import HomeLink from './components/HomeLink.vue'
export default {
	name: 'App',
	components: {
		HomeLink
	}
}
```

And these lines in the template part :

```html
<div id="app">
    <nav>
        <HomeLink />
        <a href="/about">About</a>
        <a href="/contact">Contacts</a>
    </nav>
    <p>Welcome to the page: <HomeLink /></p>
</div>
```

*(For this example, we removed the importation of the component **HelloWorld.vue** and all the default code in the template tags*.)

So, what have we done here ? We imported **HomeLink** using **ES6**. We saved the component with the property **component** in *export default*, **ES6** makes it easier to write but JavaScript will transform the code '*HomeLink*' in  '*"HomeLink":HomeLink*', during the transpilation step (transform all the Vue code in classic JS code, so that it can be interpreted by most navigator).

### Props

The above example can be useful, but has its limits as we're writing the links directly in the code of the component **HomeLink**. A better way to do this would be to configure the component so that we can use them as we want. That is why the **props** are here for.

**Props** is an attribute, similar to a promise value, that will be send directly to the template. It is define in the code of the component, so **HomeLink** here.

```html
<template>
	<a :href="url">{{ text }}</a> <!--  -->
</template>
<script>
    export default {
        name: 'NavLink', <!-- (value by default (file name), don't overwrite it) -->
        props: ['url', 'text'] <!-- PROPS DEFINED HERE -->
    }
</script>
```

Here, we defined 2 props.

- **url** : will contain the url of the component for the attribute **href** of the element **a**

- **text** : will contain the text to display, redirecting to the url

We can also rename the file **HomeLink.vue** in **NavLink.vue**, as we'll use the component for all the links in the page (Home, About, Contacts).

Here is how we'll use the component in the file **App.vue** :

```html
<template>
    <div id="app">
      <nav>
        <NavLink url="/" text="Home"/> <!-- USED HERE, VALUE ARE PASSED TO PROPS -->
        <NavLink url="/about" text="About"/>
        <NavLink url="/contacts" text="Contacts"/>
      </nav>
    </div>
</template>

<script>
  import NavLink from './components/NavLink.vue' <!-- IMPORTED HERE -->

  export default {
    name: 'App',
    components: {
    	NavLink, <!-- MUST BE EXPORTED -->
	}
  }
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
```

We now have only one file to modify if we want to change information about a link, instead of changing every component.

#### More complex example of props :

***MenuItem.vue*** (component)

```vue
<script>
export default {
	name: "MenuItem",
	props: ["addToShoppingCart", "image", "inStock", "name", "price", "quantity"]
}
</script>

<template>
	<div class="menu-item">
		<img class="menu-item__image" :src="image.source" :alt="image.alt" />
		<div>
			<h3>{{ name }}</h3>
			<p>Prix : {{ price }}</p>
			<p v-if="inStock">En stock</p>
			<p v-else>En rupture de stock</p>
			<div>
				<label for="add-item-quantity">Quantité : {{ quantity }}</label>
				<input v-model.number="quantity" id="add-item-quantity" type="number" />
				<button @click="addToShoppingCart(quantity)">
					Ajouter au panier
				</button>
			</div>
		</div>
	</div>
</template>
```

***HomeView.vue*** (View using the component with props)

```vue
<template>
	<div id="app" class="app">
	<h1>{{ restaurantName }}</h1>
        
	<section class="menu">
		<h2>Menu</h2>
		<MenuItem
			v-for="item in simpleMenu"
			:addToShoppingCart="addToShoppingCart"
			:name="item.name"
			:image="item.image"
			:price="item.price"
			:quantity="item.quantity"
			:inStock="item.inStock"
			:key="item.name"
		/>
    </section>
</template>

<script>
import MenuItem from "../components/MenuItem"

export default {
	name: "Home",
	components: {
		MenuItem
	},
	data() {
		return {
			restaurantName: "La belle vue",
			shoppingCart: 0,
			simpleMenu: [
				{
					name: "Croissant",
					image: {
						source: "/images/crossiant.jpg",
						alt: "Un croissant"
					},
					inStock: true,
					quantity: 1,
					price: 2.99
				},
				{
					name: "Baguette de pain",
					image: {
						source: "/images/french-baguette.jpeg",
						alt: "Quatre baguettes de pain"
					},
					inStock: true,
					quantity: 1,
					price: 3.99
				},
				{
					name: "Éclair",
					image: {
						source: "/images/eclair.jpg",
						alt: "Éclair au chocolat"
					},
					inStock: false,
					quantity: 1,
					price: 4.99
				}
			]
		}
	}
</script>
```



## <a name="routerVue">VI - Router  Vue</a>

### Installation

``vue add router``  

### Architecture

- **main.js** : Create the App from App.vuen, automatically add the **use(router)** to specify utilisation of Vue-router, then mount the app. 

- **App.vue** : Two new important tags for routing :

  - *router-link* : Corresponds to link of the view we want to build. 

    ```vue
    <router-link to="/about">About</router-link> |
    ```

  - *router-view* : Placed just below *router-link*, this is where the content of routes will be displayed.

    ```vue
    <router-view/>
    ```

- **index.js** : Present in the folder *router*. Contains an array of routes, representing all routes (pages) of the app. They're linked directly to URL.

  ```javascript
  const routes = [
    {
      path: '/',
      name: 'home',
      component: HomeView
    },
    {
      path: '/contact',
      name: 'contact',
      component: ContactView
    }
  ]
  ```

  Also creates the router, where you can specify the mode (history, ...), other options and give the array of routes.

  ```javascript
  const router = new VueRouter({
    mode: 'history',
    routes
  })
  ```

- **views** : Folder that contains all view of the project. A view is a page of the app.

- **components** : Folder that contains all components of the project. A component is like a view in smaller, and that can be called many times on different parts, like authentificaiton popup etc...

### Example d'utilisation

```vue

```



## <a name="cycleLifeVue">VII  - Life Cycle</a>

### Definition

*hook life cycle*

LifeCycle of a Vue components, 4 periods :

- create
- mount
- update
- destroy

Main hooks :

- `beforeCreate`
- `created`
- `beforeMount`
- `mounted`
- `beforeUpdate`
- `updated`
- `beforeDestroy`
- `destroyed` 

### Utilisation

Define it in the components/views tags :

```vue
<template>
    <h1>Je suis un nouveau composant !</h1>
</template>

<script>
    export default {
        data: { msg: 'Bonjour !' },
        beforeCreate() { console.log('Je ne suis pas encore  créé') },
        created() { console.log('Je suis créé !') },
        beforeMount() { console.log('Je vais bientôt être monté sur le DOM!') },
        mounted() { console.log('Je suis monté sur le DOM!') },
        beforeDestroy() { console.log('Je suis sur le point de disparaître du DOM !') },
        destroyed() { console.log('Je suis supprimé') }
    }
</script>
```







## <a name="axios">? - Axios </a>

*https://github.com/axios/axios*

Axios is a framework who provides an easier way to fetch data from an API. There's two ways to import Axios to your Vue project.

## Installation

### NPM

The first and easier way to use Axios is to install it directly into your project with the command below :

```shell
npm install axios
```

### CDN

An other way to use Axios is to add in your Vue file the CDN reference to it, like this :

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

Now that Axios is imported by either way, you'll need to create the object axios from the library.

```js
import axios from 'axios';
```

## Request GET

For this example, we'll use the online API "restCountries", that returns data on all countries in the word. First, we'll create a data named **countries** to stock the fetched data.

```js
data: {
    countries: null
}
```

Then, we use axios to fetch the data to the indicate url, stock them in the variable we just created and display it in the console.

```js
mounted() {
    axios
   	.get('https://restcountries.com/v3.1/all')
    .then((response) => {
    	this.pays = response;
    	console.log("pays: " + this.pays);
    });
}
```

We can also display the data on the page, like the name for example :

```html
<div :key="id" v-for="(pays,id) in pays.data">
	<h2 >{{ id }} : {{ pays.name.common }}</li>
</div>
```

We can also catch the errors with the keyword **catch**, and handle it however we want.

````js
axios
   	.get('https://restcountries.com/v3.1/all')
    .then((response) => {
    	this.pays = response;
    	console.log("pays: " + this.pays);
    }).catch(error => {
    	console.log(error);
   		this.pays = null;
     	alert("Error while fetching data");
	});
````

## Request POST

The POST request is similar to the GET request, at the difference that we can send data to the API, in the second argument of the post() function.

```js
axios
   	.post('https://restcountries.com/v3.1/all', {
    	arg1: 'Hello',
    	arg2: 'World!'
	})
    .then((response) => {
    	this.pays = response;
    	console.log("pays: " + this.pays);
    }).catch(error => {
    	console.log(error);
   		this.pays = null;
     	alert("Error while sending data");
	});
```


