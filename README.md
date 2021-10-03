# Vue3 Forms

## L2 Base Input

## V-model: Emitting the update:modelValue event

All components that are capable of being v-modeled have to emit an event in order for the parent to be able to catch the updates to that component’s data.

In Vue 3, by default all v-model contracts expect for your component to emit an update:modelValue event, regardless of what type of input, or inputs, your component contains.

Let’s go ahead and add an input event listener to our <input/> element, and emit an update:modelValue event whenever an input event occurs.

BaseInput.vue

```javaScript
<template>
  <label v-if="label">{{ label }}</label>
  <input
    :value="modelValue"
    :placeholder="label"
    @input="$emit('update:modelValue', $event.target.value)"
    class="field"
  >
</template>

<script>
export default {
  props: {
    label: {
      type: 'String',
      default: ''
    },  
    modelValue: {
      type: [String, Number],
      default: ''
    }
  }
}
</script>
```

Adding an @input listener to our input element allows us to fire-off the required event every time the user types something into the input field.

Notice that we are passing the event’s target value ($event.target.value)as the payload of the event. This is the value that the v-model will receive on the parent.

Speaking of the parent, let’s go back to our form and use our new BaseInput component instead of our native elements to test out our code. Let’s replace the Title, Description and Location inputs in our form with our new component.

App.vue

```javaScript
<form>
  <label>Select a category</label>
  <select v-model="event.category">
    <option
      v-for="option in categories"
      :value="option"
      :key="option"
      :selected="option === event.category"
    >{{ option }}</option>
  </select>

  <h3>Name & describe your event</h3>

  <BaseInput
    v-model="event.title"
    label="Title"
    type="text"
  />

  <BaseInput
    v-model="event.description"
    label="Description"
    type="text"
  />

  <h3>Where is your event?</h3>

  <BaseInput
    v-model="event.location"
    label="Location"
    type="text"
  />

  <h3>Are pets allowed?</h3>
  <div>
    <input
        type="radio"
        v-model="event.pets"
        :value="1"
        name="pets"
      />
    <label>Yes</label>
  </div>

  <div>
    <input
      type="radio"
      v-model="event.pets"
      :value="0"
      name="pets"
    />
    <label>No</label>
  </div>

  <h3>Extras</h3>
  <div>
    <input
      type="checkbox"
      v-model="event.extras.catering"
      class="field"
    />
    <label>Catering</label>
  </div>

  <div>
    <input
      type="checkbox"
      v-model="event.extras.music"
      class="field"
    />
    <label>Live music</label>
  </div>

  <button type="submit">Submit</button>
</form>
```

Our components seem to be “working”, but there seems to be a problem with the styles.

If we inspect the component further, it seems that our type attribute is nowhere to be found. We want to be able to assign attributes like type into the component’s input when we set them on the instance in the parent.

Let’s take a look at how to achieve this.

## Assigning the $attrs to the input

In Vue, whenever you pass down attributes, classes and styles from a parent to a child like we are doing with the type in our BaseInput component, Vue will attempt to automatically figure out where inside your template these attributes should be injected.

In components with a single wrapping element, also known as single root components, this behavior is very straightforward. Vue will simply inject all the attributes, classes and styles into the root element.

In multi-root components, such as our BaseInput, Vue can’t figure out without our help which one of the nodes, or fragments, it should inject the attributes to — so Vue simply gives up and issues a warning.

In the case of our BaseInput component, we want to be able to inject attributes directly into the input, so we have to manually bind the $attrs object to it. Let’s go ahead and do that now by adding v-bind="$attrs” to our input element.

```javaScript
<input
  v-bind="$attrs"
  :value="modelValue"
  :placeholder="label"
  @input="$emit('update:modelValue', $event.target.value)"
  class="field"
>
```

With this small change, the input elements will now correctly receive the type binding from the parent, and our CSS classes will be applied.

## L3 - Base Select

## L4 - Auto-importing components

Let’s navigate to the file main.js and take a look at what’s happening there together.

