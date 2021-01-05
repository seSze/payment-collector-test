<template>
  <div ref="iframe" style="width: 100%"></div>
</template>

<script>
export default {
  data: () => ({
    isLoaded: false,
    id: 'collector-payment',
    src: 'https://checkout-uat.collector.se/collector-checkout-loader.js',
    token: 'public-SE-2a0a290c184b1d840705fbb208fea241aa03322e34344b95'
  }),
  mounted() {
    this.isLoaded = document.getElementById(this.id)

    if (this.isLoaded) {
      return
    }

    this.bootstrap()
  },
  beforeDestroy() {
    this.$nextTick(() => {
      this.clear()
    })
  },
  methods: {
    bootstrap() {
      const { src, id, token } = this

      document.body.addEventListener('DOMNodeInserted', this.listenForIframe)

      const script = document.createElement('script')
      script.setAttribute('src', src)
      script.setAttribute('id', id)
      script.setAttribute('data-token', token)
      script.setAttribute('data-lang', 'sv-SE')

      document.body.appendChild(script)
    },
    listenForIframe(e) {
      const { target } = e
      if (target.className !== 'collector-checkout-iframe') {
        return
      }

      this.moveIframe(target)
      this.stopListeningForIframe()
    },
    stopListeningForIframe() {
      document.body.removeEventListener('DOMNodeInserted', this.listenForIframe)
    },
    moveIframe(iframe) {
      this.$refs.iframe.appendChild(iframe)
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
