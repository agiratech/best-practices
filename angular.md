# Angular
Follow the official best practices for each category mentioned below
1. [Security](https://angular.io/guide/security)
2. [Accessibility](https://angular.io/guide/accessibility)
3. [Keeping upto date](https://angular.io/guide/updating#keeping-your-angular-projects-up-to-date)
4. [Property Binding](https://angular.io/guide/property-binding-best-practices)
5. [lazy loading](https://angular.io/guide/lazy-loading-ngmodules)
6. [injection token](https://angular.io/guide/lightweight-injection-tokens)

### Don't use `var` keyword. Use `const` keyword as default. If you want to change the value after initialization use `let`.

### Your components should always follow onPush change detection strategy
- This reduces the number of times the component has to be checked on changes, improving run time performance and reduces memory usage.
- There were only handful of cases where onPush will be problematic, those too could be solved with a manual cd cycle trigger. Discuss with team members if you can't use onPush in any situation.
- Always prefer `cdRef.markForCheck()` instead of `cdRef.detectChanges()`, let angular optimise the cd cycle instead of detecting changes immediately.

### Use moderately long, descriptive variable names. Don't shorten variable names for easier typing. Always think about readability first

### Don't call methods directly from interpolations `{{}}` in templates

{{ }} are known as interpolations and the expression inside it will be called every time a change is detected by angular. It may look simple but when you see how many times angular calls change detection you'll be amazed.  It is not recommended to call functions directly from template interpolations because that method will be called a lot of times. Instead, create a component property to store the result of the method and use that method in the template. Or create [Angular Pipe](https://angular.io/guide/pipes "https://angular.io/guide/pipes") if it is common function so we can use another components also.

### Always use camelCase naming convention for functions, variables, members, objects, etc. Don't use _ as a word separator when naming

### Always use capitalization CamelCase for naming classes.

### Write small testable functions instead of large monolithic functions. Ex: Try not to exceed 100 lines in a functions

### Add appropriate comments for function, class, and big logic code blocks.

### Remove commented code and debug logs.
- It’s best to remove the code that’s been commented out, because it often raises more questions than it’s resolves.
- If code is commented out and left in the codebase, an explanation should be created above explaining why it was commented out and not removed.
- Commented out code should not exist in a codebase for long. The code can always be found again in the Git history. You can use [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens "https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens") in your IDE to make browsing your Git history easier to do.

### Don't write duplicated logic in many places, try to abstract to a service when appropriate

### Use proper indentation in code.

### Write functions which can appropriately handle unexpected inputs.

### Embrace [solid principles](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa "https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa") .
- This is a huge concept to be explained in this guide, Please go through the above link.

### Write each component styles in its own stylesheet.
- For long-term maintenance, it's highly recommended to keep the styles on their own component. Don't worry about performance, the build process will bundle all the styles within JS.
- Write only reusable and common styles in a global stylesheet.

### Avoid deeply nested loops and conditionals
- A common rule of thumb is to limit your loops / Conditionals with one level depth. Anything more than that is just not easy to understand and modify.

### Understand and use the power of components.
- Components are very powerful and reusable easily, Don't write everything in a single, monolithic component.
- Try to write reusable components which can be plugged and played anywhere without much effort.

### Manage your dependencies Properly
- Try to keep the dependencies of any component within 2-4 classes
- Inject only those components & data which is absolutely necessary, Hide & Remove all other information which is not related to that class.
- If your component needs more than 5-6 classes and dependency, its probably doing a lot of things and should be broken down into sub-components. (According to Single Responsibility Principle).
- Review your list of dependencies once in a while to clean up unused dependencies and imports, this is important for tree-shaking (read about it).

### Don't convert observables into promises
- It is easy to fallback to what you already know. But observables are way more flexible and easier to chain/manipulate than promises.
- Learn to use observables the right way, Spend time reading rxjs documentation
- Try to work through observables and streams most of the times

### Don't create nested subscriptions
- Nested subscriptions are bad.
- Use [combinational operators](https://www.learnrxjs.io/learn-rxjs/operators/combination) such as mergeMap, SwitchMap, CombineLatest, etc, based on your need to nest the subscriptions.
- Use the [rxjs operator decision](https://rxjs.dev/operator-decision-tree) tree to decide

### Clean up after yourself
- If you have any subscriptions, unsubscribe to them using angular's [lifecycle hooks](https://angular.io/guide/lifecycle-hooks "https://angular.io/guide/lifecycle-hooks") to avoid memory leaks.
- Use RxJs built in `takeWhile` and `takeUntil` operators to manage subscriptions easily.
- Unset any long living objects when not needed to free up memory space.
- Unset/Destroy all used members.

## Use single quotes to define strings
There is no actual technical reason to do this but there are few things to consider.
- This will make typing HTML code with " easier to type without escaping (more on this later)
- This is the default settings on tslint so it'll stop complaining about "
- This saves you one keystroke over " because you don't have to press shift.
- Lot of other style guides recommend this style.
    

## Use code linting tool and fix linting and static analysis errors
- Whatever your code editor/IDE is, install eslint plugin for it and fix the linting and style errors it reports.

