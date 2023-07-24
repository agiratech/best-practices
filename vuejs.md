# Vuejs

Vue.js is a popular JavaScript framework for building user interfaces. Here are some best coding practices for Vue.js that can help you write cleaner, more efficient code:

## 1. Follow the Vue.js style guide:

* The Vue.js team provides an official style guide that outlines best practices for coding in Vue.js. This guide covers everything from naming conventions to component structure. [Style guide](https://vuejs.org/style-guide/)

* **A gist of essential guide is listed below**
  * Multi word component name
  * Data must be a function
  * Detailed prop definition
  * Keyed v-for - always use key with v-for
  * Never use v-if on the same element as v-for
  * Component style scoping
  * Use module scoping to keep private functions inaccessible from the outside.

* **A gist of strongly recommended conventions listed below**
  * Split components
  * Single-file components should either be always PascalCase or dom templates should be kebab-case
  * Base component naming conventions - Should begin with  ‘App’ / ‘Base’ / ‘V’
  * Self closing components
    ```html
    <!-- In single-file components, string templates, and JSX -->
    <MyComponent/>

    <!-- In DOM templates -->
    <my-component></my-component>
    ```
  * Component name casing in js or jsx - Pascal case
  * Full word component names
  * Prop name casing - camelCase during declaration and kebab-case in templates and jsx
  * Multi attribute elements should span multiple lines
  * Complex expressions in templates should be refactored into computed properties or methods.
  * Complex computed properties should be split into as many simpler properties as possible.
  * Non-empty HTML attribute values should always be inside quotes (single or double, whichever is not used in JS).
  * Directive shorthands (: for v-bind:, @ for v-on: and # for v-slot) should be used always or never.

* For a list of other recommended conventions refer https://v2.vuejs.org/v2/style-guide/?redirect=true#Priority-C-Rules-Recommended-Minimizing-Arbitrary-Choices-and-Cognitive-Overhead.

* For a list of potentially dangerous patterns refer https://v2.vuejs.org/v2/style-guide/?redirect=true#Priority-D-Rules-Use-with-Caution-Potentially-Dangerous-Patterns

## 2. Keep component files small:
* Try to keep your component files small and focused. If a component starts to get too complex, consider breaking it up into smaller components.

## 3. Use computed properties:
* Computed properties are a powerful feature of Vue.js that allow you to calculate values based on other properties. Use computed properties to simplify your template code and keep your logic separate from your markup.

## 4. Use Vuex / Pinia for state management:
* Vuex / Pinia is a state management library for Vue.js that makes it easy to manage application-level state. If you have complex state management needs, consider using Vuex / Pinia.

## 5. Use v-bind and v-on shortcuts:
* Vue.js provides shortcuts for v-bind and v-on, which allow you to bind data to attributes and handle events with less boilerplate code.

By following these best practices, you can create clean, efficient, and maintainable code in Vue.js.

## Following are few more details on the best practices to follow:

### 1. [Always use key with v-for](https://learnvue.co/tutorials/vue-best-practices#1-always-use--inside-v-for)

* Using the key attribute with the v-for [directive](https://learnvue.co/2020/01/creating-your-first-vuejs-custom-directive/) helps your application be constant and predictable whenever you want to manipulate the data.

* This is necessary so that Vue can track your component state as well as have a constant reference to your different elements. An example where keys are extremely useful is when using animations or Vue transitions.

* Without keys, Vue will just try to make the DOM has efficient as possible. This may mean elements in the v-for may appear out of order or their behavior will be less predictable. If we have a _unique_ key reference to each element, then we can better predict how exactly our Vue application will handle DOM manipulation.

  ```html
  <template>
  <!-- BAD -->
  <div v-for="product in products">{{ product }}</div>
  
  <!-- GOOD! -->
  <div v-for="product in products" :key="product.id">
  {{ product }}
  </div>
  </template>
  ```

### 2. [Use kebab-case for events](https://learnvue.co/tutorials/vue-best-practices#2-use-kebab-case-for-events)

* When it comes to emitting custom events, it’s always best to use kebab-case. This is because in the parent component, that’s the same syntax we use to listen to that event.

* So for consistency across our components, and to make your code more readable, stick to using kebab-case in both places.

  ```javascript
  PopupWindow.vue

  this.$emit("close-window");
  ```

  ```html
  ParentComponent.vue

  <template>
  <popup-window @close-window='handleEvent()' />
  </template>
  ```

### 3. [Declare props with camelCase and use kebab-case in templates](https://learnvue.co/tutorials/vue-best-practices#3-declare-props-with-camelcase-and-use-kebab-case-in-templates)

* This best practice simply just follows the conventions for each language. In JavaScript, camelCase is the standard and in HTML, it’s kebab-case Therefore, we use them accordingly.

* Luckily for us, VueJS converts between kebab-case and camelCase for us so we don’t have to worry about anything besides actually declaring them.

* In JavaScript, camelCase is the standard and in HTML, it’s kebab-case Therefore, we use them accordingly.

  ```html
  <template>
  <PopupWindow title-text="hello world" />
  </template>
  <script>
  export default {
  props: {
    titleText: String,
  },
  }
  </script>
  ```

### 4. [Data should always return a function](https://learnvue.co/tutorials/vue-best-practices#4-data-should-always-return-a-function)

* When declaring component data in the Options API, the data option should always return a function. If it does not, and we just simply return an object, then that data will be shared across all instances of the component.

  ```javascript
  export default {
  data() {
    return {
      name: 'My Window',
      articles: [],
    }
  },
  }
  ```
* However, most of the time, the goal is to build reusable components, so we want each object to return a unique object. We accomplish this by returning our data object inside a function.

### 5. [Don’t use v-if with v-for elements](https://learnvue.co/tutorials/vue-best-practices#5-dont-use-v-if-with-v-for-elements)

* It’s super tempting to want to use v-if with v-for in order to filter elements of an array.

  ```html
  <!--BAD-->
  <div
    v-for='product in products'
    v-if='product.price < 500'
  >
  ```

* The problem with this is that VueJS prioritizes the v-for directive over the v-if directive. So under the hood, it loops through every element and THEN checks the v-if conditional.
This means that even if we only want to render a few elements from a list, we’ll have to loop through the entire array.

* This is no good.A smarter solution would be to iterate over a computed property. The above example would look something like this.
This means that even if we only want to render a few elements from a list, we’ll have to loop through the entire array.

  ```html
  <template>
  <div v-for="product in cheapProducts">{{ product }}</div>
  </template>
  <script>
  export default {
  computed: {
    cheapProducts: () => {
      return this.products.filter(function (product) {
        return product.price < 100
      })
    },
  },
  }
  </script>
  ```

* This is good for a few reasons.
  * Rendering is much more efficient because we don’t loop over every item
  * The filtered list will only be re-evaluated when a dependency changes
  * It helps separate our component logic from the template, making our component more readable

### 6. [Validate your props with good definitions](https://learnvue.co/tutorials/vue-best-practices#6-validate-your-props-with-good-definitions)

* This is arguably the most important best practice to follow.

* Well. It basically saves the future you from the current you. When designing a large scale project, it’s easy to forget exactly the exact format, type, and other conventions you used for a prop.

* And if you’re in a larger dev team, your coworkers aren’t mind-readers so make it clear to them how to use your components! So save everyone the hassle of having to painstakingly trace your component to determine a prop’s formatting and please just write prop validations.

  ```javascript
  export default {
  props: {
    status: {
      type: String,
      required: true,
      validator: function (value) {
        return (
          ['syncing', 'synced', 'version-conflict', 'error'].indexOf(value) !== -1
        )
      },
    },
  },
  }
  ```

### 7. [Use PascalCase or kebab-case for components](https://learnvue.co/tutorials/vue-best-practices#7-use-pascalcase-or-kebab-case-for-components)

* A common naming convention for components is to use PascalCase or kebab-case.

* No matter which one you choose for your project, it’s most important that 
you stay consistent all the time.

* PascalCase works best because it is supported by most IDE autocomplete features.

  ```
  # BAD
  mycomponent.vue 
  myComponent.vue 
  Mycomponent.vue

  # GOOD
  MyComponent.vue
  ```

### 8. [Stay consistent with your directive shorthand A common technique among](https://learnvue.co/tutorials/vue-best-practices#10-stay-consistent-with-your-directive-shorthand-a-common-technique-among)

* Vue developers use shorthand for directives. For example,

  ```
  @ is short for v-on:
  : is short for v-bind
  # is short for v-slot
  ```

* It is great to use these shorthands in your Vue project. But to create some sort of convention across your project, you should either always use them or never use them. This will make your project more cohesive and readable.

### 9. [Don’t call a method on created AND watch](https://learnvue.co/tutorials/vue-best-practices#11-dont-call-a-method-on-created-and-watch)

* A common mistake Vue developers make (or maybe it was just me) is they unnecessarily call a method created and watch.

* The thought behind this is that we want to run the watch hook as soon as a component is initialized.

  ```javascript
  <script>
    // BAD!
    export default {
      created: () {
        this.handleChange()
      },
      methods: {
        handleChange() {
          // stuff happens
        }
      },
      watch () {
        property() {
          this.handleChange()
        }
      }
    }
  </script>
  ```

* However, Vue has a built in solution for this. And it’s a property of Vue watchers that we often forget.

* All we have to do is restructure our watcher a little bit and declare two properties:
  * handler (newVal, oldVal) – this is our watcher method itself
  * immediate: true – this makes our handler run when our instance is created

  ```javascript
  <script>
    export default {
      methods: {
        handleChange() {
          // stuff happens
        }
      },
      watch () {
        property {
          immediate: true
          handler() {
            this.handleChange()
          }
        }
      }
    }
  </script>
  ```

### 10. [Template expressions should only have basic JavaScript expressions](https://learnvue.co/tutorials/vue-best-practices#12-template-expressions-should-only-have-basic-javascript-expressions)


* It’s natural to want to add as much inline functionality in your templates as possible. But this makes our template less declarative and more complex. Meaning that our template just gets extremely cluttered.

* For this, let’s check out another example from the Vue style guide.Look how confusing it is.

  ```html
  <template>
  <!--BAD-->
  {{
    fullName
      .split(' ')
      .map(function (word) {
        return word[0].toUpperCase() + word.slice(1)
      }).join(' ')
  }}
  </template>
  ```

* Basically, we want everything in our template to be intuitive as to what it is displaying. To keep this, we should refactor complex expressions into appropriately named component options. Another benefit of separating out complex expressions is that it means these values can be reused.

  ```javascript
  <template>{{ normalizedFullName }}</template>
  <script>
  export default {
  // The complex expression has been moved to a computed property
    computed: {
      normalizedFullName: function () {
        return this.fullName
          .split(' ')
          .map(function (word) {
            return word[0].toUpperCase() + word.slice(1)
          })
          .join(' ')
      },
    },
  }
  </script>
  ```

### 11. Avoid direct DOM manipulation

* When working with Vue, direct DOM manipulation is a no-go

  ```javascript
  yourDiv.className.add('my-class')
  ```

* Instead, you want to make use of refs, like so:

  ```javascript
  <template>
    <div class="my-div" ref="myDivRef">
      ....
    </div>
  </template>
  <script lang="ts">
  import { defineComponent, ref } from "vue";


  export default defineComponent({
    name: 'Index',
    setup() {
      const myDivRef = ref<HTMLElement>();
      onMounted(() => {
        myDivRef.value.classList.add('my-class');
      })
    }
  });
  </script>
  ```

### 12. It is not recommended to include a store directly in the state of another store:

* **Reactivity and State Management**: Vuex/Pinia provides a well-defined pattern for managing state mutations and actions. Including a store directly in the state breaks this pattern and can introduce reactivity issues. Changes made to the original store might not propagate correctly to the included store, leading to unpredictable behavior and potential bugs.

* **Encapsulation and Modularity**: Each store in Vuex / Pinia should ideally represent a separate module or domain of your application. By including the store data of one store in another, you break the encapsulation and modularity of your stores, making it harder to reason about their functionality and maintain them independently.

* **Data Consistency and Dependencies**: When one store directly includes the data of another store, you introduce a dependency between the two stores. If the included store's data changes, it can lead to inconsistent state and make it difficult to track down the source of the changes. This can result in bugs and make it harder to maintain and debug your application.

* **Performance and Efficiency**: Including the data of one store in another can lead to unnecessary reactivity and re-renders. When a change occurs in the included store's data, it triggers updates in both stores and potentially re-renders components unnecessarily. This can impact the performance and efficiency of your application.

* **Testing and Scalability**: Testing becomes more complex when stores depend on each other directly. Isolating and testing individual stores becomes harder, and it can hinder the scalability of your application as it grows. Maintaining a clear separation between stores makes testing and scaling easier.

* **Maintenance Challenges**: As your application grows, maintaining the relationship between stores becomes increasingly complex. You might face difficulties in tracking dependencies, debugging issues, and extending functionality due to the interdependence of the stores.

* Instead of including the store data of one store in another, it is recommended to design your stores to have clear responsibilities and utilize actions and mutations to communicate and coordinate between them when necessary. This approach promotes better modularity, separation of concerns, and maintainability in your Vue.js application.

### 13. Use Slots to Make Your Components Easier to Understand and More Powerful

* I recently wrote an article about some important things you need to know regarding slots in Vue.js. It highlights how slots can make your components more reusable and easier to maintain and why you should use them.

* But what does this have to do with large Vue.js projects? A picture is usually worth a thousand words, so I will paint you a picture about the first time I deeply regretted not using them.

* One day, I simply had to create a popup. Nothing really complex at first sight as it was just including a title, a description and some buttons. So what I did was to pass everything as props. I ended up with three props that you would use to customize the components and an event was emitted when people clicked on the buttons. Easy peasy!

* But, as the project grew over time, the team requested that we display a lot of other new things in it: form fields, different buttons depending on which page it was displayed on, cards, a footer, and the list goes on. I figured out that if I kept using props to make this component evolve, it would be ok. But god, how wrong I was! The component quickly became too complex to understand as it was including countless child components, using way too many props and emitting a large number of events. I came to experience that terrible situation in which when you make a change somewhere and somehow it ends up breaking something else on another page. I had built a Frankenstein monster instead of a maintainable component!

* However, things could have been better if I had relied on slots from the start. I ended up refactoring everything to come up with this tiny component. Easier to maintain, faster to understand and way more extendable!

  ```html
  <template>
    <div class="c-base-popup">
      <div v-if="$slots.header" class="c-base-popup__header">
        <slot name="header">
      </div>
      <div v-if="$slots.subheader" class="c-base-popup__subheader">
        <slot name="subheader">
      </div>
      <div class="c-base-popup__body">
        <h1>{{ title }}</h1>
        <p v-if="description">{{ description }}</p>
      </div>
      <div v-if="$slots.actions" class="c-base-popup__actions">
        <slot name="actions">
      </div>
      <div v-if="$slots.footer" class="c-base-popup__footer">
        <slot name="footer">
      </div>
    </div>
  </template>

  <script>
  export default {
    props: {
      description: {
        type: String,
        default: null
      },
      title: {
        type: String,
        required: true
      }
    }
  }
  </script>
  ```

* My point is that, from experience, projects built by developers who know when to use slots does make a big difference on its future maintainability. Way fewer events are being emitted, the code is easier to understand, and it offers way more flexibility as you can display whatever components you wish inside.

### 14. Use Actions to Make API Calls and Commit the Data

* Most of my API calls (if not all) are made inside my Vuex actions. You may wonder: why is that a good place to do so?

* Simply because most of them fetch the data I need to commit in my store. Besides, they provide a level of encapsulation and reusability I really enjoy working with. Here are some other reasons I do so:

* If I need to fetch the first page of articles in two different places (let's say the blog and the homepage), I can just call the appropriate dispatcher with the right parameters. The data will be fetched, committed and returned with no duplicated code other than the dispatcher call.

* If I need to create some logic to avoid fetching this first page when it has already been fetched, I can do so in one place. In addition to decreasing the load on my server, I am also confident that it will work everywhere.

* I can track most of my Mixpanel events inside these actions, making the analytics code base really easy to maintain. I do have some applications where all the Mixpanel calls are solely made in the actions. I can't tell you how much of a joy it is to work this way when I don't have to understand what is tracked from what is not and when they are being sent.

### 15. Simplify Your Code Base with mapState, mapGetters, mapMutations and mapActions

* There usually is no need to create multiple computed properties or methods when you just need to access your state/getters or call your actions/mutations inside your components. Using mapState, mapGetters, mapMutations and mapActions can help you shorten your code and make things easier to understand by grouping what is coming from your store modules in one place.

  ```javascript
  // NPM
  import { mapState, mapGetters, mapActions, mapMutations } from "vuex";

  export default {
    computed: {
      // Accessing root properties
      ...mapState("my_module", ["property"]),
      // Accessing getters
      ...mapGetters("my_module", ["property"]),
      // Accessing non-root properties
      ...mapState("my_module", {
        property: state => state.object.nested.property
      })
    },

    methods: {
      // Accessing actions
      ...mapActions("my_module", ["myAction"]),
      // Accessing mutations
      ...mapMutations("my_module", ["myMutation"])
    }
  };
  ```

### 16. Use API Factories

* I usually like to create a this.$api helper that I can call anywhere to fetch my API endpoints. At the root of my project, I have an api folder that includes all my classes (see one of them below).

  ```
  api
  ├── auth.js
  ├── notifications.js
  └── teams.js
  ```

* Each one is grouping all the endpoints for its category. Here is how I initialize this pattern with a plugin in my Nuxt applications (it is quite a similar process in a standard Vue app).

  ```javascript
  // PROJECT: API
  import Auth from "@/api/auth";
  import Teams from "@/api/teams";
  import Notifications from "@/api/notifications";

  export default (context, inject) => {
    if (process.client) {
      const token = localStorage.getItem("token");
      // Set token when defined
      if (token) {
        context.$axios.setToken(token, "Bearer");
      }
    }
    // Initialize API repositories
    const repositories = {
      auth: Auth(context.$axios),
      teams: Teams(context.$axios),
      notifications: Notifications(context.$axios)
    };
    inject("api", repositories);
  };
  JavaScript
  ```

  ```javascript
  export default $axios => ({
    forgotPassword(email) {
      return $axios.$post("/auth/password/forgot", { email });
    },

    login(email, password) {
      return $axios.$post("/auth/login", { email, password });
    },

    logout() {
      return $axios.$get("/auth/logout");
    },

    register(payload) {
      return $axios.$post("/auth/register", payload);
    }
  });
  ```

* Now, I can simply call them in my components or Vuex actions like this:

  ```javascript
  export default {
    methods: {
      onSubmit() {
        try {
          this.$api.auth.login(this.email, this.password);
        } catch (error) {
          console.error(error);
        }
      }
    }
  };
  ```

### 17. Always Freeze Your Package Versions When Your Project is in Production

* To avoid having to wake up in the middle of the night because one of your dependencies broke your entire project, locking all your package versions should make your mornings at work less stressful.

* What it means is simply this: avoid versions prefixed with ^:

  ```
  {
    "name": "my project",

    "version": "1.0.0",

    "private": true,

    "dependencies": {
      "axios": "0.19.0",
      "imagemin-mozjpeg": "8.0.0",
      "imagemin-pngquant": "8.0.0",
      "imagemin-svgo": "7.0.0",
      "nuxt": "2.8.1",
    },

    "devDependencies": {
      "autoprefixer": "9.6.1",
      "babel-eslint": "10.0.2",
      "eslint": "6.1.0",
      "eslint-friendly-formatter": "4.0.1",
      "eslint-loader": "2.2.1",
      "eslint-plugin-vue": "5.2.3"
    }
  }
  ```

### 18. Follow a Single Convention to Name Your Commits

* As the project grows, you will need to browse the history for your components on a regular basis. If your team does not follow the same convention to name their commits, it will make it harder to understand what each one does.

* I always use and recommend the Angular commit message guidelines. I follow it in every project I work on, and in many cases other team members are quick to figure out that it's better to follow too.

* Following these guidelines leads to more readable messages that make commits easier to track when looking through the project history. In a nutshell, here is how it works:

  ```
  git commit -am "<type>(<scope>): <subject>"

  # Here are some samples
  git commit -am "docs(changelog): update changelog to beta.5"
  git commit -am "fix(release): need to depend on latest rxjs and zone.js"
  ```