First we import createApp from Vue, in order to create and later mount our application. We also import the App.vue component to set it as the entry point of our application.

Please note that this is not specific to auto-importing components, but part of the process for creating and mounting our Vue 3 application.

Finally, we import upperFirst and camelCase from Lodash, a JavaScript library that provides utility functions for common programming tasks.

- upperCase converts the first character of a string to upper case
- camelCase converts a string to camel case (camelCase isWritten likeThis)

Next up, we use Webpack’s function require.context to allow webpack to know that it needs to require all of the files in the components folder that start with the Base prefix. Notice that we are also allowing it to require both .vueand .js extensions.

Further down into the code we’re going to call keys() on our requireComponent to get an array of fileNames, which will contain the route to each of our “Base” components.

We are going to loop through each of our components in this array. Ready?

First, we get the component object by using the filename to extract if from the array.

Next, we use Lodash’s upperFirst and camelCase functions to ensure that both file names written like BaseInput.vue and base-input.vue, or even baseInput.vue get turned into BaseInput.vue.

The regular expression here will strip anything before the component name and the extension, leaving us with just the name of the component BaseInput.

Finally, we use the component name that we just extracted and the component configuration that we got earlier to register the component as a global component.

We wrap up the file by mounting our application into the element with an id of app.

## L9 Basic a11y for our components

### Appropriate types

In HTML we have a wide variety of input elements to craft our forms, but one element in particular rules them all. The catch-all input allows us the flexibility of creating text inputs, but we can also transform it into checkboxes and radio buttons with the type property.

A common mistake is to ignore this type property when creating text inputs. Most of us know and commonly use two regularly: type email and password.

When using a specific type in an input element, not only do we get better autocompletion for our form, but it also allows screen readers to better understand what type of data we want to retrieve from the user. A type of tel for example, will provide the user on a mobile phone with a handy numeric keyboard with phone symbols like + * #.

Your users with mobility problems will definitely be grateful for this one!

Bottom line: Don’t forget to set your type, even when the input is not of type password or email.

Here is a list of the available types for an input element:

- button
- checkbox
- color
- date
- datetime-local
- email
- file
- hidden
- image
- month
- number
- password
- radio
- range
- reset
- search
- submit
- tel
- text
- time
- url
- week

### Use Fieldset and Legend

Two often overlooked or under-taught elements in HTML are fieldset and legend.

In forms, usually we group our inputs logically. For example, you would usually code your form to first ask the user for their personal data like Name, Last Name, and Phone. Later on, another section may ask them for a shipping address.

For accessible users, this information may not be as immediately available without having to tab through the whole form, this is where <fieldset> and <legend> come to play.

You should always try to wrap up sections of your form inside a fieldset element. This will logically group the inputs inside of it. Then, the first element of the fieldset will be a legend element which will provide a Title for that particular fieldset.

If for some reason you don’t want the legend to show on your form (usually because of design reasons), you can always position it absolutely, outside of the visible screen.

### Do NOT rely on placeholders

A popular design pattern that emerged a few years ago used the placeholder attribute of inputs to describe the type of content that the element was expecting. Sadly this is still sometimes used now-a-days instead of a proper label.

Placeholders should only be used to describe the intended value, but not as a replacement for a descriptive label. Placeholders disappear whenever a user starts typing into the field, forcing the user to keep in mind what that field was expecting. Additionally, some users can have problems differentiating between a field with a placeholder and a field that has pre-populated or filled content.

As far as screen readers go, each screen reader may treat the placeholder attribute differently, but as long as a correctly set label is in place, it shouldn’t be much of a concern to leave it in.

### Labels

Speaking about labels, let’s talk about a really powerful accessibility feature that is sadly very commonly underused, or misused, in forms.

If we navigate to FireFox again in the accessibility tab and inspect our Title input, we can see a ⚠️ icon right next to it. This means we have a problem.

Let’s take a look at the information panel. The Checks section is already telling us the issue: “Form elements should have a visible text label”

