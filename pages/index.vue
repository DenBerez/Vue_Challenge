<template>
    <CBox
      v-bind="mainStyles[colorMode]"
      d="flex"
      flex-dir="column"
      justify-content="center"
      m="0 auto"
      position="absolute"
      top="0"
      left="0"
      right="0"
      bottom="0"
      user-select="none"
    >

      <CIconButton
        mr="3"
        position="fixed"
        top="5"
        left="5"
        :icon="colorMode === 'light' ? 'moon' : 'sun'"
        :aria-label="`Switch to ${
          colorMode === 'light' ? 'dark' : 'light'
        } mode`"
        @click="toggleColorMode"
      />

      <CFlex v-show="!loading" justify="center" direction="row" wrap="wrap" align="center">
        <Card v-for="pic in pics" :key="pic" :img="pic" ></Card>
      </CFlex>

    </CBox>
</template>

<script lang="js">
import {
  CBox,
  CIconButton,
  CFlex,
} from '@chakra-ui/vue';

import Card from './card';

import axios from 'axios';



export default {
  name: 'IndexPage',
  props: { img: String },
  components: {
    CBox,
    CIconButton,
    CFlex,
    Card,
  },
  inject: ['$chakraColorMode', '$toggleColorMode'],
  data () {
    return {
      mainStyles: {
        dark: {
          bg: 'gray.700',
          color: 'whiteAlpha.900'
        },
        light: {
          bg: 'white',
          color: 'gray.900'
        },
      },
      loading: true
    }
  },
  computed: {
    colorMode () {
      return this.$chakraColorMode()
    },
    theme () {
      return this.$chakraTheme()
    },
    toggleColorMode () {
      return this.$toggleColorMode
    }
  },
  mounted() {
    setTimeout(() => {this.loading = false}, 500);
  },
  async asyncData() {
    let x = 0;
    let urlArr = [];
    while (x < 10) {
      const {data} = await axios.get('https://dog.ceo/api/breeds/image/random');
      urlArr.push(data.message);
      x++;
    }
    return {
      pics: urlArr,
    }
  }
}
</script>

