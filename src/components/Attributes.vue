<template>
  <div class="card dark padding-right-tiny padding-top-tiny padding-left-tiny width-half">
    <h3 class="attributes margin-top-nano align-center">Attributes</h3>
    <Attribute :attribute="attributes.battle" />
    <Attribute :attribute="attributes.agility" />
    <Attribute :attribute="attributes.spirit" />
    <Attribute :attribute="attributes.knowledge" />
    <Attribute :attribute="attributes.physique" />
  </div>
</template>

<script>
  import Attribute from './Attribute.vue'
	import { attributes } from '../rules/characteristics/attributes'
	import { useStore } from '../stores/character'
  //import { diceRoll } from '../rules/rolls'
  //import { damageRoll } from '../rules/health'

  // console.log('diceRoll(6, 3, 1, 2): ', diceRoll(6, 3, 1, -1));

  export default {
		components: {
      Attribute
    },
    setup() {
      const character = useStore()
      return {
        attributes,
        character
      }
    }
  }
</script>


<style></style>