This may come as a surprise, since our Title field clearly has a label on top of it describing what we intend for this input.

For our sighted users, however, this is not evident. We have not yet linked these two HTML elements together, and that is an assumption that a screen reader cannot afford to make. Thankfully this is a very easy fix!

There are a few ways to link an input element with its label, the first one is to actually nest the input inside of the label element.

This is one of the easiest ways to make sure that your input is always correctly linked to the related label, but I want to go into depth into the second and usually more “common” way to relate HTML elements because it’s going to come in handy later when we look at error messages. This method involves using IDs.

Let’s jump directly into our BaseInput component and figure out how to create a relationship between our <label> and <input> by using an ID.

You may be thinking that perhaps the most obvious option would be to add a property, so that the parent can determine the id of the element, and then we don’t have to worry about it inside our component. And you would be right… But what if there were a way we could dynamically generate unique number identifiers for every component in our form without having to resort to manual props?

We are going to create a Vue 3 composable that allows us to create these dynamic unique identifiers, or UUIDs for short. I know this is a bit of a jump from the pace of the course, but if you need a refresher on Vue 3 composition API or composables we have a course titled Build a Gmail Clone with Vue 3 here on Vue Mastery to get you up to speed. At any rate, don’t worry too much, it’s going to be a really simple one.

If you’re following along with the repository files, I’ve gone ahead and created a UniqueID.js file inside the features folder.

UniqueID.js

```javaScript
let UUID = 0

export default function UniqueID () {
  const getID = () => {
    UUID++
    return UUID
  }

  return {
    getID
  }
}
```

First we declare a let variable with a default value of 0. This will increase as we create more and more components - the first component will have an id of 1, the second of 2, and so on.

We are going to export a function UniqueID. When executed, this function will return an object, which contains a function under the property getID. This function will increase by 1 the global UUID counter and return it.

Know also that there are plenty of UUID libraries out there that you can use in place of this custom solution, but I wanted to show you just how easy it can be.

Let’s look at this in action to better understand it, by looking at BaseInput.vue.

First, we are going to import our new composable.

BaseInput.vue

```JavaScript
<script>
import UniqueID from '../features/UniqueID'
export default { ... }
</script>
```

Now that we have it ready, we can generate a new unique ID inside our component’s setup method. Let’s go ahead and do that.

```BaseInput.vue
<script>
import UniqueID from '../features/UniqueID'
export default {
  props: { ... },
  setup () {
    const uuid = UniqueID().getID()
    return {
      uuid
    }
  }
}
</script>
```

Notice that we are executing both the UniqueIDcomposable, and then the getID method inside. This will give us a completely unique ID number every time a component is instantiated.

Finally, we return an object with the uuid so that we can use it in our template.

Speaking of which, let’s go back into the template and tie the label and input together.

To accomplish this, we need to give the input element an id attribute value. We will bind the id to our uuid. After we have the input setup with its own unique ID, we can now tell the label that its describing the input by setting the for attribute of the label.

Note: All of these are vanilla HTML attributes, no crazy Vue magic here other than the ease of binding all of them.

BaseInput.vue

```JavaScript
<template>
  <label :for="uuid" v-if="label">{{ label }}</label>
  <input
    v-bind="$attrs"
    :value="modelValue"
    :placeholder="label"
    @input="$emit('update:modelValue', $event.target.value)"
    class="field"
    :id="uuid"
  >
</template>
```

Let’s head back to the browser. The first thing I want to point out to you is that the warning sign on our field is gone. And if we check under the bit where it says relations inside the accessible properties, we can see now that it shows a new entry: labelledby: "Title". If you hover over this element you can now see in the browser which element exactly its referring to. Neat, right?

Now, if we take a look at the inspector tab, and we look at our input elements, we can see that they have automatically been assigned ids 1 and 2, respectively.

We still need to add a uuid to our Checkbox, Radio, and Select components. Are you up for a challenge? Try doing this bit yourself. It will be as straightforward as replicating exactly what we just did here with BaseInput.
