+++
title =  "Using data-* Custom Attributes for style agnostic UI tests in Vue.js"
date = 2020-02-12
+++

We can use a **"data-\*"** attribute in our html to make accessing DOM elements for our unit tests more easily. Grabbing elements based on id, class, or html symantics is completely viable, but it may also lead ot some unexpected unit test failures due to simple UI changes -- say the removal or renaming of a class. Thus using a custom data attribute allows us to still access the elements, but avoids the potential failure of a functional unit test when a style changes. Take the following example using Vue.js:

```html
// form.vue
<template>
  <div id="form">
    <input v-model="formText" type="text" />
    <button id="submit-button" @click="submitForm()">Submit</button>
  <div>
</template>

<script>
export default {
  name: "Form",
  date() {
    return {
      formText: '',
    }
  },
  methods: {
    submitForm() {
      this.formText = '';
    }
  }
}
</script>
```

```javascript
// from.spec.js
import form from '../components/form.vue';
import { shallowMount } from '@vue/test-utils';

describe('Submit Form', () => {
  it('Clears the text input on button click', () => {
    const wrapper = shallowMount(form);
    wrapper.vm.setData({formText: "My Mesage"}); // simulate entering some text
    const submitButton = wrapper.find('#submit-button');
    submitButton.trigger('click');

    expect(wrapper.vm.formText).equals('');
  });
});
```

When the user clicks the sublit button, we clear out the form, or rather change tha v-model binding to empty string which in turn clears out the input textbox. Now imagine a new requirement came in to build several more buttons. Now naming conventions no longer make sense because all of the buttons perform a submit adn need their own id's. Thus we need to rename our button id's to be more unique. 

This will cause our unit test to fail, even though we changed no functionality on the submit() method. A way to circumvent this is to write out template code in a way that makes it decoupled from our UI using the "data-*" attribute. 

Thus we can add the following to our submit button:

```html
<button 
  id="submit-button" 
  data-test="submitButton" 
  @click="submitForm()"
>Submit</button>
```

and update the selector in our test to find using the new data attribute:
```javascript
const submitButton = wrapper.find('[data-test="submitButton"]');
```

Now, if we ever need to go back and update the id on the submit we can rest assured it will not break any unit tests unnecessarily. 
