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
    token: 'public-SE-2a0a290c184b1d840705fbb208fea241aa03322e34344b95'
  }),
  mounted() {
    this.isLoaded = document.getElementById(this.id)

    if (this.isLoaded) {
      return
    }

    this.$nextTick(() => {
      this.bootstrap()
    })
  },
  beforeDestroy() {
    this.$nextTick(() => {
      this.clear()
    })
  },
  methods: {
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
<style>
#collector-payment {
  display: none;
}
</style>
