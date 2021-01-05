<template>
  <div :id="containerId" style="width: 100%"></div>
</template>
<script>
export default {
  data: () => ({
    isLoaded: false,
    id: 'collector-payment',
    containerId: 'collector-payment-iframe',
    src: 'https://checkout-uat.collector.se/collector-checkout-loader.js',
    token: null,
    lang: null
  }),
  mounted() {
    this.isLoaded = document.getElementById(this.id)

    if (this.isLoaded) {
      return
    }

    this.$nextTick(async () => {
      await this.fetchConfig()
      this.bootstrap()
    })
  },
  beforeDestroy() {
    this.$nextTick(() => {
      this.clear()
    })
  },
  methods: {
    async fetchConfig() {
      const response = await fetch('http://api/payment-endpoint')
      const {token, lang, src} = response.json()

      this.token = token
      this.lang = lang
      this.src = src
    },
    bootstrap() {
      const { src, id, token, containerId } = this

      const script = document.createElement('script')
      script.setAttribute('src', src)
      script.setAttribute('id', id)
      script.setAttribute('data-container-id', containerId)
      script.setAttribute('data-token', token)
      script.setAttribute('data-lang', 'sv-SE')

      document.body.appendChild(script)
    },

    clear() {
      const script = document.getElementById(this.id)
      if (script) {
        script.remove()
      }
    }
  }
}
</script>
