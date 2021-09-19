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
