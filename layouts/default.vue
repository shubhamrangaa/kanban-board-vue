<template>
  <CThemeProvider>
    <CColorModeProvider>
      <Nuxt />
    </CColorModeProvider>
  </CThemeProvider>
</template>

<script>
import { CThemeProvider, CReset, CButton } from "@chakra-ui/vue";
export default {
  data() {
    return {};
  },
  components: {
    CThemeProvider,
    CReset,
    CButton
  }
};
</script>

<style scoped></style>
