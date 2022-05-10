<template>
  <CBox
    d="flex"
    flex-dir="column"
    justify-content="center"
    align-content="center"
    text-align="center"
    mb="3"
    border="2px solid"
    border-radius="5px"
    box-shadow="2px 2px gray"
    padding="1em"
    margin=".25em"
    w="18%"
    h="50%"
    position="relative"
    cursor="pointer"
    @click="toggle"
  >
    <CText :style="{opacity: toggled ? 1 : 0}" fontSize="xl" fontWeight="bold" bg="gray" margin="10px" border-radius="10px" h="3em" padding=".1em" vertical-align="center" overflow="hidden">{{dogImage.dogBreed}}</CText>
    <CBox
      d="flex"
      justify-content="center"
      align-content="center"
      max-width="100%"
    >
      <CImage
        :src="img"
        :style="{opacity: toggled ? 1 : 0}"
        alt="Dog"
        width="120"
        height="120"
        border-radius="5px"
        m="1em"
        draggable="false"
      >
      </CImage>
      <CImage
          :src="img"
          :style="{opacity: !toggled ? 1 : 0}"
          alt="Dog"
          border-radius="20px"
          position="absolute"
          maxW="96%"
          h="auto"
          maxH="96%"
          right="1"
          left="1"
          top="0"
          bottom="0"
          margin="auto auto"
          draggable="false"
      >
      </CImage>
    </CBox>
    <CText :style="{opacity: toggled ? 1 : 0}" fontSize="l" minH="6em">This is the {{dogImage.dogBreed}}.</CText>
  </CBox>
</template>

<script lang="js">
import {
  CBox,
  CHeading,
  CIconButton,
  CFlex,
  CText,
  CImage,
} from '@chakra-ui/vue'

export default {
  name: 'Card',
  components: {
    CBox,
    CHeading,
    CIconButton,
    CFlex,
    CText,
    CImage,
  },
  inject: ['$chakraColorMode', '$toggleColorMode'],
  props: ['img'],
  data () {
    return {
      toggled: false,
      mainStyles: {
        dark: {
          bg: 'gray.700',
          color: 'whiteAlpha.900'
        },
        light: {
          bg: 'white',
          color: 'gray.900'
        }
      },
      dogImage: ((img) => {
        let dBreedArrProcessed = [];
        let dTrimmed = img.slice(0, img.lastIndexOf('/'));
        let dBreedArr = dTrimmed.slice(dTrimmed.lastIndexOf('/') + 1).split('-').reverse();
        for (let i = 0; i < dBreedArr.length; i++) {
          let letters = dBreedArr[i].split('');
          letters[0] = letters[0].toUpperCase();
          dBreedArrProcessed.push(letters.join(''));
        }
        let dBreed = dBreedArrProcessed.join(' ');
        return {
          dogBreed: dBreed,
        }
      })(this.img),
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
  methods: {
    toggle() {
      this.toggled = !this.toggled;
    }
  },
}
</script>
